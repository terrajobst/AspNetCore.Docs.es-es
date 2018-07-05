---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
title: Acceso a los datos del modelo desde un controlador (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: cad00de1-3c68-4ff4-a436-54236d449459
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/accessing-your-models-data-from-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 0c0cdb7214b99908e4d64583e880fb03f69252c2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369307"
---
<a name="accessing-your-models-data-from-a-controller-vb"></a>Acceso a los datos del modelo desde un controlador (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)
> 
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [C# versión](../cs/accessing-your-models-data-from-a-controller.md) de este tutorial.


En esta sección, creará un nuevo `MoviesController` clase y escribir código que recupera los datos de película y lo muestra en el explorador con una plantilla de vista. Asegúrese de compilar la aplicación antes de continuar.

Haga clic en el *controladores* carpeta y cree un nuevo `MoviesController` controlador. Seleccione las opciones siguientes:

- Nombre del controlador: **nombre moviescontroller al**. (Esto es el valor predeterminado).
- Plantilla: **controlador con acciones de lectura/escritura y vistas, usando Entity Framework**.
- Clase de modelo: **Movie (MvcMovie.Models)**.
- Clase de contexto de datos: **MovieDBContext (MvcMovie.Models)**.
- Vistas: **Razor (CSHTML)**. (El predeterminado).

[![5addMovieController](accessing-your-models-data-from-a-controller/_static/image2.png)](accessing-your-models-data-from-a-controller/_static/image1.png)

Haga clic en **Agregar**. Visual Web Developer crea los siguientes archivos y carpetas:

- *Un MoviesController.vb* archivo en el proyecto *controladores* carpeta.
- Un *películas* carpeta del proyecto *vistas* carpeta.
- *Create.vbhtml, Delete.vbhtml, Details.vbhtml, Edit.vbhtml*, y *Index.vbhtml* en el nuevo *vistas\películas* carpeta.

[![5_ScaffoldMovie](accessing-your-models-data-from-a-controller/_static/image4.png)](accessing-your-models-data-from-a-controller/_static/image3.png)

El mecanismo de scaffolding de ASP.NET MVC 3 crea automáticamente el CRUD (crear, leer, actualizar y eliminar) los métodos de acción y vistas para usted. Ahora tiene una aplicación web totalmente funcional que le permite crear, enumerar, editar y eliminar las entradas de la película.

Ejecute la aplicación y vaya a la `Movies` controlador anexando */Movies* a la dirección URL en la barra de direcciones del explorador. Dado que la aplicación depende de la ruta predeterminada (definido en el *Global.asax* archivo), la solicitud del explorador `http://localhost:xxxxx/Movies` se enruta al valor predeterminado `Index` método de acción de la `Movies` controlador. En otras palabras, la solicitud del explorador `http://localhost:xxxxx/Movies` es lo mismo que la solicitud de explorador `http://localhost:xxxxx/Movies/Index`. El resultado es una lista vacía de películas, dado que aún no ha agregado ninguno.

![](accessing-your-models-data-from-a-controller/_static/image5.png)

## <a name="creating-a-movie"></a>Creación de una película

Seleccione el vínculo **Crear nuevo**. Escriba algunos detalles sobre una película y, a continuación, haga clic en el **crear** botón.

![](accessing-your-models-data-from-a-controller/_static/image6.png)

Al hacer clic en el **crear** botón hace que el formulario se envía al servidor, donde se guarda la información de la película en la base de datos. A continuación, se le redirigirá a la */Movies* dirección URL, donde puede ver la película en la lista recién creada.

[![IndexWhenHarryMet](accessing-your-models-data-from-a-controller/_static/image8.png)](accessing-your-models-data-from-a-controller/_static/image7.png)

Cree un par de entradas más de película. Compruebe que todos los vínculos **Editar**, **Detalles** y **Eliminar** son funcionales.

## <a name="examining-the-generated-code"></a>Examinar el código generado

Abra el *Controllers\MoviesController.vb* de archivo y examine el generado `Index` método. Una parte del controlador de película con el `Index` método se muestra a continuación.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample1.vb)]

La siguiente línea de la `MoviesController` clase crea una instancia de un contexto de base de datos de película, como se describió anteriormente. Puede usar el contexto de base de datos de películas para consultar, modificar y eliminar películas.

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample2.vb)]

Una solicitud para el `Movies` controlador devuelve todas las entradas de la `Movies` tabla de la base de datos de la película y, a continuación, envía los resultados a la `Index` vista.

## <a name="strongly-typed-models-and-the-model-keyword"></a>Modelos fuertemente tipados y la @model palabra clave

Anteriormente en este tutorial, vimos cómo un controlador puede pasar datos u objetos a una plantilla de vista mediante el `ViewBag` objeto. El `ViewBag` es un objeto dinámico que proporciona una manera cómoda de tiempo de ejecución para pasar información a una vista.

ASP.NET MVC también proporciona la capacidad de pasar fuertemente tipado de datos u objetos a una plantilla de vista. Inflexible este enfoque permite una mejor comprobación tiempo de compilación de su código y IntelliSense con más funcionalidades en el editor de Visual Web Developer. Usamos este enfoque con la `MoviesController` clase y *Index.vbhtml* plantilla de vista.

Observe cómo el código crea un [ `List` ](https://msdn.microsoft.com/library/6sh2ey19.aspx) objeto cuando llama a la `View` método auxiliar en el `Index` método de acción. El código, a continuación, pasa esto `Movies` lista desde el controlador a la vista:

[!code-vb[Main](accessing-your-models-data-from-a-controller/samples/sample3.vb)]

Mediante la inclusión de un `@ModelType` instrucción en la parte superior del archivo de plantilla de vista, puede especificar el tipo de objeto que espera la vista. Cuando se creó el controlador de película, Visual Web Developer incluye automáticamente lo siguiente `@model` instrucción en la parte superior de la *Index.vbhtml* archivo:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample4.vbhtml)]

Esto `@ModelType` directiva le permite acceder a la lista de películas que el controlador pasó a la vista usando un `Model` objeto fuertemente tipado. Por ejemplo, en el *Index.vbhtml* plantilla, el código recorre en bucle las películas realizando una `foreach` instrucción sobre fuertemente tipado `Model` objeto:

[!code-vbhtml[Main](accessing-your-models-data-from-a-controller/samples/sample5.vbhtml)]

Dado que el `Model` objeto fuertemente tipado (como un `IEnumerable<Movie>` objeto), cada `item` objeto en el bucle se escribe como `Movie`. Entre otras ventajas, esto significa que obtenga la comprobación en tiempo de compilación del código y compatibilidad de IntelliSense en el editor de código completa:

[![5_Intellisense](accessing-your-models-data-from-a-controller/_static/image10.png)](accessing-your-models-data-from-a-controller/_static/image9.png)

## <a name="working-with-sql-server-compact"></a>Trabajar con SQL Server Compact

Entity Framework Code First detecta que señala la cadena de conexión de base de datos que se proporcionó un `Movies` base de datos que todavía no existía, por lo que Code First crea la base de datos automáticamente. Puede comprobar que se ha creado mediante una búsqueda en el *aplicación\_datos* carpeta. Si no ve el *Movies.sdf* de archivos, haga clic en el **mostrar todos los archivos** situado en la **el Explorador de soluciones** barra de herramientas, haga clic en el **actualizar** botón y, a continuación, expanda el *aplicación\_datos* carpeta.

[![SDF_in_SolnExp](accessing-your-models-data-from-a-controller/_static/image12.png)](accessing-your-models-data-from-a-controller/_static/image11.png)

Haga doble clic en *Movies.sdf* para abrir **Explorador de servidores**. A continuación, expanda el **tablas** carpeta para ver las tablas que se han creado en la base de datos.

> [!NOTE]
> Si se produce un error al hacer doble clic *Movies.sdf*, asegúrese de que ha instalado **Visual Studio 2010 SP1 Tools para SQL Server Compact 4.0**. (Para obtener vínculos para el software, consulte la lista de requisitos previos en la parte 1 de esta serie de tutoriales). Si instala la versión ahora, tendrá que cerrar y volver a abrir Visual Web Developer.


[![DB_explorer](accessing-your-models-data-from-a-controller/_static/image14.png)](accessing-your-models-data-from-a-controller/_static/image13.png)

Hay dos tablas, una para el `Movie` conjunto de entidades y, a continuación, el `EdmMetadata` tabla. El `EdmMetadata` tabla se usa Entity Framework para determinar si el modelo y la base de datos no están sincronizados.

Haga clic en el `Movies` de tabla y seleccione **mostrar datos de tabla** para ver los datos que creó.

[![MoviesTable](accessing-your-models-data-from-a-controller/_static/image16.png)](accessing-your-models-data-from-a-controller/_static/image15.png)

Haga clic en el `Movies` de tabla y seleccione **Editar esquema de tabla**.

[![EditTableSchema](accessing-your-models-data-from-a-controller/_static/image18.png)](accessing-your-models-data-from-a-controller/_static/image17.png)

![TableSchemaSM](accessing-your-models-data-from-a-controller/_static/image19.png)

Observe cómo el esquema de la `Movies` se asigna a la tabla el `Movie` clase que creó anteriormente. Entity Framework Code First crea automáticamente este esquema para usted según su `Movie` clase.

Cuando haya terminado, cierre la conexión. (Si no cierra la conexión, puede aparecer un error la próxima vez que ejecute el proyecto).

[![CloseConnection](accessing-your-models-data-from-a-controller/_static/image21.png)](accessing-your-models-data-from-a-controller/_static/image20.png)

Ahora dispone de la base de datos y una página de lista simple para mostrar el contenido de él. En el siguiente tutorial, deberá examinar el resto del código con scaffolding y agregue un `SearchIndex` método y un `SearchIndex` vista que le permite buscar películas en esta base de datos.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Siguiente](examining-the-edit-methods-and-edit-view.md)
