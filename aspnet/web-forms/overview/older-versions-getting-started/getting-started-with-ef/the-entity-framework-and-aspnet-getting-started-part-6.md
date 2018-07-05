---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Introducción a Entity Framework 4.0 Database First y 4 de ASP.NET Web Forms, parte 6 | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. La aplicación de ejemplo es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 99861197e00a3f2f6811ef13136fac63b993ef32
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372111"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Introducción a Entity Framework 4.0 Database First y ASP.NET 4 Web Forms, parte 6
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Implementar la herencia de tabla por jerarquía

En el tutorial anterior ha trabajado con los datos relacionados mediante la adición y eliminación de relaciones y agregando una nueva entidad que tenían una relación a una entidad existente. En este tutorial se muestra cómo implementar la herencia en el modelo de datos.

En la programación orientada a objetos, puede usar la herencia para que sea más fácil trabajar con las clases relacionadas. Por ejemplo, podría crear `Instructor` y `Student` clases que derivan de un `Person` clase base. Puede crear los mismos tipos de estructuras de herencia entre las entidades de Entity Framework.

En esta parte del tutorial, no creará las nuevas páginas web. En su lugar, deberá agregar entidades derivadas para el modelo de datos y modificar las páginas existentes para usar las nuevas entidades.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabla por jerarquía en comparación con la herencia de tabla por tipo

Una base de datos puede almacenar información sobre los objetos relacionados en una tabla o en varias tablas. Por ejemplo, en el `School` base de datos, el `Person` tabla incluye información acerca de los estudiantes y profesores en una sola tabla. Algunas de las columnas se aplican solo a los instructores (`HireDate`), otras solo a los alumnos (`EnrollmentDate`) y otros con ambos (`LastName`, `FirstName`).

[![Image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

Puede configurar Entity Framework para crear `Instructor` y `Student` entidades que heredan de la `Person` entidad. Este patrón de generar una estructura de herencia de entidad de una tabla de base de datos única se denomina *tabla por jerarquía* herencia (TPH).

Para los cursos, el `School` base de datos utiliza un patrón diferente. Cursos en línea y los cursos presenciales se almacenan en tablas independientes, cada uno de los cuales tiene una clave externa que apunta a la `Course` tabla. Información común a ambos tipos de curso se almacena únicamente en el `Course` tabla.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Puede configurar el modelo de datos de Entity Framework para que `OnlineCourse` y `OnsiteCourse` entidades que heredan de la `Course` entidad. Este patrón de generar una estructura de herencia de entidad en tablas independientes para cada tipo, con cada tabla independiente haciendo referencia a una tabla que almacena datos comunes a todos los tipos, se denomina *tabla por tipo* herencia (TPT).

Patrones de herencia de TPH acostumbran un mejor rendimiento en Entity Framework que patrones de herencia de TPT, porque estos pueden provocar consultas de combinaciones complejas. Este tutorial muestra cómo implementar la herencia de TPH. Hágalo mediante los pasos siguientes:

- Crear `Instructor` y `Student` tipos de entidad que se derivan de `Person`.
- Las propiedades de movimiento que pertenecen a las entidades derivadas de la `Person` entidades a las entidades derivadas.
- Establecer restricciones sobre las propiedades de los tipos derivados.
- Realizar el `Person` entidad una entidad abstracta.
- Derivado de mapa de cada entidad a la `Person` tabla con una condición que especifica cómo se determina si un `Person` fila representa el tipo derivado.

## <a name="adding-instructor-and-student-entities"></a>Adición de un Instructor y Student entidades

Abra el <em>SchoolModel.edmx</em> de archivos, haga doble clic en el diseñador, seleccione un área sin ocupar <strong>agregar</strong>, a continuación, seleccione <strong>entidad</strong><em>.</em>

[![Image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

En el **Agregar entidad** cuadro de diálogo, el nombre de la entidad `Instructor` y establezca su **tipo Base** opción `Person`.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

Haga clic en **Aceptar**. El diseñador crea un `Instructor` entidad que se deriva el `Person` entidad. La nueva entidad aún no tiene ninguna propiedad.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

Repita el procedimiento para crear un `Student` entidad que también se deriva de `Person`.

Solo los instructores tienen fechas de contratación, por lo que deberá mover esa propiedad de la `Person` entidad a la `Instructor` entidad. En el `Person` entidad, haga clic en el `HireDate` propiedad y haga clic en **cortar**. A continuación, haga clic en **propiedades** en el `Instructor` entidad y haga clic en **pegar**.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

La fecha de contratación de un `Instructor` entidad no puede ser null. Haga clic en el `HireDate` propiedad, haga clic en **propiedades**y, a continuación, en el **propiedades** cambio de ventana `Nullable` a `False`.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

Repita el procedimiento para mover el `EnrollmentDate` propiedad desde la `Person` entidad a la `Student` entidad. Asegúrese de que también establecer `Nullable` a `False` para el `EnrollmentDate` propiedad.

Ahora que un `Person` entidad tiene solo las propiedades que son comunes a `Instructor` y `Student` entidades (aparte de las propiedades de navegación, que no se están moviendo), la entidad sólo puede usarse como una entidad base en la estructura de herencia. Por lo tanto, deberá asegurarse de que nunca se tratan como una entidad independiente. Haga clic en el `Person` entidad, seleccione **propiedades**y, a continuación, en el **propiedades** ventana cambie el valor de la **abstracta** propiedad  **True**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Asignación de Instructor y Student entidades a la tabla Person

Ahora tiene que indicar a Entity Framework para diferenciar entre `Instructor` y `Student` las entidades de la base de datos.

Haga clic en el `Instructor` entidad y seleccione **asignación de tabla**. En el **detalles de Mapping** ventana, haga clic en **agregar una tabla o vista** y seleccione **persona**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

Haga clic en **agregar una condición**y, a continuación, seleccione **HireDate**.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

Cambio **operador** a **es** y **valor / propiedad** a **no Null**.

[![Image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

Repita el procedimiento para la `Students` entidad, especificando que esta entidad se asigna a la `Person` tabla cuando la `EnrollmentDate` columna no es null. A continuación, guarde y cierre el modelo de datos.

Compile el proyecto con el fin de crear las nuevas entidades como clases y que estén disponibles en el diseñador.

## <a name="using-the-instructor-and-student-entities"></a>Utilizando el Instructor y Student entidades

Al crear las páginas web que funcionan con datos student e instructor, enlace de datos les el `Person` del conjunto de entidades y filtrados en el `HireDate` o `EnrollmentDate` propiedad para restringir los datos devueltos a los estudiantes o instructores. Sin embargo, ahora al enlazar cada control de origen de datos a la `Person` del conjunto de entidades, puede especificar que solo `Student` o `Instructor` se deben seleccionar los tipos de entidad. Dado que Entity Framework sabe cómo diferenciar los alumnos e instructores en la `Person` conjunto de entidades, puede quitar el `Where` valores de las propiedades especificadas manualmente para hacerlo.

En el Diseñador de Visual Studio, puede especificar el tipo de entidad que un `EntityDataSource` debe seleccionar el control en el **EntityTypeFilter** cuadro de lista desplegable de la `Configure Data Source` asistente, tal como se muestra en el ejemplo siguiente.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

Y, en el **propiedades** ventana puede quitar `Where` valores de la cláusula que ya no sean necesarios, tal como se muestra en el ejemplo siguiente.

[![Image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

Sin embargo, porque ha cambiado el marcado para `EntityDataSource` controles para usar el `ContextTypeName` atributo, no se puede ejecutar el **Configurar origen de datos** en `EntityDataSource` los controles que ya ha creado. Por lo tanto, deberá realizar los cambios necesarios cambiando marcado en su lugar.

Abra el *Students.aspx* página. En el `StudentsEntityDataSource` controlar, quite el `Where` atributo y agregue un `EntityTypeFilter="Student"` atributo. El marcado ahora tendrá un aspecto similar en el ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

Establecer el `EntityTypeFilter` atributo garantiza que el `EntityDataSource` control seleccionará el tipo de entidad especificado. Si desea recuperarlos `Student` y `Instructor` tipos de entidad, no debe establecer este atributo. (Tendrá la opción de recuperación de varios tipos de entidad con una `EntityDataSource` control solo si está usando el control de acceso a datos de solo lectura. Si usas un `EntityDataSource` controlar para insertar, actualizar o eliminar entidades y si el conjunto de entidades que está enlazado puede contener varios tipos, sólo se puede trabajar con un tipo de entidad y se deben establecer este atributo.)

Repita el procedimiento para la `SearchEntityDataSource` controlar, excepto quitar solo la parte de la `Where` atributo que selecciona `Student` entidades en lugar de quitar por completo la propiedad. La etiqueta de apertura del control ahora será similar en el ejemplo siguiente:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

Ejecute la página para comprobar que sigue funcionando como antes.

[![Image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

Actualizar las páginas siguientes que creó en los tutoriales anteriores para que usen el nuevo `Student` y `Instructor` entidades en lugar de `Person` entidades, a continuación, ejecutarlos para comprobar que funcionan como lo hacía antes:

- En *StudentsAdd.aspx*, agregar `EntityTypeFilter="Student"` a la `StudentsEntityDataSource` control. El marcado ahora tendrá un aspecto similar en el ejemplo siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![Image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- En *About.aspx*, agregar `EntityTypeFilter="Student"` a la `StudentStatisticsEntityDataSource` controlar y quitar `Where="it.EnrollmentDate is not null"`. El marcado ahora tendrá un aspecto similar en el ejemplo siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![Image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- En *Instructors.aspx* y *InstructorsCourses.aspx*, agregar `EntityTypeFilter="Instructor"` a la `InstructorsEntityDataSource` controlar y quitar `Where="it.HireDate is not null"`. El marcado en *Instructors.aspx* ahora es similar al siguiente: 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![Image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    El marcado en *InstructorsCourses.aspx* ahora tendrá un aspecto similar en el ejemplo siguiente:

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![Image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

Como resultado de estos cambios, ha mejorado la facilidad de mantenimiento de la aplicación Contoso University de varias maneras. Ha movido la lógica de selección y validación de fuera de la capa de interfaz de usuario (*.aspx* marcado) y lo hizo una parte integral de la capa de acceso a datos. Esto ayuda a aislar el código de aplicación de los cambios que se podrían hacer en el futuro para el esquema de base de datos o el modelo de datos. Por ejemplo, podría decidir que los alumnos, es posible que lo contraten de ayudas de los profesores y, por tanto, se obtendría una fecha de contratación. A continuación, podría agregar una nueva propiedad para diferenciar los estudiantes de instructores y actualizar el modelo de datos. Código en la aplicación web no tendría que cambiar, excepto donde desea mostrar una fecha de contratación para estudiantes. Otra ventaja de agregar `Instructor` y `Student` entidades es que el código sea más fácil lectura que cuando hace referencia a `Person` objetos que eran realmente estudiantes o instructores.

Ahora ha visto una forma de implementar un patrón de herencia en Entity Framework. En el siguiente tutorial, obtendrá información sobre cómo utilizar procedimientos almacenados con el fin de tener más control sobre cómo Entity Framework tiene acceso a la base de datos.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-7.md)
