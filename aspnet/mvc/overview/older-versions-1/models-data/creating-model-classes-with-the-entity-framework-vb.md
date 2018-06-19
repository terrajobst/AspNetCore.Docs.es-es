---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Crear clases de modelo con Entity Framework (VB) | Documentos de Microsoft
author: microsoft
description: En este tutorial, aprenderá a usar ASP.NET MVC con Microsoft Entity Framework. Obtenga información acerca de cómo usar al Asistente para Entity para crear el primero de una entidad de ADO.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: 3442435c7b2b9ce2ce6bd016ba74fe671eb76f62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874452"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Crear clases de modelo con Entity Framework (VB)
====================
por [Microsoft](https://github.com/microsoft)

> En este tutorial, aprenderá a usar ASP.NET MVC con Microsoft Entity Framework. Obtenga información acerca de cómo usar al Asistente para Entity para crear un Entity Data Model de ADO.NET. En el transcurso de este tutorial, creamos una aplicación web que se muestra cómo seleccionar, insertar, actualizar y eliminar datos de la base de datos mediante el uso de Entity Framework.


El objetivo de este tutorial es explicar cómo puede crear las clases de acceso a datos mediante Microsoft Entity Framework al compilar una aplicación ASP.NET MVC. Este tutorial se da por supuesto ningún conocimiento previo de Microsoft Entity Framework. Al final de este tutorial, comprenderá cómo usar Entity Framework para seleccionar, insertar, actualizar y eliminar registros de base de datos.

Microsoft Entity Framework es una herramienta de asignación relacional de objeto (O/RM) que le permite generar automáticamente una capa de acceso a datos desde una base de datos. Entity Framework permite evitar el trabajo tedioso de compilar las clases de acceso de datos manualmente.

> [!NOTE] 
> 
> No hay ninguna conexión fundamental entre MVC de ASP.NET y Microsoft Entity Framework. Hay varias alternativas para Entity Framework que pueden usar con ASP.NET MVC. Por ejemplo, puede generar las clases de modelo de MVC con otras herramientas O/RM como Microsoft LINQ to SQL, NHibernate o SubSonic.


Para ilustrar cómo puede usar Microsoft Entity Framework con ASP.NET MVC, vamos a crear una aplicación de ejemplo simple. Vamos a crear una aplicación de base de datos de película que permite mostrar y editar registros de base de datos de la película.

Este tutorial se da por supuesto que tiene Visual Studio 2008 o Visual Web Developer 2008 con Service Pack 1. Necesita el Service Pack 1 para poder usar Entity Framework. Puede descargar Visual Studio 2008 Service Pack 1 o Visual Web Developer con Service Pack 1 desde la siguiente dirección:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Crear la base de datos de ejemplo de película

La aplicación de base de datos de película utiliza una tabla de base de datos denominada películas que contiene las columnas siguientes:

| Nombre de columna | Tipo de datos | ¿Permitir valores null? | ¿Es la clave principal? |
| --- | --- | --- | --- |
| Id. | int | False | True |
| Title | nvarchar(100) | False | False |
| Director de | nvarchar(100) | False | False |

Puede agregar esta tabla a un proyecto de MVC de ASP.NET, siga estos pasos:

1. Haga clic en la aplicación\_carpeta de datos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento.**
2. Desde el **Agregar nuevo elemento** cuadro de diálogo, seleccione **base de datos de SQL Server**, asigne el nombre MoviesDB.mdf a la base de datos y haga clic en el **agregar** botón.
3. Haga doble clic en el archivo MoviesDB.mdf para abrir la ventana Explorador de base de datos o el Explorador de servidores.
4. Expanda la conexión de base de datos de MoviesDB.mdf, haga clic en la carpeta de tablas y seleccione la opción de menú **agregar nueva tabla**.
5. En el Diseñador de tablas, agregue las columnas Id., título y Director.
6. Haga clic en el **guardar** botón (tiene el icono del disquete) para guardar la nueva tabla con las películas de nombre.

Después de crear la tabla de base de datos de películas, debe agregar algunos datos de ejemplo a la tabla. Haga clic en la tabla de películas y seleccione la opción de menú **mostrar datos de tabla**. Puede escribir datos de la película falsa en la cuadrícula que aparece.

## <a name="creating-the-adonet-entity-data-model"></a>Creación de ADO.NET Entity Data Model

Para poder usar Entity Framework, debe crear un Entity Data Model. Puede aprovechar las ventajas de Visual Studio *Entity Data Model Wizard* para generar automáticamente un Entity Data Model de una base de datos.

Siga estos pasos:

1. Haga clic en la carpeta de modelos en la ventana Explorador de soluciones y seleccione la opción de menú **agregar, nuevo elemento**.
2. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la categoría de datos (consulte la figura 1).
3. Seleccione el **ADO.NET Entity Data Model** plantilla, asigne el nombre MoviesDBModel.edmx de Entity Data Model y haga clic en el **agregar** botón. Al hacer clic en el **agregar** botón inicia el Asistente para modelo de datos.
4. En el **Elegir contenido del modelo** paso a paso, elija la **generar desde una base de datos** opción y haga clic en el **siguiente** botón (consulte la figura 2).
5. En el **elegir la conexión de datos** paso a paso, seleccione la conexión de base de datos MoviesDB.mdf, especifique las entidades de configuración de conexión nombre MoviesDBEntities y haga clic en el **siguiente** botón (consulte la figura 3).
6. En el **elija los objetos de base de datos** paso a paso, seleccione la tabla de base de datos de la película y haga clic en el **finalizar** botón (consulte la figura 4).

Después de completar estos pasos, se abre ADO.NET Entity Data Model Designer (Entity Designer).

**Ilustración 1: crear un nuevo Entity Data Model**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Figura 2: elija el paso del modelo de contenido**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Figura 3: elegir la conexión de datos**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Figura 4: elija los objetos de base de datos**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modificación de ADO.NET Entity Data Model

Después de crear un Entity Data Model, puede modificar el modelo aprovechando las ventajas de Entity Designer (Véase la figura 5). Puede abrir el Diseñador de entidades en cualquier momento haciendo doble clic en el archivo MoviesDBModel.edmx contenido en la carpeta modelos dentro de la ventana Explorador de soluciones.

**Figura 5: ADO.NET Entity Data Model Designer**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Por ejemplo, puede utilizar el Diseñador de entidades para cambiar los nombres de las clases que genera el Asistente para datos de modelo de entidad. El Asistente para crea una nueva clase de acceso de datos denominada películas. En otras palabras, el Asistente dio a la clase el mismo nombre como la tabla de base de datos. Dado que se utilizará esta clase para representar una instancia determinada de la película, deberíamos cambiar la clase a partir de películas a película.

Si desea cambiar el nombre de una clase de entidad, puede hacer doble clic en el nombre de clase en el Diseñador de entidades y escriba un nombre nuevo (consulte la figura 6). Como alternativa, puede cambiar el nombre de una entidad en la ventana Propiedades después de seleccionar una entidad en Entity Designer.

**Figura 6: cambiar el nombre de una entidad**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

No olvide guardar su Entity Data Model después de realizar una modificación, haga clic en el botón Guardar (el icono del disquete). En segundo plano, el Diseñador de entidades genera un conjunto de clases de Visual Basic. NET. Puede ver estas clases, abra el archivo MoviesDBModel.Designer.vb desde la ventana Explorador de soluciones.


No modifique el código en el archivo Designer.vb puesto que los cambios se sobrescribirá la próxima vez que utilice el Diseñador de entidades. Si desea ampliar la funcionalidad de las clases de entidad definido en el archivo Designer.vb, a continuación, puede crear *clases parciales* en archivos independientes.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Seleccionar registros de la base de datos con Entity Framework

Vamos a empezar a crear nuestra aplicación de base de datos de película mediante la creación de una página que muestra una lista de registros de la película. El controlador Home en el listado 1 expone una acción denominada Index(). La acción de Index() devuelve todos los registros de la película de la tabla de base de datos de la película aprovechando las ventajas de Entity Framework.

**Lista 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Tenga en cuenta que el controlador en el listado 1 incluye un constructor. El constructor inicializa un campo de nivel de clase denominado \_base de datos. El \_db campo representa las entidades de base de datos generadas por Microsoft Entity Framework. El \_campo de base de datos es una instancia de la clase MoviesDBEntities que fue generada por el Diseñador de entidades.

El \_campo de base de datos se usa dentro de la acción de Index() para recuperar los registros de la tabla de base de datos de películas. La expresión \_base de datos. MovieSet representa todos los registros de la tabla de base de datos de películas. Se usa el método ToList() para convertir el conjunto de películas en una colección genérica de objetos de película: lista (de película).

Los registros de la película se recuperan con la Ayuda de LINQ to Entities. La acción de Index() en el listado 1 utiliza LINQ *sintaxis del método* para recuperar el conjunto de registros de base de datos. Si lo prefiere, puede usar LINQ *sintaxis de consulta* en su lugar. Las dos instrucciones siguientes hacen lo mismo:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Use la sintaxis LINQ: sintaxis de método o sintaxis de consulta: que consideran más intuitivo. No hay ninguna diferencia de rendimiento entre los dos enfoques: la única diferencia es el estilo.

La vista en el listado 2 se usa para mostrar los registros de la película.

**La lista 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

La vista en el listado 2 contiene un **For Each** bucle que recorre en iteración cada registro de la película y muestra los valores de propiedades de título y el Director del registro de la película. Tenga en cuenta que un vínculo de edición y eliminación se muestra junto a cada registro. Además, un vínculo de película agregar aparece en la parte inferior de la vista (consulte la figura 7).

**Figura 7: la vista de índice**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

La vista de índice es un *con el tipo de vista*. La vista de índice tiene un &lt;% @ Page %&gt; directiva que incluye un atributo Inherits. El atributo Inherits convierte la propiedad ViewData.Model para una colección fuertemente tipada genérica lista de objetos de película, una lista (de película).

## <a name="inserting-database-records-with-the-entity-framework"></a>Insertar registros de base de datos con Entity Framework

Puede utilizar Entity Framework para que resulte sencillo insertar nuevos registros en una tabla de base de datos. El listado 3 contiene dos nuevas acciones que se agregan a la clase de controlador de inicio que puede utilizar para insertar nuevos registros en la tabla de base de datos de la película.

**El listado 3 – Controllers\HomeController.vb (agregar métodos)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

La primera acción Add() simplemente devuelve una vista. La vista contiene un formulario para agregar una nueva base de datos de la película grabar (consulte la figura 8). Cuando se envía el formulario, se invoca la segunda acción Add().

Tenga en cuenta que la segunda acción Add() se decora con el atributo AcceptVerbs. Esta acción se puede invocar solo al realizar una operación HTTP POST. En otras palabras, esta acción solo puede invocar al enviar un formulario HTML.

La segunda acción Add() crea una nueva instancia de la clase de Entity Framework película con la Ayuda del método TryUpdateModel() de MVC de ASP.NET. El método TryUpdateModel() toma los campos en el ColecciónFormulario pasado al método Add() y asigna los valores de estos campos de formulario HTML a la clase de la película.


Al utilizar Entity Framework, debe proporcionar una "lista de permitidos" de propiedades cuando se usan métodos TryUpdateModel o UpdateModel para actualizar las propiedades de una clase de entidad.


A continuación, la acción Add() realiza alguna validación de forma sencilla. La acción comprueba que el título y el Director de propiedades tienen valores. Si se produce un error de validación, se agrega un mensaje de error de validación para ModelState.

Si no hay ningún error de validación se agrega un nuevo registro de película a la tabla de base de datos de películas con la Ayuda de Entity Framework. El nuevo registro se agrega a la base de datos con las siguientes dos líneas de código:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

La primera línea de código agrega la nueva entidad de película al conjunto de películas sometidos a seguimiento por Entity Framework. La segunda línea de código guarda los cambios se realizaron en las películas sometidas a seguimiento en la base de datos subyacente.

**Figura 8: la vista de complemento**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Actualizando los registros de base de datos con Entity Framework

Puede seguir casi el mismo enfoque para editar un registro de base de datos con Entity Framework como el enfoque que seguimos simplemente para insertar un nuevo registro de base de datos. Listado 4 contiene dos nuevas acciones de controlador, denominadas Edit(). La primera acción Edit() devuelve un formulario HTML para editar un registro de la película. La segunda acción Edit() intenta actualizar la base de datos.

**Listado 4 – Controllers\HomeController.vb (métodos de edición)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

La segunda acción Edit() se inicia si se recupera el registro de la película de la base de datos que coincide con el identificador de la película que se está editando. LINQ siguiente a la instrucción de entidades toma el primer registro de base de datos que coincida con un identificador determinado:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

A continuación, el método TryUpdateModel() se utiliza para asignar los valores de los campos de formulario HTML a las propiedades de la entidad de la película. Tenga en cuenta que se proporciona una lista de permitidos para especificar las propiedades exactas para actualizar.

A continuación, se realiza alguna validación simple para comprobar que el título de la película y el Director propiedades tienen valores. Si falta un valor de propiedad, a continuación, se agrega un mensaje de error de validación para ModelState y ModelState.IsValid devuelve el valor false.

Por último, si no hay ningún error de validación, se actualiza la tabla de base de datos subyacente de películas con los cambios llamando al método SaveChanges().

Al editar los registros de base de datos, debe pasar el identificador del registro se está editando con la acción del controlador que realiza la actualización de la base de datos. En caso contrario, la acción de controlador no sabrá qué registro se desea para actualizar la base de datos subyacente. La vista de edición, incluida en el listado 5, incluye un campo oculto del formulario que representa el identificador del registro de base de datos que se está editando.

**Listado 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Eliminar registros de base de datos con Entity Framework

La operación final de la base de datos, que es necesario abordar en este tutorial, está eliminando registros de base de datos. Puede usar la acción del controlador en el listado 6 para eliminar un registro de base de datos determinada.

**Listado 6--\Controllers\HomeController.vb (acción de eliminación)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

La acción Delete() primero recupera la película en la entidad que coincide con el identificador pasado a la acción. A continuación, la película se elimina de la base de datos llamando al método DeleteObject() seguido por el método SaveChanges(). Por último, el usuario se redirige a la vista de índice.

## <a name="summary"></a>Resumen

El objetivo de este tutorial era demostrar cómo puede compilar aplicaciones orientadas a base de datos web aprovechando las ventajas de ASP.NET MVC y Microsoft Entity Framework. Aprendió a crear una aplicación que permite seleccionar, insertar, actualizar y eliminar registros de base de datos.

En primer lugar, se explica cómo puede usar al Asistente para Entity Data Model para generar un Entity Data Model desde dentro de Visual Studio. A continuación, se muestra cómo usar LINQ to Entities para recuperar un conjunto de registros de base de datos de una tabla de base de datos. Por último, Entity Framework se utiliza para insertar, actualizar y eliminar registros de base de datos.

> [!div class="step-by-step"]
> [Anterior](validation-with-the-data-annotation-validators-cs.md)
> [Siguiente](creating-model-classes-with-linq-to-sql-vb.md)
