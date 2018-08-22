---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
title: Crear clases de modelo con LINQ to SQL (C#) | Microsoft Docs
author: microsoft
description: El objetivo de este tutorial es explicar un método para crear clases de modelo para una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo compilar modelo c...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: f84b4a16-e8bb-49e8-87a0-1832879a3501
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-linq-to-sql-cs
msc.type: authoredcontent
ms.openlocfilehash: c76e92ebfab1db162151ea5753888a60a4d54e59
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829415"
---
<a name="creating-model-classes-with-linq-to-sql-c"></a>Crear clases de modelo con LINQ to SQL (C#)
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_10_CS.pdf)

> El objetivo de este tutorial es explicar un método para crear clases de modelo para una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo crear clases de modelo y realizar el acceso de la base de datos aprovechando las ventajas de Microsoft LINQ to SQL.


El objetivo de este tutorial es explicar un método para crear clases de modelo para una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo crear clases de modelo y realizar el acceso de la base de datos aprovechando las ventajas de Microsoft LINQ to SQL

En este tutorial, creamos una aplicación básica de la base de datos de película. Comenzamos creando la aplicación de base de datos de la película en la forma más rápida y sencilla posible. Llevamos a cabo todo el acceso de datos directamente desde las acciones del controlador.

A continuación, obtenga información sobre cómo usar el modelo de repositorio. Con el patrón de repositorio requiere un poco más trabajo. Sin embargo, la ventaja de la adopción de este patrón es que permite crear aplicaciones que son adaptables a cambiar y se puede probar fácilmente.

## <a name="what-is-a-model-class"></a>¿Qué es una clase de modelo?

Un modelo de MVC contiene toda la lógica de aplicación que no está contenida en una vista de MVC o el controlador de MVC. En concreto, un modelo de MVC contiene todos los negocios de la aplicación y la lógica de acceso a datos.

Puede usar una variedad de tecnologías diferentes para implementar la lógica de acceso de datos. Por ejemplo, puede crear las clases de acceso de datos mediante las clases de Microsoft Entity Framework, NHibernate, Subsonic o ADO.NET.

En este tutorial, uso de LINQ to SQL para consultar y actualizar la base de datos. LINQ to SQL proporciona un método muy fácil de interactuar con una base de datos de Microsoft SQL Server. Sin embargo, es importante comprender que el marco de MVC de ASP.NET no está asociado a LINQ to SQL de ninguna manera. ASP.NET MVC es compatible con cualquier tecnología de acceso a datos.

## <a name="create-a-movie-database"></a>Crear una base de datos de película

En este tutorial, para ilustrar cómo puede crear clases de modelo, creamos una aplicación de base de datos de película simple. El primer paso es crear una nueva base de datos. Haga clic en la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**. Seleccione el **base de datos de SQL Server** plantilla, asígnele el nombre MoviesDB.mdf y haga clic en el **agregar** (consulte la figura 1).


[![Agregar una nueva base de datos de SQL Server](creating-model-classes-with-linq-to-sql-cs/_static/image2.png)](creating-model-classes-with-linq-to-sql-cs/_static/image1.png)

**Figura 01**: agregar una nueva base de datos de SQL Server ([haga clic aquí para ver imagen en tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image3.png))


Después de crear la nueva base de datos, puede abrir la base de datos haciendo doble clic en el archivo MoviesDB.mdf en la aplicación\_carpeta de datos. Haga doble clic en el archivo MoviesDB.mdf se abre la ventana del explorador de servidores (consulte la figura 2).

La ventana del explorador de servidores se denomina la ventana del explorador de base de datos al usar Visual Web Developer.


[![Uso de la ventana del explorador de servidores](creating-model-classes-with-linq-to-sql-cs/_static/image5.png)](creating-model-classes-with-linq-to-sql-cs/_static/image4.png)

**Figura 02**: uso de la ventana del explorador de servidores ([haga clic aquí para ver imagen en tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image6.png))


Necesitamos agregar una tabla a nuestra base de datos que representa nuestro películas. Haga clic en la carpeta Tables y seleccione la opción de menú **agregar nueva tabla**. Al seleccionar esta opción de menú abre el Diseñador de tablas (consulte la figura 3).


[![Uso de la ventana del explorador de servidores](creating-model-classes-with-linq-to-sql-cs/_static/image8.png)](creating-model-classes-with-linq-to-sql-cs/_static/image7.png)

**Figura 03**: el Diseñador de tablas ([haga clic aquí para ver imagen en tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image9.png))


Es necesario agregar las columnas siguientes a la tabla de base de datos:

| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Title | Nvarchar (200) | False |
| Director | nvarchar (50) | False |

Deberá realizar dos pasos especiales para la columna de identificador. En primer lugar, deberá marcar la columna de identificador como una columna de clave principal, seleccione la columna en el Diseñador de tablas y haciendo clic en el icono de una clave. LINQ to SQL, deberá especificar las columnas de clave principales al realizar inserta o actualiza en la base de datos.

A continuación, deberá marcar la columna de identificador como una columna de identidad asignando el valor Sí para la **identidad** propiedad (consulte la figura 3). Una columna de identidad es una columna que se le asigna a un nuevo número automáticamente siempre que agregue una nueva fila de datos a una tabla.

## <a name="create-linq-to-sql-classes"></a>Crear clases LINQ to SQL

Nuestro modelo MVC contiene LINQ a las clases SQL que representan la tabla de base de datos tblMovie. La manera más fácil para crear estas clases de LINQ to SQL es haga clic en la carpeta Models, seleccione **agregar, nuevo elemento**, seleccione la plantilla de LINQ to SQL clases, asigne el nombre Movie.dbml a las clases y haga clic en el **agregar**(consulte la figura 4).


[![Creación de LINQ a las clases SQL](creating-model-classes-with-linq-to-sql-cs/_static/image11.png)](creating-model-classes-with-linq-to-sql-cs/_static/image10.png)

**Figura 04**: crear clases de LINQ to SQL ([haga clic aquí para ver imagen en tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image12.png))


Inmediatamente después de crear la película clases LINQ to SQL, aparece el Object Relational Designer. Puede arrastrar las tablas de base de datos desde la ventana Explorador de servidores hasta Object Relational Designer para crear clases LINQ to SQL que representan las tablas de base de datos determinada. Tenemos que agregar la tabla de base de datos de tblMovie hasta Object Relational Designer (consulte la figura 5).


[![Uso de Object Relational Designer](creating-model-classes-with-linq-to-sql-cs/_static/image14.png)](creating-model-classes-with-linq-to-sql-cs/_static/image13.png)

**Figura 05**: uso de Object Relational Designer ([haga clic aquí para ver imagen en tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image15.png))


De forma predeterminada, el Object Relational Designer crea una clase con el nombre mismo que la tabla de base de datos que arrastre al diseñador. Sin embargo, no queremos llamar a nuestra clase `tblMovie`. Por lo tanto, haga clic en el nombre de la clase en el diseñador y cambie el nombre de la clase a la película.

Por último, recuerde hacer clic en el **guardar** botón (la imagen del disquete) para guardar el LINQ a las clases de SQL. En caso contrario, no se generará las clases de LINQ to SQL mediante Object Relational Designer.

## <a name="using-linq-to-sql-in-a-controller-action"></a>Usar LINQ to SQL en una acción de controlador

Ahora que tenemos nuestra clases LINQ to SQL, podemos usar estas clases para recuperar datos de la base de datos. En esta sección, aprenderá a usar LINQ para las clases de SQL directamente dentro de una acción de controlador. Se mostrará la lista de películas de la tabla de base de datos tblMovies en una vista de MVC.

En primer lugar, necesitamos modificar la clase HomeController. Esta clase puede encontrarse en la carpeta de controladores de la aplicación. Modifique la clase por lo que se parece a la clase en el listado 1.

**Listado 1: `Controllers\HomeController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample1.cs)]

El `Index()` acción en el listado 1 usa una clase LINQ to SQL DataContext (el `MovieDataContext`) para representar el `MoviesDB` base de datos. La `MoveDataContext` clase generada por Visual Studio Object Relational Designer.

Se realiza una consulta LINQ en el DataContext para recuperar todas las películas desde el `tblMovies` tabla de base de datos. La lista de películas se asigna a una variable local denominada `movies`. Por último, la lista de películas se pasa a la vista de datos de la vista.

Para mostrar las películas, a continuación, debe modificar la vista de índice. Puede encontrar la vista de índice en el `Views\Home\` carpeta. Actualizar la vista de índice para que se parece a la vista en el listado 2.

**Listado 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample2.aspx)]

Tenga en cuenta que la vista de índice modificada incluye un `<%@ import namespace %>` la directiva en la parte superior de la vista. Esta directiva se importa el `MvcApplication1.Models namespace`. Se necesita este espacio de nombres con el fin de trabajar con el `model` clases: en concreto, el `Movie` clase--en la vista.

La vista en el listado 2 contiene un `foreach` bucle que recorre en iteración todos los elementos representados por el `ViewData.Model` propiedad. El valor de la `Title` propiedad se muestra para cada `movie`.

Tenga en cuenta que el valor de la `ViewData.Model` propiedad se convierte en un `IEnumerable`. Esto es necesario para recorrer en iteración el contenido de `ViewData.Model`. Otra opción aquí consiste en crear fuertemente tipadas `view`. Cuando creas fuertemente tipadas `view`, convierte el `ViewData.Model` propiedad a un tipo determinado en la clase de código subyacente de la vista.

Si ejecuta la aplicación después de modificar el `HomeController` clase y el índice de la vista, se le devolverán una página en blanco. Obtendrá una página en blanco porque no hay ningún registro de la película en el `tblMovies` tabla de base de datos.

Para agregar registros a la `tblMovies` tabla de base de datos, haga clic en el `tblMovies` base de datos de tabla en la ventana del explorador de servidores (ventana de explorador de base de datos en Visual Web Developer) y seleccione la opción de menú Mostrar datos de tabla. Puede insertar `movie` registros mediante el uso de la cuadrícula que aparece (consulte la figura 6).


[![Inserción de películas](creating-model-classes-with-linq-to-sql-cs/_static/image17.png)](creating-model-classes-with-linq-to-sql-cs/_static/image16.png)

**Figura 06**: inserción de películas ([haga clic aquí para ver imagen en tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image18.png))


Después de agregar algunas entradas de la base de datos a la `tblMovies` tabla y ejecute la aplicación, verá la página en la figura 7. Todos los registros de base de datos de la película se muestran en una lista con viñetas.


[![Mostrar películas con la vista de índice](creating-model-classes-with-linq-to-sql-cs/_static/image20.png)](creating-model-classes-with-linq-to-sql-cs/_static/image19.png)

**Figura 07**: mostrar películas con la vista de índice ([haga clic aquí para ver imagen en tamaño completo](creating-model-classes-with-linq-to-sql-cs/_static/image21.png))


## <a name="using-the-repository-pattern"></a>Con el patrón de repositorio

En la sección anterior, hemos usado LINQ a las clases SQL directamente dentro de una acción de controlador. Hemos usado el `MovieDataContext` clase directamente desde el `Index()` acción del controlador. No hay nada malo en hacer esto en el caso de una aplicación sencilla. Sin embargo, trabajar directamente con LINQ to SQL en una clase de controlador genera problemas cuando se necesita para compilar una aplicación más compleja.

Usar LINQ to SQL dentro de una clase de controlador hace difícil cambiar en el futuro las tecnologías de acceso de datos. Por ejemplo, podría decidir cambiar al uso de Microsoft Entity Framework como la tecnología de acceso a datos de Microsoft LINQ to SQL. En ese caso, necesitaría volver a escribir todos los controladores que tiene acceso a la base de datos dentro de la aplicación.

Usar LINQ to SQL dentro de una clase de controlador también hace difícil crear pruebas unitarias para la aplicación. Normalmente, no desea interactuar con una base de datos al realizar pruebas unitarias. Desea usar las pruebas unitarias para probar la lógica de aplicación y no en el servidor de base de datos.

Para compilar una aplicación de MVC es más adaptable para futuros cambios y que se pueden probar más fácilmente, puede utilizar el modelo de repositorio. Cuando se usa el modelo de repositorio, cree una clase de repositorio independiente que contiene toda la lógica de acceso de la base de datos.

Cuando se crea la clase del repositorio, cree una interfaz que representa todos los métodos utilizados por la clase del repositorio. Los controladores, escribir su código con la interfaz en lugar del repositorio. De este modo, puede implementar el repositorio mediante tecnologías de acceso a datos diferentes en el futuro.

La interfaz en el listado 3 se denomina `IMovieRepository` y representa un solo método denominado `ListAll()`.

**Listado 3: `Models\IMovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample3.cs)]

La clase de repositorio en el listado 4 implementa el `IMovieRepository` interfaz. Tenga en cuenta que contiene un método denominado `ListAll()` que corresponde al método requerido por la `IMovieRepository` interfaz.

**Listado 4: `Models\MovieRepository.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample4.cs)]

Por último, el `MoviesController` clase en el listado 5 usa el modelo de repositorio. Ya no utiliza LINQ a las clases SQL directamente.

**Listado 5: `Controllers\MoviesController.cs`**

[!code-csharp[Main](creating-model-classes-with-linq-to-sql-cs/samples/sample5.cs)]

Tenga en cuenta que el `MoviesController` clase en el listado 5 tiene dos constructores. El primer constructor, el constructor sin parámetros, se llama cuando se ejecuta la aplicación. Este constructor crea una instancia de la `MovieRepository` clase y lo pasa al constructor de segundo.

El segundo constructor tiene un único parámetro: un `IMovieRepository` parámetro. Este constructor simplemente asigna el valor del parámetro a un campo de nivel de clase denominado `_repository`.

La `MoviesController` clase está sacando partido de un patrón de diseño de software que llama el patrón de inserción de dependencias. En concreto, es usar algo llamado inserción de dependencias de Constructor. Puede leer más sobre este patrón, lea el artículo siguiente de Martin Fowler:

[http://martinfowler.com/articles/injection.html](http://martinfowler.com/articles/injection.html)

Tenga en cuenta que todo el código en el `MoviesController` clase (salvo el primer constructor) interactúa con el `IMovieRepository` interfaz en lugar de los datos reales `MovieRepository` clase. El código interactúa con una interfaz abstracta en lugar de una implementación concreta de la interfaz.

Si desea modificar la tecnología de acceso de datos utilizada por la aplicación, a continuación, se puede implementar simplemente el `IMovieRepository` interfaz con una clase que usa la tecnología de acceso de la base de datos alternativo. Por ejemplo, podría crear un `EntityFrameworkMovieRepository` clase o un `SubSonicMovieRepository` clase. Dado que la clase de controlador programada en la interfaz, puede pasar una nueva implementación de `IMovieRepository` al controlador de clase y la clase seguiría funcionando.

Además, si desea probar la `MoviesController` clase, a continuación, puede pasar una clase de repositorio de película falsa para la `HomeController`. Puede implementar la `IMovieRepository` clase con una clase que no es realmente el acceso a la base de datos pero que contiene todos los métodos necesarios de la `IMovieRepository` interfaz. De este modo, se puede realizar pruebas unitarias la `MoviesController` clase sin realmente tener acceso a una base de datos real.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era demostrar cómo puede crear clases de modelo MVC aprovechando las ventajas de Microsoft LINQ to SQL. Se examinan dos estrategias para mostrar datos de la base de datos en una aplicación ASP.NET MVC. En primer lugar, creamos LINQ a las clases SQL y utiliza las clases directamente dentro de una acción de controlador. Uso de LINQ a las clases SQL en un controlador de le permite rápidamente y mostrar fácilmente la base de datos en una aplicación MVC.

A continuación, exploramos una ruta de acceso de un poco más difícil, pero sin duda más virtuoso, para mostrar los datos de la base de datos. Se aprovechó el patrón de repositorio y coloca toda nuestra lógica de acceso base de datos en una clase de repositorio independiente. En nuestro controlador, hemos escrito todo nuestro código contra una interfaz en lugar de una clase concreta. La ventaja del patrón de repositorio es que nos permite cambiar fácilmente las tecnologías de acceso de base de datos en el futuro y es que nos permite probar fácilmente nuestras clases de controlador.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-the-entity-framework-cs.md)
> [Siguiente](displaying-a-table-of-database-data-cs.md)
