---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
title: Introducción a Entity Framework 4.0 Database First y 4 de ASP.NET Web Forms, parte 4 | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. La aplicación de ejemplo es...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: ceb9e60f-957c-4d25-9331-cc527de96a33
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-4
msc.type: authoredcontent
ms.openlocfilehash: f4a8e7f1cafd169f392485ff4ecb260073209210
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827974"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-4"></a>Introducción a Entity Framework 4.0 Database First y ASP.NET 4 Web Forms, parte 4
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data"></a>Trabajar con datos relacionados

En el tutorial anterior ha utilizado el `EntityDataSource` control para filtrar, ordenar y agrupar los datos. En este tutorial podrá mostrar y actualizar datos relacionados.

Se creará la página de instructores que se muestra una lista de instructores. Cuando se selecciona un instructor, verá una lista de cursos impartidos por ese instructor. Cuando se selecciona un curso, vea los detalles para el curso y una lista de alumnos inscritos en el curso. Puede editar el nombre de un instructor, fecha de contratación y asignación de oficina. La asignación de oficina es un conjunto de entidades independientes que se accede a través de una propiedad de navegación.

Puede vincular los datos maestros para los datos detallados en el marcado o en el código. En esta parte del tutorial, usará ambos métodos.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-4/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image1.png)

## <a name="displaying-and-updating-related-entities-in-a-gridview-control"></a>Mostrar y actualizar las entidades relacionadas en un Control GridView

Crear una nueva página web denominada *Instructors.aspx* que usa el *Site.Master* página principal y agregue el siguiente marcado para la `Content` control denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample1.aspx)]

Este marcado crea un `EntityDataSource` control que selecciona los instructores y habilita las actualizaciones. El `div` elemento configura marcado para representar a la izquierda, por lo que puede agregar una columna posterior a la derecha.

Entre los `EntityDataSource` marcado y el cierre `</div>` etiquetar, agregue el siguiente marcado que crea un `GridView` control y un `Label` control que va a usar para los mensajes de error:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample2.aspx)]

Esto `GridView` control habilita la selección de fila, se resalta la fila seleccionada con un color de fondo gris claro y especifica controladores (que creará más adelante) para el `SelectedIndexChanged` y `Updating` eventos. También especifica `PersonID` para el `DataKeyNames` propiedad, por lo que el valor de clave de la fila seleccionada se puede pasar a otro control que se agregará más adelante.

La última columna contiene la asignación de oficina del instructor, que se almacena en una propiedad de navegación de la `Person` entidad porque procede de una entidad asociada. Tenga en cuenta que el `EditItemTemplate` elemento especifica `Eval` en lugar de `Bind`, porque el `GridView` control no se puede enlazar directamente a las propiedades de navegación con el fin de actualizarlos. Actualizará la asignación de oficina en el código. Para ello, necesitará una referencia a la `TextBox` control, deberá obtener y guardar en el `TextBox` del control `Init` eventos.

Siguiendo el `GridView` control es un `Label` control que se usa para los mensajes de error. El control `Visible` propiedad es `false`, y el estado de vista está desactivado, para que la etiqueta aparezca solo cuando código lo hace visible en respuesta a un error.

Abra el *Instructors.aspx.cs* de archivo y agregue la siguiente `using` instrucción:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample3.cs)]

Agregue un campo de clase privada inmediatamente después de la declaración de nombre de clase parcial para mantener una referencia en el cuadro de texto de la asignación de office.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample4.cs)]

Agregar un código auxiliar para el `SelectedIndexChanged` controlador de eventos que se agregará código para su uso posterior. Agregue también un controlador para la asignación de oficina `TextBox` del control `Init` eventos para que pueda almacenar una referencia a la `TextBox` control. Usará esta referencia para obtener el valor especificado para actualizar la entidad asociada a la propiedad de navegación por el usuario.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample5.cs)]

Deberá usar el `GridView` del control `Updating` eventos para actualizar el `Location` propiedad del asociado `OfficeAssignment` entidad. Agregue el siguiente controlador para el `Updating` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample6.cs)]

Este código se ejecuta cuando el usuario hace clic en **actualización** en un `GridView` fila. El código usa LINQ to Entities para recuperar el `OfficeAssignment` entidad que está asociado con la actual `Person` entidad, mediante el `PersonID` de la fila seleccionada en el argumento de evento.

El código, a continuación, toma una de las siguientes acciones según el valor de la `InstructorOfficeTextBox` control:

- Si el cuadro de texto tiene un valor y no hay ningún `OfficeAssignment` entidad que se va a actualizar, crea uno.
- Si el cuadro de texto tiene un valor y hay un `OfficeAssignment` entidad, actualiza el `Location` valor de propiedad.
- Si el cuadro de texto está vacío y un `OfficeAssignment` entidad existe, se elimina la entidad.

Una vez hecho esto, guarda los cambios en la base de datos. Si se produce una excepción, muestra un mensaje de error.

Ejecute la página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-4/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image3.png)

Haga clic en **editar** y cambian todos los campos en los cuadros de texto.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-4/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image5.png)

Cambiar cualquiera de estos valores, incluidos **asignación de oficina**. Haga clic en **actualización** y verá los cambios reflejados en la lista.

## <a name="displaying-related-entities-in-a-separate-control"></a>Mostrar las entidades relacionadas en un Control independiente

Cada instructor puede impartir cursos de uno o más, así que agregaremos un `EntityDataSource` control y un `GridView` control para enumerar los cursos asociados con el instructor seleccionado en los instructores `GridView` control. Para crear un encabezado y el `EntityDataSource` control para las entidades de cursos, agregue el marcado siguiente entre el mensaje de error `Label` control y el cierre `</div>` etiqueta:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample7.aspx)]

El `Where` parámetro contiene el valor de la `PersonID` del instructor cuya fila está seleccionado en el `InstructorsGridView` control. El `Where` propiedad contiene un comando de Subselección que obtiene todos los elementos `Person` entidades desde un `Course` la entidad `People` propiedad de navegación y selecciona el `Course` solo si de entidad uno de los asociados `Person`entidades contiene seleccionado `PersonID` valor.

Para crear el `GridView` control. Agregue el marcado siguiente inmediatamente después de la `CoursesEntityDataSource` control (antes del cierre `</div>` etiqueta):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample8.aspx)]

Dado que no hay cursos se mostrará si no se selecciona ningún instructor, un `EmptyDataTemplate` elemento está incluido.

Ejecute la página.

[![Image04](the-entity-framework-and-aspnet-getting-started-part-4/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image7.png)

Seleccione un instructor quién tiene uno o varios de los cursos asignados y el curso o los cursos que aparecen en la lista. (Nota: aunque el esquema de base de datos permite varios cursos, en los datos de prueba proporcionados con la base de datos un instructor realmente no tiene más de un curso. Puede agregar cursos a la base de datos usted mismo mediante la **Explorador de servidores** ventana o el *CoursesAdd.aspx* página, que agregará en un tutorial posterior.)

[![Image05](the-entity-framework-and-aspnet-getting-started-part-4/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image9.png)

El `CoursesGridView` control muestra solo unos cuantos campos de curso. Para mostrar todos los detalles para un curso, usará un `DetailsView` control para el curso que selecciona el usuario. En *Instructors.aspx*, agregue el siguiente marcado después del cierre `</div>` etiqueta (asegúrese de colocar este marcado **después** etiqueta div de cierre, antes de que no):

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample9.aspx)]

Este marcado crea un `EntityDataSource` control que está enlazado a la `Courses` conjunto de entidades. El `Where` propiedad selecciona un curso utilizando el `CourseID` valor de la fila seleccionada en los cursos `GridView` control. El marcado especifica un controlador para el `Selected` evento, que usará más adelante para mostrar las notas del alumno, que es otro nivel inferior en la jerarquía.

En *Instructors.aspx.cs*, crear el código auxiliar siguiente para el `CourseDetailsEntityDataSource_Selected` método. (Deberá rellenar este código auxiliar más adelante en el tutorial; por ahora, se necesitará para que se compilará y ejecutará la página).

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample10.cs)]

Ejecute la página.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-4/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image11.png)

Inicialmente, hay no hay detalles del curso porque no se ha seleccionado ningún curso. Seleccione un instructor que tenga un curso asignado y, a continuación, seleccione un curso para ver los detalles.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-4/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image13.png)

## <a name="using-the-entitydatasource-selected-event-to-display-related-data"></a>Uso de EntityDataSource "Selected" eventos para mostrar datos relacionados

Por último, desea mostrar todos los estudiantes inscritos y sus calificaciones para el curso seleccionado. Para ello, deberá usar el `Selected` eventos de la `EntityDataSource` control enlazado al curso `DetailsView`.

En *Instructors.aspx*, agregue el siguiente marcado después el `DetailsView` control:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample11.aspx)]

Este marcado crea un `ListView` control que muestra una lista de los alumnos y sus calificaciones para el curso seleccionado. Se especifica ningún origen de datos porque tendrá databind del control en el código. El `EmptyDataTemplate` elemento proporciona un mensaje que se muestra cuando no se selecciona ningún curso, en ese caso, no hay ningún estudiante para mostrar. El `LayoutTemplate` elemento crea una tabla HTML para mostrar la lista y el `ItemTemplate` especifica las columnas para mostrar. Son el identificador de estudiante y el grado de estudiante de la `StudentGrade` proviene de entidad y el nombre del alumno el `Person` entidad que Entity Framework hace que estén disponible en el `Person` propiedad de navegación de la `StudentGrade` entidad.

En *Instructors.aspx.cs*, reemplace el código auxiliar fuera `CourseDetailsEntityDataSource_Selected` método con el código siguiente:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample12.cs)]

El argumento de evento para este evento proporciona los datos seleccionados en el formulario de una colección, que tendrá cero elementos si se selecciona nada o un elemento si un `Course` está seleccionada la entidad. Si un `Course` entidad está activada, el código usa el `First` método para convertir la colección en un único objeto. A continuación, obtiene `StudentGrade` convierte en una colección de entidades de la propiedad de navegación y enlaza la `GradesListView` control a la colección.

Esto es suficiente mostrar grados, pero desea asegurarse de que el mensaje en la plantilla de datos vacía se muestra la primera vez que se muestra la página y siempre que no se selecciona un curso. Para ello, cree el método siguiente, que llamará desde dos lugares:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample13.cs)]

Llamar a este nuevo método desde el `Page_Load` método para mostrar la hora de la primera plantilla de datos vacía se muestra la página. Y llámelo desde el `InstructorsGridView_SelectedIndexChanged` método porque ese evento se genera cuando se selecciona un instructor, lo que significa que los nuevos cursos se cargan en los cursos `GridView` control y ninguno está seleccionada todavía. Estas son las dos llamadas:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample14.cs)]

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-4/samples/sample15.cs)]

Ejecute la página.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-4/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image15.png)

Seleccione un instructor que tiene un curso asignado y, a continuación, seleccione el curso.

[![Image09](the-entity-framework-and-aspnet-getting-started-part-4/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-4/_static/image17.png)

Ahora ha visto algunos ejemplos de cómo trabajar con datos relacionados. En el siguiente tutorial, obtendrá información sobre cómo agregar las relaciones entre las entidades existentes, cómo eliminar relaciones y cómo agregar una nueva entidad que tiene una relación con una entidad existente.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-3.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-5.md)
