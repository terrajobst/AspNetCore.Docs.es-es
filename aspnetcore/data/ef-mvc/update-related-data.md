---
title: "Núcleo de ASP.NET MVC con EF Core - actualización relacionadas con datos - 7 de 10"
author: tdykstra
description: "En este tutorial, actualizará los datos relacionados mediante la actualización de campos de clave externa y las propiedades de navegación."
keywords: "Combinaciones de núcleo de ASP.NET, Entity Framework Core, datos relacionados,"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 67bd162b-bfb7-4750-9e7f-705228b5288c
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/update-related-data
ms.openlocfilehash: 655fefc0f9d884300bea670795c39a7a9aa10bb8
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2017
---
# <a name="updating-related-data---ef-core-with-aspnet-core-mvc-tutorial-7-of-10"></a>Actualizar datos relacionados - Core EF con el tutorial de MVC de ASP.NET Core (7 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En el tutorial anterior se muestran datos relacionados; en este tutorial, actualizará los datos relacionados mediante la actualización de campos de clave externa y las propiedades de navegación.

Las ilustraciones siguientes muestran algunas de las páginas que va a trabajar con.

![Página de edición de curso](update-related-data/_static/course-edit.png)

![Página de edición de instructor](update-related-data/_static/instructor-edit-courses.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar la creación y edición de páginas para cursos

Cuando se crea una nueva entidad de curso, debe tener una relación con un departamento existente. Para facilitar esto, el código con scaffolding incluye métodos de controlador y crear y editar vistas que incluyen una lista desplegable para seleccionar el departamento. Los conjuntos de la lista desplegable el `Course.DepartmentID` propiedad de clave externa, y eso es todo lo que necesita de Entity Framework para cargar la `Department` propiedad de navegación con la entidad Department adecuada. Podrá usar el código con scaffolding, pero cambiar ligeramente para agregar el control de errores y ordenar la lista desplegable.

En *CoursesController.cs*, elimine los cuatro métodos de creación y edición y reemplácelas con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_CreatePost)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_EditPost)]

Después de la `Edit` método HttpPost, cree un nuevo método que carga la información de departamento de la lista desplegable.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_Departments)]

El `PopulateDepartmentsDropDownList` /método siguiente obtiene una lista de todos los departamentos que se ordenan por nombre, se crea un `SelectList` colección para obtener una lista desplegable y pasa la colección a la vista en `ViewBag`. El método acepta opcional `selectedDepartment` parámetro que permita al código que realiza la llamada especificar el elemento que se seleccionará cuando se procesa la lista desplegable. La vista pasará el nombre "DepartmentID" a la `<select>` auxiliar de etiquetas y la aplicación auxiliar, a continuación, sepa que puede para buscar en el `ViewBag` de objeto para un `SelectList` denominado "DepartmentID".

El HttpGet `Create` llamadas al método el `PopulateDepartmentsDropDownList` método sin tener que configurar el elemento seleccionado, ya que para un nuevo curso el departamento aún no está establecido:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=3&name=snippet_CreateGet)]

El HttpGet `Edit` método establece el elemento seleccionado, basándose en el identificador del departamento que ya está asignado a la línea que se está edita:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=15&name=snippet_EditGet)]

Los métodos HttpPost para ambos `Create` y `Edit` también incluir código que establece el elemento seleccionado cuando vuelva a mostrar la página después de un error. Esto garantiza que cuando se vuelve a mostrar la página para mostrar el mensaje de error, se seleccionó cualquier departamento permanece seleccionada.

### <a name="add-asnotracking-to-details-and-delete-methods"></a>Agregar. AsNoTracking para obtener más información y eliminar métodos

Para optimizar el rendimiento de los detalles de curso y eliminar páginas, agregar `AsNoTracking` llama el `Details` y HttpGet `Delete` métodos.

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_Details)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?highlight=10&name=snippet_DeleteGet)]

### <a name="modify-the-course-views"></a>Modificar las vistas de curso

En *Views/Courses/Create.cshtml*, agregar una opción de "Departamento de seleccionar" para el **departamento** lista desplegable lista, cambie el título de **DepartmentID** a  **Departamento**y agregar un mensaje de validación.

[!code-html[Main](intro/samples/cu/Views/Courses/Create.cshtml?highlight=2-6&range=29-34)]

En *Views/Courses/Edit.cshtml*, realizar el mismo cambio en el campo de departamento que acaba de *Create.cshtml*.

También en *Views/Courses/Edit.cshtml*, agregar un campo de número de curso antes de la **título** campo. Dado que el número de curso es la clave principal, se muestra, pero no se puede cambiar.

[!code-html[Main](intro/samples/cu/Views/Courses/Edit.cshtml?range=15-18)]

Ya hay un campo oculto (`<input type="hidden">`) para el número de curso en la vista de edición. Agregar un `<label>` auxiliar de etiqueta no elimina la necesidad de para el campo oculto porque no hacer que el número de curso que se incluirá en los datos enviados cuando el usuario hace clic en **guardar** en el **editar** página.

En *Views/Courses/Delete.cshtml*, agregar un campo de número de curso en la parte superior y cambie el Id. de departamento al nombre de departamento.

[!code-html[Main](intro/samples/cu/Views/Courses/Delete.cshtml?highlight=14-19,36)]

En *Views/Courses/Details.cshtml*, realizar el mismo cambio en la que se hizo simplemente *Delete.cshtml*.

### <a name="test-the-course-pages"></a>Probar las páginas de curso

Ejecute el **crear** página (mostrar la página de índice de curso y haga clic en **crear nuevo**) y escriba los datos de un curso nuevo:

![Página de creación de curso](update-related-data/_static/course-create.png)

Haga clic en **crear**. Se muestra la página de índice de cursos con el nuevo curso agregado a la lista. El nombre de departamento en la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.

Ejecute el **editar** página (haga clic en **editar** en un curso en la página de índice de curso).

![Página de edición de curso](update-related-data/_static/course-edit.png)

Cambiar los datos en la página y haga clic en **guardar**. Se muestra la página de índice de cursos con los datos de curso actualizado.

## <a name="add-an-edit-page-for-instructors"></a>Agregar una página de edición para instructores

Cuando se edita un registro instructor, desea poder actualizar la asignación de office del instructor. La entidad Instructor tiene una relación de uno a cero o uno a la entidad OfficeAssignment, lo que significa que el código tiene que controlar las situaciones siguientes:

* Si el usuario borra la asignación de office y que tenía originalmente un valor, elimine la entidad OfficeAssignment.

* Si el usuario escribe un valor de asignación de office y originalmente estaba vacío, cree una nueva entidad OfficeAssignment.

* Si el usuario cambia el valor de una asignación de office, cambie el valor en una entidad OfficeAssignment existente.

### <a name="update-the-instructors-controller"></a>Actualice el controlador de instructores

En *InstructorsController.cs*, cambie el código en el HttpGet `Edit` método por lo que TI carga la entidad Instructor `OfficeAssignment` propiedad de navegación y llamadas `AsNoTracking`:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=9,10&name=snippet_EditGetOA)]

Reemplace el HttpPost `Edit` método con el siguiente código para controlar las actualizaciones de asignación de office:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EditPostOA)]

El código hace lo siguiente:

-  Cambia el nombre del método para `EditPost` porque la firma ahora es el mismo que el HttpGet `Edit` método (el `ActionName` atributo especifica que el `/Edit/` aún se utiliza la dirección URL).

-  Obtiene la entidad Instructor actual de la base de datos mediante diligente para cargar la `OfficeAssignment` propiedad de navegación. Esto equivale a lo que hizo en el HttpGet `Edit` método.

-  Actualiza la entidad Instructor recuperada con valores desde el enlazador de modelos. El `TryUpdateModel` sobrecarga permite a la lista blanca las propiedades que van a incluir. Esto evita que el registro excesivo, como se explica en la [segundo tutorial](crud.md).

    <!-- Snippets do not play well with <ul> [!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=241-244)] -->

    ```csharp
    if (await TryUpdateModelAsync<Instructor>(
        instructorToUpdate,
        "",
        i => i.FirstMidName, i => i.LastName, i => i.HireDate, i => i.OfficeAssignment))
    ```
    
-   Si la ubicación de la oficina está en blanco, Establece la propiedad Instructor.OfficeAssignment en null para que se eliminará la fila en la tabla OfficeAssignment relacionada.

    <!-- Snippets do not play well with <ul>  "intro/samples/cu/Controllers/InstructorsController.cs"} -->

    ```csharp
    if (String.IsNullOrWhiteSpace(instructorToUpdate.OfficeAssignment?.Location))
    {
        instructorToUpdate.OfficeAssignment = null;
    }
    ```

- Guarda los cambios en la base de datos.

### <a name="update-the-instructor-edit-view"></a>Actualizar la vista de edición Instructor

En *Views/Instructors/Edit.cshtml*, agregar un nuevo campo para editar la ubicación de la oficina, al final antes de la **guardar** botón:

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=30-34)]

Ejecute la página (seleccione la **instructores** ficha y, a continuación, haga clic en **editar** en un instructor). Cambiar el **oficina** y haga clic en **guardar**.

![Página de edición de instructor](update-related-data/_static/instructor-edit-office.png)

## <a name="add-course-assignments-to-the-instructor-edit-page"></a>Agregar trabajos del curso en la página Editar Instructor

Instructores pueden enseñar a cualquier número de cursos. Ahora mejorará la página Editar Instructor agregando la capacidad de cambiar las asignaciones de curso mediante un grupo de casillas de verificación, tal y como se muestra en la siguiente captura de pantalla:

![Página de edición de instructor con cursos](update-related-data/_static/instructor-edit-courses.png)

La relación entre las entidades de curso e Instructor es varios a varios. Para agregar y eliminar las relaciones, agregar y quitar entidades a y desde el conjunto de entidades de la combinación de CourseAssignments.

La interfaz de usuario que permite cambiar los cursos un instructor está asignado a es un grupo de casillas de verificación. Se muestra una casilla de verificación para cada curso en la base de datos y se seleccionan los que está asignado actualmente el instructor a. El usuario puede activar o desactivar casillas de verificación para cambiar las asignaciones de curso. Si el número de cursos era mucho mayor, probablemente desee utilizar un método diferente de presentar los datos en la vista, pero usaría el mismo método de manipulación de una entidad de combinación para crear o eliminar relaciones.

### <a name="update-the-instructors-controller"></a>Actualice el controlador de instructores

Para proporcionar datos a la vista de la lista de casillas de verificación, deberá usar una clase de modelo de vista.

Crear *AssignedCourseData.cs* en el *SchoolViewModels* carpeta y reemplace el código existente por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/AssignedCourseData.cs)]

En *InstructorsController.cs*, reemplace el HttpGet `Edit` método por el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=10,17,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36&name=snippet_EditGetCourses)]

El código agrega carga diligente de la `Courses` propiedad de navegación y llama a la nueva `PopulateAssignedCourseData` método para proporcionar información de la matriz de casilla utilizando la `AssignedCourseData` Ver clase de modelo.

El código en el `PopulateAssignedCourseData` método lee a través de todas las entidades de curso para cargar una lista de cursos mediante la clase de modelo de vista. Para cada curso, el código comprueba si existe el curso en el instructor `Courses` propiedad de navegación. Para crear una búsqueda eficaz al comprobar si se asigna un curso para el instructor, los cursos asignados al instructor se colocan en un `HashSet` colección. El `Assigned` propiedad está establecida en true para cursos el instructor se asigna a. La vista utilizará esta propiedad para determinar qué Compruebe cuadros deben mostrarse como seleccionado. Por último, la lista se pasa a la vista en `ViewData`.

A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **guardar**. Reemplace el `EditPost` método con el siguiente código y agregar un nuevo método que actualiza el `Courses` propiedad de navegación de la entidad Instructor.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=1,3,12,13,25,39-40&name=snippet_EditPostCourses)]

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=1-31)]

La firma de método ahora es diferente de la HttpGet `Edit` método, por lo que cambia el nombre del método de `EditPost` a `Edit`.

Puesto que la vista no tiene una colección de entidades del curso, el enlazador de modelos no se puede actualizar automáticamente el `CourseAssignments` propiedad de navegación. En lugar de usar el enlazador de modelos para actualizar la `CourseAssignments` propiedad de navegación, hacerlo en el nuevo `UpdateInstructorCourses` método. Por lo tanto, tendrá que excluir el `CourseAssignments` propiedad de enlace de modelos. Esto no requiere ningún cambio en el código que llama `TryUpdateModel` porque lo está utilizando la sobrecarga de la creación de listas blancas y `CourseAssignments` no se encuentra en la lista de inclusión.

Si ninguna comprobación cuadros estaban seleccionados, el código en `UpdateInstructorCourses` inicializa el `CourseAssignments` propiedad de navegación con una colección vacía y devuelve:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_UpdateCourses&highlight=3-7)]

A continuación, el código recorre en bucle todos los cursos en la base de datos y comprueba cada curso en los que está asignado actualmente al instructor frente a los que se han seleccionado en la vista. Para facilitar las búsquedas eficaces, las dos colecciones este últimas se almacenan en `HashSet` objetos.

Si se activó la casilla de verificación para un curso pero no se encuentra en el transcurso del `Instructor.CourseAssignments` propiedad de navegación, el curso se agrega a la colección en la propiedad de navegación.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=14-20&name=snippet_UpdateCourses)]

Si no se ha seleccionado la casilla de verificación para un curso, pero el curso está en el `Instructor.CourseAssignments` propiedad de navegación, el curso se quita de la propiedad de navegación.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=21-29&name=snippet_UpdateCourses)]

### <a name="update-the-instructor-views"></a>Actualizar las vistas de Instructor

En *Views/Instructors/Edit.cshtml*, agregar un **cursos** campo con una matriz de casillas de verificación agregando el siguiente código inmediatamente después de la `div` elementos para el **Office**  campo y antes de la `div` (elemento) para la **guardar** botón.

<a id="notepad"></a>
> [!NOTE] 
> Cuando pegue el código en Visual Studio, saltos de línea se cambiará de forma que el código se interrumpe.  Presione CTRL+z una vez para deshacer el formato automático.  Esto solucionará los saltos de línea para que se muestren como lo que ve aquí. La sangría no tiene que ser perfecto, pero la `@</tr><tr>`, `@:<td>`, `@:</td>`, y `@:</tr>` líneas deben estar en una sola línea tal como se muestra o se obtendrá un error en tiempo de ejecución. Con el bloque de código nuevo seleccionado, presione la tecla Tab tres veces para alinear el nuevo código con el código existente.

[!code-html[Main](intro/samples/cu/Views/Instructors/Edit.cshtml?range=35-61)]

Este código crea una tabla HTML que tiene tres columnas. En cada columna es una casilla de verificación seguida de un título que está formada por el número y el título. Las casillas de verificación todos tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que se van a tratar como un grupo. El atributo de valor de cada casilla de verificación se establece en el valor de `CourseID`. Cuando se envía la página, el enlazador de modelos pasa una matriz al controlador que está formada por el `CourseID` valores de las casillas de verificación que están seleccionadas.

Cuando inicialmente se representan las casillas de verificación, aquellos que por cursos asignados al instructor comprobaron atributos, que selecciona (muestra ellos activadas).

Ejecute la página de índice de Instructor y haga clic en **editar** en un instructor para ver el **editar** página.

![Página de edición de instructor con cursos](update-related-data/_static/instructor-edit-courses.png)

Cambiar algunos trabajos del curso y haga clic en Guardar. Los cambios que realice se reflejan en la página de índice.

> [!NOTE] 
> El enfoque adoptado aquí para editar datos de curso instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, una interfaz de usuario diferente y un método de actualización diferentes sería necesarios.

## <a name="update-the-delete-page"></a>Actualizar la página de borrado

En *InstructorsController.cs*, eliminar el `DeleteConfirmed` método e insertar el siguiente código en su lugar.

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?highlight=5-7,9-12&name=snippet_DeleteConfirmed)]

Este código realiza los cambios siguientes:

* Eager hace para cargar la `CourseAssignments` propiedad de navegación.  Tiene que incluir esto o EF no sabe todo sobre relacionados `CourseAssignment` entidades y no se eliminan.  Para evitar la necesidad de leerlos aquí puede configurar la eliminación en cascada en la base de datos.

* Si el instructor va a eliminar está asignado como administrador de los departamentos, quita la asignación de instructor de los departamentos.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Agregar ubicación de oficina y cursos en la página Crear

En *InstructorsController.cs*, elimine el HttpGet y HttpPost `Create` métodos y, a continuación, agregue el código siguiente en su lugar:

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Create&highlight=3-5,12,14-22,29)]

Este código es similar a los que ha visto el `Edit` métodos, excepto que inicialmente no cursos están seleccionadas. El HttpGet `Create` llamadas al método el `PopulateAssignedCourseData` método no porque puede haber cursos seleccionados pero en orden para proporcionar una colección vacía para el `foreach` bucle en la vista (en caso contrario, el código de vista podría producir una excepción de referencia nula).

El HttpPost `Create` método agrega cada curso seleccionado para la `CourseAssignments` propiedad de navegación antes de que se comprueba si hay errores de validación y agrega el instructor nuevo a la base de datos. Se agregan cursos incluso si hay errores del modelo para que cuando hay errores del modelo (por ejemplo, el usuario ordenado una fecha no válida) y se volverá a abrir la página con un mensaje de error, automáticamente se restauran todas las selecciones de curso que se realizaron.

Tenga en cuenta que para poder agregar cursos a los `CourseAssignments` propiedad de navegación se deben inicializar la propiedad como una colección vacía:

```csharp
instructor.CourseAssignments = new List<CourseAssignment>();
```

Como alternativa a hacerlo en el código del controlador, podría hacer en el modelo de Instructor cambiando el captador de propiedad para crear automáticamente la colección si no existe, como se muestra en el ejemplo siguiente:

```csharp
private ICollection<CourseAssignment> _courseAssignments;
public ICollection<CourseAssignment> CourseAssignments
{
    get
    {
        return _courseAssignments ?? (_courseAssignments = new List<CourseAssignment>());
    }
    set
    {
        _courseAssignments = value;
    }
}
```

Si modifica el `CourseAssignments` propiedad de esta manera, puede quitar el código de inicialización de propiedad explícitos en el controlador.

En *Views/Instructor/Create.cshtml*, agregue un cuadro de texto de la ubicación de office y casillas de verificación para cursos antes el botón Enviar. Como en el caso de la página Editar, [corrija el formato si Visual Studio vuelve a dar formato el código al pegarlo](#notepad).

[!code-html[Main](intro/samples/cu/Views/Instructors/Create.cshtml?range=29-61)]

Prueba ejecutando el **crear** página y la adición de un instructor. 

## <a name="handling-transactions"></a>Controlar transacciones

Como se explica en la [tutorial CRUD](crud.md), Entity Framework implementa de manera implícita las transacciones. Para escenarios donde necesita más control, por ejemplo, si van a incluir operaciones realizadas fuera de Entity Framework en una transacción, consulte [transacciones](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="summary"></a>Resumen

Ahora ha completado la introducción a trabajar con los datos relacionados. En el siguiente tutorial verá cómo tratar los conflictos de simultaneidad.

>[!div class="step-by-step"]
[Anterior](read-related-data.md)
[Siguiente](concurrency.md)  
