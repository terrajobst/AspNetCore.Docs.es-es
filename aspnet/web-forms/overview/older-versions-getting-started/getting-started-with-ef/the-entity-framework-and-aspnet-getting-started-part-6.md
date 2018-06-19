---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introducción a la base de datos de Entity Framework 4.0 en primer lugar y 4 de ASP.NET Web Forms - parte 6 | Documentos de Microsoft
author: tdykstra
description: La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. Es la aplicación de ejemplo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888804"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introducción a la base de datos de Entity Framework 4.0 en primer lugar y ASP.NET 4 Web Forms - parte 6
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementar la herencia de tabla por jerarquía

En el tutorial anterior ha trabajado con los datos relacionados agregando y eliminando las relaciones y agregando una nueva entidad que tenía una relación a una entidad existente. En este tutorial se muestra cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede utilizar la herencia para que sea más fácil trabajar con clases relacionadas. Por ejemplo, podría crear `Instructor` y `Student` clases que derivan de un `Person` clase base. Puede crear los mismos tipos de estructuras de herencia entre las entidades de Entity Framework.

En esta parte del tutorial, no crea las nuevas páginas web. En su lugar, podrá agregar entidades derivadas para el modelo de datos y modificar las páginas existentes para usar las nuevas entidades.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabla por jerarquía frente a la herencia de tabla por tipo

Una base de datos puede almacenar información acerca de los objetos relacionados en una tabla o en varias tablas. Por ejemplo, en la `School` base de datos, la `Person` tabla incluye información acerca de los alumnos e instructores en una sola tabla. Algunas de las columnas solo se aplican a instructores (`HireDate`), algunas solo a los alumnos (`EnrollmentDate`) y otros con ambos (`LastName`, `FirstName`).

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Puede configurar el Entity Framework para crear `Instructor` y `Student` entidades que heredan de la `Person` entidad. Este patrón consiste en generar una estructura de herencia de la entidad de una tabla de base de datos único se denomina *tabla por jerarquía* herencia (TPH).

En cursos, los `School` base de datos utiliza un patrón diferente. Cursos en línea y cursos in situ se almacenan en tablas independientes, cada uno de los cuales tiene una clave externa que señala a la `Course` tabla. Información común a ambos tipos de curso se almacena únicamente en el `Course` tabla.

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Puede configurar el modelo de datos de Entity Framework para que `OnlineCourse` y `OnsiteCourse` heredan de la `Course` entidad. Este patrón consiste en generar una estructura de herencia de la entidad de tablas diferentes para cada tipo, con cada tabla independiente si nos referimos a una tabla que almacena los datos comunes a todos los tipos, se denomina *tabla por tipo* herencia (TPT).

Patrones de herencia TPH suele proporcionar un mejor rendimiento en Entity Framework de patrones de herencia de TPT, porque pueden dar lugar a patrones TPT en consultas de combinaciones complejas. Este tutorial muestra cómo implementar la herencia TPH. Llevará a cabo mediante la realización de los pasos siguientes:

- Crear `Instructor` y `Student` tipos de entidad que se derivan de `Person`.
- Propiedades de movimiento que pertenecen a las entidades derivadas de la `Person` entidades a las entidades derivadas.
- Establecer restricciones en las propiedades de los tipos derivados.
- Realizar el `Person` entidad una entidad abstracta.
- Derivado de mapa de cada entidad a la `Person` tabla con una condición que especifica cómo determinar si un `Person` fila representa el tipo derivado.

## <a name="adding-instructor-and-student-entities"></a>Agregar Instructor y entidades de estudiante

Abra la <em>SchoolModel.edmx</em> de archivos, haga clic en un área sin ocupar en el diseñador, seleccione <strong>agregar</strong>, a continuación, seleccione <strong>entidad</strong><em>.</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

En el **Agregar entidad** cuadro de diálogo, el nombre de la entidad `Instructor` y establecer su **tipo Base** opción `Person`.

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Haga clic en **Aceptar**. El diseñador crea un `Instructor` entidad que se deriva de la `Person` entidad. La nueva entidad no tiene todavía ninguna propiedad.

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Repita el procedimiento para crear un `Student` entidad que también se deriva de `Person`.

Solo instructores tienen fechas de contratación, por lo que necesita mover esa propiedad desde el `Person` entidad a la `Instructor` entidad. En el `Person` entidad, haga clic en el `HireDate` propiedad y haga clic en **cortar**. A continuación, haga clic en **propiedades** en el `Instructor` entidad y haga clic en **pegar**.

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La fecha de contratación de un `Instructor` entidad no puede ser nula. Haga clic en el `HireDate` propiedad, haga clic en **propiedades**y, a continuación, en la **propiedades** cambio de ventana `Nullable` a `False`.

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Repita el procedimiento para mover el `EnrollmentDate` propiedad desde el `Person` entidad a la `Student` entidad. Asegúrese de que también se establezca `Nullable` a `False` para el `EnrollmentDate` propiedad.

Ahora que un `Person` entidad tiene solo las propiedades que son comunes a `Instructor` y `Student` entidades (además de propiedades de navegación, que no se mueva), la entidad sólo puede utilizarse como una entidad base en la estructura de herencia. Por lo tanto, debe asegurarse de que nunca se tratan como una entidad independiente. Haga clic en el `Person` entidad, seleccione **propiedades**y, a continuación, en la **propiedades** ventana cambie el valor de la **abstracta** propiedad  **True**.

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Asignación Instructor y entidades de estudiante en la tabla Person

Ahora es necesario indicar a Entity Framework cómo diferenciar `Instructor` y `Student` las entidades de la base de datos.

Haga clic en el `Instructor` entidad y seleccione **asignaciones de tablas**. En el **detalles de la asignación** ventana, haga clic en **agregar una tabla o vista** y seleccione **persona**.

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Haga clic en **agregar una condición**y, a continuación, seleccione **HireDate**.

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Cambio **operador** a **es** y **valor / propiedad** a **Not Null**.

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Repita el procedimiento para la `Students` entidad, especificando que esta entidad se asigna a la `Person` tabla cuando la `EnrollmentDate` columna no es null. A continuación, guarde y cierre el modelo de datos.

Compile el proyecto con el fin de crear las nuevas entidades como clases y que estén disponibles en el diseñador.

## <a name="using-the-instructor-and-student-entities"></a>Utilizando el Instructor y entidades de estudiante

Al crear las páginas web que funcionan con datos student y instructor, enlace de datos se puedan el `Person` del conjunto de entidades y filtrados en el `HireDate` o `EnrollmentDate` propiedad para restringir los datos devueltos a los alumnos o instructores. Sin embargo, ahora al enlazar cada control de origen de datos para la `Person` del conjunto de entidades, puede especificar que solo `Student` o `Instructor` deben seleccionarse los tipos de entidad. Dado que Entity Framework sabe cómo diferenciar los alumnos e instructores en el `Person` conjunto de entidades, puede quitar el `Where` valores de las propiedades especificadas manualmente para hacerlo.

En el Diseñador de Visual Studio, puede especificar el tipo de la entidad que un `EntityDataSource` control debe seleccionar en la **EntityTypeFilter** cuadro de lista desplegable de la `Configure Data Source` asistente, tal como se muestra en el ejemplo siguiente.

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Y en el **propiedades** ventana puede quitar `Where` valores de la cláusula que ya no son necesarios, tal como se muestra en el ejemplo siguiente.

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Sin embargo, porque ha cambiado el marcado de `EntityDataSource` controles para utilizar el `ContextTypeName` atributo, no se puede ejecutar el **Configurar origen de datos** asistente en `EntityDataSource` controles que ya ha creado. Por lo tanto, realizará los cambios necesarios, cambie el marcado en su lugar.

Abra la *Students.aspx* página. En el `StudentsEntityDataSource` controlar, quite el `Where` de atributo y agregar un `EntityTypeFilter="Student"` atributo. El marcado ahora parecerán al ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Establecer el `EntityTypeFilter` atributo garantiza que el `EntityDataSource` control seleccionará el tipo de entidad especificado. Si desea recuperarlos `Student` y `Instructor` tipos de entidad, no debe establecer este atributo. (Tiene la opción de recuperación de varios tipos de entidad con una `EntityDataSource` el control solo si está usando el control de acceso a datos de solo lectura. Si usas un `EntityDataSource` controlar para insertar, actualizar o eliminar entidades y si el conjunto de entidades que está enlazado puede contener varios tipos, sólo se puede trabajar con un tipo de entidad y se deben establecer este atributo.)

Repita el procedimiento para la `SearchEntityDataSource` controlar, excepto extraer solo la parte de la `Where` atributo que selecciona `Student` entidades en lugar de quitar por completo la propiedad. La etiqueta de apertura del control ahora parecerán al ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Ejecute la página para comprobar que sigue funcionando como lo hacía anteriormente.

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Actualizar las siguientes páginas que creó en tutoriales anteriores para que usen la nueva `Student` y `Instructor` entidades en lugar de `Person` entidades, a continuación, ejecutarlos para comprobar que funcionan como lo hacían antes:

- En *StudentsAdd.aspx*, agregar `EntityTypeFilter="Student"` a la `StudentsEntityDataSource` control. El marcado ahora parecerán al ejemplo siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- En *About.aspx*, agregar `EntityTypeFilter="Student"` a la `StudentStatisticsEntityDataSource` controlar y quitar `Where="it.EnrollmentDate is not null"`. El marcado ahora parecerán al ejemplo siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- En *Instructors.aspx* y *InstructorsCourses.aspx*, agregar `EntityTypeFilter="Instructor"` a la `InstructorsEntityDataSource` controlar y quitar `Where="it.HireDate is not null"`. El marcado en *Instructors.aspx* ahora es similar al siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    El marcado en *InstructorsCourses.aspx* ahora tendrá un aspecto similar en el ejemplo siguiente:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Como resultado de estos cambios, ha mejorado la facilidad de mantenimiento de la aplicación de la Universidad de Contoso de varias maneras. Se ha movido lógica de selección y validación de fuera de la capa de interfaz de usuario (*.aspx* marcado) y realiza una parte integral de la capa de acceso a datos. Esto ayuda a aislar el código de aplicación de los cambios que se podrían hacer en el futuro en el esquema de base de datos o el modelo de datos. Por ejemplo, podría decidir que los alumnos podrían contratados como ayudas de profesores y, por tanto, se obtendría una fecha de contratación. A continuación, puede agregar una nueva propiedad para diferenciar los estudiantes de instructores y actualizar el modelo de datos. Ningún código de la aplicación web deberá cambiar excepto donde desea mostrar una fecha de contratación para estudiantes. Otra ventaja de poder añadir `Instructor` y `Student` entidades es que el código sea más fácil comprender que cuando hace referencia a `Person` objetos que eran realmente estudiantes o instructores.

Ahora que ha visto una manera de implementar un patrón de herencia en Entity Framework. En el siguiente tutorial, aprenderá cómo utilizar procedimientos almacenados con el fin de tener más control sobre cómo Entity Framework tiene acceso a la base de datos.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-7.md)
