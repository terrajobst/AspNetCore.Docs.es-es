---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Actualización de datos relacionados con Entity Framework en una aplicación ASP.NET MVC (6 de 10) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e85162f58ed9826132db8bd854914a14709f709d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809438"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Actualización de datos relacionados con Entity Framework en una aplicación ASP.NET MVC (6 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio de este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y comience aquí.
> 
> > [!NOTE] 
> > 
> > Si surge un problema que no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior se muestran datos relacionados; en este tutorial, actualizará datos relacionados. Para la mayoría de las relaciones, esto puede hacerse mediante la actualización de los campos de clave externos correspondientes. Para las relaciones de varios a varios, Entity Framework no expone la tabla de combinación directamente, por lo que debe agregar explícitamente y quitar entidades hacia y desde las propiedades de navegación correspondientes.

En las ilustraciones siguientes se muestran las páginas con las que va a trabajar.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar las páginas Create y Edit de Courses

Cuando se crea una entidad de curso, debe tener una relación con un departamento existente. Para facilitar esto, el código con scaffolding incluye métodos de controlador y vistas de Create y Edit que incluyen una lista desplegable para seleccionar el departamento. La lista desplegable establece la `Course.DepartmentID` propiedad de clave externa, y eso es todo lo que necesita de Entity Framework con el fin de cargar el `Department` propiedad de navegación con los valores adecuados `Department` entidad. Podrá usar el código con scaffolding, pero cámbielo ligeramente para agregar el control de errores y ordenar la lista desplegable.

En *CourseController.cs*, elimine los cuatro `Edit` y `Create` métodos y reemplácelas con el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

El `PopulateDepartmentsDropDownList` método obtiene una lista de todos los departamentos ordenados por nombre, crea un `SelectList` colección para obtener una lista desplegable y pasa la colección a la vista en un `ViewBag` propiedad. El método acepta el parámetro opcional `selectedDepartment`, que permite al código que realiza la llamada especificar el elemento que se seleccionará cuando se procese la lista desplegable. La vista pasará el nombre `DepartmentID` a [el `DropDownList` auxiliar](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), y, a continuación, la aplicación auxiliar sabe para buscar en el `ViewBag` de objeto para un `SelectList` denominado `DepartmentID`.

El `HttpGet` `Create` llamadas al método el `PopulateDepartmentsDropDownList` método sin tener que establecer el elemento seleccionado, ya que para un nuevo curso al departamento aún no está establecido:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El `HttpGet` `Edit` método establece el elemento seleccionado, según el identificador del departamento que ya está asignado al curso que se está editando:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

El `HttpPost` métodos tanto para `Create` y `Edit` también incluye el código que establece el elemento seleccionado cuando vuelven a mostrar la página después de un error:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Este código garantiza que cuando se vuelve a mostrar la página para mostrar el mensaje de error, el departamento que se haya seleccionado permanece seleccionado.

En *Views\Course\Create.cshtml*, agregue el código resaltado para crear un nuevo campo de número de curso antes de la **título** campo. Como se explicó en un tutorial anterior, no se ha aplicado scaffolding a campos de clave principal de forma predeterminada, pero esta clave principal es significativa, por lo que desea que el usuario pueda escribir el valor de clave.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

En *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, y *Views\Course\Details.cshtml*, agregue un campo de número de curso antes de la **título**  campo. Dado que es la clave principal, se muestra, pero no se puede cambiar.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Ejecute el **crear** página (mostrar la página de índice de cursos y haga clic en **crear nuevo**) y escriba los datos de un nuevo curso:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Haga clic en **Crear**. Se muestra la página de índice de cursos con el nuevo curso agregado a la lista. El nombre de departamento de la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ejecute el **editar** página (mostrar la página de índice de cursos y haga clic en **editar** en un curso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Cambie los datos en la página y haga clic en **Save**. Se muestra la página de índice de cursos con los datos del curso actualizados.

## <a name="adding-an-edit-page-for-instructors"></a>Agregar una página de edición de instructores

Al editar un registro de instructor, necesita poder actualizar la asignación de la oficina del instructor. El `Instructor` entidad tiene una relación uno a cero o uno con el `OfficeAssignment` entidad, lo que significa que debe controlar las situaciones siguientes:

- Si el usuario desactiva la asignación de oficina y esta tenía originalmente un valor, debe quitar y eliminar el `OfficeAssignment` entidad.
- Si el usuario escribe un valor de asignación de oficina y originalmente estaba vacío, debe crear un nuevo `OfficeAssignment` entidad.
- Si el usuario cambia el valor de una asignación de oficina, debe cambiar el valor en una existente `OfficeAssignment` entidad.

Abra *InstructorController.cs* y examine el `HttpGet` `Edit` método:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

El código con scaffolding aquí no es lo que desea. Es la configuración de datos para una lista desplegable, pero lo que necesita es un cuadro de texto. Reemplace este método con el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Este código quita la `ViewBag` instrucción y agrega la carga diligente para la asociada `OfficeAssignment` entidad. No se puede realizar la carga diligente con el `Find` método, por lo que la `Where` y `Single` métodos se usan en su lugar para seleccionar el instructor.

Reemplace el `HttpPost` `Edit` método con el código siguiente. que controla las actualizaciones de asignación de office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

El código realiza lo siguiente:

- Obtiene la entidad `Instructor` actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`. Este es el mismo que hizo el `HttpGet` `Edit` método.
- Actualiza la entidad `Instructor` recuperada con valores del enlazador de modelos. El [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) sobrecarga utiliza permite *lista de permitidos de direcciones* las propiedades que van a incluir. Esto evita el registro excesivo, como se explica en [el segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Si la ubicación de la oficina está en blanco, Establece el `Instructor.OfficeAssignment` propiedad en null para que la fila relacionada en el `OfficeAssignment` se eliminará la tabla.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Guarda los cambios en la base de datos.

En *Views\Instructor\Edit.cshtml*, después el `div` elementos para el **fecha de contratación** campo, agregue un nuevo campo para editar la ubicación de la oficina:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Ejecute la página (seleccione la **instructores** pestaña y, a continuación, haga clic en **editar** en un instructor). Cambie el valor de **Office Location** y haga clic en **Save**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Agregar asignaciones de cursos para el Instructor edición página

Los instructores pueden impartir cualquier número de cursos. Ahora mejorará la página Edit Instructor al agregar la capacidad de cambiar las asignaciones de cursos mediante un grupo de casillas, tal y como se muestra en la siguiente captura de pantalla:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relación entre el `Course` y `Instructor` entidades es varios a varios, lo que significa que no tiene acceso directo a la tabla de combinación. En su lugar, agregue y quite entidades desde y hacia el `Instructor.Courses` propiedad de navegación.

La interfaz de usuario que le permite cambiar los cursos a los que está asignado un instructor es un grupo de casillas. Se muestra una casilla para cada curso en la base de datos y se seleccionan aquellos a los que está asignado actualmente el instructor. El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos. Si el número de cursos fuera mucho mayor, probablemente desee usar un método diferente de presentar los datos en la vista, pero usaría el mismo método de manipulación de las propiedades de navegación con el fin de crear o eliminar relaciones.

Para proporcionar datos a la vista de la lista de casillas, deberá usar una clase de modelo de vista. Crear *AssignedCourseData.cs* en el *ViewModels* carpeta y reemplace el código existente por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

En *InstructorController.cs*, reemplace el `HttpGet` `Edit` método con el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

El código agrega carga diligente para la propiedad de navegación `Courses` y llama al método `PopulateAssignedCourseData` nuevo para proporcionar información de la matriz de casilla mediante la clase de modelo de vista `AssignedCourseData`.

El código en el `PopulateAssignedCourseData` método lee a través de todos `Course` entidades con el fin de cargar una lista de cursos mediante la vista de clase de modelo. Para cada curso, el código comprueba si existe el curso en la propiedad de navegación `Courses` del instructor. Para crear una búsqueda eficaz al comprobar si un curso está asignado al instructor, los cursos asignados al instructor se colocan en un [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) colección. El `Assigned` propiedad está establecida en `true` para cursos el instructor está asignado. La vista usará esta propiedad para determinar qué casilla debe mostrarse como seleccionada. Por último, la lista se pasa a la vista en un `ViewBag` propiedad.

A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **Save**. Reemplace el `HttpPost` `Edit` método con el código siguiente, que llama a un método nuevo que actualiza el `Courses` propiedad de navegación de la `Instructor` entidad. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Puesto que la vista no tiene una colección de `Course` entidades, el enlazador de modelos no se puede actualizar automáticamente el `Courses` propiedad de navegación. En lugar de usar el enlazador de modelos para actualizar la propiedad de navegación de cursos, lo hará en el nuevo `UpdateInstructorCourses` método. Por lo tanto, tendrá que excluir la propiedad `Courses` del enlace de modelos. Esta opción no requiere ningún cambio en el código que llama a [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque está utilizando el *whitelisting* sobrecarga y `Courses` no se encuentra en la lista de inclusión.

Si ninguna comprobación se ha seleccionado, el código en `UpdateInstructorCourses` inicializa el `Courses` propiedad de navegación con una colección vacía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

A continuación, el código recorre en bucle todos los cursos de la base de datos y coteja los que están asignados actualmente al instructor frente a los que se han seleccionado en la vista. Para facilitar las búsquedas eficaces, estas dos últimas colecciones se almacenan en objetos `HashSet`.

Si se ha activado la casilla para un curso pero este no se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se agrega a la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Si no se ha activado la casilla para un curso pero este se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se quita de la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

En *Views\Instructor\Edit.cshtml*, agregue un **cursos** campo con una matriz de casillas de verificación agregando lo siguiente resaltadas código inmediatamente después la `div` elementos para el `OfficeAssignment` campo:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Este código crea una tabla HTML que tiene tres columnas. En cada columna hay una casilla seguida de un título que está formado por el número y el título del curso. Todas las casillas tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que están a tratarse como un grupo. El `value` de cada casilla de verificación está establecido en el valor de `CourseID.` cuando se envía la página, el enlazador de modelos pasa una matriz al controlador que consta de los `CourseID` los valores de las casillas de verificación que se han seleccionado.

Cuando inicialmente se representan las casillas de verificación, aquéllas que son para cursos asignados al instructor tienen `checked` atributos, que selecciona (las muestra activadas).

Después de cambiar las asignaciones de cursos, desea ser capaz de comprobar los cambios cuando el sitio vuelve a la `Index` página. Por lo tanto, deberá agregar una columna a la tabla en esa página. En este caso no es necesario utilizar el `ViewBag` objeto porque ya está en la información que desea mostrar el `Courses` propiedad de navegación de la `Instructor` entidad que está pasando a la página que el modelo.

En *Views\Instructor\Index.cshtml*, agregue un **cursos** encabezado inmediatamente después de la **Office** encabezado, tal como se muestra en el ejemplo siguiente:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

A continuación, agregue una nueva celda de detalle inmediatamente después de la celda de detalle de la ubicación de office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Ejecute el **índice de instructores** página para ver los cursos asignados a cada instructor:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Haga clic en **editar** en un instructor para ver la página de edición.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Cambie algunas asignaciones de cursos y haga clic en **guardar**. Los cambios que haga se reflejan en la página de índice.

 Nota: El enfoque adoptado para editar datos cursos del instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, se necesitaría una interfaz de usuario y un método de actualización diferentes.  
 

## <a name="update-the-delete-method"></a>Actualizar el método Delete

Cambie el código en el método HttpPost Delete para que el registro de asignación de office (si existe) se elimina cuando se elimine el instructor:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Si se intenta eliminar un instructor está asignado a un departamento como administrador, obtendrá un error de integridad referencial. Consulte [la versión actual de este tutorial](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) código adicional que se quitará automáticamente el instructor de cualquier departamento donde el instructor está asignado como administrador.

## <a name="summary"></a>Resumen

Ahora ha completado esta introducción al trabajar con datos relacionados. Hasta ahora en estos tutoriales que ha realizado una operación de intervalo completo de CRUD, pero aún no ha trabajado con problemas de simultaneidad. El siguiente tutorial incluirá el tema de simultaneidad, explica las opciones para el manejo y agregue el código CRUD que ya ha escrito para un tipo de entidad de control de simultaneidad.

Vínculos a otros recursos de Entity Framework, puede encontrarse al final de [el último tutorial de esta serie](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Anterior](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
