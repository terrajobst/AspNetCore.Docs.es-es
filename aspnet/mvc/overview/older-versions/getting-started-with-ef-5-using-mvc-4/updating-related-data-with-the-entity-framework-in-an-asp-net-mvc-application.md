---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Actualizar datos relacionados con Entity Framework en una aplicación de ASP.NET MVC (6 de 10) | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 2ca76364a2e9a71dc92644bd579345ae3c304a69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Actualizar datos relacionados con Entity Framework en una aplicación de ASP.NET MVC (6 de 10)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empiece aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, vea [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior se muestran datos relacionados; en este tutorial, actualizará los datos relacionados. Para la mayoría de las relaciones, esto puede hacerse mediante la actualización de los campos de clave externos correspondientes. Para las relaciones de varios a varios, Entity Framework no ofrece la tabla de combinación directamente, por lo que debe agregar explícitamente y quitar entidades a y desde las propiedades de navegación adecuado.

Las ilustraciones siguientes muestran las páginas que trabajará con.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar la creación y edición de páginas para cursos

Cuando se crea una nueva entidad de curso, debe tener una relación con un departamento existente. Para facilitar esto, el código con scaffolding incluye métodos de controlador y crear y editar vistas que incluyen una lista desplegable para seleccionar el departamento. Los conjuntos de la lista desplegable el `Course.DepartmentID` propiedad de clave externa, y eso es todo lo que necesita de Entity Framework para cargar la `Department` propiedad de navegación con el adecuado `Department` entidad. Podrá usar el código con scaffolding, pero cambiar ligeramente para agregar el control de errores y ordenar la lista desplegable.

En *CourseController.cs*, elimine las cuatro `Edit` y `Create` métodos y reemplácelas con el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

El `PopulateDepartmentsDropDownList` /método siguiente obtiene una lista de todos los departamentos que se ordenan por nombre, se crea un `SelectList` colección para obtener una lista desplegable y pasa la colección a la vista en un `ViewBag` propiedad. El método acepta opcional `selectedDepartment` parámetro que permita al código que realiza la llamada especificar el elemento que se seleccionará cuando se procesa la lista desplegable. La vista pasará el nombre `DepartmentID` a [el `DropDownList` auxiliar](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), y, a continuación, sabe que la aplicación auxiliar debe para buscar en el `ViewBag` de objeto para un `SelectList` denominado `DepartmentID`.

El `HttpGet` `Create` llamadas al método el `PopulateDepartmentsDropDownList` método sin tener que configurar el elemento seleccionado, ya que para un nuevo curso el departamento aún no está establecido:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El `HttpGet` `Edit` método establece el elemento seleccionado, basándose en el identificador del departamento que ya está asignado a la línea que se está edita:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

El `HttpPost` métodos para ambos `Create` y `Edit` también incluir código que establece el elemento seleccionado cuando vuelva a mostrar la página después de un error:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Este código garantiza que cuando se vuelve a mostrar la página para mostrar el mensaje de error, se seleccionó cualquier departamento permanece seleccionada.

En *Views\Course\Create.cshtml*, agregue el código resaltado para crear un nuevo campo de número de curso antes de la **título** campo. Como se explica en un tutorial anterior, los campos de clave principal no están scaffolding de forma predeterminada, pero esta clave principal es significativa, por lo que desea que el usuario pueda especificar el valor de clave.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

En *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, y *Views\Course\Details.cshtml*, agregar un campo de número de curso antes de la **título**  campo. Dado que es la clave principal, se muestra, pero no se puede cambiar.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Ejecute el **crear** página (mostrar la página de índice de curso y haga clic en **crear nuevo**) y escriba los datos de un curso nuevo:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Haga clic en **Crear**. La página de índice de curso se muestra con el nuevo curso agregado a la lista. El nombre de departamento en la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Ejecute el **editar** página (mostrar la página de índice de curso y haga clic en **editar** en un curso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Cambiar los datos en la página y haga clic en **guardar**. La página de índice de curso se muestra con los datos de curso actualizado.

## <a name="adding-an-edit-page-for-instructors"></a>Agregar una página de edición para instructores

Cuando se edita un registro instructor, desea poder actualizar la asignación de office del instructor. El `Instructor` entidad tiene una relación de uno a cero o uno con el `OfficeAssignment` entidad, lo que significa que debe controlar las situaciones siguientes:

- Si el usuario borra la asignación de office y que tenía originalmente un valor, debe quitar y eliminar el `OfficeAssignment` entidad.
- Si el usuario especifica un valor de asignación de office y originalmente estaba vacío, debe crear un nuevo `OfficeAssignment` entidad.
- Si el usuario cambia el valor de una asignación de office, debe cambiar el valor en una existente `OfficeAssignment` entidad.

Abra *InstructorController.cs* y examine el `HttpGet` `Edit` método:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

El código con scaffolding aquí no es lo que desea. Está configurando los datos de una lista desplegable, pero lo que necesita es un cuadro de texto. Reemplace este método por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Este código quita la `ViewBag` instrucción y agrega carga diligente de asociado `OfficeAssignment` entidad. No se puede realizar una carga diligente con el `Find` método, por lo que la `Where` y `Single` métodos se usan en su lugar para seleccionar el instructor.

Reemplace el `HttpPost` `Edit` método por el código siguiente. que controla las actualizaciones de asignaciones de office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

El código hace lo siguiente:

- Obtiene el actual `Instructor` entidad desde la base de datos con carga diligente de la `OfficeAssignment` propiedad de navegación. Esto equivale a lo que hizo el `HttpGet` `Edit` método.
- Actualiza el objeto recuperado `Instructor` entidad con valores desde el enlazador de modelos. El [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) sobrecarga utiliza permite *lista blanca de direcciones* las propiedades que van a incluir. Esto evita que el registro excesivo, como se explica en [el segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Si la ubicación de la oficina está en blanco, Establece el `Instructor.OfficeAssignment` propiedad en null para que la fila relacionada en el `OfficeAssignment` se eliminará la tabla.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Guarda los cambios en la base de datos.

En *Views\Instructor\Edit.cshtml*, después la `div` elementos para la **fecha de contratación** campo, agregar un nuevo campo para modificar la ubicación de office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Ejecute la página (seleccione la **instructores** ficha y, a continuación, haga clic en **editar** en un instructor). Cambiar el **oficina** y haga clic en **guardar**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Agregar trabajos del curso para el Instructor y editar páginas

Instructores pueden enseñar a cualquier número de cursos. Ahora mejorará la página Editar Instructor agregando la capacidad de cambiar las asignaciones de curso mediante un grupo de casillas de verificación, tal y como se muestra en la siguiente captura de pantalla:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

La relación entre el `Course` y `Instructor` entidades es de varios a varios, lo que significa que no tiene acceso directo a la tabla de combinación. En su lugar, se agrega y quita las entidades a y desde el `Instructor.Courses` propiedad de navegación.

La interfaz de usuario que permite cambiar los cursos un instructor está asignado a es un grupo de casillas de verificación. Se muestra una casilla de verificación para cada curso en la base de datos y se seleccionan los que está asignado actualmente el instructor a. El usuario puede activar o desactivar casillas de verificación para cambiar las asignaciones de curso. Si el número de cursos era mucho mayor, probablemente desee utilizar un método diferente de presentar los datos en la vista, pero podría utilizar el mismo método de manipulación de propiedades de navegación con el fin de crear o eliminar relaciones.

Para proporcionar datos a la vista de la lista de casillas de verificación, deberá usar una clase de modelo de vista. Crear *AssignedCourseData.cs* en el *ViewModels* carpeta y reemplace el código existente por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

En *InstructorController.cs*, reemplace la `HttpGet` `Edit` método por el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

El código agrega carga diligente de la `Courses` propiedad de navegación y llama a la nueva `PopulateAssignedCourseData` método para proporcionar información de la matriz de casilla utilizando la `AssignedCourseData` Ver clase de modelo.

El código en el `PopulateAssignedCourseData` método lee a través de todos los `Course` clase de modelo de entidades para cargar una lista de cursos mediante la vista. Para cada curso, el código comprueba si existe el curso en el instructor `Courses` propiedad de navegación. Para crear una búsqueda eficaz al comprobar si se asigna un curso para el instructor, los cursos asignados al instructor se colocan en un [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) colección. El `Assigned` propiedad está establecida en `true` cursos está asignado el instructor. La vista utilizará esta propiedad para determinar qué Compruebe cuadros deben mostrarse como seleccionado. Por último, la lista se pasa a la vista en un `ViewBag` propiedad.

A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **guardar**. Reemplace el `HttpPost` `Edit` método con el código siguiente, que llama a un método nuevo que actualiza el `Courses` propiedad de navegación de la `Instructor` entidad. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Puesto que la vista no tiene una colección de `Course` entidades, el enlazador de modelos no se puede actualizar automáticamente el `Courses` propiedad de navegación. En lugar de usar el enlazador de modelos para actualizar la propiedad de navegación de cursos, lo hará en el nuevo `UpdateInstructorCourses` método. Por lo tanto, tendrá que excluir el `Courses` propiedad de enlace de modelos. Esto no requiere ningún cambio en el código que llama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque lo está utilizando la *crear listas blancas* sobrecarga y `Courses` no se encuentra en la lista de inclusión.

Si ninguna comprobación cuadros estaban seleccionados, el código en `UpdateInstructorCourses` inicializa el `Courses` propiedad de navegación con una colección vacía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

A continuación, el código recorre en bucle todos los cursos en la base de datos y comprueba cada curso en los que está asignado actualmente al instructor frente a los que se han seleccionado en la vista. Para facilitar las búsquedas eficaces, las dos colecciones este últimas se almacenan en `HashSet` objetos.

Si se activó la casilla de verificación para un curso pero no se encuentra en el transcurso del `Instructor.Courses` propiedad de navegación, el curso se agrega a la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Si no se ha seleccionado la casilla de verificación para un curso, pero el curso está en el `Instructor.Courses` propiedad de navegación, el curso se quita de la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

En *Views\Instructor\Edit.cshtml*, agregar un **cursos** campo con una matriz de casillas de verificación agregando las siguientes resaltadas de código inmediatamente después de la `div` elementos para el `OfficeAssignment` campo:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Este código crea una tabla HTML que tiene tres columnas. En cada columna es una casilla de verificación seguida de un título que está formada por el número y el título. Las casillas de verificación todos tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que se van a tratar como un grupo. El `value` atributo de cada casilla de verificación se establece en el valor de `CourseID.` cuando se envía la página, el enlazador de modelos pasa una matriz al controlador que está formada por el `CourseID` valores de las casillas de verificación que están seleccionadas.

Cuando inicialmente se representan las casillas de verificación, aquellos que por cursos asignados al instructor tienen `checked` atributos, que selecciona (muestra ellos activadas).

Después de cambiar los trabajos del curso, deseará poder comprobar los cambios cuando el sitio se vuelve a la `Index` página. Por lo tanto, debe agregar una columna a la tabla en esa página. En este caso no es necesario usar el `ViewBag` objeto, porque la información que desea mostrar ya está en el `Courses` propiedad de navegación de la `Instructor` entidad que se está pasando a la página que el modelo.

En *Views\Instructor\Index.cshtml*, agregue un **cursos** encabezado inmediatamente después de la **Office** encabezado, tal como se muestra en el ejemplo siguiente:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

A continuación, agregue una nueva celda de detalle inmediatamente después de la celda de detalle de la ubicación de office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Ejecute el **Instructor índice** página para ver los cursos asignados a cada instructor:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Haga clic en **editar** en un instructor para ver la página de edición.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Cambiar algunos trabajos del curso y haga clic en **guardar**. Los cambios que realice se reflejan en la página de índice.

 Nota: Al enfoque tomado para editar datos de curso instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, una interfaz de usuario diferente y un método de actualización diferentes sería necesarios.  
 

## <a name="update-the-delete-method"></a>El método Delete de actualización

Cambie el código en el método HttpPost Delete para que el registro de la asignación de office (si existe) se elimina cuando se elimina el instructor:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Si intenta eliminar un instructor que está asignado a un departamento como administrador, obtendrá un error de integridad referencial. Vea [la versión actual de este tutorial](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) para obtener código adicional que se quitará automáticamente el instructor de cualquier departamento donde se asigna el instructor como administrador.

## <a name="summary"></a>Resumen

Ahora ha completado esta introducción a trabajar con los datos relacionados. Hasta ahora en estos tutoriales hecho las operaciones de un intervalo de CRUD completa, pero no ha trabajado con problemas de simultaneidad. El siguiente tutorial se presente el tema de simultaneidad, explican las opciones para controlarlo y agregar el control al código CRUD que ya haya escrito para un tipo de entidad de simultaneidad.

Vínculos a otros recursos de Entity Framework, puede encontrarse al final de [del último tutorial de esta serie](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

>[!div class="step-by-step"]
[Anterior](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
