---
title: "Páginas de Razor con EF básica: leer datos relacionados - 6 de 8"
author: rick-anderson
description: "En este tutorial leer y mostrar datos relacionados, es decir, los datos que Entity Framework se carga en las propiedades de navegación."
keywords: "Combinaciones de núcleo de ASP.NET, Entity Framework Core, datos relacionados,"
ms.author: riande
manager: wpickett
ms.date: 11/05/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ba9b17ecdcb605d39117d03230b1db37e8e4d0dd
ms.sourcegitcommit: 05e798c9bac7b9e9983599afb227ef393905d023
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2017
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a>Lectura relacionadas con datos - Core EF con páginas de Razor (6 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

En este tutorial, los datos relacionados lee y muestra. Datos relacionados son datos que EF Core carga en las propiedades de navegación.

Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).

Las ilustraciones siguientes muestran las páginas completadas para este tutorial:

![Página de índice de cursos](read-related-data/_static/courses-index.png)

![Página de índice de instructores](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a>Carga diligente, explícita y diferida de los datos relacionados

Hay varias maneras que EF Core puede cargar datos relacionados en las propiedades de navegación de una entidad:

* [Carga diligente](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading). Carga diligente es cuando una consulta para un tipo de entidad también carga las entidades relacionadas. Cuando se lee la entidad, se recuperan los datos relacionados. Esto normalmente como resultado de una consulta de combinación única que recupera todos los datos que se necesitan. Núcleo EF emitirá varias consultas para algunos tipos de carga diligente. Emitir varias consultas puede ser más eficaz que era el caso de algunas consultas de EF6 donde hubiera una sola consulta. Carga diligente se especifica con el `Include` y `ThenInclude` métodos.

 ![En el ejemplo se carga diligente](read-related-data/_static/eager-loading.png)
 
 Carga diligente envía varias consultas cuando se incluye una colección nvavigation:

 * Una consulta para la consulta principal 
 * Una consulta para cada colección "borde" en el árbol de la carga.

* Separar las consultas con `Load`: se pueden recuperar los datos en distintas consultas y EF Core "corrige" las propiedades de navegación. "correcciones de seguridad" significa que EF Core rellena automáticamente las propiedades de navegación. Separar las consultas con `Load` más parecido a Explicit carga a carga diligente.

 ![Ejemplo de separar las consultas](read-related-data/_static/separate-queries.png)

 Nota: Core EF corrige automáticamente las propiedades de navegación para todas las entidades que se cargaron previamente en la instancia de contexto. Incluso si los datos de una propiedad de navegación son *no* incluidos explícitamente, la propiedad también puede rellenar si algunas o todas las entidades relacionadas se cargaron previamente.

* [Carga explícita](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading). Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados. Debe escribir código para recuperar los datos relacionados cuando sea necesario. Carga explícita con consultas independientes da como resultado varias consultas que se envían a la base de datos. Con carga explícita, el código especifica las propiedades de navegación que va a cargarse. Use la `Load` método para realizar la carga explícita. Por ejemplo:

 ![En el ejemplo se carga explícita](read-related-data/_static/explicit-loading.png)

* [Carga diferida](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading). [Núcleo EF no admite actualmente la carga diferida](https://github.com/aspnet/EntityFrameworkCore/issues/3797). Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados. La primera vez que se tiene acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Se envía una consulta a la base de datos cada vez que se accede a una propiedad de navegación por primera vez.

* El `Select` operador carga únicamente los datos relacionados necesarios.

## <a name="create-a-courses-page-that-displays-department-name"></a>Crear una página de cursos que muestra el nombre de departamento

La entidad de curso incluye una propiedad de navegación que contiene el `Department` entidad. El `Department` entidad contiene el departamento que el curso se asigna a.

Para mostrar el nombre del departamento asignado en una lista de cursos:

* Obtener la `Name` propiedad desde el `Department` entidad.
* El `Department` entidad procede del `Course.Department` propiedad de navegación.

![curso. Departamento](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a>Aplicar la técnica scaffolding el modelo de curso

* Salga de Visual Studio.
* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Ejecute el siguiente comando:

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

El scaffolds del comando anterior la `Course` modelo. Abra el proyecto en Visual Studio.

Compile el proyecto. La compilación genera errores similar al siguiente:

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 Cambiar globalmente `_context.Course` a `_context.Courses` (es decir, agregue una "s" para `Course`). 7 apariciones se encuentra y se actualizan.

Abra *Pages/Courses/Index.cshtml.cs* y examine el `OnGetAsync` método. El motor de scaffolding especificado carga diligente de la `Department` propiedad de navegación. El `Include` método especifica carga diligente.

Ejecute la aplicación y seleccione la **cursos** vínculo. Muestra la columna department el `DepartmentID`, lo que no es útil.

Actualice el método `OnGetAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

Agrega el código anterior `AsNoTracking`. `AsNoTracking`mejora el rendimiento porque no se realiza el seguimiento de las entidades devueltas. No se realiza un seguimiento de las entidades porque no se actualizan en el contexto actual.

Actualización *Views/Courses/Index.cshtml* con el siguiente marcado resaltado:

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

Los siguientes cambios se realizaron en el código con scaffolding:

* Cambia el encabezado de índice a Courses.
* Agrega un **número** columna que muestra la `CourseID` valor de propiedad. De forma predeterminada, las claves principales no son scaffolding porque normalmente son importantes para los usuarios finales. Sin embargo, en este caso la clave principal es significativa.
* Cambiar el **departamento** columna para mostrar el nombre del departamento. El código muestra la `Name` propiedad de la `Department` entidad que se carga en el `Department` propiedad de navegación:

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

Ejecute la aplicación y seleccione la **cursos** pestaña para ver la lista con los nombres de departamento.

![Página de índice de cursos](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a>Carga los datos con Select relacionados

El `OnGetAsync` método carga de datos relacionados con el `Include` método:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

El `Select` operador carga únicamente los datos relacionados necesarios. Para los elementos individuales, como el `Department.Name` utiliza un INNER JOIN de SQL. Para las colecciones utiliza otro acceso de base de datos, pero también lo hace el.`Include` operador en colecciones.

El siguiente código carga los datos relacionados con el `Select` método:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

El `CourseViewModel`:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

Vea [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) y [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) para obtener un ejemplo completo.

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Crear una página de instructores que muestra los cursos y las inscripciones

En esta sección, se crea la página de instructores.

<a name="IP"></a>
![Página de índice de instructores](read-related-data/_static/instructors-index.png)

Esta página se lee y muestra los datos relacionados de las maneras siguientes:

* La lista de instructores muestra datos relacionados de la `OfficeAssignment` entidad (Office en la imagen anterior). El `Instructor` y `OfficeAssignment` entidades se encuentran en una relación de uno a cero o uno. Carga diligente se usa para la `OfficeAssignment` entidades. Carga diligente es normalmente más eficaz cuando es necesitan mostrar los datos relacionados. En este caso, se muestran las asignaciones de oficina de los instructores.
* Cuando el usuario selecciona un instructor (Harui en la imagen anterior), relacionados con `Course` se muestran las entidades. El `Instructor` y `Course` entidades se encuentran en una relación de varios a varios. Eager para cargar el `Course` entidades y sus relacionados `Department` entidades se utiliza. En este caso, separar las consultas pueden ser más eficaces porque solo los cursos para el instructor seleccionado se necesitan. Este ejemplo muestra cómo utilizar carga diligente de propiedades de navegación de entidades que se encuentran en las propiedades de navegación.
* Cuando el usuario selecciona un curso (química en la imagen anterior), los datos de relacionados con el `Enrollments` aparece la entidad. En la imagen anterior, se muestran el grado y nombre del alumno. El `Course` y `Enrollment` entidades se encuentran en una relación uno a varios.

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Crear un modelo de vista para la vista de índice Instructor

La página de instructores muestra datos de tres tablas diferentes. Se crea un modelo de vista que incluya las tres entidades que representan las tres tablas.

En el *SchoolViewModels* carpeta, crear *InstructorIndexData.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a>Aplicar la técnica scaffolding el modelo Instructor

* Salga de Visual Studio.
* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Ejecute el siguiente comando:

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

El scaffolds del comando anterior la `Instructor` modelo. Abra el proyecto en Visual Studio.

Compile el proyecto. La compilación genera errores.

Cambiar globalmente `_context.Instructor` a `_context.Instructors` (es decir, agregue una "s" para `Instructor`). 7 apariciones se encuentra y se actualizan.

Ejecutar la aplicación y navegue hasta la página de instructores.

Reemplace *Pages/Instructors/Index.cshtml.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

El `OnGetAsync` método acepta datos de ruta opcional para el Id. del instructor seleccionado.

Examine la consulta en el *Pages/Instructors/Index.cshtml* página:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

La consulta tiene dos incluye:

* `OfficeAssignment`: Mostrada en el [instructores vista](#IP).
* `CourseAssignments`: Abre en los cursos impartidos.


### <a name="update-the-instructors-index-page"></a>Actualizar la página de índice de instructores

Actualización *Pages/Instructors/Index.cshtml* con el siguiente marcado:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

El marcado anterior realiza los cambios siguientes:

* Las actualizaciones de la `page` la directiva de `@page` a `@page "{id:int?}"`. `"{id:int?}"`es una plantilla de ruta. La plantilla de ruta cambia las cadenas de consulta de entero en la dirección URL para enrutar los datos. Por ejemplo, al hacer clic en el **seleccione** vínculo para un instructor cuando la directiva de página genera una dirección URL similar al siguiente:

    `http://localhost:1234/Instructors?id=2`

    Cuando la directiva de página es `@page "{id:int?}"`, la dirección URL anterior es:

    `http://localhost:1234/Instructors/2`

* Título de página es **instructores**.
* Agrega un **Office** columna que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es null. Dado que se trata de una relación de uno a cero o uno, no puede haber una entidad OfficeAssignment relacionada.

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
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* Agrega un nuevo hipervínculo con la etiqueta **seleccione**. Este vínculo envía el Id. del instructor seleccionado para la `Index` método y establece un color de fondo.

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

Ejecute la aplicación y seleccione la **instructores** ficha. La página muestra la `Location` (office) de relacionado `OfficeAssignment` entidad. Si OfficeAssignment' es null, se muestra una celda de tabla vacía.

![Página de índice de instructores que no hay nada seleccionado](read-related-data/_static/instructors-index-no-selection.png)

Haga clic en el **seleccione** vínculo. Los cambios de estilo de la fila.

### <a name="add-courses-taught-by-selected-instructor"></a>Agregar cursos impartidos instructor seleccionado

Actualización de la `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

Examine la consulta actualizada:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

La consulta anterior se agrega el `Department` entidades.

El siguiente código se ejecuta cuando se selecciona un instructor (`id != null`). El instructor seleccionado se recupera de la lista de instructores en el modelo de vista. El modelo de vista `Courses` propiedad se carga con la `Course` entidades desde ese instructor `CourseAssignments` propiedad de navegación.

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

El `Where` método devuelve una colección. En el anterior `Where` método, solo una `Instructor` se devuelve la entidad. El `Single` método convierte la colección en una sola `Instructor` entidad. El `Instructor` entidad proporciona acceso a la `CourseAssignments` propiedad. `CourseAssignments`proporciona acceso a relacionado `Course` entidades.

![Instructor a cursos m:M](complex-data-model/_static/courseassignment.png)

El `Single` método se utiliza en una colección cuando la colección tiene un solo elemento. El `Single` método produce una excepción si la colección está vacía o si hay más de un elemento. Una alternativa es `SingleOrDefault`, que devuelve una valor predeterminado (null, en este caso) si la colección está vacía. Usar `SingleOrDefault` en una colección vacía:

* Produce una excepción (de tratar de buscar un `Courses` propiedad en una referencia nula).
* El mensaje de excepción menos claramente indicaría la causa del problema.

El código siguiente rellena el modelo de vista `Enrollments` propiedad cuando se selecciona un curso:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

Agregue el siguiente marcado al final de la *Pages/Courses/Index.cshtml* Razor página:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

El marcado anterior muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.

Probar la aplicación. Haga clic en un **seleccione** vínculo en la página de instructores.

![Instructor de página de índice de instructores seleccionado](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a>Mostrar datos de estudiante

En esta sección, la aplicación se actualiza para mostrar los datos de estudiante para un curso seleccionado.

Actualizar la consulta en el `OnGetAsync` método *Pages/Instructors/Index.cshtml.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Actualización *Pages/Instructors/Index.cshtml*. Agregue el siguiente marcado al final del archivo:

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

El marcado anterior muestra una lista de los alumnos que están inscritos en el curso seleccionado.

Actualice la página y seleccione un instructor. Seleccione un curso para ver la lista de estudiantes inscritos y sus calificaciones.

![Instructor de página de índice de instructores y curso seleccionado](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a>Mediante un único

El `Single` método puede pasar en el `Where` condición en lugar de llamar el `Where` método por separado:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

Los anteriores `Single` enfoque no ofrece ninguna ventaja con respecto a `Where`. Algunos desarrolladores prefieren los `Single` aproximarse al estilo.

## <a name="explicit-loading"></a>Carga explícita

El código actual especifica carga diligente de `Enrollments` y `Students`:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

Imagine que rara vez, los usuarios desean ver las inscripciones en un curso. En ese caso, una optimización sería sólo se puede cargar los datos de inscripción si se solicita. En esta sección, el `OnGetAsync` se actualiza para usar la carga explícita de `Enrollments` y `Students`.

Actualización de la `OnGetAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

El código anterior quita el *ThenInclude* método se llama para los datos de inscripción y estudiantes. Si se selecciona un curso, se recupera el código resaltado:

* El `Enrollment` entidades para el curso seleccionado.
* El `Student` entidades para cada `Enrollment`.

Tenga en cuenta el anterior código convierte en comentario `.AsNoTracking()`. Propiedades de navegación solo se pueden cargar explícitamente para entidades sometidas a seguimiento.

Probar la aplicación. Desde la perspectiva de los usuarios, la aplicación se comporta exactamente igual a la versión anterior.

El siguiente tutorial muestra cómo actualizar datos relacionados.

>[!div class="step-by-step"]
>[Anterior](xref:data/ef-rp/complex-data-model)
>[Siguiente](xref:data/ef-rp/update-related-data)