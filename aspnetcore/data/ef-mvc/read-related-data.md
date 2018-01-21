---
title: "Núcleo de ASP.NET MVC con EF básica: leer datos relacionados - 6 de 10"
author: tdykstra
description: "En este tutorial podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework se carga en las propiedades de navegación."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 1321cb00a432669b4a97ad20063b6cf9ea75f24c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a>Lectura relacionadas con datos - Core EF con el tutorial de MVC de ASP.NET Core (6 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En el tutorial anterior, ha completado el modelo de datos School. En este tutorial, podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework se carga en las propiedades de navegación.

Las ilustraciones siguientes muestran las páginas que trabajará con.

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Carga diligente, explícita y diferida de los datos relacionados

Existen varias formas de que el software de asignación relacional de objetos (ORM) como Entity Framework puede cargar datos relacionados en las propiedades de navegación de una entidad:

* Carga diligente. Cuando se lee la entidad, se recuperan los datos relacionados con él. Esto normalmente como resultado de una consulta de combinación única que recupera todos los datos que se necesitan. Especificar carga diligente de Entity Framework Core mediante la `Include` y `ThenInclude` métodos.

  ![En el ejemplo se carga diligente](read-related-data/_static/eager-loading.png)

  Puede recuperar algunos de los datos en distintas consultas y EF "corrige" las propiedades de navegación.  Es decir, EF agrega automáticamente las entidades recuperadas por separado que pertenecen de propiedades de navegación de entidades recuperadas previamente. En la consulta que recupera los datos relacionados, puede usar el `Load` método en lugar de un método que devuelva una lista o un objeto, como `ToList` o `Single`.

  ![Ejemplo de separar las consultas](read-related-data/_static/separate-queries.png)

* Carga explícita. Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados. Escribir código que recupera los datos relacionados si es necesario. Como en el caso de carga diligente con consultas independientes, explícitas de carga de resultados de varias consultas envían a la base de datos. La diferencia es que con carga explícita, el código especifica las propiedades de navegación que va a cargarse. En Entity Framework Core 1.1 se puede utilizar el `Load` método para realizar la carga explícita. Por ejemplo:

  ![En el ejemplo se carga explícita](read-related-data/_static/explicit-loading.png)

* Carga diferida. Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados. Sin embargo, la primera vez que intente acceder a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Se envía una consulta a la base de datos cada vez que intente obtener datos de una propiedad de navegación por primera vez. Entity Framework Core 1.0 no admite la carga diferida.

### <a name="performance-considerations"></a>Consideraciones sobre el rendimiento

Si sabe que necesita datos relacionados para cada entidad recuperar, a menudo carga diligente ofrece el mejor rendimiento, dado que una sola consulta que se envía a la base de datos es normalmente más eficaz que las consultas independientes para cada entidad recuperados. Por ejemplo, suponga que cada departamento tiene diez cursos relacionados. Carga diligente de todos los datos relacionados se crearán simplemente una consulta sencilla (combinación) y un único viaje de ida y vuelta a la base de datos. Una consulta independiente para cursos para cada departamento, se crearán en once viajes de ida y la base de datos. La ida y vuelta adicional a la base de datos son especialmente afecta negativamente al rendimiento cuando la latencia es alta.

Por otro lado, en algunos escenarios de separar las consultas es más eficaz. Carga diligente de todos los datos relacionados en una consulta puede dar lugar a una combinación muy compleja para generarse, que SQL Server no puede procesar eficazmente. O bien, si necesita tener acceso a propiedades de navegación de una entidad solo para un subconjunto de un conjunto de las entidades que se procesan, separar las consultas dará mejores resultados porque carga diligente de todo el contenido por adelantado recuperaría más datos de los que necesita. Si el rendimiento es crítico, es mejor probar el rendimiento de ambas formas para elegir la mejor opción.

## <a name="create-a-courses-page-that-displays-department-name"></a>Crear una página de cursos que muestra el nombre de departamento

La entidad de curso incluye una propiedad de navegación que contiene la entidad de departamento del departamento que el curso se asigna a. Para mostrar el nombre del departamento asignado en una lista de cursos, tendrá que obtener la propiedad de nombre de la entidad de departamento que se encuentra en la `Course.Department` propiedad de navegación.

Crear un controlador denominado CoursesController para el tipo de entidad de curso, utilizando las mismas opciones para la **controlador de MVC con vistas que usan Entity Framework** scaffolder que lo hizo anteriormente para el controlador de estudiantes, como se muestra en el la siguiente ilustración:

![Agregar controlador de cursos](read-related-data/_static/add-courses-controller.png)

Abra *CoursesController.cs* y examine el `Index` método. La técnica scaffolding automática se especificado carga diligente de la `Department` propiedad de navegación mediante el uso de la `Include` método.

Reemplace el `Index` método con el siguiente código que utiliza un nombre más adecuado para la `IQueryable` que devuelve las entidades de curso (`courses` en lugar de `schoolContext`):

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

Abra *Views/Courses/Index.cshtml* y reemplace el código de plantilla con el código siguiente. Se resaltan los cambios:

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Ha realizado los siguientes cambios en el código con scaffolding:

* Cambia el encabezado de índice a Courses.

* Agrega un **número** columna que muestra la `CourseID` valor de propiedad. De forma predeterminada, las claves principales no son scaffolding porque normalmente son importantes para los usuarios finales. Sin embargo, en este caso, la clave principal es significativa y desea mostrarla.

* Cambiar el **departamento** columna para mostrar el nombre del departamento. El código muestra la `Name` propiedad de la entidad de departamento que se carga en el `Department` propiedad de navegación:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Ejecute la aplicación y seleccione la **cursos** pestaña para ver la lista con los nombres de departamento.

![Página de índice de cursos](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Crear una página de instructores que muestra los cursos y las inscripciones

En esta sección, creará un controlador y la vista de la entidad Instructor con el fin de mostrar la página de instructores:

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

Esta página se lee y muestra los datos relacionados de las maneras siguientes:

* La lista de instructores muestra datos relacionados de la entidad OfficeAssignment. Las entidades Instructor y OfficeAssignment se encuentran en una relación de uno a cero o uno. Carga diligente usará para las entidades OfficeAssignment. Como se explicó anteriormente, carga diligente normalmente es más eficaz cuando necesite los datos relacionados para todas las filas recuperadas de la tabla principal. En este caso, desea mostrar las asignaciones de office para todos los instructores mostradas.

* Cuando el usuario selecciona un instructor, se muestran las entidades relacionadas de curso. Las entidades Instructor y el curso se encuentran en una relación de varios a varios. Carga diligente usará para las entidades de curso y sus entidades relacionadas de departamento. En este caso, separar las consultas pueden ser más eficaces porque necesita cursos solo para el instructor seleccionado. Sin embargo, en este ejemplo se muestra cómo utilizar carga diligente para las propiedades de navegación dentro de las entidades que se encuentran a sí mismos en las propiedades de navegación.

* Cuando el usuario selecciona un curso, se muestran datos relacionados desde el conjunto de entidades de inscripciones. Las entidades de curso e inscripción están en una relación uno a varios. Va a utilizar consultas independientes para las entidades de inscripción y sus entidades relacionadas de estudiante.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Crear un modelo de vista para la vista de índice Instructor

La página de instructores muestra datos de tres tablas diferentes. Por lo tanto, creará un modelo de vista que incluye tres propiedades, que contiene los datos de una de las tablas.

En el *SchoolViewModels* carpeta, crear *InstructorIndexData.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Crear el controlador de Instructor y vistas

Cree un controlador de instructores con acciones de lectura/escritura EF tal como se muestra en la siguiente ilustración:

![Agregar controlador instructores](read-related-data/_static/add-instructors-controller.png)

Abra *InstructorsController.cs* y agregue un mediante declaración para el espacio de nombres ViewModels:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

Reemplace el método de índice con el código siguiente para realizar una carga diligente de los datos relacionados y colocarla en el modelo de vista.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

El método acepta datos de ruta opcional (`id`) y un parámetro de cadena de consulta (`courseID`) que proporcionan los valores de Id. del instructor seleccionado y el curso seleccionado. Los parámetros se proporcionan con el **seleccione** hipervínculos en la página.

El código comienza creando una instancia del modelo de vista y colocar en ella la lista de instructores. El código especifica una carga diligente de la `Instructor.OfficeAssignment` y `Instructor.CourseAssignments` propiedades de navegación. Dentro de la `CourseAssignments` propiedad, el `Course` propiedad esté cargado y dentro de ese, la `Enrollments` y `Department` propiedades están cargados y dentro de cada `Enrollment` entidad la `Student` propiedad está cargada.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

Debido a que la vista siempre requiere la entidad OfficeAssignment, resulta más eficaz capturar en la misma consulta. Entidades de curso son necesarias cuando se selecciona un instructor en la página web, por lo que es mejor que varias consultas una sola consulta solo si la página se muestra con más frecuencia con un curso seleccionado que sin.

El código se repite `CourseAssignments` y `Course` porque tiene dos propiedades de `Course`. La primera cadena de `ThenInclude` llama Obtiene `CourseAssignment.Course`, `Course.Enrollments`, y `Enrollment.Student`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

En ese momento en el código, otra `ThenInclude` sería para las propiedades de navegación de `Student`, que no es necesario. Pero la llamada a `Include` inicia sobre con `Instructor` propiedades, por lo que tiene que pasar a través de la cadena de nuevo, esta vez especificando `Course.Department` en lugar de `Course.Enrollments`.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

El siguiente código se ejecuta cuando se ha seleccionado un instructor. El instructor seleccionado se recupera de la lista de instructores en el modelo de vista. El modelo de vista `Courses` propiedad, a continuación, se carga con las entidades de curso de esa instructor `CourseAssignments` propiedad de navegación.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

El `Where` método devuelve una colección, pero en este caso los criterios se pasan a ese método generan solo en una única entidad Instructor devolverse. El `Single` método convierte la colección en una única entidad Instructor, que proporciona acceso a esa entidad `CourseAssignments` propiedad. El `CourseAssignments` propiedad contiene `CourseAssignment` entidades, que se desea solo relacionado `Course` entidades.

Usa el `Single` método en una colección cuando se sabe que la colección tiene un único elemento. El único método produce una excepción si la colección pasada a la está vacía o si hay más de un elemento. Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (null, en este caso) si la colección está vacía. Sin embargo, en este caso que todavía produciría una excepción (de tratar de buscar un `Courses` propiedad en una referencia nula), y el mensaje de excepción menos claramente indicaría la causa del problema. Cuando se llama a la `Single` método, también puede pasar la donde condición en lugar de llamar el `Where` método por separado:

```csharp
.Single(i => i.ID == id.Value)
```

En lugar de:

```csharp
.Where(I => i.ID == id.Value).Single()
```

A continuación, si se ha seleccionado un curso, el curso seleccionado se recupera de la lista de cursos en el modelo de vista. A continuación, el modelo de vista `Enrollments` propiedad se carga con las entidades de inscripción de dicho curso `Enrollments` propiedad de navegación.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a>Modificar la vista de índice Instructor

En *Views/Instructors/Index.cshtml*, reemplace el código de plantilla con el código siguiente. Los cambios aparecen resaltados.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

Ha realizado los siguientes cambios en el código existente:

* Cambiar la clase de modelo para `InstructorIndexData`.

* Cambiar el título de la página de **índice** a **instructores**.

* Agrega un **Office** columna que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es null. (Dado que se trata de una relación de uno a cero o uno, no habrá una entidad OfficeAssignment relacionada.)

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* Agrega un **cursos** columna que muestra los cursos se imparte por cada instructor. Vea [transición línea explícita con `@:` ](xref:mvc/views/razor#explicit-line-transition-with-) para obtener más información acerca de esta sintaxis razor.

* Ha agregado código que agrega dinámicamente `class="success"` a la `tr` elemento del instructor seleccionado. Esto establece el color de fondo de la fila seleccionada mediante una clase de arranque.

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Agrega un nuevo hipervínculo con la etiqueta **seleccione** inmediatamente antes de los otros vínculos en cada fila, lo que hace que Id. del instructor seleccionado para enviarse a la `Index` método.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Ejecute la aplicación y seleccione la **instructores** ficha. La página muestra la propiedad de ubicación de las entidades relacionadas de OfficeAssignment y una celda de tabla vacía cuando no hay ninguna entidad OfficeAssignment relacionada.

![Página de índice de instructores que no hay nada seleccionado](read-related-data/_static/instructors-index-no-selection.png)

En el *Views/Instructors/Index.cshtml* archivo, después de que el cierre de la tabla elemento (situado al final del archivo), agregue el código siguiente. Este código muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

Este código lee el `Courses` propiedad del modelo de vista para mostrar una lista de cursos. También proporciona un **seleccione** hipervínculo que envía el Id. del curso seleccionado para la `Index` método de acción.

Actualice la página y seleccione un instructor. Ahora verá una cuadrícula que muestra los cursos asignados al instructor seleccionado, y para cada curso verá el nombre del departamento asignado.

![Instructor de página de índice de instructores seleccionado](read-related-data/_static/instructors-index-instructor-selected.png)

Después del bloque de código que acaba de agregar, agregue el código siguiente. Esto muestra una lista de los estudiantes que están inscritos en un curso cuando se selecciona dicho curso.

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

Este código lee la propiedad de las inscripciones del modelo de vista para mostrar una lista de estudiantes inscritos en el curso.

Actualice la página de nuevo y seleccione un instructor. A continuación, seleccione un curso para ver la lista de estudiantes inscritos y sus calificaciones.

![Instructor de página de índice de instructores y curso seleccionado](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a>Carga explícita

Cuando se recupera la lista de instructores en *InstructorsController.cs*, especifica una carga diligente de la `CourseAssignments` propiedad de navegación.

Suponga que esperaba a los usuarios apenas desea ver las inscripciones en un instructor seleccionado y en curso. En ese caso, puede cargar los datos de inscripción sólo si se solicita. Para ver un ejemplo de cómo realizar la carga explícita, reemplace la `Index` método con el código siguiente, que quita carga diligente de inscripciones y los carga explícitamente esa propiedad. Se resaltan los cambios de código.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

El nuevo código quita la *ThenInclude* método se llama para los datos de inscripción desde el código que recupera entidades instructor. Si se seleccionan un instructor y el curso, el código resaltado recupera entidades de inscripción para el curso seleccionado y entidades de estudiante para cada inscripción.

Ejecute que la aplicación, vaya a la página de índice de instructores ahora y no se verá ninguna diferencia en lo que se muestra en la página, aunque ha cambiado la forma en que se recuperan los datos.

## <a name="summary"></a>Resumen

Ahora que ha utilizado carga diligente con una consulta y con varias consultas para leer los datos relacionados en las propiedades de navegación. En el siguiente tutorial, aprenderá cómo actualizar datos relacionados.

>[!div class="step-by-step"]
>[Anterior](complex-data-model.md)
>[Siguiente](update-related-data.md)  
