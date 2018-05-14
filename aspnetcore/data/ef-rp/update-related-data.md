---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Actualización de datos relacionados (7 de 8)'
author: rick-anderson
description: En este tutorial, actualizará datos relacionados mediante la actualización de campos de clave externa y propiedades de navegación.
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 2eff6cd5f4bb737cb79875c9b04c889914376cd0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---update-related-data---7-of-8"></a>Páginas de Razor con EF Core en ASP.NET Core: Actualización de datos relacionados (7 de 8)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

En este tutorial se muestra cómo actualizar datos relacionados. Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

En las ilustraciones siguientes se muestran algunas de las páginas completadas.

![Página de edición de cursos](update-related-data/_static/course-edit.png)
![Página de edición de instructores](update-related-data/_static/instructor-edit-courses.png)

Examine y pruebe las páginas de cursos Create y Edit. Cree un curso. El departamento se selecciona por su clave principal (un entero), no su nombre. Modifique el curso nuevo. Cuando haya terminado las pruebas, elimine el curso nuevo.

## <a name="create-a-base-class-to-share-common-code"></a>Crear una clase base para compartir código común

En las páginas Courses/Create y Courses/Edit se necesita una lista de nombres de departamento. Cree la clase base *Pages/Courses/DepartmentNamePageModel.cshtml.cs* para las páginas Create y Edit:

[!code-csharp[](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

En el código anterior se crea una clase [SelectList](/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para que contenga la lista de nombres de departamento. Si se especifica `selectedDepartment`, se selecciona ese departamento en la `SelectList`.

Las clases de modelo de página de Create y Edit se derivan de `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Personalizar las páginas de cursos

Cuando se crea una entidad de curso, debe tener una relación con un departamento existente. Para agregar un departamento durante la creación de un curso, la clase base para Create y Edit contiene una lista desplegable para seleccionar el departamento. La lista desplegable establece la propiedad de clave externa (FK) `Course.DepartmentID`. EF Core usa la FK `Course.DepartmentID` para cargar la propiedad de navegación `Department`.

![Crear el curso](update-related-data/_static/ddl.png)

Actualice el modelo de página de Create con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-999)]

El código anterior:

* Deriva de `DepartmentNamePageModel`.
* Usa `TryUpdateModelAsync` para evitar la [publicación excesiva](xref:data/ef-rp/crud#overposting).
* Reemplaza `ViewData["DepartmentID"]` con `DepartmentNameSL` (de la clase base).

`ViewData["DepartmentID"]` se reemplaza con `DepartmentNameSL` fuertemente tipado. Los modelos fuertemente tipados son preferibles a los de establecimiento flexible de tipos. Para obtener más información, vea [Establecimiento flexible de datos (ViewData y ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Actualizar la página Courses Create

Actualice *Pages/Courses/Create.cshtml* con el marcado siguiente:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

En el marcado anterior se realizan los cambios siguientes:

* Se cambia el título de **DepartmentID** a **Department**.
* Se reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).
* Se agrega la opción "Select Department" (Seleccionar departamento). Este cambio representa "Select Department" en lugar del primer departamento.
* Se agrega un mensaje de validación cuando el departamento no está seleccionado.

La página de Razor usa la [Aplicación auxiliar de etiquetas de selección](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Pruebe la página Create. En la página Create se muestra el nombre del departamento en lugar del identificador.

### <a name="update-the-courses-edit-page"></a>Actualice la página Courses Edit.

Actualice el modelo de página de Edit con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-999)]

Los cambios son similares a los realizados en el modelo de página de Create. En el código anterior, `PopulateDepartmentsDropDownList` pasa el identificador de departamento, que selecciona el departamento especificado en la lista desplegable.

Actualice *Pages/Courses/Edit.cshtml* con el marcado siguiente:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

En el marcado anterior se realizan los cambios siguientes:

* Se muestra el identificador del curso. Por lo general no se muestra la clave principal (PK) de una entidad. Las PK normalmente no tienen sentido para los usuarios. En este caso, la clave principal es el número de curso.
* Se cambia el título de **DepartmentID** a **Department**.
* Se reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).
* Se agrega la opción "Select Department" (Seleccionar departamento). Este cambio representa "Select Department" en lugar del primer departamento.
* Se agrega un mensaje de validación cuando el departamento no está seleccionado.

La página contiene un campo oculto (`<input type="hidden">`) para el número de curso. Agregar una aplicación auxiliar de etiquetas `<label>` con `asp-for="Course.CourseID"` no elimina la necesidad del campo oculto. Se requiere `<input type="hidden">` para que el número de curso se incluya en los datos enviados cuando el usuario hace clic en **Guardar**.

Pruebe el código actualizado. Cree, modifique y elimine un curso.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Agregar AsNoTracking a los modelos de página de Details y Delete

[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) puede mejorar el rendimiento cuando el seguimiento no es necesario. Agregue `AsNoTracking` al modelo de página de Delete y Details. En el código siguiente se muestra el modelo de página de Delete actualizado:

[!code-csharp[](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Actualice el método `OnGetAsync` en el archivo *Pages/Courses/Details.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modificar las páginas Delete y Details

Actualice la página de Razor Delete con el marcado siguiente:

[!code-cshtml[](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Realice los mismos cambios en la página Details.

### <a name="test-the-course-pages"></a>Probar las páginas Course

Pruebe las páginas Create, Edit, Details y Delete.

## <a name="update-the-instructor-pages"></a>Actualizar las páginas de instructor

En las siguientes secciones se actualizan las páginas de instructor.

### <a name="add-office-location"></a>Agregar la ubicación de la oficina

Al editar un registro de instructor, es posible que quiera actualizar la asignación de la oficina del instructor. La entidad `Instructor` tiene una relación de uno a cero o uno con la entidad `OfficeAssignment`. El código de instructor debe controlar lo siguiente:

* Si el usuario desactiva la asignación de la oficina, elimine la entidad `OfficeAssignment`.
* Si el usuario especifica una asignación de oficina y estaba vacía, cree una entidad `OfficeAssignment`.
* Si el usuario cambia la asignación de oficina, actualice la entidad `OfficeAssignment`.

Actualice el modelo de página de Edit de los instructores con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-999)]

El código anterior:

- Obtiene la entidad `Instructor` actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`.
- Actualiza la entidad `Instructor` recuperada con valores del enlazador de modelos. `TryUpdateModel` evita la [publicación excesiva](xref:data/ef-rp/crud#overposting).
- Si la ubicación de la oficina está en blanco, establece `Instructor.OfficeAssignment` en NULL. Cuando `Instructor.OfficeAssignment` es NULL, se elimina la fila relacionada en la tabla `OfficeAssignment`.

### <a name="update-the-instructor-edit-page"></a>Actualizar la página Edit del instructor

Actualice *Pages/Instructors/Edit.cshtml* con la ubicación de la oficina:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Compruebe que puede cambiar la ubicación de la oficina de un instructor.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Agregar asignaciones de cursos a la página Edit de los instructores

Los instructores pueden impartir cualquier número de cursos. En esta sección, agregará la capacidad de cambiar las asignaciones de cursos. En la imagen siguiente se muestra la página Edit actualizada de los instructores:

![Página de edición de instructores con cursos](update-related-data/_static/instructor-edit-courses.png)

`Course` e `Instructor` tienen una relación de varios a varios. Para agregar y eliminar relaciones, agregue y quite entidades del conjunto de entidades combinadas `CourseAssignments`.

Las casillas permiten cambios en los cursos a los que está asignado un instructor. Se muestra una casilla para cada curso en la base de datos. Los cursos a los que el instructor está asignado se activan. El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos. Si el número de cursos fuera mucho mayor:

* Probablemente usaría una interfaz de usuario diferente para mostrar los cursos.
* El método de manipulación de una entidad de combinación para crear o eliminar relaciones no cambiaría.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Agregar clases para admitir las páginas de instructor Create y Edit

Cree *SchoolViewModels/AssignedCourseData.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

La clase `AssignedCourseData` contiene datos para crear las casillas para los cursos asignados por un instructor.

Cree la clase base *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs*:

[!code-csharp[](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

`InstructorCoursesPageModel` es la clase base que se usará para los modelos de página de Edit y Create. `PopulateAssignedCourseData` lee todas las entidades `Course` para rellenar `AssignedCourseDataList`. Para cada curso, el código establece el `CourseID`, el título y si el instructor está asignado o no al curso. Se usa un [HashSet](/dotnet/api/system.collections.generic.hashset-1) para crear búsquedas eficaces.

### <a name="instructors-edit-page-model"></a>Modelo de página de edición de instructores

Actualice el modelo de página de edición de instructores con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-999)]

El código anterior controla los cambios de asignación de oficina.

Actualice la vista de Razor del instructor:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Al pegar el código en Visual Studio, se cambian los saltos de línea de tal forma que el código se interrumpe. Presione Ctrl+Z una vez para deshacer el formato automático. Ctrl+Z corrige los saltos de línea para que se muestren como se ven aquí. No es necesario que la sangría sea perfecta, pero las líneas `@</tr><tr>`, `@:<td>`, `@:</td>` y `@:</tr>` deben estar en una única línea tal y como se muestra. Con el bloque de código nuevo seleccionado, presione tres veces la tecla Tab para alinearlo con el código existente. Puede votar o revisar el estado de este error [con este vínculo](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

En el código anterior se crea una tabla HTML que tiene tres columnas. Cada columna tiene una casilla y una leyenda que contiene el número y el título del curso. Todas las casillas tienen el mismo nombre ("selectedCourses"). Al usar el mismo nombre se informa al enlazador de modelos que las trate como un grupo. El atributo de valor de cada casilla se establece en `CourseID`. Cuando se envía la página, el enlazador de modelos pasa una matriz formada solo por los valores `CourseID` de las casillas activadas.

Cuando se representan las casillas por primera vez, los cursos asignados al instructor tienen atributos checked.

Ejecute la aplicación y pruebe la página Edit de los instructores actualizada. Cambie algunas asignaciones de cursos. Los cambios se reflejan en la página Index.

Nota: El enfoque que se aplica aquí para modificar datos de los cursos del instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, una interfaz de usuario y un método de actualización diferentes serían más eficaces y útiles.

### <a name="update-the-instructors-create-page"></a>Actualizar la página de creación de instructores

Actualice el modelo de página de creación de instructores con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

El código anterior es similar al de *Pages/Instructors/Edit.cshtml.cs*.

Actualice la página de Razor de creación de instructores con el marcado siguiente:

[!code-cshtml[](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Pruebe la página de creación de instructores.

## <a name="update-the-delete-page"></a>Actualizar la página Delete

Actualice el modelo de la página Delete con el código siguiente:

[!code-csharp[](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-999)]

En el código anterior se realizan los cambios siguientes:

* Se usa la carga diligente para la propiedad de navegación `CourseAssignments`. Es necesario incluir `CourseAssignments` o no se eliminarán cuando se elimine el instructor. Para evitar la necesidad de leerlos, configure la eliminación en cascada en la base de datos.

* Si el instructor que se va a eliminar está asignado como administrador de cualquiera de los departamentos, quita la asignación de instructor de esos departamentos.

> [!div class="step-by-step"]
> [Anterior](xref:data/ef-rp/read-related-data)
> [Siguiente](xref:data/ef-rp/concurrency)
