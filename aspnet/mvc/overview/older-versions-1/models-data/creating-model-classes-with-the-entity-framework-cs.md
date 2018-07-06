---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
title: Crear clases de modelo con Entity Framework (C#) | Microsoft Docs
author: microsoft
description: En este tutorial, aprenderá a usar ASP.NET MVC con Entity Framework de Microsoft. Aprenda a usar al Asistente para Entity para crear un acelerador de desarrollo de ADO.NET Entity...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 61644169-e8b1-45dd-bf96-9c2301b69879
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-cs
msc.type: authoredcontent
ms.openlocfilehash: 8b88aaaae21323fe3e3e8548cc04110c9caafef7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808147"
---
<a name="creating-model-classes-with-the-entity-framework-c"></a>Crear clases de modelo con Entity Framework (C#)
====================
por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá a usar ASP.NET MVC con Entity Framework de Microsoft. Aprenda a usar al Asistente para Entity para crear un Entity Data Model de ADO.NET. En el transcurso de este tutorial, creamos una aplicación web que se muestra cómo seleccionar, insertar, actualizar y eliminar datos de la base de datos mediante Entity Framework.


El objetivo de este tutorial es explicar cómo puede crear clases de acceso a datos mediante Microsoft Entity Framework al compilar una aplicación ASP.NET MVC. Este tutorial asume ningún conocimiento anterior de Microsoft Entity Framework. Al final de este tutorial, comprenderá cómo usar Entity Framework para seleccionar, insertar, actualizar y eliminar registros de base de datos.

Microsoft Entity Framework es una herramienta de asignación relacional de objetos (O/RM) que le permite generar automáticamente una capa de acceso a datos desde una base de datos. Entity Framework permite evitar el trabajo de una tarea tediosa de crear las clases de acceso de datos a mano.

Para ilustrar cómo puede usar Microsoft Entity Framework con ASP.NET MVC, vamos a crear una aplicación de ejemplo simple. Vamos a crear una aplicación de base de datos de película que permite mostrar y editar registros de base de datos de películas.

En este tutorial se supone que tiene Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. Necesita el Service Pack 1 para poder usar Entity Framework. Puede descargar Visual Studio 2008 Service Pack 1 o Visual Web Developer con Service Pack 1 desde la siguiente dirección:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


> [!NOTE] 
> 
> No hay ninguna conexión fundamental entre MVC de ASP.NET y Microsoft Entity Framework. Existen varias alternativas a Entity Framework que puede usar con ASP.NET MVC. Por ejemplo, puede crear las clases de modelo de MVC con otras herramientas O/RM como Microsoft LINQ to SQL, NHibernate o SubSonic.


## <a name="creating-the-movie-sample-database"></a>Creación de la base de datos de ejemplo de película

La aplicación de base de datos de películas usa una tabla de base de datos denominada películas que contiene las columnas siguientes:

| Nombre de columna | Tipo de datos | ¿Permitir valores null? | ¿Es la clave principal? |
| --- | --- | --- | --- |
| Id. | int | False | True |
| Title | Nvarchar (100) | False | False |
| Director | Nvarchar (100) | False | False |

Puede agregar esta tabla a un proyecto de ASP.NET MVC, siga estos pasos:

1. Haga clic en la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, elemento nuevo.**
2. Desde el **Agregar nuevo elemento** cuadro de diálogo, seleccione **base de datos de SQL Server**, asigne el nombre MoviesDB.mdf a la base de datos y haga clic en el **agregar** botón.
3. Haga doble clic en el archivo MoviesDB.mdf para abrir la ventana Server Explorer/Database Explorer.
4. Expanda la conexión de base de datos MoviesDB.mdf, haga clic en la carpeta de tablas y seleccione la opción de menú **agregar nueva tabla**.
5. En el Diseñador de tablas, agregue las columnas Id, Title y Director.
6. Haga clic en el **guardar** botón (tiene el icono del disquete) para guardar la nueva tabla con el nombre de películas.

Después de crear la tabla de base de datos de películas, debe agregar algunos datos de ejemplo a la tabla. Haga clic en la tabla de películas y seleccione la opción de menú **mostrar datos de tabla**. Puede escribir datos falsa de película en la cuadrícula que aparece.

## <a name="creating-the-adonet-entity-data-model"></a>Creación de ADO.NET Entity Data Model

Para poder usar Entity Framework, deberá crear un Entity Data Model. Puede sacar partido de Visual Studio *Asistente para Entity Data Model* para generar automáticamente un Entity Data Model de una base de datos.

Siga estos pasos:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la categoría de datos (consulte la figura 1).
3. Seleccione el **ADO.NET Entity Data Model** plantilla, asigne el nombre MoviesDBModel.edmx de Entity Data Model y haga clic en el **agregar** botón. Al hacer clic en el **agregar** botón inicia el Asistente para modelo de datos.
4. En el **Elegir contenido de Model** paso, elija la **generar desde una base de datos** opción y haga clic en el **siguiente** botón (consulte la figura 2).
5. En el **elegir la conexión de datos** paso, seleccione la conexión de base de datos MoviesDB.mdf, especificar las entidades de configuración de conexión nombre MoviesDBEntities y haga clic en el **siguiente** (consulte la figura 3).
6. En el **elija los objetos de base de datos** paso, seleccione la tabla de base de datos de película y haga clic en el **finalizar** (consulte la figura 4).

Después de completar estos pasos, se abre el ADO.NET Entity Data Model Designer (Entity Designer).

**Ilustración 1: crear un nuevo Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-cs/_static/image1.jpg)

**Ilustración 2: elija el paso del modelo de contenido**

![clip_image004](creating-model-classes-with-the-entity-framework-cs/_static/image2.jpg)

**Ilustración 3: elegir la conexión de datos**

![clip_image006](creating-model-classes-with-the-entity-framework-cs/_static/image3.jpg)

**Figura 4: elija los objetos de base de datos**

![clip_image008](creating-model-classes-with-the-entity-framework-cs/_static/image4.jpg)

#### <a name="modifying-the-adonet-entity-data-model"></a>Modificación de ADO.NET Entity Data Model

Después de crear un Entity Data Model, puede modificar el modelo aprovechando las ventajas de Entity Designer (consulte la figura 5). Puede abrir el Diseñador de entidades en cualquier momento haciendo doble clic en el archivo MoviesDBModel.edmx contenido en la carpeta de modelos dentro de la ventana del explorador de soluciones.

**Figura 5: ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-cs/_static/image5.jpg)

Por ejemplo, puede usar Entity Designer para cambiar los nombres de las clases que genera el Asistente para Entity Data Model. El Asistente para crea una nueva clase de acceso de datos denominada películas. En otras palabras, el Asistente asignado a la clase el muy mismo nombre que la tabla de base de datos. Porque se usará esta clase para representar una instancia determinada de la película, nos debemos cambiar el nombre de la clase de películas a Movie.

Si desea cambiar el nombre de una clase de entidad, puede hacer doble clic en el nombre de clase en el Diseñador de entidades y escriba un nombre nuevo (consulte la figura 6). Como alternativa, puede cambiar el nombre de una entidad en la ventana Propiedades después de seleccionar una entidad en Entity Designer.

**Figura 6: cambiar el nombre de una entidad**

![clip_image012](creating-model-classes-with-the-entity-framework-cs/_static/image6.jpg)

No olvide guardar su Entity Data Model después de realizar una modificación, haga clic en el botón Guardar (el icono del disquete). En segundo plano, el Diseñador de entidades genera un conjunto de clases de C#. Puede ver estas clases abriendo el archivo MoviesDBModel.Designer.cs desde la ventana Explorador de soluciones.


No modifique el código en el archivo Designer.cs puesto que los cambios se sobrescribirán la próxima vez que utilice el Diseñador de entidades. Si desea ampliar la funcionalidad de las clases de entidad definido en el archivo Designer.cs, a continuación, puede crear *clases parciales* en archivos independientes.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Selección de registros de base de datos con Entity Framework

Vamos a empezar a crear nuestra aplicación de base de datos de películas mediante la creación de una página que muestra una lista de registros de películas. El controlador Home en el listado 1 expone una acción denominada Index(). La acción de Index() devuelve todos los registros de la película de la tabla de base de datos de película aprovechando las ventajas de Entity Framework.

**Listado 1 – controllers\homecontroller. cs**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample1.cs)]

Tenga en cuenta que el controlador en el listado 1 incluye un constructor. El constructor inicializa un campo de nivel de clase denominado \_db. El \_db campo representa las entidades de base de datos generadas por Entity Framework de Microsoft. El \_campo de base de datos es una instancia de la clase MoviesDBEntities generado por el Diseñador de entidades.


Para poder usar la clase theMoviesDBEntities en el controlador Home, debe importar el espacio de nombres MovieEntityApp.Models (*MVCProjectName*. Modelos).


El \_campo de base de datos se utiliza dentro de la acción de Index() para recuperar los registros de la tabla de base de datos de películas. La expresión \_db. MovieSet representa todos los registros de la tabla de base de datos de películas. El método ToList() se utiliza para convertir el conjunto de películas en una colección genérica de objetos de película (lista&lt;Movie&gt;).

Los registros de la película se recuperan con la Ayuda de LINQ to Entities. La acción de Index() en el listado 1 usa LINQ *sintaxis de método* para recuperar el conjunto de registros de base de datos. Si lo prefiere, puede usar LINQ *sintaxis de consulta* en su lugar. Las dos instrucciones siguientes hacer lo mismo:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample2.cs)]

Usar cualquier sintaxis LINQ, sintaxis de método o sintaxis de consulta, que encontrará más intuitiva. No hay ninguna diferencia de rendimiento entre los dos enfoques: la única diferencia es el estilo.

La vista en el listado 2 se usa para mostrar los registros de la película.

**Listado 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample3.aspx)]

La vista en el listado 2 contiene un **foreach** bucle que recorre en iteración cada registro de la película y muestra los valores de propiedades de título y el Director del registro de la película. Tenga en cuenta que un vínculo de edición y eliminación se muestra al lado de cada registro. Además, un vínculo de agregar la película aparece en la parte inferior de la vista (consulte la figura 7).

**Figura 7: la vista de índice**

![clip_image014](creating-model-classes-with-the-entity-framework-cs/_static/image7.jpg)

La vista de índice es un *con el tipo de vista*. La vista de índice incluye una &lt;% @ Page %&gt; la directiva con un *Inherits* atributo que proyecta la propiedad del modelo a una colección fuertemente tipada de lista genérica de objetos de película (lista&lt;película).

## <a name="inserting-database-records-with-the-entity-framework"></a>Insertar registros de base de datos con Entity Framework

Puede usar Entity Framework para que sea fácil insertar nuevos registros en una tabla de base de datos. Listado 3 contiene dos nuevas acciones que se agrega a la clase de controlador principal que puede usar para insertar nuevos registros en la tabla de base de datos de la película.

**Listado 3 – controllers\homecontroller. cs (agregar métodos)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample4.cs)]

La primera acción Add() simplemente devuelve una vista. La vista contiene un formulario para agregar una nueva base de datos de película grabar (consulte la figura 8). Cuando se envía el formulario, se invoca la segunda acción Add().

Tenga en cuenta que la segunda acción Add() está decorada con el atributo AcceptVerbs. Esta acción puede invocarse únicamente al realizar una operación HTTP POST. En otras palabras, solo se puede invocar esta acción al registrar un formulario HTML.

La segunda acción Add() crea una nueva instancia de la clase Movie de Entity Framework con la Ayuda del método TryUpdateModel() de MVC de ASP.NET. El método TryUpdateModel() toma los campos en el pasado al método Add() de ColecciónFormulario y asigna los valores de estos campos de formulario HTML a la clase de la película.


Al utilizar Entity Framework, debe proporcionar una "lista de permitidos" de propiedades cuando se usan métodos TryUpdateModel o UpdateModel para actualizar las propiedades de una clase de entidad.


A continuación, la acción Add() realiza alguna validación de formulario simple. La acción comprueba que el título y el Director de las propiedades tienen valores. Si se produce un error de validación, se agrega un mensaje de error de validación para ModelState.

Si no hay ningún error de validación se agrega un nuevo registro de la película a la tabla de base de datos de películas con la Ayuda de Entity Framework. El nuevo registro se agrega a la base de datos con las siguientes dos líneas de código:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample5.cs)]

La primera línea de código agrega la nueva entidad de la película al conjunto de Entity Framework realiza el seguimiento de películas. La segunda línea de código guarda los cambios se realizaron en las películas que se realiza un seguimiento a la base de datos subyacente.

**Figura 8: la vista de complemento**

![clip_image016](creating-model-classes-with-the-entity-framework-cs/_static/image8.jpg)

#### <a name="updating-database-records-with-the-entity-framework"></a>Actualizar los registros de la base de datos con Entity Framework

Puede seguir casi el mismo enfoque para editar un registro de base de datos con Entity Framework como el enfoque que simplemente seguimos para insertar un nuevo registro de base de datos. Listado 4 contiene dos nuevas acciones de controlador denominadas Edit(). La primera acción Edit() devuelve un formulario HTML para editar un registro de la película. La segunda acción Edit() intenta actualizar la base de datos.

**Listado 4 – controllers\homecontroller. cs (métodos)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample6.cs)]

La segunda acción Edit() empieza recuperando el registro de la película de la base de datos que coincide con el identificador de la película que se está editando. LINQ siguiente a la instrucción de entidades toma el primer registro de base de datos que coincide con un identificador determinado:

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample7.cs)]

A continuación, el método TryUpdateModel() se usa para asignar los valores de los campos de formulario HTML en las propiedades de la entidad de la película. Tenga en cuenta que se proporciona una lista de permitidos para especificar las propiedades exactas para actualizar.

A continuación, se realiza una validación sencilla para comprobar que el título de la película y el Director de las propiedades tienen valores. Si faltan un valor en cualquiera de las propiedades, a continuación, se agrega un mensaje de error de validación para ModelState y ModelState.IsValid devuelve el valor false.

Por último, si no hay ningún error de validación, se actualiza la tabla de base de datos subyacente de películas con los cambios llamando al método SaveChanges().

Al editar los registros de la base de datos, deberá pasar el identificador del registro que se está editando la acción de controlador que realiza la actualización de la base de datos. En caso contrario, la acción de controlador no sabrá qué registro se actualizará en la base de datos subyacente. La vista de edición, incluida en el listado 5 incluye un campo de formulario oculto que representa el identificador del registro de base de datos que se está editando.

**Listado 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Eliminación de registros de base de datos con Entity Framework

La operación de base de datos final, que es necesario abordar en este tutorial, está eliminando registros de base de datos. Puede usar la acción del controlador en el listado 6 para eliminar un registro de base de datos determinada.

**Listado 6--\Controllers\HomeController.cs (acción de eliminación)**

[!code-csharp[Main](creating-model-classes-with-the-entity-framework-cs/samples/sample9.cs)]

La acción Delete() primero recupera la entidad que coincide con el identificador pasado a la acción de película. A continuación, la película se elimina de la base de datos llamando al método DeleteObject() seguido por el método SaveChanges(). Por último, se redirige al usuario a la vista de índice.

## <a name="summary"></a>Resumen

El propósito de este tutorial era demostrar cómo puede crear aplicaciones web controladas por la base de datos aprovechando las ventajas de ASP.NET MVC y Entity Framework de Microsoft. Ha aprendido a crear una aplicación que le permite seleccionar, insertar, actualizar y eliminar registros de base de datos.

En primer lugar, se explica cómo puede usar al Asistente para Entity Data Model para generar un Entity Data Model desde dentro de Visual Studio. A continuación, obtenga información sobre cómo usar LINQ to Entities para recuperar un conjunto de registros de base de datos de una tabla de base de datos. Por último, Entity Framework se usa para insertar, actualizar y eliminar registros de base de datos.

> [!div class="step-by-step"]
> [Siguiente](creating-model-classes-with-linq-to-sql-cs.md)
