---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
title: "Introducción a la base de datos de Entity Framework 4.0 en primer lugar y 4 de ASP.NET Web Forms - parte 3 | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. Es la aplicación de ejemplo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: ccdc3f8c-2568-40a7-8f8b-3c23d2e05388
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-3
msc.type: authoredcontent
ms.openlocfilehash: 1ec8891c4ccf71494389ba562fdfb4b88055d12f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-3"></a>Introducción a la base de datos de Entity Framework 4.0 en primer lugar y ASP.NET 4 Web Forms - parte 3
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="filtering-ordering-and-grouping-data"></a>Filtrado, ordenación y agrupación de datos

En el tutorial anterior ha utilizado el `EntityDataSource` control para mostrar y editar datos. En este tutorial filtrar, ordenar y agrupar los datos. Al hacer esto estableciendo las propiedades de la `EntityDataSource` (control), la sintaxis es diferente de otros controles de origen de datos. Como verá, sin embargo, puede usar el `QueryExtender` control minimizar estas diferencias.

Cambiará la *Students.aspx* página para filtrar para estudiantes, ordene por nombre y búsqueda de nombre. También podrá cambiar la *Courses.aspx* página para mostrar cursos para el departamento seleccionado y Buscar cursos por su nombre. Por último, agregará las estadísticas de estudiante en la *About.aspx* página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image1.png)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image3.png)

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image5.png)

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image7.png)

## <a name="using-the-entitydatasource-where-property-to-filter-data"></a>Mediante la propiedad de EntityDataSource "Where" para filtrar los datos

Abra la *Students.aspx* página que creó en el tutorial anterior. Tal como está configurado, el `GridView` control en la página muestra todos los nombres de los `People` conjunto de entidades. Sin embargo, en el que desea mostrar solo los estudiantes, que puede encontrar seleccionando `Person` entidades que tienen fechas de inscripción no null.

Cambie a **diseño** ver y seleccionar el `EntityDataSource` control. En el **propiedades** ventana, establezca el `Where` propiedad `it.EnrollmentDate is not null`.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-3/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image9.png)

La sintaxis usada en la `Where` propiedad de la `EntityDataSource` control es Entity SQL. Entity SQL es similar a Transact-SQL, pero se personaliza para su uso con entidades en lugar de objetos de base de datos. En la expresión `it.EnrollmentDate is not null`, la palabra `it` representa una referencia a la entidad devuelta por la consulta. Por lo tanto, `it.EnrollmentDate` hace referencia a la `EnrollmentDate` propiedad de la `Person` entidad que la `EntityDataSource` controlar devuelve.

Ejecute la página. La lista de estudiantes contiene ahora sólo los alumnos. (No hay ninguna fila muestra donde no hay ninguna fecha de inscripción.)

[![Image02](the-entity-framework-and-aspnet-getting-started-part-3/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image11.png)

## <a name="using-the-entitydatasource-orderby-property-to-order-data"></a>Mediante la propiedad de "OrderBy" EntityDataSource datos del pedido

También desea que esta lista estará en el orden de los nombres cuando se muestra por primera vez. Con el *Students.aspx* página todavía abierta en **diseño** vista y con el `EntityDataSource` control aún seleccionado, en la **propiedades** ventana conjunto el  **OrderBy** propiedad `it.LastName`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-3/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image13.png)

Ejecute la página. La lista de estudiantes ahora está en orden por apellido.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-3/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image15.png)

## <a name="using-a-control-parameter-to-set-the-where-property"></a>Usar un parámetro de Control para establecer la propiedad "Donde"

Como con otros controles de origen de datos, puede pasar valores de parámetro para el `Where` propiedad. En el *Courses.aspx* página que creó en la parte 2 del tutorial, puede utilizar este método para mostrar cursos en los que están asociados con el departamento de que un usuario selecciona en la lista desplegable.

Abra *Courses.aspx* y cambie a **diseño** vista. Agregue un segundo `EntityDataSource` el control a la página y asígnele el nombre `CoursesEntityDataSource`. Conectarse a la `SchoolEntities` de modelo y seleccione `Courses` como el **EntitySetName** valor.

En el **propiedades** ventana, haga clic en el botón de puntos suspensivos en el **donde** cuadro de la propiedad. (Asegúrese de que el `CoursesEntityDataSource` control aún está seleccionado antes de usar el **propiedades** ventana.)

[![Image06](the-entity-framework-and-aspnet-getting-started-part-3/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image17.png)

El **Editor de expresiones** se muestra el cuadro de diálogo. En este cuadro de diálogo, seleccione **genere automáticamente expresión basada en los parámetros proporcionados**y, a continuación, haga clic en **Agregar parámetro**. Nombre del parámetro `DepartmentID`, seleccione **Control** como el **origen del parámetro** valor y seleccione **DepartmentsDropDownList** como el **ControlID**  valor.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-3/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image19.png)

Haga clic en **mostrar propiedades avanzadas**y en el **propiedades** ventana de la **Editor de expresiones** cuadro de diálogo, cambie el `Type` propiedad `Int32`.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-3/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image21.png)

Cuando haya terminado, haga clic en **Aceptar**.

Debajo de la lista desplegable, agregue un `GridView` el control a la página y asígnele el nombre `CoursesGridView`. Conectarse a la `CoursesEntityDataSource` del origen de datos (control), haga clic en **actualizar esquema**, haga clic en **Editar columnas**y quitar la `DepartmentID` columna. El `GridView` marcado del control es similar al siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample1.aspx)]

Cuando el usuario cambia el departamento seleccionado en la lista desplegable, desea que la lista de cursos asociados que cambie automáticamente. Para que esto suceda, seleccione la lista desplegable y en el **propiedades** ventana conjunto el `AutoPostBack` propiedad `True`.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-3/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image23.png)

Ahora que ha terminado de usar el diseñador, cambie a **origen** ver y reemplace la `ConnectionString` y `DefaultContainer` nombres de las propiedades de la `CoursesEntityDataSource` controlar con el `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Cuando haya terminado, el marcado para el control tendrá un aspecto similar al ejemplo siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample2.aspx)]

Ejecute la página y utilice la lista desplegable para seleccionar diferentes departamentos. Solo los cursos que ofrecerán el departamento seleccionado se muestran en el `GridView` control.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-3/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image25.png)

## <a name="using-the-entitydatasource-groupby-property-to-group-data"></a>Mediante la propiedad de "GroupBy" EntityDataSource para agrupar los datos

Supongamos que Contoso Universidad desea dejar algunas estadísticas de cuerpo del estudiante en su página de información sobre. En concreto, desea que se muestre un desglose de un número de alumnos por la fecha en que realizó.

Abra *About.aspx*y en **origen** ver, reemplace el contenido existente de la `BodyContent` controlar con "Estadísticas de cuerpo de estudiante" entre `h2` etiquetas:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample3.aspx)]

Después del encabezado, agregue un `EntityDataSource` controlar y asígnele el nombre `StudentStatisticsEntityDataSource`. Conectarse a `SchoolEntities`, seleccione la `People` entidad establece y deja el **seleccione** cuadro en el asistente sin cambios. Establezca las propiedades siguientes el **propiedades** ventana:

- Para filtrar sólo para estudiantes, establezca el `Where` propiedad `it.EnrollmentDate is not null`.
- Para agrupar los resultados por la fecha de inscripción, establecer el `GroupBy` propiedad `it.EnrollmentDate`.
- Para seleccionar la fecha de inscripción y el número de alumnos, establezca el `Select` propiedad `it.EnrollmentDate, Count(it.EnrollmentDate) AS NumberOfStudents`.
- Para ordenar los resultados por la fecha de inscripción, establecer el `OrderBy` propiedad `it.EnrollmentDate`.

En **origen** ver, reemplace la `ConnectionString` y `DefaultContainer` nombres de las propiedades con un `ContextTypeName` propiedad. El `EntityDataSource` marcado del control ahora similar al siguiente ejemplo.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample4.aspx)]

La sintaxis de la `Select`, `GroupBy`, y `Where` propiedades es similar a Transact-SQL, excepto para el `it` palabra clave que especifica la entidad actual.

Agregue el siguiente marcado para crear un `GridView` control para mostrar los datos.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample5.aspx)]

Ejecute la página para ver una lista que muestra el número de alumnos por fecha de inscripción.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-3/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image27.png)

## <a name="using-the-queryextender-control-for-filtering-and-ordering"></a>Mediante el Control QueryExtender para el filtrado y ordenación

El `QueryExtender` control proporciona una manera de especificar el filtro y ordenación en el marcado. La sintaxis es independiente del sistema de administración de base de datos (DBMS) que está usando. También es generalmente independiente de Entity Framework, con la excepción que es único a Entity Framework que se utiliza para las propiedades de navegación.

En esta parte del tutorial usará un `QueryExtender` control para filtrar y ordenar datos y uno de los campos de order by será una propiedad de navegación.

(Si lo prefiere utilizar código en lugar de marcado para ampliar las consultas generadas automáticamente por el `EntityDataSource` (control), puede hacerlo control de la `QueryCreated` eventos. Se trata cómo la `QueryExtender` el control se extiende `EntityDataSource` controlar las consultas también.)

Abra la *Courses.aspx* página y, a continuación el marcado que agregó previamente, inserte el siguiente marcado para crear un encabezado, un cuadro de texto para especificar cadenas de búsqueda, un botón de búsqueda y un `EntityDataSource` control que está enlazado a la `Courses` conjunto de entidades.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample6.aspx)]

Tenga en cuenta que la `EntityDataSource` del control `Include` propiedad está establecida en `Department`. En la base de datos, el `Course` tabla no contiene el nombre de departamento; contiene un `DepartmentID` columna de clave externa. Si se consulta la base de datos directamente, para obtener el nombre del departamento junto con datos de curso, se tiene que combinar la `Course` y `Department` tablas. Estableciendo la `Include` propiedad `Department`, permite especificar que Entity Framework debe hacer el trabajo de obtener relacionado `Department` entidad cuando llega un `Course` entidad. El `Department` entidad, a continuación, se almacena en la `Department` propiedad de navegación de la `Course` entidad. (De forma predeterminada, el `SchoolEntities` clase generada por el Diseñador de modelos de datos recupera datos relacionados cuando sea necesario y se ha enlazado el control de origen de datos a esa clase, por lo que establecer el `Include` propiedad no es necesaria. Sin embargo, si se establece mejora el rendimiento de la página, porque de lo contrario el Entity Framework haría que una llamada a la base de datos para recuperar datos para el `Course` entidades y para relacionado `Department` entidades.)

Después de la `EntityDataSource` control recién creado, inserte el siguiente marcado para crear un `QueryExtender` control que está enlazado a la `EntityDataSource` control.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample7.aspx)]

El `SearchExpression` elemento especifica que desea seleccionar cursos cuyos títulos coincidan con el valor especificado en el cuadro de texto. Solo cuando se comparan todos los caracteres que se escriben en el cuadro de texto porque el `SearchType` propiedad especifica `StartsWith`.

El `OrderByExpression` elemento especifica que el conjunto de resultados se ordenarán por título del curso en el nombre de departamento. Tenga en cuenta cómo se especifica el nombre de departamento: `Department.Name`. Dado que la asociación entre el `Course` entidad y la `Department` entidad es uno a uno, el `Department` contiene la propiedad de navegación un `Department` entidad. (Si se tratara de una relación uno a varios, la propiedad contendría una colección). Para obtener el nombre del departamento, se debe especificar el `Name` propiedad de la `Department` entidad.

Por último, agregue un `GridView` control para mostrar la lista de cursos:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample8.aspx)]

La primera columna es un campo de plantilla que muestra el nombre del departamento. Especifica la expresión de enlace de datos `Department.Name`, tal y como se vio en el `QueryExtender` control.

Ejecute la página. La pantalla inicial muestra una lista de todos los cursos en orden por departamento y, a continuación, por título del curso.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-3/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image29.png)

Escriba una "m" y haga clic en **búsqueda** para ver todos los cursos cuyos títulos comienzan por "m" (la búsqueda no es mayúsculas de minúsculas).

[![Image12](the-entity-framework-and-aspnet-getting-started-part-3/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image31.png)

## <a name="using-the-like-operator-to-filter-data"></a>Utilizar el operador "Como" para filtrar los datos

Puede lograr un efecto similar a la `QueryExtender` del control `StartsWith`, `Contains`, y `EndsWith` buscar tipos mediante un `Like` operador en el `EntityDataSource` del control `Where` propiedad. En esta parte del tutorial, verá cómo utilizar el `Like` operador que se va a buscar un estudiante por nombre.

Abra *Students.aspx* en **origen** vista. Después de la `GridView` de control, agregue el siguiente marcado:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-3/samples/sample9.aspx)]

Este marcado es similar a lo que ha visto anteriormente excepto la `Where` valor de propiedad. La segunda parte de la `Where` expresión define una búsqueda de la subcadena (`LIKE %FirstMidName% or LIKE %LastName%`) que busca los nombres y apellidos para lo que se escribe en el cuadro de texto.

Ejecute la página. Inicialmente se ven todos los alumnos porque el valor predeterminado para el `StudentName` parámetro es "%".

[![Image13](the-entity-framework-and-aspnet-getting-started-part-3/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image33.png)

Escriba la letra "g" en el cuadro de texto y haga clic en **búsqueda**. Ver una lista de alumnos que tienen una "g" en la vista el nombre de la primero o último.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-3/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-3/_static/image35.png)

Ahora ha muestra, actualiza, filtran, ordenan y agrupan los datos de tablas individuales. En el siguiente tutorial comenzará a trabajar con los datos relacionados (escenarios de principal-detalle).

>[!div class="step-by-step"]
[Anterior](the-entity-framework-and-aspnet-getting-started-part-2.md)
[Siguiente](the-entity-framework-and-aspnet-getting-started-part-4.md)
