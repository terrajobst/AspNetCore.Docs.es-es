---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
title: "Introducción a la base de datos de Entity Framework 4.0 en primer lugar y 4 de ASP.NET Web Forms - parte 2 | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. Es la aplicación de ejemplo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: fb63a326-a4ae-4b0c-a4f5-412327197216
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2
msc.type: authoredcontent
ms.openlocfilehash: 4e2a3176aaedccd40ef6b619efa3c4052dd8470b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-2"></a>Introducción a la base de datos de Entity Framework 4.0 en primer lugar y ASP.NET 4 Web Forms - parte 2
====================
Por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="the-entitydatasource-control"></a>El Control EntityDataSource

En el tutorial anterior creó un sitio web, una base de datos y un modelo de datos. En este tutorial se trabaja con el `EntityDataSource` control que proporciona ASP.NET para que sea más fácil trabajar con un modelo de datos de Entity Framework. Creará un `GridView` control para mostrar y editar datos de estudiante, un `DetailsView` control para agregar nuevos alumnos y un `DropDownList` control para seleccionar un departamento (que va a utilizar más adelante para mostrar cursos asociados).

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image1.png)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image3.png)

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image5.png)

Tenga en cuenta que en esta aplicación no puede agregar validación de entrada a páginas que actualizan la base de datos y algunos de control de errores no serán tan sólida como sean necesarias en una aplicación de producción. Que mantiene el tutorial se centra en Entity Framework y mantiene obtengan demasiado largo. Para obtener más información acerca de cómo agregar estas características a la aplicación, consulte [validar entrada de usuario en ASP.NET Web Pages](https://msdn.microsoft.com/en-us/library/7kh55542.aspx) y [control de errores en las páginas ASP.NET y aplicaciones](https://msdn.microsoft.com/en-us/library/w16865z6.aspx).

## <a name="adding-and-configuring-the-entitydatasource-control"></a>Agregar y configurar el Control EntityDataSource

Comenzará configurando un `EntityDataSource` control leer `Person` entidades desde el `People` conjunto de entidades.

Asegúrese de que tiene Visual Studio abra y que está trabajando con el proyecto creado en la parte 1. Si aún no lo ha generado el proyecto desde que se creó el modelo de datos o desde el último cambio realizados, compile el proyecto ahora. Cambios en el modelo de datos no están disponibles para el diseñador hasta que se compila el proyecto.

Cree una nueva página web mediante la **formulario Web con página maestra** plantilla y asígnele el nombre *Students.aspx*.

[![Image23](the-entity-framework-and-aspnet-getting-started-part-2/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image7.png)

Especifique *Site.Master* como la página maestra. Todas las páginas que se crean para estos tutoriales usará esta página maestra.

[![Image24](the-entity-framework-and-aspnet-getting-started-part-2/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image9.png)

En **origen** ver, agregar un `h2` encabezado para el `Content` control denominado `Content2`, tal y como se muestra en el ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample1.aspx)]

Desde el **datos** pestaña de la **cuadro de herramientas**, arrastre un `EntityDataSource` el control a la página, colóquela debajo del encabezado y cambie el identificador a `StudentsEntityDataSource`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample2.aspx)]

Cambie a **diseño** ver, haga clic en la etiqueta inteligente del control de origen de datos y, a continuación, haga clic en **Configurar origen de datos** para iniciar el **Configurar origen de datos** asistente.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-2/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image11.png)

En el **Configurar ObjectContext** paso del asistente, seleccione **SchoolEntities** como el valor de **conexión denominada**y seleccione **SchoolEntities**como el **DefaultContainerName** valor. Después, haga clic en **Siguiente**.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-2/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image13.png)

Nota: Si se produce el siguiente cuadro de diálogo en este punto, tiene que generar el proyecto antes de continuar.

[![Image25](the-entity-framework-and-aspnet-getting-started-part-2/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image15.png)

En el **Configurar selección de datos** paso, seleccione **personas** como el valor de **EntitySetName**. En **seleccione**, asegúrese de que el **seleccione A** ll casilla está activada. A continuación, seleccione las opciones para habilitar la actualización y eliminación. Cuando haya terminado, haga clic en **finalizar**.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-2/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image17.png)

## <a name="configuring-database-rules-to-allow-deletion"></a>Configurar reglas de la base de datos para permitir la eliminación

Que va a crear una página que permite a los usuarios eliminar alumnos desde el `Person` tabla, que tiene tres relaciones con otras tablas (`Course`, `StudentGrade`, y `OfficeAssignment`). De forma predeterminada, la base de datos no podrá eliminar una fila de `Person` si hay filas relacionadas en una de las otras tablas. Puede eliminar manualmente las filas relacionadas en primer lugar, o puede configurar la base de datos para eliminar automáticamente cuando se elimina un `Person` fila. Para los registros de estudiantes en este tutorial, configurará la base de datos para eliminar automáticamente los datos relacionados. Dado que los estudiantes pueden haber filas relacionadas solo en el `StudentGrade` tabla, debe configurar solo una de las tres relaciones.

Si usas el *School.mdf* archivo que descargó desde el proyecto que va con este tutorial, puede omitir esta sección porque ya se han realizado estos cambios de configuración. Si ha creado la base de datos mediante la ejecución de una secuencia de comandos, configure la base de datos mediante la realización de los procedimientos siguientes.

En **Explorador de servidores**, abra el diagrama de base de datos que creó en la parte 1. Haga clic en la relación entre `Person` y `StudentGrade` (la línea entre las tablas) y, a continuación, seleccione **propiedades**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-2/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image19.png)

En el **propiedades** ventana, expanda **INSERT y UPDATE especificación** y establezca el **DeleteRule** propiedad **Cascade**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-2/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image21.png)

Guarde y cierre el diagrama. Si se le pregunta si desea actualizar la base de datos, haga clic en **Sí**.

Para asegurarse de que el modelo mantiene las entidades que están en la memoria en sincronización con lo que está haciendo la base de datos, debe establecer las reglas correspondientes en el modelo de datos. Abra *SchoolModel.edmx*, haga clic en la línea de asociación entre `Person` y `StudentGrade`y, a continuación, seleccione **propiedades**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-2/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image23.png)

En el **propiedades** ventana, establezca **End1 OnDelete** a **Cascade**.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-2/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image25.png)

Guarde y cierre el *SchoolModel.edmx* de archivos y, a continuación, volver a generar el proyecto.

En general, cuando se cambia la base de datos, tiene varias opciones sobre cómo el modelo de sincronización:

- Para ciertos tipos de cambios (por ejemplo, agregar o actualizar tablas, vistas o procedimientos almacenados), pulse el botón derecho en el diseñador y seleccione **actualizar modelo desde base de datos** para que el diseñador realizar los cambios automáticamente.
- Volver a generar el modelo de datos.
- Realizar actualizaciones manuales como esta.

En este caso, puede regenerar el modelo o actualizar las tablas afectadas por el cambio de relación, pero, a continuación, tendría que realizar el cambio de nombre de campo nuevo (desde `FirstName` a `FirstMidName`).

## <a name="using-a-gridview-control-to-read-and-update-entities"></a>Utilizar un Control GridView para leer y actualizar entidades

En esta sección usará un `GridView` control para mostrar, actualizar o eliminar los alumnos.

Abra o cambie a *Students.aspx* y cambie a **diseño** vista. Desde el **datos** pestaña de la **cuadro de herramientas**, arrastre un `GridView` control a la derecha de la `EntityDataSource` de control, asígnele el nombre `StudentsGridView`, haga clic en la etiqueta inteligente y, a continuación, seleccione  **StudentsEntityDataSource** como origen de datos.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-2/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image27.png)

Haga clic en **actualizar esquema** (haga clic en **Sí** si se le pide que confirme), a continuación, haga clic en **Habilitar paginación**, **Habilitar ordenación**, **Habilitar edición**, y **Habilitar eliminación**.

Haga clic en **Editar columnas**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-2/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image29.png)

En el **campos seleccionados** box, elimine **PersonID**, **LastName**, y **HireDate**. Normalmente no mostrar una clave de registro a los usuarios, fecha de contratación no es relevante para los alumnos y pondrá en práctica todas las partes del nombre en un campo, para que solo tenga uno de los campos de nombre.)

[![Image11](the-entity-framework-and-aspnet-getting-started-part-2/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image31.png)

Seleccione el **FirstMidName** campo y, a continuación, haga clic en **convertir este campo en TemplateField**.

Haga lo mismo **EnrollmentDate**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-2/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image33.png)

Haga clic en **Aceptar** y, a continuación, cambie a **origen** vista. Los cambios restantes será más fáciles realizar directamente en el marcado. El `GridView` controlar marcado ahora parece similar al ejemplo siguiente.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample3.aspx)]

La primera columna después de que el campo de comando es un campo de plantilla que actualmente muestra el nombre. Cambie el marcado para este campo de la plantilla para que sea similar al ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample4.aspx)]

En el modo de presentación, dos `Label` controles muestran el nombre y apellidos. En modo de edición, se proporcionan dos cuadros de texto para que pueda cambiar el nombre y apellidos. Al igual que con la `Label` controles en modo de presentación, usar `Bind` y `Eval` expresiones exactamente como se harían con controles de origen de datos ASP.NET que se conectan directamente a las bases de datos. La única diferencia es que debe especificar propiedades de la entidad en lugar de columnas de base de datos.

La última columna es un campo de plantilla que muestra la fecha de inscripción. Cambie el marcado para este campo debe ser similar en el ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample5.aspx)]

En ambos, mostrar y modo de edición, la fecha que se mostrará en el formato "fecha corta" hace que la cadena de formato "{0, d}". (El equipo puede configurarse para mostrar este formato de manera diferente a las imágenes de pantalla que se muestra en este tutorial.)

Observe que en cada uno de estos campos de plantilla, el diseñador utiliza un `Bind` expresión de manera predeterminada, pero se ha cambiado a un `Eval` expresión en el `ItemTemplate` elementos. El `Bind` expresión hace que los datos estén disponibles en `GridView` propiedades del control en caso de que necesite obtener acceso a los datos en el código. En esta página no es necesario tener acceso a estos datos en el código, por lo que puede usar `Eval`, lo que resulta más eficaz. Para obtener más información, consulte [obtener sus datos fuera de los controles de datos](https://weblogs.asp.net/davidfowler/archive/2008/12/12/getting-your-data-out-of-the-data-controls.aspx).

## <a name="revising-entitydatasource-control-markup-to-improve-performance"></a>Revisión de marcado del Control EntityDataSource para mejorar el rendimiento

En el marcado para la `EntityDataSource` controlar, quite el `ConnectionString` y `DefaultContainerName` atributos y sustituirlas por un `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. Se trata de un cambio que se debe realizar cada vez que cree un `EntityDataSource` controlar, a menos que necesite usar una conexión que es diferente de la que está codificado de forma rígida en la clase de contexto de objeto. Mediante el `ContextTypeName` atributo proporciona las siguientes ventajas:

- Mejor rendimiento. Cuando el `EntityDataSource` control inicializa el modelo de datos con la `ConnectionString` y `DefaultContainerName` atributos, realiza un trabajo adicional para cargar los metadatos en cada solicitud. Esto no es necesario si se especifica la `ContextTypeName` atributo.
- Carga diferida está activada de forma predeterminada en las clases de contexto del objeto generado (como `SchoolEntities` en este tutorial) en Entity Framework 4.0. Esto significa que las propiedades de navegación se cargan con los datos relacionados automáticamente derecha cuando lo necesite. Carga diferida se explica con más detalle más adelante en este tutorial.
- Las personalizaciones que se hayan aplicado a la clase de contexto de objeto (en este caso, el `SchoolEntities` clase) estará disponible para los controles que utilizan el `EntityDataSource` control. La personalización de la clase de contexto de objeto es un tema avanzado que no se trata en esta serie de tutoriales. Para obtener más información, consulte [extender Entity Framework genera tipos](https://msdn.microsoft.com/en-us/library/dd456844.aspx).

El marcado ahora parecerán al ejemplo siguiente (el orden de las propiedades puede variar):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample6.aspx)]

El `EnableFlattening` atributo hace referencia a una característica que se necesitaba en versiones anteriores de Entity Framework porque las columnas de clave externa no se exponen como propiedades de la entidad. La versión actual permite usar *asociaciones de clave externas*, lo que significa que las propiedades de clave externa se exponen para todos menos-to-many asociaciones. Si las entidades tienen propiedades de clave externa y no [tipos complejos](https://msdn.microsoft.com/en-us/library/bb738472.aspx), puede dejar este atributo establecido en `False`. No quite el atributo de marcado, ya que el valor predeterminado es `True`. Para obtener más información, consulte [Aplanar objetos (EntityDataSource)](https://msdn.microsoft.com/en-us/library/ee404746.aspx).

Ejecute la página y ver una lista de los alumnos y los empleados (se filtran para estudiantes solo en el tutorial siguiente). El nombre y apellidos se muestran a la vez.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-2/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image35.png)

Para ordenar la presentación, haga clic en un nombre de columna.

Haga clic en **editar** en ninguna fila. Se muestran cuadros de texto donde puede cambiar el nombre y apellidos.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-2/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image37.png)

El **eliminar** botón también funciona. Haga clic en Eliminar para una fila que tenga una fecha de inscripción y desaparece de la fila. (Filas sin una fecha de inscripción representan instructores y es posible que obtenga un error de integridad referencial. En el siguiente tutorial se filtrará esta lista para incluir solo los alumnos.)

## <a name="displaying-data-from-a-navigation-property"></a>Mostrar datos de una propiedad de navegación

Ahora suponga que desea saber cuántos cursos cada estudiante está inscrito en. Entity Framework proporciona esa información en el `StudentGrades` propiedad de navegación de la `Person` entidad. Dado que el diseño de base de datos no permite un estudiante para inscribirse en un curso sin tener un grado asignado, para este tutorial puede suponer que tiene una fila la `StudentGrade` fila de la tabla que está asociado a un curso es el mismo que se inscribe en el curso. (El `Courses` propiedad de navegación es solo para instructores.)

Cuando se usa el `ContextTypeName` atributo de la `EntityDataSource` (control), Entity Framework recupera automáticamente la información para una propiedad de navegación cuando tiene acceso a esa propiedad. Esto se denomina *carga diferida*. Sin embargo, esto puede ser ineficaz, ya que tiene como resultado una llamada independiente a la base de datos que se necesita cada vez más información. Si necesita datos de la propiedad de navegación para cada entidad devuelta por la `EntityDataSource` (control), resulta más eficaz para recuperar los datos relacionados junto con la entidad en una sola llamada a la base de datos. Esto se denomina *carga diligente*, y especifique carga diligente para una propiedad de navegación estableciendo la `Include` propiedad de la `EntityDataSource` control.

En *Students.aspx*, desea mostrar el número de cursos para cada estudiante, carga diligente por lo que es la mejor opción. Si se muestra todos los alumnos pero que muestra el número de cursos solo para algunas de ellas (lo que requeriría escribiendo código además del marcado), carga diferida podría ser una opción mejor.

Abra o cambie a *Students.aspx*, cambie a **diseño** visualizarla, seleccione `StudentsEntityDataSource`y en el **propiedades** ventana conjunto el **Include**propiedad **StudentGrades**. (Si desea obtener varias propiedades de navegación, puede especificar sus nombres separados por comas, por ejemplo, **StudentGrades, cursos**.)

[![Image19](the-entity-framework-and-aspnet-getting-started-part-2/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image39.png)

Cambie a **origen** vista. En el `StudentsGridView` control después de la última `asp:TemplateField` elemento, agregue el siguiente campo de plantilla nueva:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample7.aspx)]

En el `Eval` expresión, puede hacer referencia a la propiedad de navegación `StudentGrades`. Dado que esta propiedad contiene una colección, tiene un `Count` propiedad que puede usar para mostrar el número de cursos en el que está inscrito los estudiantes. En un tutorial posterior verá cómo mostrar los datos de las propiedades de navegación que contienen entidades únicas en lugar de colecciones. (Tenga en cuenta que no se puede utilizar `BoundField` elementos que se va a mostrar los datos de las propiedades de navegación.)

Ejecute la página y verá ahora cuántos cursos estudiante está inscrito en.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-2/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image41.png)

## <a name="using-a-detailsview-control-to-insert-entities"></a>Usar un Control DetailsView para insertar las entidades

El paso siguiente consiste en crear una página que tiene un `DetailsView` control que le permitirá agregar nuevos alumnos. Cierre el explorador y, a continuación, cree una nueva página web mediante la *Site.Master* página maestra. Nombre de la página *StudentsAdd.aspx*y, a continuación, cambie a **origen** vista.

Agregue el siguiente marcado para reemplazar el marcado existente para la `Content` control denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample8.aspx)]

Este marcado crea un `EntityDataSource` control que es similar a la que creó en *Students.aspx*, excepto en que permite la inserción. Al igual que con la `GridView` controlar, los campos enlazados de la `DetailsView` control se codifican de forma exactamente como se haría para un control de datos que se conecta directamente a una base de datos, salvo que hagan referencia a propiedades de la entidad. En este caso, el `DetailsView` control solo se utiliza para insertar filas, por lo que se ha establecido el modo predeterminado `Insert`.

Ejecute la página y agregue un nuevo alumno.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-2/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image43.png)

No sucederá nada después de insertar un nuevo alumno, pero si ejecuta ahora *Students.aspx*, verá la nueva información de estudiantes.

## <a name="displaying-data-in-a-drop-down-list"></a>Mostrar datos en una lista desplegable

En los pasos siguientes le enlazar un `DropDownList` control a una entidad que se establecen a través de un `EntityDataSource` control. En esta parte del tutorial, no hace mucho con esta lista. Sin embargo, en las siguientes partes, deberá usar la lista para permitir al usuario seleccionar un departamento para mostrar cursos asociados con el departamento.

Crear una nueva página web denominada *Courses.aspx*. En **origen** ver, agregar un título a la `Content` control denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample9.aspx)]

En **diseño** ver, agregar un `EntityDataSource` control a la página que lo hizo antes, pero esta vez asígnele el nombre `DepartmentsEntityDataSource`. Seleccione **departamentos** como el **EntitySetName** valor y seleccione solo la **DepartmentID** y **nombre** propiedades.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-2/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image45.png)

Desde el **estándar** pestaña de la **cuadro de herramientas**, arrastre un `DropDownList` a la página de control, asígnele el nombre `DepartmentsDropDownList`, haga clic en la etiqueta inteligente y seleccione **Elegir origen de datos** a iniciar el **Asistente de configuración de origen de datos**.

[![Image16](the-entity-framework-and-aspnet-getting-started-part-2/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image47.png)

En el **elegir un origen de datos** paso, seleccione **DepartmentsEntityDataSource** como origen de datos, haga clic en **actualizar esquema**y, a continuación, seleccione **nombre** como el campo de datos para mostrar y **DepartmentID** como el campo de datos de valor. Haga clic en **Aceptar**.

[![Image17](the-entity-framework-and-aspnet-getting-started-part-2/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image49.png)

El método que se utiliza para enlazar con el control mediante Entity Framework es el mismo que con otros datos ASP.NET está especificando los controles de origen excepto si las entidades y propiedades de la entidad.

Cambie a **origen** ver y agregar "Seleccione un departamento:" inmediatamente antes de la `DropDownList` control.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-2/samples/sample10.aspx)]

Como recordatorio, cambian el marcado para la `EntityDataSource` en este punto de control si se reemplaza el `ConnectionString` y `DefaultContainerName` atributos con un `ContextTypeName="ContosoUniversity.DAL.SchoolEntities"` atributo. A menudo es mejor que esperar hasta después de haber creado el control enlazado a datos que está vinculado al control del origen de datos antes de cambiar el `EntityDataSource` control marcado, ya que después de realizar el cambio, el diseñador no le proporcionarán una **actualizar Esquema** opción en el control enlazado a datos.

Ejecute la página y puede seleccionar un departamento de la lista desplegable.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-2/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-2/_static/image51.png)

Con esto finaliza la introducción al uso de la `EntityDataSource` control. Trabajar con este control generalmente no es diferente de trabajar con otros datos ASP.NET, controles de origen, salvo que se hacen referencia a entidades y propiedades en lugar de tablas y columnas. La única excepción es cuando desea tener acceso a propiedades de navegación. En el siguiente tutorial, verá que la sintaxis utilice con `EntityDataSource` control también puede diferir de otros controles de origen de datos al filtrar, agrupar y ordenar los datos.

>[!div class="step-by-step"]
[Anterior](the-entity-framework-and-aspnet-getting-started-part-1.md)
[Siguiente](the-entity-framework-and-aspnet-getting-started-part-3.md)
