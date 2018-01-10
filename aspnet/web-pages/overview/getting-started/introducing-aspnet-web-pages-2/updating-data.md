---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: "Introducción a ASP.NET Web Pages: actualizar la base de datos | Documentos de Microsoft"
author: tfitzmac
description: "Este tutorial muestra cómo actualizar la entrada de una base de datos existente (cambiar) cuando se usa ASP.NET Web Pages (Razor). Supone que ha completado la serie th..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: b016231975bf8d359f4c390b0b478edc383117d4
ms.sourcegitcommit: df2157ae9aeea0075772719c29784425c783e82a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2018
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Introducción a ASP.NET Web Pages: actualizar la base de datos
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo actualizar la entrada de una base de datos existente (cambiar) cuando se usa ASP.NET Web Pages (Razor). Supone que ha completado la serie a través de [introducción de datos por utilizando formularios utilizando ASP.NET Web Pages](entering-data.md).
> 
> Lo que aprenderá:
> 
> - Cómo seleccionar un registro individual en el `WebGrid` auxiliar.
> - Cómo leer un único registro de una base de datos.
> - Cómo cargar previamente un formulario con los valores del registro de base de datos.
> - Cómo actualizar un registro existente en una base de datos.
> - Cómo almacenar información en la página sin mostrarla.
> - Describe cómo usar un campo oculto para almacenar información.
>   
> 
> Características y tecnologías que se tratan:
> 
> - El `WebGrid` auxiliar.
> - La instrucción SQL `Update` comando.
> - Método `Database.Execute`.
> - Campos ocultos (`<input type="hidden">`).


## <a name="what-youll-build"></a>Lo que vamos a compilar

En el tutorial anterior, aprendió a agregar un registro a una base de datos. A continuación, aprenderá a mostrar un registro para su edición. En el *películas* página, podrá actualizar la `WebGrid` auxiliar por lo que muestra un **editar** vínculo situado junto a cada película:

![WebGrid mostrar con un vínculo "Editar" para cada película](updating-data/_static/image1.png)

Al hacer clic en el **editar** vínculo, le lleva a una página distinta, donde la información de películas ya está en un formulario:

![Editar página de la película que muestra la película a editarse](updating-data/_static/image2.png)

Puede cambiar cualquiera de los valores. Al enviar los cambios, el código en la página actualiza la base de datos y vuelve a la lista de películas.

Esta parte del proceso funciona casi exactamente igual que el *AddMovie.cshtml* página que ha creado en el tutorial anterior, por lo que gran parte de este tutorial le resultarán familiar.

Hay varias maneras podría implementar un medio para editar una película individual. El método que se muestra se ha elegido porque es fácil de implementar y fácil de entender.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Agregar un vínculo de edición a la lista de películas

Para empezar, deberá actualizar el *películas* página para que cada película lista también contiene un **editar** vínculo.

Abra la *Movies.cshtml* archivo.

En el cuerpo de la página, cambie la `WebGrid` marcado mediante la adición de una columna. Este es el marcado modificado:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

Esta es la nueva columna:

[!code-html[Main](updating-data/samples/sample2.html)]

El punto de esta columna es mostrar un vínculo (`<a>` elemento) cuyo texto dice "Editar". ¿Qué estamos después consiste en crear un vínculo que es similar a la siguiente cuando se ejecuta la página, con el `id` valor diferente para cada película:

[!code-css[Main](updating-data/samples/sample3.css)]

Este vínculo va a invocar una página denominada *EditMovie*, y pasará a la cadena de consulta `?id=7` a esa página.

La sintaxis para la nueva columna podría parecer un poco compleja, pero que es solo porque reúne varios elementos. Cada elemento individual es sencillo. Si concentrarse en la que solo los `<a>` elemento, vea este marcado:

[!code-html[Main](updating-data/samples/sample4.html)]

Algunos fondo acerca del funcionamiento de la cuadrícula: la cuadrícula muestra filas, una para cada registro de la base de datos, y muestra las columnas para cada campo en el registro de base de datos. Mientras se está construyendo cada fila de la cuadrícula, la `item` objeto contiene el registro de base de datos (elemento) para esa fila. Esta disposición ofrece una forma de código para tener acceso a los datos para esa fila. Que es lo que ve aquí: la expresión `item.ID` está obteniendo el valor de identificador del elemento de base de datos actual. Se podría obtener cualquiera de los valores de la base de datos (título, género o año) la misma manera mediante `item.Title`, `item.Genre`, o `item.Year`.

La expresión `"~/EditMovie?id=@item.ID` combina la parte modificable de la dirección URL de destino (`~/EditMovie?id=`) con este identificador dinámicamente derivada. (Vio el `~` operador en el tutorial anterior; es un operador de ASP.NET que representa la raíz del sitio Web actual.)

El resultado es que esta parte del marcado en la columna simplemente genera similar el siguiente marcado en tiempo de ejecución:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, el valor real del `id` será diferente para cada fila.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Crear una visualización personalizada para una columna de cuadrícula

Volvamos a la columna de cuadrícula. Las tres columnas que tenía originalmente en la cuadrícula muestra solo valores de datos (título, género y año). Especificar esta presentación pasando el nombre de la columna de base de datos &mdash; por ejemplo, `grid.Column("Title")`.

Esta nueva **editar** vínculo columna es diferente. En lugar de especificar un nombre de columna, que está pasando un `format` parámetro. Este parámetro le permite definir el marcado que la `WebGrid` auxiliar se representará junto con la `item` valor para mostrar los datos de columna como negrita o verde o en el formato que desee. Por ejemplo, si desea que el título aparece en negrita, podría crear una columna mostrado en este ejemplo:

[!code-html[Main](updating-data/samples/sample6.html)]

(Los distintos `@` caracteres que se ven en la `format` propiedad marcar la transición entre el marcado y un valor de código.)

Una vez que sepa sobre el `format` propiedad, resulta más fácil de entender cómo la nueva **editar** se junta columna vínculo:

[!code-html[Main](updating-data/samples/sample7.html)]

Consta de la columna *sólo* de marcado que representa el vínculo, además de cierta información (Id.) que se extrae del registro de base de datos de la fila.

> [!TIP] 
> 
> **Parámetros con nombre y parámetros posicionales para un método**
> 
> Muchas veces cuando se ha llamado a un método y parámetros que se pasan a él, simplemente ha enumerado los valores de parámetros separados por comas. A continuación se muestran un par de ejemplos:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> No lo hemos mencionado el problema cuando vio primero este código, pero en cada caso, está pasar parámetros a los métodos en un orden específico &mdash; ; es decir, el orden en que los parámetros se definen en ese método. Para `db.Execute` y `Validation.RequireFields`, si se combina el orden de los valores pasados, obtendría un mensaje de error cuando se ejecuta la página, o al menos resultados extraños. Claramente, es preciso conocer el orden para pasar los parámetros en. (En WebMatrix, IntelliSense puede ayudarle averiguar el nombre, tipo y orden de los parámetros.)
> 
> Como alternativa a pasar valores en orden, puede usar *parámetros con nombre*. (Pasar parámetros de orden se denomina mediante *parámetros posicionales*.) Para los parámetros con nombre, se incluye explícitamente el nombre del parámetro al pasar su valor. Se ha usado parámetros con nombre ya un número de veces en estos tutoriales. Por ejemplo:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> y
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Parámetros con nombre son útiles para un par de situaciones, especialmente cuando un método tiene demasiados parámetros. Una es cuando desee pasar solo uno o dos parámetros, pero no son los valores que van a pasar entre el primer posiciones en la lista de parámetros. Otra situación es cuando desea que el código sea más legible, pasando los parámetros en el orden en que mejor se relacione para usted.
> 
> Obviamente, para usar parámetros con nombre, tendrá que conocer los nombres de los parámetros. WebMatrix IntelliSense puede *mostrar* los nombres, pero no puede actualmente rellenarlo automáticamente.


## <a name="creating-the-edit-page"></a>Creación de la página de edición

Ahora puede crear el *EditMovie* página. Cuando el usuario hace clic en el **editar** vínculo, encontrará en esta página.

Crear una página denominada *EditMovie.cshtml* y sustituir lo que aparece en el archivo con el siguiente marcado:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Este código y marcado es similar a lo que tiene en el *AddMovie* página. Hay una pequeña diferencia en el texto para el botón Enviar. Al igual que con la *AddMovie* página, hay un `Html.ValidationSummary` llamada que se mostrará los errores de validación si existe alguna. Este tiempo se está omitiendo las llamadas a `Validation.Message`, ya que los errores se mostrarán en el resumen de validación. Como se indicó en el tutorial anterior, puede usar el resumen de validación y los mensajes de error individuales en diversas combinaciones.

Nuevo, observe que la `method` atributo de la `<form>` elemento está establecido en `post`. Al igual que con la *AddMovie.cshtml* página, esta página realiza cambios en la base de datos. Por lo tanto, debe realizar este formulario un `POST` operación. (Para obtener más información acerca de las diferencias entre `GET` y `POST` operaciones, consulte el [GET, POST y HTTP verbo seguridad](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) sidebar en el tutorial en formularios HTML.)

Como se vio en un tutorial anterior, el `value` se establecen atributos de los cuadros de texto con código Razor para cargarlas. Esta vez, sin embargo, usa variables como `title` y `genre` de esa tarea en lugar de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Como antes, esta marca se cargar previamente los valores del cuadro de texto con los valores de la película. Podrá ver en un momento ¿por qué resulta útil usar variables en este momento en lugar de utilizar el `Request` objeto.

También hay un `<input type="hidden">` elemento en esta página. Este elemento almacena el identificador de película sin hacerlo visible en la página. El identificador inicialmente se pasa a la página mediante utilizando un valor de cadena de consulta (`?id=7` o similar en la dirección URL). Al colocar el valor de identificador en un campo oculto, puede asegurarse de que está disponible cuando se envía el formulario, incluso si ya no tiene acceso a la dirección URL original que se ha invocado la página con.

A diferencia de la *AddMovie* página, el código de la *EditMovie* página tiene dos funciones distintas. La primera función es que cuando se muestra la página por primera vez (y *sólo* , a continuación,), el código obtiene el identificador de película de la cadena de consulta. El código, a continuación, utiliza el Id. para leer la película correspondiente de la base de datos y mostrar (precargar), en los cuadros de texto.

La segunda función es que cuando el usuario hace clic en el **enviar cambios** botón, el código tiene que leer los valores de los cuadros de texto y validarlos. El código también tiene que actualizar el elemento de la base de datos con los nuevos valores. Esta técnica es similar a agregar un registro, tal y como se vio en *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Agregar código para leer una sola película

Para llevar a cabo la primera función, agregue este código a la parte superior de la página:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

La mayor parte de este código es dentro de un bloque que se inicia `if(!IsPost)`. El `!` operador significa "no", por lo que significa que la expresión *si esta solicitud no es un envío de post*, que es una forma indirecta de decir *si esta solicitud es la primera vez que esta página se ha ejecutado*. Como se indicó anteriormente, este código debe ejecutarse *sólo* la primera vez que se ejecuta la página. Si no incluye el código en `if(!IsPost)`, podría ejecutar cada vez que se invoca la página, si la primera vez o en respuesta a un botón, haga clic en.

Tenga en cuenta que el código incluye un `else` bloquear este momento. Como dijimos cuando introdujimos `if` bloques, a veces, desea ejecutar código alternativo si la condición que se está probando no es cierto. Es el caso aquí. Si la condición pasa (es decir, si el identificador pasado a la página es correcto), leer una fila de la base de datos. Sin embargo, si no pasa la condición, el `else` bloque se ejecuta y el código establece un mensaje de error.

## <a name="validating-a-value-passed-to-the-page"></a>Validar un valor pasado a la página

El código usa `Request.QueryString["id"]` para obtener el identificador que se pasa a la página. El código se asegura de que realmente se pasó un valor para el identificador. Si se pasa ningún valor, el código establece un error de validación.

Este código muestra una manera diferente para validar la información. En el tutorial anterior, con la que trabajó el `Validation` auxiliar. Registrado campos para validar y ASP.NET automáticamente no la validación y muestran errores mediante `Html.ValidationMessage` y `Html.ValidationSummary`. En este caso, sin embargo, realmente no está validando proporcionados por el usuario. En su lugar, que se está validando un valor que se pasó a la página desde cualquier otro punto. El `Validation` auxiliar no hacerlo.

Por lo tanto, compruebe el valor usted mismo, probándolo con `if(!Request.QueryString["ID"].IsEmpty()`). Si hay un problema, puede mostrar el error mediante `Html.ValidationSummary`, tal y como lo hizo con el `Validation` auxiliar. Para ello, se llama a `Validation.AddFormError` y pasarle un mensaje para mostrar. `Validation.AddFormError`es un método integrado que permite definir mensajes personalizados que asociar con el sistema de validación que ya está familiarizado. (Más adelante en este tutorial hablaremos sobre cómo realizar este proceso de validación un poco más sólidas.)

Después de asegurarse de que hay un identificador para la película, el código lee la base de datos, buscando solo un elemento único de la base de datos. (Es probable que haya observado el patrón general para las operaciones de base de datos: abra la base de datos, defina una instrucción SQL y ejecute la instrucción.) Esta vez, la instrucción SQL `Select` instrucción incluye `WHERE ID = @0`. Dado que el identificador es único, se puede devolver un único registro.

La consulta se realiza mediante `db.QuerySingle` (no `db.Query`, tal como utiliza para obtener una lista de películas), y el código sitúa el resultado en la `row` variable. El nombre `row` es arbitrario; puede designar las variables que desee. Las variables que se inicializan en la parte superior, a continuación, se rellenan con los detalles de la película para que estos valores se pueden mostrar en los cuadros de texto.

## <a name="testing-the-edit-page-so-far"></a>Probar la página de edición (hasta ahora)

Si desea probar la página, ejecute el *películas* página ahora y haga clic en un **editar** vínculo situado junto a cualquier película. Verá el *EditMovie* página con los detalles se rellena para la película que ha seleccionado:

![Editar página de la película que muestra la película a editarse](updating-data/_static/image3.png)

Tenga en cuenta que la dirección URL de la página incluye algo parecido a `?id=10` (u otro número). Hasta ahora ha probado que **editar** vincula el *película* página trabajo, que la página está leyendo el identificador de la cadena de consulta y que la base de datos de consulta para obtener un registro único de película funciona.

Puede cambiar la información de la película, pero no ocurre nada al hacer clic en **enviar cambios**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Agregar código para actualizar la película con los cambios del usuario

En el *EditMovie.cshtml* de archivos, para implementar la segunda función (guardar los cambios), agregue el código siguiente justo dentro de la llave de cierre de la `@` bloque. (Si no está seguro de exactamente dónde colocar el código, puede mirar la [el código completo de la página Editar película](#Complete_Page_Listing_for_EditMovie) que aparece al final de este tutorial.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Una vez más, es similar al código de este código y marcado *AddMovie*. El código está en un `if(IsPost)` bloquear, dado que este código se ejecuta sólo cuando el usuario hace clic en el **enviar cambios** botón &mdash; es decir, cuando (y sólo cuando) se ha registrado el formulario. En este caso, no usa una prueba como `if(IsPost && Validation.IsValid())`, es decir, no está combinar ambas pruebas mediante el uso de and. En esta página, primero determinar si hay un envío de formulario (`if(IsPost)`) y solo, a continuación, registre los campos para la validación. A continuación, puede probar los resultados de validación (`if(Validation.IsValid()`). El flujo es ligeramente distinto en el *AddMovie.cshtml* página, pero el efecto es el mismo.

Obtener los valores de los cuadros de texto mediante el uso de `Request.Form["title"]` y un código similar para los demás `<input>` elementos. Tenga en cuenta que esta vez, el código obtiene el identificador de película fuera del campo oculto (`<input type="hidden">`). Cuando la página ejecutó la primera vez, el código tiene el identificador de la cadena de consulta. Obtener el valor del campo oculto para asegurarse de que está obteniendo el Id. de la película que aparecía originalmente, en caso de que la cadena de consulta se modifica de alguna manera desde entonces.

La diferencia entre lo realmente importante la *AddMovie* código y este código es que en este código se usa la instrucción SQL `Update` instrucción en lugar de la `Insert Into` instrucción. En el ejemplo siguiente se muestra la sintaxis de la instrucción SQL `Update` instrucción:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Puede especificar las columnas en cualquier orden, y no necesariamente deben actualizar todas las columnas durante una `Update` operación. (No se puede actualizar el Id. de sí misma, dado que guarde en vigor el registro como un nuevo registro, y que no se permite para un `Update` operación.)

> [!NOTE] 
> 
> **Importante** el `Where` cláusula con el Id. es muy importante, ya que ese es cómo la base de datos sepa qué base de datos de registro que desea actualizar. Si lo dejó el `Where` cláusula, la base de datos actualizaría *cada* registros en la base de datos. En la mayoría de los casos, que sería un desastre.


En el código, los valores para actualizar se pasan a la instrucción SQL con marcadores de posición. Para repetir lo que hemos dicho antes: por motivos de seguridad, *sólo* utilizar marcadores de posición para pasar valores a una instrucción SQL.

Después de que el código usa `db.Execute` para ejecutar el `Update` instrucción, redirige a la página de lista, donde puede ver los cambios.

> [!TIP] 
> 
> **Instrucciones SQL diferente, distintos métodos**
> 
> Puede que haya observado que usar métodos ligeramente diferentes para ejecutar instrucciones SQL diferentes. Para ejecutar un `Select` es potencialmente consulta devuelve varios registros, usar el `Query` método. Para ejecutar un `Select` consulta que sepa que devolverá solo un elemento de la base de datos, usar el `QuerySingle` método. Para ejecutar comandos que realizar cambios, pero que no devuelven los elementos de la base de datos, use la `Execute` método.
> 
> Tendrá que tienen métodos diferentes debido a que cada uno de ellos devuelve resultados diferentes, tal y como ya se mostró la diferencia entre `Query` y `QuerySingle`. (El `Execute` método realmente devuelve un valor también &mdash; ; es decir, el número de filas de la base de datos que se vieron afectados por el comando &mdash; pero se ha ha omitiendo que hasta ahora.)
> 
> Por supuesto, la `Query` método puede devolver una sola fila de la base de datos. Sin embargo, ASP.NET siempre trata los resultados de la `Query` método como una colección. Aunque el método devuelve una sola fila, tendrá que extraer esa única fila de la colección. Por lo tanto, en situaciones donde se *saber* obtendrá una sola fila, es un poco más conveniente utilizar `QuerySingle`.
> 
> Existen algunos otros métodos que realizan determinados tipos de operaciones de base de datos. Puede encontrar una lista de métodos de la base de datos en el [referencia rápida de ASP.NET Web Pages API](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Realizar la validación de Id. de más sólida

La primera vez que se ejecuta la página, obtendrá el identificador de película de la cadena de consulta para que puedan ir obtener esa película de la base de datos. Se haya asegurado de que realmente se ha producido un valor para ir a buscar, que hizo utilizando este código:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Utilice este código para asegurarse de que si un usuario llega a la *EditMovies* página sin activar primero una película en la *películas* página, la página mostrará un mensaje de error descriptivo. (En caso contrario, los usuarios verían un error que probablemente confundirían a ellos).

Sin embargo, esta validación no es muy eficaz. La página también se puede invocar con estos errores:

- El identificador no es un número. Por ejemplo, se puede invocar la página con una dirección URL como `http://localhost:nnnnn/EditMovie?id=abc`.
- El identificador es un número, pero hace referencia a una película que no existe (por ejemplo, `http://localhost:nnnnn/EditMovie?id=100934`).

Si tiene curiosidad ver los errores que se derivan de estas direcciones URL, ejecute el *películas* página. Seleccione una película para editar y, a continuación, cambie la dirección URL de la *EditMovie* página a una dirección URL que contiene un carácter alfabético, identificador o el identificador de una película inexistente.

¿Qué debe hacer? La primera corrección es asegurarse de que no solo es un Id. de pasar a la página, pero que el identificador es un entero. Cambiar el código para el `!IsPost` prueba debe ser similar a este ejemplo:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Ha agregado una segunda condición para la `IsEmpty` prueba, vinculado con `&&` (AND lógico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Quizá recuerde de la [Introducción a la programación de páginas Web de ASP.NET](../introducing-razor-syntax-c.md) tutorial que métodos como `AsBool` un `AsInt` convertir una cadena de caracteres en algún otro tipo de datos. El `IsInt` método (y otros, como `IsBool` y `IsDateTime`) son similares. Sin embargo, se prueban solo si se *puede* convertir la cadena, sin necesidad de realizar la conversión. Aquí está diciendo básicamente *si el valor de cadena de consulta se puede convertir en un entero de...* .

El posible problema está buscando una película que no existe. El código para obtener una película aspecto este código:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Si se pasa un `movieId` valor para el `QuerySingle` método que no se corresponde con una película real, no se devuelve nada y las instrucciones que siguen (por ejemplo, `title=row.Title`) dar lugar a errores.

Nuevo hay una solución fácil. Si el `db.QuerySingle` método no devuelve ningún resultado, el `row` variable será null. Por lo que puede comprobar si el `row` variable es null antes de intentar obtener valores de él. El código siguiente agrega un `if` bloque alrededor de las instrucciones que obtienen los valores de la `row` objeto:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Con estos dos pruebas de validación adicional, la página pasa a ser más infalibles. El código completo de la `!IsPost` bifurcación ahora es similar a este ejemplo:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Se podrá observar una vez más que esta tarea es un buen uso de un `else` bloque. Si no superan las pruebas, el `else` conjunto de bloques de mensajes de error.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Si agrega un vínculo para volver a la página de películas

Un detalle final y útil consiste en Agregar un vínculo a la *películas* página. En el flujo normal de eventos, los usuarios comenzarán en el *películas* página y haga clic en un **editar** vínculo. Que pone la *EditMovie* página, donde puede editar la película y haga clic en el botón. Una vez que el código procese el cambio, redirige a la *películas* página.

Sin embargo:

- El usuario puede decidir no cambia nada.
- El usuario puede aparecer en esta página sin hacer clic en un **editar** vincular en el *películas* página.

En cualquier caso, desea que resulte sencillo para que puedan volver a la lista principal. Es una solución fácil &mdash; agregue el siguiente marcado después de la cierre `</form>` etiqueta en el marcado:

[!code-html[Main](updating-data/samples/sample19.html)]

Este marcado utiliza la misma sintaxis para una `<a>` elemento que ha visto en otro lugar. La dirección URL incluye `~` para indicar "raíz del sitio Web".

## <a name="testing-the-movie-update-process"></a>Probar el proceso de actualización de película

Ahora puede probar. Ejecute el *películas* página y haga clic en **editar** situada junto a una película. Cuando el *EditMovie* aparece en la página, realizar cambios en la película y haga clic en **enviar cambios**. Cuando aparezca la lista de películas, asegúrese de que se muestran los cambios.

Para asegurarse de que funciona la validación, haga clic en **editar** para otra película. Cuando se obtiene la *EditMovie* página, desactive la **género** campo (o **año** campo, o ambos) e intenta enviar los cambios. Verá un error, tal como se esperaría:

![Editar página de la película que muestra los errores de validación](updating-data/_static/image4.png)

Haga clic en el **volver a la lista de películas** vínculo para descartar los cambios y volver a la *películas* página.

## <a name="coming-up-next"></a>Próxima

En el siguiente tutorial, verá cómo eliminar un registro de la película.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Muestra la lista completa de página de la película (actualizada con vínculos de edición)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Completar la lista de la página para editar página de la película

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Recursos adicionales

- [Introducción a la programación Web de ASP.NET mediante la sintaxis de Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrucción UPDATE de SQL](http://www.w3schools.com/sql/sql_update.asp) en el sitio W3Schools

>[!div class="step-by-step"]
[Anterior](entering-data.md)
[Siguiente](deleting-data.md)
