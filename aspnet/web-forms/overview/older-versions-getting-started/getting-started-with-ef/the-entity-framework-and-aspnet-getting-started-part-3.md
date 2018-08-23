---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: Introducción a Entity Framework 4.0 Database First y 4 de ASP.NET Web Forms, parte 3 | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 162f93c0249d8fa11d67ea10c68bc2ca8bae7259
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836321"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Introducción a Entity Framework 4.0 Database First y ASP.NET 4 Web Forms, parte 3
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtrado, ordenación y agrupación de datos

En el tutorial anterior ha utilizado el `EntityDataSource` control para mostrar y editar datos. En este tutorial filtrar, ordenar y agrupar los datos. Al hacerlo estableciendo las propiedades de la `EntityDataSource` (control), la sintaxis es diferente de otros controles de origen de datos. Como verá, sin embargo, puede usar el `QueryExtender` control para minimizar estas diferencias.

Va a cambiar el *Students.aspx* página para filtrar para estudiantes, ordene por nombre y la búsqueda de nombre. También podrá cambiar la *Courses.aspx* página para mostrar cursos para el departamento seleccionado y Buscar cursos por su nombre. Por último, agregará las estadísticas de alumno el *About.aspx* página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Uso de la propiedad de EntityDataSource "Where" para filtrar los datos

Abra el *Students.aspx* página que ha creado en el tutorial anterior. Tal como está configurado, el `GridView` control en la página muestra todos los nombres de los `People` conjunto de entidades. Sin embargo, desea mostrar solo los estudiantes, que puede encontrar seleccionando `Person` las entidades que tienen fechas de inscripción no es null.

Cambie a **diseño** ver y seleccionar el `EntityDataSource` control. En el **propiedades** ventana, establezca el `Where` propiedad `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La sintaxis usada en el `Where` propiedad de la `EntityDataSource` control es Entity SQL. Entity SQL es similar a Transact-SQL, pero se personaliza para su uso con las entidades en lugar de objetos de base de datos. En la expresión `it.EnrollmentDate is not null`, la palabra `it` representa una referencia a la entidad devuelta por la consulta. Por lo tanto, `it.EnrollmentDate` hace referencia a la `EnrollmentDate` propiedad de la `Person` entidad que el `EntityDataSource` controlar devuelve.

Ejecute la página. La lista de alumnos ahora contiene solo los estudiantes. (No hay ninguna fila muestra donde no hay ninguna fecha de inscripción.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Mediante la propiedad "OrderBy" de EntityDataSource para ordenar datos

También desea que esta lista estará en orden de los nombres cuando se muestra por primera vez. Con el *Students.aspx* abierta, en la página **diseño** vista y con el `EntityDataSource` control aún seleccionado, en el **propiedades** ventana conjunto el  **OrderBy** propiedad `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Ejecute la página. La lista de estudiantes es ahora ordenados por apellido.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Usar un parámetro de Control para establecer la propiedad "Where"

Como con otros controles de origen de datos, puede pasar valores de parámetro para el `Where` propiedad. En el *Courses.aspx* página que creó en la parte 2 del tutorial, puede usar este método para mostrar cursos que están asociados con el departamento que un usuario selecciona en la lista desplegable.

Abra *Courses.aspx* y cambie a **diseño** vista. Agregue un segundo `EntityDataSource` control a la página y asígnele el nombre `CoursesEntityDataSource`. Conectarse a la `SchoolEntities` del modelo y seleccione `Courses` como el **EntitySetName** valor.

En el **propiedades** ventana, haga clic en el botón de puntos suspensivos en el **donde** cuadro de la propiedad. (Asegúrese de que el `CoursesEntityDataSource` control aún está seleccionado antes de usar el **propiedades** ventana.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

El **Editor de expresiones** se muestra el cuadro de diálogo. En este cuadro de diálogo, seleccione **genere automáticamente expresión basada en los parámetros proporcionados**y, a continuación, haga clic en **Agregar parámetro**. Nombre del parámetro `DepartmentID`, seleccione **Control** como el **origen del parámetro** valor y seleccione **DepartmentsDropDownList** como el **ControlID**  valor.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Haga clic en **mostrar propiedades avanzadas**y en el **propiedades** ventana de la **Editor de expresiones** cuadro de diálogo, cambie el `Type` propiedad `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Cuando haya terminado, haga clic en **Aceptar**.

Debajo de la lista desplegable, agregue un `GridView` control a la página y asígnele el nombre `CoursesGridView`. Conéctelo al `CoursesEntityDataSource` del origen de datos de control, haga clic en **actualizar esquema**, haga clic en **Editar columnas**y quitar el `DepartmentID` columna. El `GridView` marcado del control es similar al siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Cuando el usuario cambia el departamento seleccionado en la lista desplegable, desea que la lista de cursos asociados que cambie automáticamente. Para que esto suceda, seleccione la lista desplegable y en el **propiedades** ventana conjunto el `AutoPostBack` propiedad `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Ahora que se ha terminado de utilizar el diseñador, cambie a **origen** ver y reemplace el `ConnectionString` y `DefaultContainer` nombres de las propiedades de la `CoursesEntityDataSource` controlar con el `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Cuando haya terminado, el marcado para el control tendrá un aspecto similar al ejemplo siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Ejecute la página y use la lista desplegable para seleccionar distintos departamentos. Solo los cursos que se ofrecen por el departamento seleccionado se muestran en el `GridView` control.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Mediante la propiedad "GroupBy" EntityDataSource para agrupar los datos

Supongamos que Contoso University desea colocar algunas estadísticas de alumno cuerpo en la página About. En concreto, desea mostrar un desglose del número de estudiantes por la fecha en que se inscriben.

Abra *About.aspx*y en **origen** ver, reemplace el contenido existente de la `BodyContent` controlar con "Estadísticas de alumno cuerpo" entre `h2` etiquetas:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Después del encabezado, agregue un `EntityDataSource` controlar y asígnele el nombre `StudentStatisticsEntityDataSource`. Conéctelo al `SchoolEntities`, seleccione el `People` entidad establece y deja el **seleccione** cuadro en el asistente sin cambios. Establezca las siguientes propiedades el **propiedades** ventana:

- Para filtrar solo los estudiantes, establezca el `Where` propiedad `it.EnrollmentDate is not null`.
- Para agrupar los resultados por la fecha de inscripción, establezca el `GroupBy` propiedad `it.EnrollmentDate`.
- Para seleccionar la fecha de inscripción y el número de alumnos, establezca el `Select` propiedad `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Para ordenar los resultados por la fecha de inscripción, establezca el `OrderBy` propiedad `it.EnrollmentDate`.

En **origen** ver, reemplace el `ConnectionString` y `DefaultContainer` nombres de las propiedades con un `ContextTypeName` propiedad. El `EntityDataSource` marcado del control ahora es similar al siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La sintaxis de la `Select`, `GroupBy`, y `Where` propiedades es similar a Transact-SQL, excepto el `it` palabra clave que especifica la entidad actual.

Agregue el siguiente marcado para crear un `GridView` control para mostrar los datos.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Ejecute la página para ver una lista que muestra el número de alumnos por fecha de inscripción.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Mediante el Control QueryExtender para el filtrado y ordenación

El `QueryExtender` control proporciona una manera de especificar el filtrado y ordenación en el marcado. La sintaxis es independiente del que está utilizando el sistema de administración de base de datos (DBMS). También es generalmente independiente de Entity Framework, con la excepción de que la sintaxis que debe utilizarse para las propiedades de navegación es único a Entity Framework.

En esta parte del tutorial usará un `QueryExtender` control para filtrar y ordenar datos y uno de los campos de order by será una propiedad de navegación.

(Si prefiere utilizar código en lugar de marcado para ampliar las consultas que se generan automáticamente mediante la `EntityDataSource` control, puede hacerlo controlar el `QueryCreated` eventos. Se trata cómo el `QueryExtender` control extiende `EntityDataSource` controlar también las consultas.)

Abra el *Courses.aspx* página y, a continuación el marcado que agregó previamente, inserte el siguiente marcado para crear un encabezado, un cuadro de texto para escribir cadenas de búsqueda, un botón de búsqueda y un `EntityDataSource` control que está enlazado a la `Courses` conjunto de entidades.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Tenga en cuenta que el `EntityDataSource` del control `Include` propiedad está establecida en `Department`. En la base de datos, el `Course` tabla no contiene el nombre del departamento; contiene un `DepartmentID` columna de clave externa. Si se consulta la base de datos directamente, para obtener el nombre de departamento, junto con los datos del curso, tendría que unir el `Course` y `Department` tablas. Estableciendo el `Include` propiedad `Department`, especifique que Entity Framework debe hacer el trabajo de introducción que se relaciona `Department` entidad cuando llega un `Course` entidad. El `Department` entidad, a continuación, se almacena en el `Department` propiedad de navegación de la `Course` entidad. (De forma predeterminada, el `SchoolEntities` clase generado por el Diseñador de modelos de datos recupera los datos relacionados cuando sea necesario y se ha enlazado el control de origen de datos a esa clase, por lo que establecer el `Include` propiedad no es necesaria. Sin embargo, si se establece mejora el rendimiento de la página, ya que en caso contrario, Entity Framework tendría separar las llamadas a la base de datos para recuperar datos para el `Course` entidades y para que se relaciona `Department` entidades.)

Después de la `EntityDataSource` control recién creado, inserte el siguiente marcado para crear un `QueryExtender` control que está enlazado a la que `EntityDataSource` control.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

El `SearchExpression` elemento especifica que desea seleccionar cursos cuyos títulos coincidan con el valor especificado en el cuadro de texto. Solo cuando se comparan todos los caracteres que se escriben en el cuadro de texto porque el `SearchType` especifica la propiedad `StartsWith`.

El `OrderByExpression` elemento especifica que el conjunto de resultados se ordenarán por título del curso en el nombre de departamento. Tenga en cuenta cómo se especifica el nombre de departamento: `Department.Name`. Dado que la asociación entre el `Course` entidad y el `Department` entidad es uno a uno, el `Department` contiene la propiedad de navegación un `Department` entidad. (Si se tratara de una relación uno a varios, la propiedad contendría una colección). Para obtener el nombre del departamento, se debe especificar el `Name` propiedad de la `Department` entidad.

Por último, agregue un `GridView` control para mostrar la lista de cursos:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La primera columna es un campo de plantilla que muestra el nombre del departamento. Especifica la expresión de enlace de datos `Department.Name`, tal como se vio en el `QueryExtender` control.

Ejecute la página. La pantalla inicial muestra una lista de todos los cursos en orden, por departamento y, a continuación, por título del curso.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Escriba una "m" y haga clic en **búsqueda** para ver todos los cursos cuyos títulos comienzan por "m" (la búsqueda no se distingue mayúsculas de minúsculas).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Usar el operador "Like" para filtrar los datos

Puede lograr un efecto similar a la `QueryExtender` del control `StartsWith`, `Contains`, y `EndsWith` buscar tipos utilizando un `Like` operador en el `EntityDataSource` del control `Where` propiedad. En esta parte del tutorial, verá cómo usar la `Like` operador para buscar un alumno por nombre.

Abra *Students.aspx* en **origen** vista. Después de la `GridView` control, agregue el siguiente marcado:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Este marcado es similar a lo que ha visto anteriormente, excepto el `Where` valor de propiedad. La segunda parte de la `Where` expresión define una búsqueda de subcadena (`LIKE %FirstMidName% or LIKE %LastName%`) que busca los nombres y apellidos para todo lo que se escribe en el cuadro de texto.

Ejecute la página. Inicialmente verá todos los estudiantes porque el valor predeterminado para el `StudentName` parámetro es "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Escriba la letra "g" en el cuadro de texto y haga clic en **búsqueda**. Verá una lista de alumnos que tener una "g" en uno el nombre o apellido.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Ahora ha muestra, actualiza, filtrado, ordenado y agrupan los datos de tablas individuales. En el siguiente tutorial comenzará a trabajar con datos relacionados (escenarios de maestro y detalles).

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-2.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-4.md)
