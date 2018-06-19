---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Actualizar datos relacionados con Entity Framework en una aplicación ASP.NET MVC | Documentos de Microsoft
author: tdykstra
description: La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cf4a6183e068e8668eb706d9a9e311616649e863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875622"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Actualizar datos relacionados con Entity Framework en una aplicación ASP.NET MVC
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descarga de PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio 2013. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En el tutorial anterior se muestran datos relacionados; en este tutorial, actualizará los datos relacionados. Para la mayoría de las relaciones, esto puede hacerse mediante la actualización de campos de clave externa o propiedades de navegación. Para las relaciones de varios a varios, Entity Framework no ofrece la tabla de combinación directamente, por lo que agregar y quitar entidades a y desde las propiedades de navegación adecuado.

En las ilustraciones siguientes se muestran algunas de las páginas con las que va a trabajar.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Edición de instructor con cursos](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Personalizar las páginas Create y Edit de Courses

Cuando se crea una entidad de curso, debe tener una relación con un departamento existente. Para facilitar esto, el código con scaffolding incluye métodos de controlador y vistas de Create y Edit que incluyen una lista desplegable para seleccionar el departamento. Los conjuntos de la lista desplegable el `Course.DepartmentID` propiedad de clave externa, y eso es todo lo que necesita de Entity Framework para cargar la `Department` propiedad de navegación con el adecuado `Department` entidad. Podrá usar el código con scaffolding, pero cámbielo ligeramente para agregar el control de errores y ordenar la lista desplegable.

En *CourseController.cs*, elimine las cuatro `Create` y `Edit` métodos y reemplácelas con el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Agregue las siguientes `using` instrucción al principio del archivo:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El `PopulateDepartmentsDropDownList` /método siguiente obtiene una lista de todos los departamentos que se ordenan por nombre, se crea un `SelectList` colección para obtener una lista desplegable y pasa la colección a la vista en un `ViewBag` propiedad. El método acepta el parámetro opcional `selectedDepartment`, que permite al código que realiza la llamada especificar el elemento que se seleccionará cuando se procese la lista desplegable. La vista pasará el nombre `DepartmentID` a la [DropDownList](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) aplicación auxiliar y la aplicación auxiliar, a continuación, sepa que puede para buscar en el `ViewBag` de objeto para un `SelectList` denominado `DepartmentID`.

El `HttpGet` `Create` llamadas al método el `PopulateDepartmentsDropDownList` método sin tener que configurar el elemento seleccionado, ya que para un nuevo curso el departamento aún no está establecido:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

El `HttpGet` `Edit` método establece el elemento seleccionado, basándose en el identificador del departamento que ya está asignado a la línea que se está edita:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

El `HttpPost` métodos para ambos `Create` y `Edit` también incluir código que establece el elemento seleccionado cuando vuelva a mostrar la página después de un error:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Este código garantiza que cuando se vuelve a mostrar la página para mostrar el mensaje de error, se seleccionó cualquier departamento permanece seleccionada.

Las vistas de curso ya están scaffolding con listas desplegables para el campo de departamento, pero no desea el título DepartmentID para este campo, por lo que asegúrese de resaltado a continuación cambiar a la *Views\Course\Create.cshtml* del archivo a cambiar el título.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Realizar el mismo cambio en *Views\Course\Edit.cshtml*.

Normalmente el scaffolder no aplicar la técnica scaffolding una clave principal porque el valor de clave generado por la base de datos y no se puede cambiar y no es un valor significativo que se mostrará a los usuarios. Para las entidades de curso el scaffolder incluyen un cuadro de texto para el `CourseID` campo porque entiende que el `DatabaseGeneratedOption.None` atributo significa que el usuario debe ser capaz de escribir el valor de clave principal. Pero no comprende que porque el número es significativo desea ver en las otras vistas, por ello, debe agregarla manualmente.

En *Views\Course\Edit.cshtml*, agregar un campo de número de curso antes de la **título** campo. Dado que es la clave principal, se muestra, pero no se puede cambiar.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Ya hay un campo oculto (`Html.HiddenFor` auxiliar) para el número de curso en la vista de edición. Agregar un *Html.LabelFor* auxiliar no elimina la necesidad de para el campo oculto porque no hacer que el número de curso que se incluirá en los datos enviados cuando el usuario hace clic en **guardar** en la página de edición.

En *Views\Course\Delete.cshtml* y *Views\Course\Details.cshtml*, cambiar el título del nombre de departamento de "Nombre" a "Departamento" y agregar un campo de número de curso antes de la **título**  campo.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Ejecute el **crear** página (mostrar la página de índice de curso y haga clic en **crear nuevo**) y escriba los datos de un curso nuevo:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Haga clic en **Crear**. La página de índice de curso se muestra con el nuevo curso agregado a la lista. El nombre de departamento de la lista de páginas de índice proviene de la propiedad de navegación, que muestra que la relación se estableció correctamente.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Ejecute el **editar** página (mostrar la página de índice de curso y haga clic en **editar** en un curso).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Cambie los datos en la página y haga clic en **Save**. La página de índice de curso se muestra con los datos de curso actualizado.

## <a name="adding-an-edit-page-for-instructors"></a>Agregar una página de edición para instructores

Al editar un registro de instructor, necesita poder actualizar la asignación de la oficina del instructor. El `Instructor` entidad tiene una relación de uno a cero o uno con el `OfficeAssignment` entidad, lo que significa que debe controlar las situaciones siguientes:

- Si el usuario borra la asignación de office y que tenía originalmente un valor, debe quitar y eliminar el `OfficeAssignment` entidad.
- Si el usuario especifica un valor de asignación de office y originalmente estaba vacío, debe crear un nuevo `OfficeAssignment` entidad.
- Si el usuario cambia el valor de una asignación de office, debe cambiar el valor en una existente `OfficeAssignment` entidad.

Abra *InstructorController.cs* y examine el `HttpGet` `Edit` método:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

El código con scaffolding aquí no es lo que desea. Está configurando los datos de una lista desplegable, pero lo que necesita es un cuadro de texto. Reemplace este método por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Este código quita la `ViewBag` instrucción y agrega carga diligente de asociado `OfficeAssignment` entidad. No se puede realizar una carga diligente con el `Find` método, por lo que la `Where` y `Single` métodos se usan en su lugar para seleccionar el instructor.

Reemplace el `HttpPost` `Edit` método por el código siguiente. que controla las actualizaciones de asignaciones de office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

La referencia a `RetryLimitExceededException` requiere un `using` instrucción; para agregarlo, haga clic en `RetryLimitExceededException`y, a continuación, haga clic en **resolver** - **con System.Data.Entity.Infrastructure**.

![Resolver la excepción de reintento](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

El código realiza lo siguiente:

- Cambia el nombre del método para `EditPost` porque la firma ahora es el mismo que el `HttpGet` método (el `ActionName` atributo especifica que todavía se usa la dirección URL/Edit /).
- Obtiene la entidad `Instructor` actual de la base de datos mediante la carga diligente de la propiedad de navegación `OfficeAssignment`. Esto equivale a lo que hizo el `HttpGet` `Edit` método.
- Actualiza la entidad `Instructor` recuperada con valores del enlazador de modelos. El [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) sobrecarga utiliza permite *lista de permitidos de direcciones* las propiedades que van a incluir. Esto evita que el registro excesivo, como se explica en [el segundo tutorial](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Si la ubicación de la oficina está en blanco, Establece el `Instructor.OfficeAssignment` propiedad en null para que la fila relacionada en el `OfficeAssignment` se eliminará la tabla.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Guarda los cambios en la base de datos.

En *Views\Instructor\Edit.cshtml*, después la `div` elementos para la **fecha de contratación** campo, agregar un nuevo campo para modificar la ubicación de office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Ejecute la página (seleccione la **instructores** ficha y, a continuación, haga clic en **editar** en un instructor). Cambie el valor de **Office Location** y haga clic en **Save**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Agregar trabajos del curso para el Instructor y editar páginas

Los instructores pueden impartir cualquier número de cursos. Ahora mejorará la página Edit Instructor al agregar la capacidad de cambiar las asignaciones de cursos mediante un grupo de casillas, tal y como se muestra en la siguiente captura de pantalla:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

La relación entre el `Course` y `Instructor` entidades es de varios a varios, lo que significa que no tiene acceso directo a las propiedades de clave externas que se encuentran en la tabla de combinación. En su lugar, agregar y quitar entidades hacia y desde el `Instructor.Courses` propiedad de navegación.

La interfaz de usuario que le permite cambiar los cursos a los que está asignado un instructor es un grupo de casillas. Se muestra una casilla para cada curso en la base de datos y se seleccionan aquellos a los que está asignado actualmente el instructor. El usuario puede activar o desactivar las casillas para cambiar las asignaciones de cursos. Si el número de cursos era mucho mayor, probablemente desee utilizar un método diferente de presentar los datos en la vista, pero podría utilizar el mismo método de manipulación de propiedades de navegación con el fin de crear o eliminar relaciones.

Para proporcionar datos a la vista de la lista de casillas, deberá usar una clase de modelo de vista. Crear *AssignedCourseData.cs* en el *ViewModels* carpeta y reemplace el código existente por el código siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

En *InstructorController.cs*, reemplace la `HttpGet` `Edit` método por el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

El código agrega carga diligente para la propiedad de navegación `Courses` y llama al método `PopulateAssignedCourseData` nuevo para proporcionar información de la matriz de casilla mediante la clase de modelo de vista `AssignedCourseData`.

El código en el `PopulateAssignedCourseData` método lee a través de todos los `Course` clase de modelo de entidades para cargar una lista de cursos mediante la vista. Para cada curso, el código comprueba si existe el curso en la propiedad de navegación `Courses` del instructor. Para crear una búsqueda eficaz al comprobar si se asigna un curso para el instructor, los cursos asignados al instructor se colocan en un [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) colección. El `Assigned` propiedad está establecida en `true` cursos está asignado el instructor. La vista usará esta propiedad para determinar qué casilla debe mostrarse como seleccionada. Por último, la lista se pasa a la vista en un `ViewBag` propiedad.

A continuación, agregue el código que se ejecuta cuando el usuario hace clic en **Save**. Reemplace el `EditPost` método con el código siguiente, que llama a un método nuevo que actualiza el `Courses` propiedad de navegación de la `Instructor` entidad. Los cambios aparecen resaltados.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

La firma de método ahora es diferente de la `HttpGet` `Edit` método, por lo que cambia el nombre del método de `EditPost` a `Edit`.

Puesto que la vista no tiene una colección de `Course` entidades, el enlazador de modelos no se puede actualizar automáticamente el `Courses` propiedad de navegación. En lugar de usar el enlazador de modelos para actualizar la `Courses` propiedad de navegación, llevará a cabo en el nuevo `UpdateInstructorCourses` método. Por lo tanto, tendrá que excluir la propiedad `Courses` del enlace de modelos. Esto no requiere ningún cambio en el código que llama [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) porque lo está utilizando la *crear listas blancas* sobrecarga y `Courses` no se encuentra en la lista de inclusión.

Si ninguna comprobación cuadros estaban seleccionados, el código en `UpdateInstructorCourses` inicializa el `Courses` propiedad de navegación con una colección vacía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

A continuación, el código recorre en bucle todos los cursos de la base de datos y coteja los que están asignados actualmente al instructor frente a los que se han seleccionado en la vista. Para facilitar las búsquedas eficaces, estas dos últimas colecciones se almacenan en objetos `HashSet`.

Si se ha activado la casilla para un curso pero este no se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se agrega a la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Si no se ha activado la casilla para un curso pero este se encuentra en la propiedad de navegación `Instructor.Courses`, el curso se quita de la colección en la propiedad de navegación.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

En *Views\Instructor\Edit.cshtml*, agregar un **cursos** campo con una matriz de casillas de verificación agregando el siguiente código inmediatamente después de la `div` elementos para el `OfficeAssignment` campo y antes de la `div` (elemento) para la **guardar** botón:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Después de pegar el código, si los saltos de línea y la sangría no tengan el mismo aspecto hacen aquí, corregir manualmente todos los elementos para que se asemeje al igual que lo que ve aquí. No es necesario que la sangría sea perfecta, pero las líneas `@</tr><tr>`, `@:<td>`, `@:</td>` y `@</tr>` deben estar en una única línea tal y como se muestra, de lo contrario, obtendrá un error en tiempo de ejecución.

Este código crea una tabla HTML que tiene tres columnas. En cada columna hay una casilla seguida de un título que está formado por el número y el título del curso. Las casillas de verificación todos tienen el mismo nombre ("selectedCourses"), que informa al enlazador de modelos que se van a tratar como un grupo. El `value` atributo de cada casilla de verificación se establece en el valor de `CourseID.` cuando se envía la página, el enlazador de modelos pasa una matriz al controlador que está formada por el `CourseID` valores de las casillas de verificación que están seleccionadas.

Cuando inicialmente se representan las casillas de verificación, aquellos que por cursos asignados al instructor tienen `checked` atributos, que selecciona (muestra ellos activadas).

Después de cambiar los trabajos del curso, deseará poder comprobar los cambios cuando el sitio se vuelve a la `Index` página. Por lo tanto, debe agregar una columna a la tabla en esa página. En este caso no es necesario usar el `ViewBag` objeto, porque la información que desea mostrar ya está en el `Courses` propiedad de navegación de la `Instructor` entidad que se está pasando a la página que el modelo.

En *Views\Instructor\Index.cshtml*, agregue un **cursos** encabezado inmediatamente después de la **Office** encabezado, tal como se muestra en el ejemplo siguiente:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

A continuación, agregue una nueva celda de detalle inmediatamente después de la celda de detalle de la ubicación de office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Ejecute el **Instructor índice** página para ver los cursos asignados a cada instructor:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Haga clic en **editar** en un instructor para ver la página de edición.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Cambiar algunos trabajos del curso y haga clic en **guardar**. Los cambios que haga se reflejan en la página de índice.

 Nota: Al enfoque tomado aquí para editar datos de curso instructor funciona bien cuando hay un número limitado de cursos. Para las colecciones que son mucho más grandes, se necesitaría una interfaz de usuario y un método de actualización diferentes.  
 

## <a name="update-the-deleteconfirmed-method"></a>Actualizar el método DeleteConfirmed

En *InstructorController.cs*, eliminar el `DeleteConfirmed` método e insertar el siguiente código en su lugar.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Este código realiza el cambio siguiente:

- Si se asigna el instructor como administrador de cualquier departamento, quita la asignación de instructor de ese departamento. Sin este código, obtendrá un error de integridad referencial si ha intentado eliminar un instructor que se ha asignado como administrador de un departamento.

Este código no controla el escenario de un instructor asignado como administrador para varios departamentos. En el último tutorial agregará código que impide que ese escenario de elementos no utilizados.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Agregar la ubicación de la oficina y cursos a la página Create

En *InstructorController.cs*, eliminar el `HttpGet` y `HttpPost` `Create` métodos y, a continuación, agregue el código siguiente en su lugar:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Este código es similar a lo que ha visto para los métodos de edición salvo que inicialmente no se seleccionan ningún courses. El `HttpGet` `Create` llamadas al método el `PopulateAssignedCourseData` método no porque puede haber cursos seleccionados pero en orden para proporcionar una colección vacía para el `foreach` bucle en la vista (en caso contrario, el código de vista podría producir una excepción de referencia nula ).

El método Create HttpPost agrega cada curso seleccionado a la propiedad de navegación de cursos antes del código de plantilla que comprueba si hay errores de validación y agrega el instructor nuevo a la base de datos. Se agregan cursos incluso si hay errores de modelo para que cuando hay errores de modelo (por ejemplo, el usuario ordenado una fecha no válida) para que cuando se vuelve a mostrar la página con un mensaje de error, automáticamente se restauran todas las selecciones de curso que se realizaron.

Tenga en cuenta que, para poder agregar cursos a la propiedad de navegación `Courses`, debe inicializar la propiedad como una colección vacía:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Como alternativa a hacerlo en el código de control, podría hacerlo en el modelo de Instructor cambiando el captador de propiedad para que cree automáticamente la colección si no existe, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Si modifica la propiedad `Courses` de esta manera, puede quitar el código de inicialización de propiedad explícito del controlador.

En *Views\Instructor\Create.cshtml*, agregue un cuadro de texto de la ubicación de office y casillas de verificación del curso después de la contratación de campo de fecha y antes de la **enviar** botón.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Después de pegar el código, soluciona los saltos de línea y sangría como lo hizo anteriormente para la página de edición.

Ejecute la página Crear y agregar un instructor.

![Crear instructor con cursos](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Control de transacciones

Como se explica en la [tutorial de la funcionalidad básica de CRUD](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), de forma predeterminada el Entity Framework implementa de manera implícita las transacciones. Para escenarios donde necesita más control, por ejemplo, si van a incluir operaciones realizadas fuera de Entity Framework en una transacción, consulte [trabajar con transacciones](https://msdn.microsoft.com/data/dn456843) en MSDN.

## <a name="summary"></a>Resumen

Ahora ha completado esta introducción a trabajar con los datos relacionados. Hasta ahora en estos tutoriales que ha trabajado con código que realiza la E/S sincrónica. Puede hacer que la aplicación usar recursos de servidor web de forma más eficaz mediante la implementación de código asincrónico, y eso es lo que hará en el tutorial siguiente.

Vota sobre cómo le gustó este tutorial y lo que podemos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vínculos a otros recursos de Entity Framework se pueden encontrar en [ASP.NET Data Access: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Siguiente](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
