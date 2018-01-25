---
title: "Páginas de Razor con EF básica: actualizar datos relacionados - 7 de 8"
author: rick-anderson
description: "En este tutorial, actualizará los datos relacionados mediante la actualización de campos de clave externa y las propiedades de navegación."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/update-related-data
ms.openlocfilehash: 236589d0202a7f30f1e1a9d69902000fd9a2dd71
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="updating-related-data---ef-core-razor-pages-7-of-8"></a>Actualizar datos relacionados - páginas de Razor EF Core (7 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Este tutorial muestra cómo actualizar datos relacionados. Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part7).

Las siguientes ilustraciones se muestran algunas de las páginas completadas.

![Página de edición de curso](update-related-data/_static/course-edit.png)
![Instructor Editar página](update-related-data/_static/instructor-edit-courses.png)

Examine y probar las páginas de curso crear y editar. Crear un nuevo curso. El departamento se selecciona por su clave principal (un entero), no su nombre. Modifique el curso nuevo. Cuando haya terminado las pruebas, elimine el curso nuevo.

## <a name="create-a-base-class-to-share-common-code"></a>Crear una clase base para compartir código común

Las páginas cursos/creación y edición de cursos/necesitan una lista de nombres de departamento. Crear el *Pages/Courses/DepartmentNamePageModel.cshtml.cs* la clase base para la creación y edición de páginas:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/DepartmentNamePageModel.cshtml.cs?highlight=9,11,20-21)]

El código anterior crea un [SelectList](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.rendering.selectlist?view=aspnetcore-2.0) para que contenga la lista de nombres de departamento. Si `selectedDepartment` se especifica, se selecciona ese departamento en el `SelectList`.

Se derivan las clases de modelo de página de creación y edición de `DepartmentNamePageModel`.

## <a name="customize-the-courses-pages"></a>Personalizar las páginas de cursos

Cuando se crea una nueva entidad de curso, debe tener una relación con un departamento existente. Para agregar un departamento durante la creación de un curso, la clase base para crear y editar contiene una lista desplegable para seleccionar el departamento. Los conjuntos de la lista desplegable el `Course.DepartmentID` propiedad de clave externa (FK). EF Core utiliza el `Course.DepartmentID` FK para cargar la `Department` propiedad de navegación.

![Crear curso](update-related-data/_static/ddl.png)

Actualice el modelo de páginas de crear con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Create.cshtml.cs?highlight=7,18,32-)]

El código anterior:

* Deriva de `DepartmentNamePageModel`.
* Usa `TryUpdateModelAsync` para evitar [de publicación excesiva](xref:data/ef-rp/crud#overposting).
* Reemplaza `ViewData["DepartmentID"]` con `DepartmentNameSL` (de la clase base).

`ViewData["DepartmentID"]`se reemplaza con fuertemente tipado `DepartmentNameSL`. Modelos fuertemente tipados están la preferida en establecimiento flexible. Para obtener más información, consulte [establecimiento flexible de datos (ViewData y ViewBag)](xref:mvc/views/overview#VD_VB).

### <a name="update-the-courses-create-page"></a>Actualizar la página Crear cursos

Actualización *Pages/Courses/Create.cshtml* con el siguiente marcado:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?highlight=29-34)]

El marcado anterior realiza los cambios siguientes:

* Cambia el título de **DepartmentID** a **departamento**.
* Reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).
* Agrega la opción "Seleccionar departamento". Este cambio representa "Seleccione Departamento" en lugar de la primera departamento.
* Agrega un mensaje de validación si no está seleccionado el departamento.

La página de Razor utiliza el [seleccione etiqueta auxiliar](xref:mvc/views/working-with-forms#the-select-tag-helper):

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Create.cshtml?range=28-35&highlight=3-6)]

Probar la página Crear. La página Crear muestra el nombre del departamento en lugar de lo Id. de departamento.

### <a name="update-the-courses-edit-page"></a>Actualice la página Editar cursos.

Actualizar el modelo de página de edición con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Edit.cshtml.cs?highlight=8,28,35,36,40,47-)]

Los cambios son similares a las realizadas en el modelo de páginas de crear. En el código anterior, `PopulateDepartmentsDropDownList` pasa el identificador de departamento, que selecciona el departamento especificado en la lista desplegable.

Actualización *Pages/Courses/Edit.cshtml* con el siguiente marcado:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Edit.cshtml?highlight=17-20,32-35)]

El marcado anterior realiza los cambios siguientes:

* Muestra el identificador de curso. Por lo general no se muestra la clave principal (PK) de una entidad. PK es normalmente tiene sentido para los usuarios. En este caso, la clave principal es el número de curso.
* Cambia el título de **DepartmentID** a **departamento**.
* Reemplaza `"ViewBag.DepartmentID"` con `DepartmentNameSL` (de la clase base).
* Agrega la opción "Seleccionar departamento". Este cambio representa "Seleccione Departamento" en lugar de la primera departamento.
* Agrega un mensaje de validación si no está seleccionado el departamento.

La página contiene un campo oculto (`<input type="hidden">`) para el número de curso. Agregar un `<label>` etiqueta auxiliar con `asp-for="Course.CourseID"` no elimina la necesidad de que el campo oculto. `<input type="hidden">`se requiere para el número de curso que se incluirá en los datos enviados cuando el usuario hace clic en **guardar**.

Probar el código actualizado. Crear, editar y eliminar un curso.

## <a name="add-asnotracking-to-the-details-and-delete-page-models"></a>Agregar AsNoTracking a los detalles y eliminar modelos de página

[AsNoTracking](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) puede mejorar el rendimiento cuando el seguimiento no es necesario. Agregar `AsNoTracking` para el modelo de páginas de eliminación y los detalles. El código siguiente muestra el modelo de página de eliminación actualizado:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Delete.cshtml.cs?name=snippet&highlight=21,23,40,41)]

Actualización de la `OnGetAsync` método en el *Pages/Courses/Details.cshtml.cs* archivo:

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Details.cshtml.cs?name=snippet)]

### <a name="modify-the-delete-and-details-pages"></a>Modificar las páginas de eliminación y detalles

Actualice la página Eliminar Razor con el siguiente marcado:

[!code-cshtml[Main](intro/samples/cu/Pages/Courses/Delete.cshtml?highlight=15-20)]

Realizar los mismos cambios en la página de detalles.

### <a name="test-the-course-pages"></a>Probar las páginas de curso

Prueba de crear, editar, detalles y eliminar.

## <a name="update-the-instructor-pages"></a>Actualizar las páginas de instructor

En las siguientes secciones actualización las páginas de instructor.

### <a name="add-office-location"></a>Agregar ubicación de la oficina

Al editar un registro de instructor, desea actualizar la asignación de office del instructor. El `Instructor` entidad tiene una relación de uno a cero o uno con el `OfficeAssignment` entidad. El código de instructor debe administrar:

* Si el usuario desactiva la asignación de office, elimine la `OfficeAssignment` entidad.
* Si el usuario especifica una asignación de oficina y estaba vacío, cree un nuevo `OfficeAssignment` entidad.
* Si el usuario cambia la asignación de office, actualizar el `OfficeAssignment` entidad.

Actualizar el modelo de página de edición de instructores con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml.cs?name=snippet&highlight=20-23,32,39-)]

El código anterior:

- Obtiene el actual `Instructor` entidad desde la base de datos con carga diligente de la `OfficeAssignment` propiedad de navegación.
- Actualiza el objeto recuperado `Instructor` entidad con valores desde el enlazador de modelos. `TryUpdateModel`evita que [de publicación excesiva](xref:data/ef-rp/crud#overposting).
- Si la ubicación de la oficina está en blanco, establece `Instructor.OfficeAssignment` en null. Cuando `Instructor.OfficeAssignment` es null, la fila relacionada en el `OfficeAssignment` se elimina la tabla.

### <a name="update-the-instructor-edit-page"></a>Actualizar la página de edición de instructor

Actualización *Pages/Instructors/Edit.cshtml* con la ubicación de office:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit1.cshtml?highlight=29-33)]

Compruebe que puede cambiar una ubicación de oficina de instructores.

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Agregar trabajos del curso a la página de edición de instructor

Instructores pueden enseñar a cualquier número de cursos. En esta sección, agregará la capacidad de cambiar las asignaciones de curso. La siguiente imagen muestra al instructor actualizado Editar página:

![Página de edición de instructor con cursos](update-related-data/_static/instructor-edit-courses.png)

`Course`y `Instructor` tiene una relación de varios a varios. Para agregar y eliminar las relaciones, agregar y quitar entidades de la `CourseAssignments` unir el conjunto de entidades.

Casillas de verificación Permitir cambios en un instructor se asigna a los cursos. Se muestra una casilla de verificación para cada curso en la base de datos. Se comprueban los cursos en los que se asigna el instructor a. El usuario puede activar o desactivar casillas de verificación para cambiar las asignaciones de curso. Si el número de cursos era mucho más grandes:

* Probablemente utilizará una interfaz de usuario diferente para mostrar los cursos.
* No cambie el método de manipulación de una entidad de combinación para crear o eliminar relaciones.

### <a name="add-classes-to-support-create-and-edit-instructor-pages"></a>Agregar clases para admitir la creación y edición de páginas de instructor

Crear *SchoolViewModels/AssignedCourseData.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

La `AssignedCourseData` clase contiene datos para crear las casillas de verificación para cursos asignados por un instructor.

Crear el *Pages/Instructors/InstructorCoursesPageModel.cshtml.cs* clase base:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/InstructorCoursesPageModel.cshtml.cs)]

El `InstructorCoursesPageModel` es la clase base que se utilizará para la edición y crear modelos de página. `PopulateAssignedCourseData`todos los lee `Course` entidades para rellenar `AssignedCourseDataList`. Para cada curso, el código establece la `CourseID`, título, y si no se asigna el instructor al curso. A [HashSet](https://docs.microsoft.com/dotnet/api/system.collections.generic.hashset-1) se utiliza para crear búsquedas eficaces.

### <a name="instructors-edit-page-model"></a>Modelo de página de edición de instructores

Actualizar el modelo de página de edición de instructor con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml.cs?name=snippet&highlight=1,20-24,30,34,41-)]

El código anterior controla los cambios de asignación de office.

Actualice la vista Razor instructor:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Edit.cshtml?highlight=34-59)]

<a id="notepad"></a>
> [!NOTE]
> Cuando pegue el código en Visual Studio, saltos de línea se cambian de forma que el código se interrumpe. Presione CTRL+z una vez para deshacer el formato automático. CTRL+z corrige los saltos de línea para que se muestren como lo que ve aquí. La sangría no tiene que ser perfecto, pero la `@</tr><tr>`, `@:<td>`, `@:</td>`, y `@:</tr>` líneas deben estar en una sola línea tal como se muestra. Con el bloque de código nuevo seleccionado, presione la tecla Tab tres veces para alinear el nuevo código con el código existente. Votar sobre o revisar el estado de este error [con este vínculo](https://developercommunity.visualstudio.com/content/problem/147795/razor-editor-malforms-pasted-markup-and-creates-in.html).

El código anterior crea una tabla HTML que tiene tres columnas. Cada columna tiene una casilla de verificación y un título que contiene el número y el título. Las casillas de verificación todos tienen el mismo nombre ("selectedCourses"). Con el mismo nombre, informa el enlazador de modelos para tratarlos como un grupo. El atributo de valor de cada casilla de verificación se establece en `CourseID`. Cuando se envía la página, el enlazador de modelos pasa una matriz formada por el `CourseID` valores de las casillas de verificación que están seleccionadas.

Cuando inicialmente se representan las casillas de verificación, cursos asignados al instructor comprobaron atributos.

Ejecutar la aplicación y probar la página de edición de instructores actualizada. Cambiar algunos trabajos del curso. Los cambios se reflejan en la página de índice.

Nota: Al enfoque tomado aquí para editar datos de curso instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, una interfaz de usuario diferente y un método de actualización diferente sería más eficaz y utilizable.

### <a name="update-the-instructors-create-page"></a>Actualizar la página de creación de instructores

Actualice el modelo de páginas de instructor crear con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Create.cshtml.cs)]

El código anterior es similar a la *Pages/Instructors/Edit.cshtml.cs* código.

Actualizar la página de crear Razor instructor con el marcado siguiente:

[!code-cshtml[Main](intro/samples/cu/Pages/Instructors/Create.cshtml?highlight=32-62)]

Probar la página de creación de instructor.

## <a name="update-the-delete-page"></a>Actualizar la página de borrado

Actualice el modelo de páginas de eliminación con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Delete.cshtml.cs?highlight=5,40-)]

El código anterior realiza los cambios siguientes:

* Carga diligente de se utiliza el `CourseAssignments` propiedad de navegación. `CourseAssignments`se deben incluir o no se eliminan cuando se elimina el instructor. Para evitar la necesidad de leerlos, configure la eliminación en cascada en la base de datos.

* Si el instructor va a eliminar está asignado como administrador de los departamentos, quita la asignación de instructor de los departamentos.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/read-related-data)
[Siguiente](xref:data/ef-rp/concurrency)
