---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Introducción a base de datos de Entity Framework 4.0 en primer lugar y 4 de ASP.NET Web Forms - parte 7 | Documentos de Microsoft
author: tdykstra
description: La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. Es la aplicación de ejemplo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: cb84f4f3e130fedb3e2f1a17d630767ff65bfa05
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Introducción a la base de datos de Entity Framework 4.0 en primer lugar y ASP.NET 4 Web Forms - parte 7
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Utilizar procedimientos almacenados

En el tutorial anterior implementa un patrón de herencia de tabla por jerarquía. Este tutorial le mostrará cómo usar procedimientos almacenados para tener un mayor control sobre el acceso de la base de datos.

Entity Framework permite especificar que deben utilizar procedimientos almacenados para el acceso a la base de datos. Para cualquier tipo de entidad, puede especificar un procedimiento almacenado que se usará para la creación, actualización o eliminación de entidades de ese tipo. A continuación, en el modelo de datos puede agregar referencias a los procedimientos almacenados que puede usar para realizar tareas como recuperar conjuntos de entidades.

Uso de procedimientos almacenados es un requisito común para el acceso a la base de datos. En algunos casos, un administrador de base de datos puede requerir que todos los accesos de base de datos atraviesan los procedimientos almacenados por motivos de seguridad. En otros casos puede que desee crear lógica de negocios en algunos de los procesos que Entity Framework utiliza cuando actualiza la base de datos. Por ejemplo, cada vez que se elimina una entidad que desee copiar en una base de datos de archivo. O bien, cada vez que se actualiza una fila desea escribir una fila en una tabla de registro que registre quién realizó el cambio. Puede realizar estos tipos de tareas en un procedimiento almacenado que se llama cada vez que elimina una entidad de Entity Framework o actualiza una entidad.

Como se muestra en el tutorial anterior, no creará las nuevas páginas. En su lugar, va a cambiar la manera en que Entity Framework tiene acceso a la base de datos para algunas de las páginas que ya haya creado.

En este tutorial creará los procedimientos almacenados en la base de datos para insertar `Student` y `Instructor` entidades. Deberá agregarlas al modelo de datos, y especificará que Entity Framework deben usarlos para agregar `Student` y `Instructor` entidades a la base de datos. También podrá crear un procedimiento almacenado que se puede utilizar para recuperar `Course` entidades.

## <a name="creating-stored-procedures-in-the-database"></a>Crear procedimientos almacenados en la base de datos

(Si usa el *School.mdf* archivo del proyecto disponible para su descarga con este tutorial, puede omitir esta sección, dado que los procedimientos almacenados ya existen.)

En **Explorador de servidores**, expanda *School.mdf*, haga clic en **procedimientos almacenados**y seleccione **Agregar nuevo procedimiento almacenado**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Copie las instrucciones SQL siguientes y péguelos en la ventana del procedimiento almacenado, reemplazando el procedimiento almacenado esqueleto.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` las entidades tienen cuatro propiedades: `PersonID`, `LastName`, `FirstName`, y `EnrollmentDate`. La base de datos genera el valor de identificador automáticamente y el procedimiento almacenado acepta parámetros para las otras tres. El procedimiento almacenado devuelve el valor de la clave de registro de la fila nueva para que Entity Framework puede realizar un seguimiento de en la versión de la entidad que mantiene en memoria.

Guarde y cierre la ventana del procedimiento almacenado.

Crear un `InsertInstructor` procedimiento almacenado de la misma manera, mediante las siguientes instrucciones SQL:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Crear `Update` procedimientos almacenados para `Student` y `Instructor` entidades también. (La base de datos ya tiene un `DeletePerson` procedimiento almacenado que funcionará para ambos `Instructor` y `Student` entidades.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

En este tutorial, asignará las tres funciones: insert, update y delete--para cada tipo de entidad. La versión 4 de Entity Framework permite asignar solo uno o dos de estas funciones a los procedimientos almacenados sin asignación de los demás, con una excepción: si asigna la función de actualización pero no la función de eliminación, Entity Framework se iniciará una excepción cuando se ha intentado eliminar una entidad. En la versión 3.5 de Entity Framework, no tenía tanta flexibilidad en la asignación de procedimientos almacenados: si se ha asignado una función se debe asignar las tres.

Para crear un procedimiento almacenado que lee en lugar de las actualizaciones de datos, cree uno que todos los selecciona `Course` entidades, mediante las instrucciones SQL siguientes:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Agregar los procedimientos almacenados para el modelo de datos

Los procedimientos almacenados se definen ahora en la base de datos, pero se deben agregar al modelo de datos para que estén disponibles a Entity Framework. Abra *SchoolModel.edmx*, haga clic en la superficie de diseño y seleccione **actualizar modelo desde base de datos**. En el **agregar** pestaña de la **elija los objetos de base de datos** cuadro de diálogo, expanda **procedimientos almacenados**, seleccione los procedimientos almacenados recién creados y `DeletePerson` procedimiento almacenado y, a continuación, haga clic en **finalizar**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Asignación de los procedimientos almacenados

En el Diseñador de modelos de datos, haga clic en el `Student` entidad y seleccione **Stored Procedure Mapping**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

El **detalles de la asignación** aparece la ventana, en el que puede especificar los procedimientos almacenados que Entity Framework deben usar para insertar, actualizar y eliminar entidades de este tipo.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Establecer el **insertar** función **InsertStudent**. La ventana muestra una lista de parámetros de procedimiento almacenado, cada uno de los cuales debe asignarse a una propiedad de entidad. Dos de estas se asignan automáticamente porque los nombres son los mismos. No hay ninguna propiedad de entidad denominada `FirstName`, por lo que debe seleccionar manualmente `FirstMidName` desde una lista desplegable que muestra las propiedades de entidad disponible. (Esto es porque ha cambiado el nombre de la `FirstName` propiedad `FirstMidName` en el primer tutorial.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

En la misma **detalles de la asignación** ventana, asigne el `Update` función a la `UpdateStudent` procedimiento almacenado (asegúrese de especificar `FirstMidName` como el valor del parámetro `FirstName`, tal y como lo hizo el `Insert` procedimiento almacenado) y la `Delete` función a la `DeletePerson` procedimiento almacenado.

[![image01](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Siga el mismo procedimiento para asignar el insert, update y delete de procedimientos almacenados para instructores para el `Instructor` entidad.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Para los procedimientos almacenados que leen en lugar de actualizar los datos, use la **Explorador de modelos** escríbalo devuelve ventana para el procedimiento almacenado se asignan a la entidad. En el Diseñador de modelos de datos, haga clic en la superficie de diseño y seleccione **Explorador de modelos**. Abra la **SchoolModel.Store** nodo y, a continuación, abra el **procedimientos almacenados** nodo. A continuación, haga clic en el `GetCourses` procedimiento almacenado y seleccione **Agregar importación de funciones**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

En el **Agregar importación de funciones** cuadro de diálogo **devuelve una colección de** seleccione **entidades**y, a continuación, seleccione `Course` como el tipo de entidad devueltos. Cuando haya terminado, haga clic en **Aceptar**. Guarde y cierre el *.edmx* archivo.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Usar Insert, Update y Delete procedimientos almacenados

Los procedimientos almacenados para insertar, actualizar y eliminar datos se usan por Entity Framework automáticamente cuando haya agregado al modelo de datos y les asigna a las entidades adecuadas. Ahora puede ejecutar la *StudentsAdd.aspx* página, y cada vez que cree un nuevo alumno, Entity Framework utilizará la `InsertStudent` procedimiento almacenado para agregar la nueva fila a la `Student` tabla.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Ejecute el *Students.aspx* página y el nuevo alumno aparece en la lista.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Cambie el nombre para comprobar que funciona la función update y, a continuación, elimine los estudiantes para comprobar que funciona la función de eliminación.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Usar procedimientos almacenados Select

Entity Framework no se ejecuta automáticamente los procedimientos almacenados como `GetCourses`, y no se pueden usar con el `EntityDataSource` control. Para poder utilizarlos, llamarlos desde el código.

Abra la *InstructorsCourses.aspx.cs* archivo. El `PopulateDropDownLists` método usa una consulta de LINQ a Entities para recuperar todas las entidades de curso para que pueda recorrer la lista y determinar cuáles se asigna un instructor a y cuáles están sin asignar:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Reemplácelo por el código siguiente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

La página ahora utiliza la `GetCourses` procedimiento almacenado para recuperar la lista de todos los cursos. Ejecute la página para comprobar que funciona igual que antes.

(Propiedades de navegación de entidades que se recuperan mediante un procedimiento almacenado no se pueden llenar automáticamente con los datos relacionados con las entidades, dependiendo de `ObjectContext` configuración predeterminada. Para obtener más información, consulte [cargar objetos relacionados](https://msdn.microsoft.com/library/bb896272.aspx) en MSDN Library.)

En el siguiente tutorial, aprenderá cómo utilizar la funcionalidad de datos dinámicos para que resulten más fáciles de programa y probar reglas de formato y validación de datos. En lugar de especificar en las reglas de cada página web como las cadenas de formato de datos y si no se requiere un campo, puede especificar estas reglas en los metadatos del modelo de datos y se aplican automáticamente en todas las páginas.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-8.md)
