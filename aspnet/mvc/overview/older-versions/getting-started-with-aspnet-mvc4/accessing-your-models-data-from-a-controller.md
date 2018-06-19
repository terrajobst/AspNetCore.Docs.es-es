---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
title: Obtiene acceso a datos de su modelo desde un controlador | Documentos de Microsoft
author: Rick-Anderson
description: 'Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 61e0206d-7f32-4018-992d-0a51b48b37dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: cf896a6a9ce6cb8cd4adb13c3d87c4e7c3095fa6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873422"
---
<a name="accessing-your-models-data-from-a-controller"></a>Obtiene acceso a datos de su modelo desde un controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.


En esta sección, creará un nuevo `MoviesController` clase y escribir código que recupera los datos de la película y lo muestra en el explorador utilizando una plantilla de vista.

**Compile la aplicación** antes de pasar al paso siguiente.

Haga clic en el *controladores* carpeta y cree un nuevo `MoviesController` controlador. Las opciones siguientes no aparecerán hasta que se genere la aplicación. Seleccione las opciones siguientes:

- Nombre del controlador: **MoviesController**. (Este es el valor predeterminado. )
- Plantilla: **controlador de MVC con acciones de lectura/escritura y vistas, mediante Entity Framework**.
- Clase de modelo: **película (MvcMovie.Models)**.
- Clase de contexto de datos: **MovieDBContext (MvcMovie.Models)**.
- Vistas: **Razor (CSHTML)**. (El valor predeterminado.)

![AddScaffoldedMovieController](accessing-your-models-data-from-a-controller/_static/image1.png)

Haga clic en **Agregar**. Express de Visual Studio crea los siguientes archivos y carpetas:

- *Un MoviesController.cs* archivo en el proyecto *controladores* carpeta.
- A *películas* carpeta en el proyecto *vistas* carpeta.
- *Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml*, y *Index.cshtml* en el nuevo *Views\Movies* carpeta.

ASP.NET MVC 4 crea automáticamente el CRUD (crear, leer, actualizar y eliminar) los métodos de acción y vistas por usted (la creación automática de los métodos de acción CRUD y vistas se conoce como scaffolding). Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar las entradas de la película.

Ejecute la aplicación y busque el `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Dado que la aplicación es de confianza en la ruta predeterminada (definidos en el *Global.asax* archivo), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta hacia el valor predeterminado `Index` método de acción de la `Movies` controlador. En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es lo mismo que la solicitud de explorador `http://localhost:xxxxx/Movies/Index`. El resultado es una lista vacía de películas, porque no agregó ningún todavía.

![](accessing-your-models-data-from-a-controller/_static/image2.png)

### <a name="creating-a-movie"></a>Crear una película

Seleccione el vínculo **Crear nuevo**. Escriba algunos detalles acerca de una película y, a continuación, haga clic en el **crear** botón.

![](accessing-your-models-data-from-a-controller/_static/image3.png)

Al hacer clic en el **crear** botón hace que el formulario se envía al servidor, donde se guarda la información de la película en la base de datos. A continuación, se le redirigirá a la */Movies* dirección URL, donde puede ver la película recién creada en la lista.

![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image4.png "IndexWhenHarryMet")

Cree un par de entradas más de película. Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.

## <a name="examining-the-generated-code"></a>Examinar el código generado

Abra la *Controllers\MoviesController.cs* archivo y examine la generado `Index` método. Una parte del controlador de película con la `Index` método se muestra a continuación.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample1.cs)]

La siguiente línea de la `MoviesController` clase crea una instancia de un contexto de base de datos de la película, como se describió anteriormente. Puede utilizar el contexto de base de datos de la película para consultar, modificar y eliminar películas.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample2.cs)]

Una solicitud para la `Movies` controlador devuelve todas las entradas de la `Movies` tabla de la base de datos de la película y, a continuación, envía los resultados a la `Index` vista.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Establecimiento inflexible de tipos modelos y la @model (palabra clave)

Anteriormente en este tutorial, ha visto cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante la `ViewBag` objeto. El `ViewBag` es un objeto dinámico que proporciona una manera cómoda de tiempo de ejecución para pasar información a una vista.

ASP.NET MVC también proporciona la capacidad de pasar fuertemente tipados datos u objetos a una plantilla de vista. Esto fuertemente tipado enfoque permite un mejor tiempo de compilación la comprobación de su código y los transmite IntelliSense en el editor de Visual Studio. El mecanismo de scaffolding en Visual Studio usa este enfoque con el `MoviesController` plantillas de clase y la vista cuando crea los métodos y las vistas.

En el *Controllers\MoviesController.cs* archivo examinar generado `Details` método. Una parte del controlador de película con la `Details` método se muestra a continuación.

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample3.cs?highlight=3,8)]

Si un `Movie` se encuentra una instancia de la `Movie` modelo se pasa a la vista de detalles. Examine el contenido de la *Views\Movies\Details.cshtml* archivo.

Mediante la inclusión de un `@model` instrucción en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera de la vista. Cuando se creó el controlador de película, Visual Studio incluyó automáticamente la siguiente instrucción `@model` en la parte superior del archivo *Details.cshtml*:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.cshtml)]

Esta directiva `@model` permite acceder a la película que el controlador pasó a la vista usando un objeto `Model` fuertemente tipado. Por ejemplo, en la *Details.cshtml* plantilla, el código pasa cada campo de la película a la `DisplayNameFor` y [DisplayFor](https://msdn.microsoft.com/library/system.web.mvc.html.displayextensions.displayfor(VS.98).aspx) aplicaciones auxiliares de HTML con fuertemente tipado `Model` objeto. Las plantillas de vista y los métodos de creación y edición de pasan un objeto de modelo de la película.

Examine la *Index.cshtml* plantilla de vista y el `Index` método en el *MoviesController.cs* archivo. Observe cómo el código crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto cuando se llama a la `View` método auxiliar en el `Index` método de acción. A continuación, el código pasa esto `Movies` lista desde el controlador a la vista:

[!code-csharp[Main](accessing-your-models-data-from-a-controller/samples/sample5.cs?highlight=3)]

Al crear el controlador de película, Visual Studio Express incluye automáticamente los siguientes `@model` instrucción en la parte superior de la *Index.cshtml* archivo:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample6.cshtml)]

Esto `@model` directiva permite obtener acceso a la lista de películas que el controlador pasa a la vista mediante un `Model` objeto fuertemente tipado. Por ejemplo, en la *Index.cshtml* plantilla, el código recorre las películas de manera práctica un `foreach` instrucción sobre fuertemente tipado `Model` objeto:

[!code-cshtml[Main](accessing-your-models-data-from-a-controller/samples/sample7.cshtml?highlight=1,4,7,10,13,16,19-21)]

Dado que la `Model` objeto está fuertemente tipado (como un `IEnumerable<Movie>` objeto), cada `item` objeto en el bucle se escribe como `Movie`. Entre otras ventajas, esto significa que obtiene la comprobación en tiempo de compilación del código y compatibilidad con IntelliSense en el editor de código completa:

![ModelIntellisene](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="working-with-sql-server-localdb"></a>Trabajar con SQL Server LocalDB

Entity Framework Code First detectó que la cadena de conexión de base de datos que se proporcionó que apunta a un `Movies` base de datos que no existe todavía, por lo que Code First crea la base de datos automáticamente. Puede comprobar que se ha creado mediante una búsqueda en el *aplicación\_datos* carpeta. Si no ve el *Movies.mdf* de archivos, haga clic en el **mostrar todos los archivos** botón en el **el Explorador de soluciones** barra de herramientas, haga clic en el **actualizar** botón y, a continuación, expanda el *aplicación\_datos* carpeta.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Haga doble clic en *Movies.mdf* para abrir **el Explorador de base de datos**, a continuación, expanda el **tablas** carpeta para ver la tabla de películas.

![DB_explorer](accessing-your-models-data-from-a-controller/_static/image7.png "DB_explorer")

> [!NOTE]
> Si no aparece el Explorador de base de datos, desde el **herramientas** menú, seleccione **conectar con base de datos**, a continuación, cancelar la **Elegir origen de datos** cuadro de diálogo. Esto forzará abra el Explorador de base de datos.


> [!NOTE]
> Si está utilizando VWD o Visual Studio 2010 y recibe un error similar a alguno de los siguientes valores siguientes:
> 
> - La base de datos ' C:\Webs\MVC4\MVCMOVIE\MVCMOVIE\APP\_DATA\MOVIES. MDF' no se puede abrir porque está versión 706. Este servidor es compatible con la versión 655 y versiones anterior. No se admite la ruta de actualización.
> - &quot;Excepción de Objectnotfound no estaba controlada por código de usuario&quot; la SqlConnection proporcionada no especifica ningún catálogo inicial.
> 
> Debe instalar la [SQL Server Data Tools](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx) y [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0). Compruebe el `MovieDBContext` cadena de conexión especificada en la página anterior.


Haga clic en el `Movies` de tabla y seleccione **mostrar datos de tabla** para ver los datos que creó.

![](accessing-your-models-data-from-a-controller/_static/image8.png)

Haga clic en el `Movies` de tabla y seleccione **Abrir definición de tabla** para ver la tabla de la estructura que Entity Framework Code First creado para usted.

![](accessing-your-models-data-from-a-controller/_static/image9.png "MoviesTable")

![](accessing-your-models-data-from-a-controller/_static/image10.png)

Observe cómo el esquema de la `Movies` tabla se asigna a la `Movie` clase que creó anteriormente. Entity Framework Code First crea automáticamente este esquema para el se basa en su `Movie` clase.

Cuando haya terminado, cierre la conexión haciendo clic *MovieDBContext* y seleccionando **cerrar conexión**. (Si no cierra la conexión, se podría obtener un error la próxima vez que ejecute el proyecto).

![](accessing-your-models-data-from-a-controller/_static/image11.png "CloseConnection")

Ahora tiene la base de datos y una página de lista simple para mostrar el contenido del mismo. En el siguiente tutorial, podrá examinar el resto del código con scaffolding y agregue un `SearchIndex` método y un `SearchIndex` vista que le permite buscar películas en esta base de datos.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)
