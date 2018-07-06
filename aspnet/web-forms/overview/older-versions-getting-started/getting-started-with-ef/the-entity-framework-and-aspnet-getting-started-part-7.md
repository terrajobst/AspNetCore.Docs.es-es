---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introducción a Entity Framework 4.0 Database First y 4 de ASP.NET Web Forms, parte 7 | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. La aplicación de ejemplo es...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb1abe52b913785f47b7f8ec9822ed9db5ee8a05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823412"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Introducción a Entity Framework 4.0 Database First y ASP.NET 4 Web Forms, parte 7
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Utilizar procedimientos almacenados

En el tutorial anterior implementa un patrón de herencia de tabla por jerarquía. Este tutorial le mostrará cómo utilizar procedimientos almacenados para obtener más control sobre el acceso de la base de datos.

Entity Framework le permite especificar que debe usar los procedimientos almacenados para el acceso a la base de datos. Para cualquier tipo de entidad, puede especificar un procedimiento almacenado que se usará para la creación, actualización o eliminación de entidades de ese tipo. A continuación, en el modelo de datos puede agregar referencias a procedimientos almacenados que puede usar para realizar tareas como la recuperación de conjuntos de entidades.

Uso de procedimientos almacenados es un requisito común para el acceso a la base de datos. En algunos casos, un administrador de base de datos puede requerir que todos los accesos de base de datos pasen por los procedimientos almacenados por motivos de seguridad. En otros casos, puede crear lógica de negocios en algunos de los procesos que Entity Framework usa cuando actualiza la base de datos. Por ejemplo, cada vez que se elimina una entidad puede copiarlo en una base de datos de archivo. O bien, siempre que se actualiza una fila puede escribir una fila en una tabla de registro que registra en el que hizo el cambio. Puede realizar estos tipos de tareas en un procedimiento almacenado que se llama cada vez que elimina una entidad de Entity Framework o actualiza una entidad.

Como se muestra en el tutorial anterior, no creará todas las páginas nuevas. En su lugar, va a cambiar la manera en que Entity Framework tiene acceso a la base de datos para algunas de las páginas que ya ha creado.

En este tutorial podrá crear procedimientos almacenados en la base de datos para insertar `Student` y `Instructor` entidades. Deberá agregarlos al modelo de datos, y especificará el Entity Framework debe usarlos para agregar `Student` y `Instructor` entidades a la base de datos. También creará un procedimiento almacenado que se puede utilizar para recuperar `Course` entidades.

## <a name="creating-stored-procedures-in-the-database"></a>Creación de procedimientos almacenados en la base de datos

(Si está utilizando el *School.mdf* archivo del proyecto disponible para su descarga con este tutorial, puede omitir esta sección porque ya existen los procedimientos almacenados.)

En **Explorador de servidores**, expanda *School.mdf*, haga clic en **Stored Procedures**y seleccione **Agregar nuevo procedimiento almacenado**.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copie las instrucciones SQL siguientes y péguelos en la ventana de procedimiento almacenado, reemplazando el procedimiento almacenado esqueleto.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![Image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` las entidades tienen cuatro propiedades: `PersonID`, `LastName`, `FirstName`, y `EnrollmentDate`. La base de datos el valor de identificador genera automáticamente y el procedimiento almacenado acepta parámetros para los otros tres. El procedimiento almacenado devuelve el valor de la clave de registro de la fila nueva para que Entity Framework puede realizar un seguimiento de la versión de la entidad que mantiene en memoria.

Guarde y cierre la ventana de procedimiento almacenado.

Crear un `InsertInstructor` procedimiento almacenado en la misma manera, mediante las instrucciones SQL siguientes:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Crear `Update` procedimientos almacenados para `Student` y `Instructor` entidades también. (La base de datos ya tiene un `DeletePerson` procedimiento almacenado que funcionará para ambos `Instructor` y `Student` entidades.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

En este tutorial, asignará las tres funciones: insert, update y delete, para cada tipo de entidad. La versión 4 de Entity Framework permite asignar solo uno o dos de estas funciones a los procedimientos almacenados sin asignación de los demás, con una excepción: si asigna la función de actualización pero no la función de eliminación, Entity Framework se iniciará una excepción cuando se intento de eliminar una entidad. En Entity Framework versión 3.5, no tenía tanta flexibilidad para asignar procedimientos almacenados: si se ha asignado una función ha sido necesario para asignar las tres.

Para crear un procedimiento almacenado que lee en lugar de las actualizaciones de datos, cree uno que todos los selecciona `Course` entidades, mediante las instrucciones SQL siguientes:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Adición de los procedimientos almacenados al modelo de datos

Los procedimientos almacenados se definen ahora en la base de datos, pero deben agregarse al modelo de datos para que estén disponibles para Entity Framework. Abra *SchoolModel.edmx*, haga clic en la superficie de diseño y seleccione **actualizar modelo desde base de datos**. En el **agregar** pestaña de la **elija los objetos de base de datos** cuadro de diálogo, expanda **procedimientos almacenados**, seleccione los procedimientos almacenados recién creados y `DeletePerson` procedimiento almacenado y, a continuación, haga clic en **finalizar**.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Asignación de los procedimientos almacenados

En el Diseñador de modelos de datos, haga clic en el `Student` entidad y seleccione **asignación de procedimientos almacenados**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

El **detalles de Mapping** aparece la ventana, en el que puede especificar los procedimientos almacenados que Entity Framework debe usar para insertar, actualizar y eliminar entidades de este tipo.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Establecer el **insertar** función **InsertStudent**. La ventana muestra una lista de parámetros de procedimiento almacenado, los cuales deben asignarse a una propiedad de entidad. Dos de estas se asignan automáticamente porque los nombres son iguales. No hay ninguna propiedad de entidad denominada `FirstName`, por lo que debe seleccionar manualmente `FirstMidName` desde una lista desplegable que muestra las propiedades de entidad disponible. (Esto es porque ha cambiado el nombre de la `FirstName` propiedad `FirstMidName` en el primer tutorial.)

[![Image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

En el mismo **detalles de Mapping** ventana, asigne el `Update` función a la `UpdateStudent` procedimiento almacenado (asegúrese de especificar `FirstMidName` como el valor del parámetro `FirstName`, tal y como hizo con el `Insert` procedimiento almacenado) y el `Delete` función a la `DeletePerson` procedimiento almacenado.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Siga el mismo procedimiento para asignar el insert, update y delete de procedimientos almacenados para instructores para el `Instructor` entidad.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Los procedimientos almacenados que leen en lugar de actualizar los datos, usa el **Explorador de modelos** escribirlo devuelve ventana para el procedimiento almacenado se asignan a la entidad. En el Diseñador de modelos de datos, haga clic en la superficie de diseño y seleccione **Explorador de modelos**. Abra el **SchoolModel.Store** nodo y, a continuación, abra el **Stored Procedures** nodo. A continuación, haga clic en el `GetCourses` procedimiento almacenado y seleccione **Agregar importación de función**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

En el **Agregar importación de función** cuadro de diálogo **devuelve una colección de** seleccione **entidades**y, a continuación, seleccione `Course` como el tipo de entidad devueltos. Cuando haya terminado, haga clic en **Aceptar**. Guarde y cierre el *.edmx* archivo.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Mediante la inserción, actualización y eliminación de los procedimientos almacenados

Procedimientos almacenados para insertar, actualizar y eliminar datos se usan por Entity Framework automáticamente después de agregarlos al modelo de datos y les asigna a las entidades adecuadas. Ahora puede ejecutar el *StudentsAdd.aspx* página, y cada vez que crea un nuevo alumno, Entity Framework usará el `InsertStudent` procedimiento almacenado para agregar la nueva fila a la `Student` tabla.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Ejecute el *Students.aspx* página y el alumno nuevo aparece en la lista.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Cambie el nombre para comprobar que funciona la función de actualización y, a continuación, elimine el estudiante para comprobar que funciona la función de eliminación.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Uso de procedimientos almacenados Select

Entity Framework no se ejecuta automáticamente los procedimientos almacenados como `GetCourses`, y no se pueden utilizar con el `EntityDataSource` control. Para poder utilizarlos, llamarlos desde el código.

Abra el *InstructorsCourses.aspx.cs* archivo. El `PopulateDropDownLists` método usa una consulta de LINQ to Entities para recuperar todas las entidades course, por lo que puede recorrer en iteración la lista y determinar las que está asignado un instructor y cuáles están asignados:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Reemplazar por el código siguiente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La página ahora usa el `GetCourses` procedimiento almacenado para recuperar la lista de todos los cursos. Ejecute la página para comprobar que funciona igual que antes.

(Es posible que las propiedades de navegación de las entidades recuperadas por un procedimiento almacenado no se rellena automáticamente con los datos relacionados con las entidades, según `ObjectContext` configuración predeterminada. Para obtener más información, consulte [cargar objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx) en MSDN Library.)

En el siguiente tutorial, obtendrá información sobre cómo usar la funcionalidad de datos dinámicos para facilitar la prueba y el programa de reglas de formato y validación de datos. En lugar de especificar en las reglas de cada página web como las cadenas de formato de datos y si se requiere un campo, puede especificar estas reglas en los metadatos del modelo de datos y se aplican automáticamente en todas las páginas.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-8.md)
