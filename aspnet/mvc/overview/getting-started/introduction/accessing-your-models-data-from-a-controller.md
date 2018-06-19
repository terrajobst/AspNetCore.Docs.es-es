---
uid: mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
title: Obtiene acceso a datos de su modelo desde un controlador | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: caa1ba4a-f9f0-4181-ba21-042e3997861d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: d3dfa079c334e04f368531456ec2ec4e9728f893
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873562"
---
<a name="accessing-your-models-data-from-a-controller"></a>Obtiene acceso a datos de su modelo desde un controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

En esta sección, creará un nuevo `MoviesController` clase y escribir código que recupera los datos de la película y lo muestra en el explorador utilizando una plantilla de vista.

**Compile la aplicación** antes de pasar al paso siguiente. Si no compila la aplicación, obtendrá un error al agregar un controlador.

En el Explorador de soluciones, haga clic en el *controladores* carpeta y, a continuación, haga clic en **agregar**, a continuación, **controlador**.

![](accessing-your-models-data-from-a-controller/_static/image1.png)

En el **agregar scaffolding** cuadro de diálogo, haga clic en **controlador de MVC 5 con vistas, mediante Entity Framework**y, a continuación, haga clic en **agregar**.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

- Seleccione **película (MvcMovie.Models)** para la clase del modelo.
- Seleccione **MovieDBContext (MvcMovie.Models)** para la clase de contexto de datos.
- Para el nombre del controlador, escriba **MoviesController**.

  La imagen siguiente muestra el cuadro de diálogo completada.  
  
![](accessing-your-models-data-from-a-controller/_static/image3.png)   

Haga clic en **Agregar**. (Si se produce un error, probablemente no compile la aplicación antes de comenzar a agregar el controlador.) Visual Studio crea los siguientes archivos y carpetas:

- *Un MoviesController.cs* un archivo en el *controladores* carpeta.
- A *Views\Movies* carpeta.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, y *Index.cshtml* en el nuevo *Views\Movies* carpeta.

Visual Studio creada automáticamente la [CRUD](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (crear, leer, actualizar y eliminar) los métodos de acción y vistas por usted (la creación automática de los métodos de acción CRUD y vistas se conoce como scaffolding). Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar las entradas de la película.

Ejecute la aplicación y haga clic en el **MVC película** vínculo (o vaya a la `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador). Dado que la aplicación es de confianza en la ruta predeterminada (definidos en el *aplicación\_Start\RouteConfig.cs* archivo), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta hacia el valor predeterminado `Index` método de acción de la `Movies` controlador. En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es lo mismo que la solicitud de explorador `http://localhost:xxxxx/Movies/Index`. El resultado es una lista vacía de películas, porque no agregó ningún todavía.

![](accessing-your-models-data-from-a-controller/_static/image4.png)

### <a name="creating-a-movie"></a>Crear una película

Seleccione el vínculo **Crear nuevo**. Escriba algunos detalles acerca de una película y, a continuación, haga clic en el **crear** botón.


![](accessing-your-models-data-from-a-controller/_static/image5.png)

> [!NOTE]
> Es podrán que no pueda escribir decimales ni comas en el campo Precio. para admitir la validación de jQuery para configuraciones regionales no inglesas que usan una coma (&quot;,&quot;) para obtener un separador decimal y formatos de fecha no es inglés de Estados Unidos, debe incluir *globalize.js* y específica de su  *Cultures/globalize.Cultures.js* archivo (de [ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) y JavaScript para usar `Globalize.parseFloat`. Voy a explicar cómo hacerlo en el siguiente tutorial. Por ahora, escriba solamente números enteros como 10.


Al hacer clic en el **crear** botón hace que el formulario se envía al servidor, donde se guarda la información de la película en la base de datos. A continuación, se le redirigirá a la */Movies* dirección URL, donde puede ver la película recién creada en la lista.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Cree un par de entradas más de película. Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.

## <a name="examining-the-generated-code"></a>Examinar el código generado

Abra la *Controllers\MoviesController.cs* archivo y examine la generado `Index` método. Una parte del controlador de película con la `Index` método se muestra a continuación.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

Una solicitud para la `Movies` controlador devuelve todas las entradas de la `Movies` de tabla y, a continuación, envía los resultados a la `Index` vista. La siguiente línea de la `MoviesController` clase crea una instancia de un contexto de base de datos de la película, como se describió anteriormente. Puede utilizar el contexto de base de datos de la película para consultar, modificar y eliminar películas.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

## <a name="strongly-typed-models-and-the-model-keyword"></a>Establecimiento inflexible de tipos modelos y la @model (palabra clave)

Anteriormente en este tutorial, ha visto cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante la `ViewBag` objeto. El `ViewBag` es un objeto dinámico que proporciona una manera cómoda de tiempo de ejecución para pasar información a una vista.

MVC también proporciona la capacidad de pasar *fuertemente* con tipo de objetos a una plantilla de vista. Este enfoque fuertemente tipado permite un mejor tiempo de compilación más enriquecida y comprobación del código [IntelliSense](https://msdn.microsoft.com/library/hcw1s69b(v=vs.120).aspx) en el editor de Visual Studio. El mecanismo de scaffolding en Visual Studio usa este enfoque (es decir, pasando una *fuertemente* modelo con tipo) con el `MoviesController` plantillas de clase y la vista cuando crea los métodos y las vistas.

En el *Controllers\MoviesController.cs* archivo examinar generado `Details` método. El `Details` método se muestra a continuación.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs)]

El `id` parámetro suele pasarse como datos de ruta, por ejemplo `http://localhost:1234/movies/details/1` establecerá el controlador para el controlador de película, la acción que `details` y `id` en 1. También podría pasar en el identificador con una cadena de consulta como se indica a continuación:

`http://localhost:1234/movies/details?id=1`

Si un `Movie` se encuentra una instancia de la `Movie` modelo se pasa a la `Details` vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample4.cs)]

Examine el contenido de la *Views\Movies\Details.cshtml* archivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.cshtml?highlight=1-2)]

Mediante la inclusión de un `@model` instrucción en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera de la vista. Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado. Por ejemplo, en la *Details.cshtml* plantilla, el código pasa cada campo de la película a la `DisplayNameFor` y [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) aplicaciones auxiliares de HTML con fuertemente tipado `Model` objeto. El `Create` y `Edit` métodos y las plantillas de vista pasan un objeto de modelo de la película.

Examine la *Index.cshtml* plantilla de vista y el `Index` método en el *MoviesController.cs* archivo. Observe cómo el código crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto cuando se llama a la `View` método auxiliar en el `Index` método de acción. A continuación, el código pasa esto `Movies` lista desde el `Index` método de acción a la vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample7.cs?highlight=3)]

Al crear el controlador de película, Visual Studio incluye automáticamente los siguientes `@model` instrucción en la parte superior de la *Index.cshtml* archivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample8.cshtml)]

Esto `@model` directiva permite obtener acceso a la lista de películas que el controlador pasa a la vista mediante un `Model` objeto fuertemente tipado. Por ejemplo, en la *Index.cshtml* plantilla, el código recorre las películas de manera práctica un `foreach` instrucción sobre fuertemente tipado `Model` objeto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample9.cshtml?highlight=1,4,7,10,13,16,19-21)]

Dado que la `Model` objeto está fuertemente tipado (como un `IEnumerable<Movie>` objeto), cada `item` objeto en el bucle se escribe como `Movie`. Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y compatibilidad con IntelliSense en el editor de código completa:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image8.png)

## <a name="working-with-sql-server-localdb"></a>Trabajar con SQL Server LocalDB

Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó que apunta a un `Movies` base de datos que no existe todavía, por lo que Code First crea la base de datos automáticamente. Puede comprobar que se ha creado mediante una búsqueda en el *aplicación\_datos* carpeta. Si no ve el *Movies.mdf* de archivos, haga clic en el **mostrar todos los archivos** botón en el **el Explorador de soluciones** barra de herramientas, haga clic en el **actualizar** botón y, a continuación, expanda el *aplicación\_datos* carpeta.

![](accessing-your-models-data-from-a-controller/_static/image9.png)

Haga doble clic en *Movies.mdf* para abrir **Explorador de servidores**, a continuación, expanda el **tablas** carpeta para ver la tabla de películas. Tenga en cuenta el icono de llave junto al identificador. De forma predeterminada, EF hará que una propiedad denominada Id. de la clave principal. Para obtener más información sobre EF y MVC, vea el tutorial excelente de Tom Dykstra en [MVC y EF](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image10.png "DB_explorer")

Haga clic en el `Movies` de tabla y seleccione **mostrar datos de tabla** para ver los datos que creó.

![](accessing-your-models-data-from-a-controller/_static/image11.png) 

![](accessing-your-models-data-from-a-controller/_static/image12.png)

Haga clic en el `Movies` de tabla y seleccione **Abrir definición de tabla** para ver la tabla de la estructura que Entity Framework Code First creado para usted.

![](accessing-your-models-data-from-a-controller/_static/image13.png)

![](accessing-your-models-data-from-a-controller/_static/image14.png)

Observe cómo el esquema de la `Movies` tabla se asigna a la `Movie` clase que creó anteriormente. Entity Framework Code First crea automáticamente este esquema para el se basa en su `Movie` clase.

Cuando haya terminado, cierre la conexión haciendo clic *MovieDBContext* y seleccionando **cerrar conexión**. (Si no cierra la conexión, se podría obtener un error la próxima vez que ejecute el proyecto).

![](accessing-your-models-data-from-a-controller/_static/image15.png "CloseConnection")

Ahora dispone de una base de datos y de páginas para mostrar, editar, actualizar y eliminar datos. En el siguiente tutorial, podrá examinar el resto del código con scaffolding y agregue un `SearchIndex` método y un `SearchIndex` vista que le permite buscar películas en esta base de datos. Para obtener más información sobre el uso de Entity Framework con MVC, consulte [crear un modelo de datos de Entity Framework para una aplicación de MVC de ASP.NET](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-connection-string.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)
