---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
title: Introducción a Entity Framework 4.0 Database First y 4 de ASP.NET Web Forms, parte 5 | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET mediante Entity Framework. La aplicación de ejemplo es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 24ad4379-3fb2-44dc-ba59-85fe0ffcb2ae
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-5
msc.type: authoredcontent
ms.openlocfilehash: 0d0ae8385af96cf5e5951ac85f5cf6ae3c201a4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366681"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-5"></a>Introducción a Entity Framework 4.0 Database First y ASP.NET 4 Web Forms, parte 5
====================
por [Tom Dykstra](https://github.com/tdykstra)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="working-with-related-data-continued"></a>Trabajar con datos relacionados, a continuación

En el tutorial anterior se empezó a usar el `EntityDataSource` control para trabajar con datos relacionados. Muestra varios niveles de jerarquía y editar datos en las propiedades de navegación. En este tutorial aún funciona con los datos relacionados mediante la adición y eliminación de relaciones y agregando una nueva entidad que tiene una relación con una entidad existente.

Creará una página que agrega los cursos que están asignados a los departamentos de TI. Ya existen los departamentos y, cuando se crea un nuevo curso, al mismo tiempo a establecer una relación entre este y un departamento existente.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image1.png)

También creará una página que funciona con una relación varios a varios mediante la asignación de un instructor a un curso (agregar una relación entre dos entidades que seleccionó) o quitar un instructor de un curso (al quitar una relación entre dos entidades que Seleccione). En la base de datos, agregar una relación entre un instructor y un curso da como resultado una nueva fila que se agrega a la `CourseInstructor` tabla de asociación; quitar una relación implica la eliminación de una fila de la `CourseInstructor` tabla de asociación. Sin embargo, hacerlo en Entity Framework estableciendo las propiedades de navegación, sin hacer referencia a la `CourseInstructor` explícitamente la tabla.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image3.png)

## <a name="adding-an-entity-with-a-relationship-to-an-existing-entity"></a>Agregar una entidad con una relación a una entidad existente

Crear una nueva página web denominada *CoursesAdd.aspx* que usa el *Site.Master* página principal y agregue el siguiente marcado para la `Content` control denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample1.aspx)]

Este marcado crea un `EntityDataSource` control que selecciona cursos, que permite insertar, y que especifica un controlador para el `Inserting` eventos. Usará el controlador para actualizar el `Department` propiedad de navegación cuando un nuevo `Course` se crea la entidad.

El marcado crea también un `DetailsView` control que se usará para agregar un nuevo `Course` entidades. El marcado usa campos enlazados para `Course` propiedades de la entidad. Tendrá que escribir el `CourseID` valor porque no es un campo de identificador generados por el sistema. En su lugar, es un número de curso debe especificarse manualmente cuando se crea el curso.

Usar un campo de plantilla para el `Department` propiedad de navegación porque no se puede usar las propiedades de navegación con `BoundField` controles. El campo de plantilla proporciona una lista desplegable para seleccionar el departamento. La lista desplegable está enlazada a la `Departments` conjunto mediante el uso de entidades `Eval` en lugar de `Bind`, nuevo porque no se puede enlazar directamente las propiedades de navegación con el fin de actualizarlos. Especificar un controlador para el `DropDownList` del control `Init` eventos para que pueda almacenar una referencia al control para su uso por el código que actualiza el `DepartmentID` clave externa.

En *CoursesAdd.aspx.cs* justo después de la declaración de clase parcial, agregue un campo de clase para contener una referencia a la `DepartmentsDropDownList` control:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample2.cs)]

Agregar un controlador para el `DepartmentsDropDownList` del control `Init` eventos para que pueda almacenar una referencia al control. Esto le permite obtener el valor que el usuario ha escrito y usarla para actualizar el `DepartmentID` valor de la `Course` entidad.

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample3.cs)]

Agregar un controlador para el `DetailsView` del control `Inserting` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample4.cs)]

Cuando el usuario hace clic en `Insert`, el `Inserting` evento se desencadena antes de inserta el nuevo registro. Obtiene el código del controlador de la `DepartmentID` desde el `DropDownList` controlar y lo usa para establecer el valor que se usará para la `DepartmentID` propiedad de la `Course` entidad.

Entity Framework se encargará de agregar este curso para el `Courses` propiedad de navegación del asociado `Department` entidad. También agrega el departamento de TI la `Department` propiedad de navegación de la `Course` entidad.

Ejecute la página.

[![Image02](the-entity-framework-and-aspnet-getting-started-part-5/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image5.png)

Escriba un identificador, un título y un número de créditos y seleccione un departamento y luego haga clic en **insertar**.

Ejecute el *Courses.aspx* página y seleccione el mismo departamento para ver el nuevo curso.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-5/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image7.png)

## <a name="working-with-many-to-many-relationships"></a>Trabajar con relaciones de varios a varios

La relación entre el `Courses` conjunto de entidades y la `People` conjunto de entidades es una relación varios a varios. Un `Course` entidad tiene una propiedad de navegación denominada `People` que puede contener cero, uno o más relacionadas con `Person` entidades (que representan los instructores que se asigna a enseñar a ese curso). Y un `Person` entidad tiene una propiedad de navegación denominada `Courses` que puede contener cero, uno o más relacionadas con `Course` entidades (que representa los cursos de ese instructor está asignado a enseñar). Un instructor puede impartir varios cursos y un curso puede ser impartido por varios instructores. En esta sección del tutorial, deberá agregar y eliminar las relaciones entre `Person` y `Course` entidades mediante la actualización de las propiedades de navegación de las entidades relacionadas.

Crear una nueva página web denominada *InstructorsCourses.aspx* que usa el *Site.Master* página principal y agregue el siguiente marcado para la `Content` control denominado `Content2`:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample5.aspx)]

Este marcado crea un `EntityDataSource` control que recupera el nombre y `PersonID` de `Person` entidades para instructores. Un `DropDrownList` está enlazado el `EntityDataSource` control. El `DropDownList` control especifica un controlador para el `DataBound` eventos. Usará este controlador de listas desplegables de enlazar datos a los dos que se muestran los cursos.

El marcado crea también el siguiente grupo de controles que se usará para asignar un curso para el instructor seleccionado:

- Un `DropDownList` control para seleccionar un curso para asignar. Este control se rellenará con los cursos que no están asignados actualmente al instructor seleccionado.
- Un `Button` control para iniciar la asignación.
- Un `Label` control para mostrar un mensaje de error si se produce un error en la asignación.

Por último, el marcado crea también un grupo de controles que se usará para quitar un curso de instructor seleccionado.

En *InstructorsCourses.aspx.cs*, agregue una mediante declaración:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample6.cs)]

Agregue un método para rellenar las dos listas desplegables que se muestran los cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample7.cs)]

Este código obtiene todos los cursos de la `Courses` entidad establece y obtiene los cursos de la `Courses` propiedad de navegación de la `Person` entidad para el instructor seleccionado. A continuación, determina los cursos asignados a ese instructor y las listas desplegables se rellena en consecuencia.

Agregar un controlador para el `Assign` del botón `Click` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample8.cs)]

Este código obtiene el `Person` entidad para el instructor seleccionado, obtiene el `Course` entidad para el curso seleccionado y agrega el curso seleccionado a la `Courses` propiedad de navegación del instructor `Person` entidad. A continuación, guarda los cambios en la base de datos y vuelve a rellenar las listas desplegables, por lo que se pueden ver los resultados inmediatamente.

Agregar un controlador para el `Remove` del botón `Click` eventos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample9.cs)]

Este código obtiene el `Person` entidad para el instructor seleccionado, obtiene el `Course` entidad para el curso seleccionado y quita el curso seleccionado desde la `Person` la entidad `Courses` propiedad de navegación. A continuación, guarda los cambios en la base de datos y vuelve a rellenar las listas desplegables, por lo que se pueden ver los resultados inmediatamente.

Agregue código a la `Page_Load` método que se asegura de que los mensajes de error no están visibles cuando no se produce ningún error de informes, y agregar controladores para la `DataBound` y `SelectedIndexChanged` eventos de la lista desplegable de instructores para rellenar las listas de cursos:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-5/samples/sample10.cs)]

Ejecute la página.

[![Image01](the-entity-framework-and-aspnet-getting-started-part-5/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-5/_static/image9.png)

Seleccione un instructor. El <strong>asignar un curso</strong> lista desplegable muestra los cursos que no enseña el instructor, y el <strong>quitar un curso</strong> lista desplegable muestra los cursos que ya está asignado el instructor. En el <strong>asignar un curso</strong> sección, seleccione un curso y, a continuación, haga clic en <strong>asignar</strong>. El curso se mueve a la <strong>quitar un curso</strong> lista desplegable. Seleccione un curso en el <strong>quitar un curso</strong> sección y haga clic en <strong>quitar</strong><em>.</em> El curso se mueve a la <strong>asignar un curso</strong> lista desplegable.

Ahora ha visto algunas formas más para trabajar con datos relacionados. En el siguiente tutorial, obtendrá información sobre cómo usar la herencia en el modelo de datos para mejorar el mantenimiento de la aplicación.

> [!div class="step-by-step"]
> [Anterior](the-entity-framework-and-aspnet-getting-started-part-4.md)
> [Siguiente](the-entity-framework-and-aspnet-getting-started-part-6.md)
