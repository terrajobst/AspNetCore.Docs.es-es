---
uid: mvc/overview/getting-started/introduction/adding-search
title: "Búsqueda | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: a7664d16a056424ee51db2208152cb5d35d8e5d9
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2017
---
<a name="search"></a>Buscar
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>Agregar un método de búsqueda y la vista de búsqueda

En esta sección agregará capacidad de búsqueda para el `Index` método de acción que le permite buscar películas por género o por nombre.

## <a name="updating-the-index-form"></a>Actualizar el formulario de índice

Inicie actualizando el `Index` método de acción a la existente `MoviesController` clase. Este es el código:

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

La primera línea de la `Index` método crea las siguientes [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) consulta para seleccionar las películas:

[!code-csharp[Main](adding-search/samples/sample2.cs)]

La consulta se define en este momento, pero aún no se ha ejecutado en la base de datos.

Si el `searchString` parámetro contiene una cadena, se modificará la consulta de películas para filtrar según el valor de la cadena de búsqueda, utilizando el código siguiente:

[!code-csharp[Main](adding-search/samples/sample3.cs)]

El código `s => s.Title` anterior es una [expresión Lambda](https://msdn.microsoft.com/en-us/library/bb397687.aspx). Las lambdas se usan en basadas en métodos [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx) consultas como argumentos para los métodos de operador de consulta estándar, como el [donde](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.where.aspx) método usado en el código anterior. Las consultas LINQ no se ejecutan cuando se definen o cuando se modifican mediante una llamada a un método como `Where` o `OrderBy`. En su lugar, se aplaza la ejecución de la consulta, lo que significa que la evaluación de una expresión se retrasa hasta que su valor producida realmente se recorre en iteración o el [ `ToList` ](https://msdn.microsoft.com/en-us/library/bb342261.aspx) se llama al método. En el `Search` ejemplo, la consulta se ejecuta en el *Index.cshtml* vista. Para más información sobre la ejecución de consultas en diferido, vea [Ejecución de la consulta](https://msdn.microsoft.com/en-us/library/bb738633.aspx).

> [!NOTE]
> El [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) el método se ejecuta en la base de datos, no el código c# anterior. En la base de datos, [Contains](https://msdn.microsoft.com/en-us/library/bb155125.aspx) se asigna a [SQL como](https://msdn.microsoft.com/en-us/library/ms179859.aspx), que distingue mayúsculas de minúsculas.

Ahora puede actualizar el `Index` vista que se mostrará el formulario al usuario.

Ejecute la aplicación y vaya a */películas/índice*. Anexe una cadena de consulta como `?searchString=ghost` a la dirección URL. Se muestran las películas filtradas.

![SearchQryStr](adding-search/_static/image1.png)

Si cambia la firma de la `Index` método que tiene un parámetro denominado `id`, el `id` parámetro coincidirá la `{id}` enruta el marcador de posición para el valor predeterminado establecido en el *aplicación\_Start RouteConfig.cs* archivo.

[!code-json[Main](adding-search/samples/sample4.json)]

La versión original `Index` método tiene el siguiente aspecto::

[!code-csharp[Main](adding-search/samples/sample5.cs)]

Modificados `Index` método tendría el siguiente aspecto:

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

Ahora puede pasar el título de la búsqueda como datos de ruta (un segmento de dirección URL) en lugar de como un valor de cadena de consulta.

![](adding-search/_static/image2.png)

Sin embargo, no se puede esperar que los usuarios modifiquen la dirección URL cada vez que quieran buscar una película. Por lo que ahora se agregará la interfaz de usuario para ayudar a ellos filtrar películas. Si cambia la firma de la `Index` cambiarlo de método para probar cómo pasar el parámetro de identificador límite de ruta, modo que su `Index` método toma un parámetro de cadena denominado `searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

Abra la *Views\Movies\Index.cshtml* archivo y, justo después `@Html.ActionLink("Create New", "Create")`, agregue el marcado de formulario resaltado a continuación:

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

El `Html.BeginForm` auxiliar crea apertura `<form>` etiqueta. El `Html.BeginForm` auxiliar hace que el formulario registrar a sí mismo cuando el usuario envía el formulario, haga clic en el **filtro** botón.

Visual Studio 2013 tiene una mejora "nice" al mostrar y editar archivos de vista. Al ejecutar la aplicación con un archivo de vista abierta, Visual Studio 2013, se invoca el método de acción de controlador correcto para mostrar la vista.

![](adding-search/_static/image3.png)

Con la vista de índice abierto en Visual Studio (como se muestra en la imagen anterior), pulse Ctr F5 o F5 para ejecutar la aplicación y, a continuación, pruebe a buscar una película.

![](adding-search/_static/image4.png)

No hay ningún `HttpPost` sobrecarga de la `Index` método. No es necesario, porque el método no cambia el estado de la aplicación, simplemente filtrar datos.

Después, puede agregar el método `HttpPost Index` siguiente. En ese caso, coincidiría con el invocador de acción el `HttpPost Index` método y el `HttpPost Index` método ejecutaría tal como se muestra en la imagen siguiente.

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

Sin embargo, aunque agregue esta versión de `HttpPost` al método `Index`, hay una limitación en cómo se ha implementado todo esto. Supongamos que quiere marcar una búsqueda en particular o que quiere enviar un vínculo a sus amigos donde puedan hacer clic para ver la misma lista filtrada de películas. Tenga en cuenta que la dirección URL de la solicitud HTTP POST es el mismo que la dirección URL de la solicitud de obtención (localhost:xxxxx/películas/índice), no hay información de búsqueda en la propia dirección URL. Derecha ahora, la información de la cadena de búsqueda se envía al servidor como un valor de campo de formulario. Esto significa que no se puede capturar dicha información de búsqueda para marcar o enviar a amigos en una dirección URL.

La solución consiste en utilizar una sobrecarga de `BeginForm` que especifica que la solicitud POST debe agregar la información de búsqueda a la dirección URL y que se enrute a la `HttpGet` versión de la `Index` método. Reemplazar el existente sin parámetros `BeginForm` método con el siguiente marcado:

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

Ahora cuando se envía una búsqueda, la dirección URL contiene una cadena de consulta de búsqueda. La búsqueda también será dirigida al método de acción `HttpGet Index`, aunque tenga un método `HttpPost Index`.

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>Adición de búsqueda por género

Si agregó la `HttpPost` versión de la `Index` método elimina ahora.

A continuación, agregará una característica para que los usuarios puedan buscar películas por género. Reemplace el método `Index` con el código siguiente:

[!code-csharp[Main](adding-search/samples/sample11.cs)]

Esta versión de la `Index` método toma un parámetro adicional, es decir, `movieGenre`. Las primeras líneas de código crean un `List` objeto para mantener géneros de película de la base de datos.

El código siguiente es una consulta LINQ que recupera todos los géneros de la base de datos.

[!code-csharp[Main](adding-search/samples/sample12.cs)]

El código usa el `AddRange` método de la clase genérica `List` colección para agregar todos los géneros distintos a la lista. (Sin el `Distinct` modificador, se agregaría géneros duplicados, por ejemplo, se agregaría Comedia dos veces en nuestro ejemplo). El código, a continuación, almacena la lista de géneros en la `ViewBag.movieGenre` objeto. Almacenar datos de categoría (del tal un película género) como un [SelectList](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlist(v=vs.108).aspx) objeto en un `ViewBag`, a continuación, el acceso a los datos de categoría en un cuadro de lista desplegable es un enfoque típico para las aplicaciones de MVC.

El código siguiente muestra cómo comprobar el `movieGenre` parámetro. Si no está vacía, el código más restringe la consulta de películas para limitar las películas seleccionadas para el género especificado.

[!code-csharp[Main](adding-search/samples/sample13.cs)]

Como se mencionó anteriormente, la consulta no se ejecuta en la base de datos hasta que se recorre en iteración la lista de películas (lo que ocurre en la vista, después de la `Index` devuelve el método de acción).

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>Agregar marcas a la vista de índice para admitir la búsqueda por género

Agregar un `Html.DropDownList` auxiliar para la *Views\Movies\Index.cshtml* archivo, justo antes del `TextBox` auxiliar. A continuación se muestra el marcado completado:

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

En el código siguiente:

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

El parámetro "movieGenre" proporciona la clave para la `DropDownList` auxiliar para buscar un `IEnumerable<SelectListItem>` en el `ViewBag`. El `ViewBag` se rellena en el método de acción:

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

El parámetro "All" proporciona una etiqueta de opción. Si examina esta elección en el explorador, verá que el atributo "valor" está vacío. Puesto que nuestro controlador solo filtra `if` la cadena no es `null` o está vacía, enviando un valor vacío para `movieGenre` muestra todos los juegos.

También puede establecer una opción que se seleccione de forma predeterminada. Si deseara "Comedia" como opción de forma predeterminada, debería cambiar el código en el controlador de este modo:

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

Ejecute la aplicación y vaya a */películas/índice*. Intenta una búsqueda por género, nombre de la película y ambos criterios.

![](adding-search/_static/image8.png)

En esta sección se crean un método de acción de búsqueda y una vista que los usuarios pueden buscar por título de la película y el género. En la siguiente sección, examinará cómo agregar una propiedad a la `Movie` modelo y cómo agregar un inicializador que creará automáticamente una base de datos de prueba.

>[!div class="step-by-step"]
[Anterior](examining-the-edit-methods-and-edit-view.md)
[Siguiente](adding-a-new-field.md)
