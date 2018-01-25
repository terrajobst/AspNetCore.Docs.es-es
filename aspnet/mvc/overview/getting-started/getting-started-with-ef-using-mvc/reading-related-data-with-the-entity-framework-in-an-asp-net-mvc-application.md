---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Leer datos relacionados con Entity Framework en una aplicación ASP.NET MVC | Documentos de Microsoft"
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7a74d01f306abeeac5ac28c942f03001e0fe00f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Lectura relacionadas con datos con Entity Framework en una aplicación ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descarga de PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio 2013. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En el tutorial anterior ha completado el modelo de datos School. En este tutorial podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework se carga en las propiedades de navegación.

Las ilustraciones siguientes muestran las páginas que trabajará con.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Carga diferida, diligente y explícita de los datos relacionados

Hay varias maneras que Entity Framework pueda cargar los datos relacionados en las propiedades de navegación de una entidad:

- *Carga diferida*. Cuando la entidad es de lectura en primer lugar, no recuperan datos relacionados. Sin embargo, la primera vez que intente acceder a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Esto da como resultado varias consultas que se envían a la base de datos: uno para la propia entidad y otro se debe recuperar cada vez que los datos de la entidad relacionados. La `DbContext` clase habilita la carga diferida de forma predeterminada. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carga diligente*. Cuando se lee la entidad, se recuperan los datos relacionados con él. Esto normalmente como resultado de una consulta de combinación única que recupera todos los datos que se necesitan. Carga diligente se especifica mediante el `Include` método.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carga explícita*. Esto es similar a la carga diferida, salvo que explícitamente recuperar los datos relacionados en el código. no sucede automáticamente cuando tiene acceso a una propiedad de navegación. Cargar datos relacionados manualmente obteniendo la entrada del Administrador de estado de objeto para una entidad y llamar a la [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) método para las colecciones o [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) método para propiedades que contienen un entidad única. (En el siguiente ejemplo, si desea cargar la propiedad de navegación del administrador, podría reemplazar `Collection(x => x.Courses)` con `Reference(x => x.Administrator)`.) Normalmente se usaría carga explícita cuando has activado la carga desactivar diferida.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Porque no recuperan inmediatamente los valores de propiedad, carga diferida y carga explícita también se las conoce como *carga aplazada*.

### <a name="performance-considerations"></a>Consideraciones sobre el rendimiento

Si sabe que necesita datos relacionados para cada entidad recuperar, a menudo carga diligente ofrece el mejor rendimiento, dado que una sola consulta que se envía a la base de datos es normalmente más eficaz que las consultas independientes para cada entidad recuperados. Por ejemplo, en los ejemplos anteriores, suponga que cada departamento tiene diez cursos relacionados. El ejemplo carga diligente se crearán simplemente una consulta sencilla (combinación) y un único viaje de ida y vuelta a la base de datos. La carga diferida y los ejemplos de carga explícita ambos produciría once consultas y once viajes de ida y vuelta a la base de datos. La ida y vuelta adicional a la base de datos son especialmente afecta negativamente al rendimiento cuando la latencia es alta.

Por otro lado, en algunos casos la carga diferida es más eficaz. Carga diligente puede provocar una combinación muy compleja para generarse, que SQL Server no puede procesar eficazmente. O bien, si necesita tener acceso a propiedades de navegación de una entidad solo para un subconjunto de un conjunto de las entidades que está procesando, la carga diferida dará mejores resultados porque carga diligente recuperaría más datos de los que necesita. Si el rendimiento es crítico, es mejor probar el rendimiento de ambas formas para elegir la mejor opción.

Carga diferida puede enmascarar el código que causa problemas de rendimiento. Por ejemplo, es posible que el código que no se especifica la carga diligente o explícita pero procesa un gran volumen de entidades y usa varias propiedades de navegación en cada iteración podría ser muy ineficaz (debido a muchos viajes de ida y la base de datos). Una aplicación que funciona perfectamente en desarrollo mediante una de servidor SQL server local podría tener problemas de rendimiento cuando se mueve a la base de datos de SQL Azure debido a la mayor latencia y la carga diferida. Generación de perfiles de las consultas de base de datos con una carga de prueba realistas le ayudará a determinar si la carga diferida es adecuada. Para obtener más información, consulte [Desmitificación de estrategias de Entity Framework: cargar datos relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) y [mediante Entity Framework para reducir la latencia de red a SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Deshabilitar la carga diferida antes de la serialización

Si deja la carga diferida habilitado durante la serialización, puede acabar consultar significativamente más datos de los previstos. Serialización generalmente funciona mediante el acceso a cada propiedad en una instancia de un tipo. Acceso a la propiedad desencadena la carga diferida y se serializan las entidades de carga diferidas. El proceso de serialización, a continuación, tiene acceso a cada propiedad de las entidades de carga diferida, lo que puede aún más la carga diferida y serialización. Para evitar esta reacción en cadena fuera de control, active la carga antes de serializar una entidad diferida.

Serialización también puede verse complicada por las clases de proxy que usa Entity Framework, como se explica en la [tutorial de escenarios avanzados](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Es una manera de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad, como se muestra en el [usar Web API con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) tutorial.

Si no utiliza dto, puede deshabilitar la carga diferida y evitar problemas de proxy por [deshabilitar la creación del proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Estos son algunos otros [maneras para deshabilitar la carga diferida](https://msdn.microsoft.com/data/jj574232):

- Para las propiedades de navegación específica, omita el `virtual` palabra clave cuando se declara la propiedad.
- Para todas las propiedades de navegación, establezca `LazyLoadingEnabled` a `false`, coloque el siguiente código en el constructor de la clase de contexto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a>Crear una página de cursos ese nombre de departamento de muestra

El `Course` entidad incluye una propiedad de navegación que contiene el `Department` entidad del departamento que el curso se asigna a. Para mostrar el nombre del departamento asignado en una lista de cursos, tendrá que obtener el `Name` propiedad desde el `Department` entidad que se encuentra en la `Course.Department` propiedad de navegación.

Crear un controlador denominado `CourseController` (no CoursesController) para la `Course` tipo de entidad, con las mismas opciones para la **controlador de MVC 5 con vistas, mediante Entity Framework** scaffolder que anteriormente se hizo la `Student` controlador, tal como se muestra en la siguiente ilustración:

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Abra *Controllers\CourseController.cs* y examine el `Index` método:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

La técnica scaffolding automática se especificado carga diligente de la `Department` propiedad de navegación mediante el uso de la `Include` método.

Abra *Views\Course\Index.cshtml* y reemplace el código de plantilla con el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Ha realizado los siguientes cambios en el código con scaffolding:

- Cambiar el título de **índice** a **cursos**.
- Agrega un **número** columna que muestra la `CourseID` valor de propiedad. De forma predeterminada, las claves principales no son scaffolding porque normalmente son importantes para los usuarios finales. Sin embargo, en este caso, la clave principal es significativa y desea mostrarla.
- Mover el **departamento** columna a la derecha y cambiar su encabezado. El scaffolder decidió mostrar correctamente el `Name` propiedad desde el `Department` entidad, pero aquí, en la página del curso el encabezado de columna debe ser **departamento** en lugar de **nombre**.

Tenga en cuenta que para la columna Department, se muestra el código con scaffolding el `Name` propiedad de la `Department` entidad que se carga en el `Department` propiedad de navegación:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Ejecute la página (seleccione la **cursos** pestaña en la página de inicio de la Universidad de Contoso) para ver la lista con los nombres de departamento.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a>Crear una página de instructores que muestra los cursos y las inscripciones

En esta sección creará un controlador y ver de la `Instructor` entidad con el fin de mostrar la página de instructores:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Esta página se lee y muestra los datos relacionados de las maneras siguientes:

- La lista de instructores muestra datos relacionados de la `OfficeAssignment` entidad. El `Instructor` y `OfficeAssignment` entidades se encuentran en una relación de uno a cero o uno. Deberá usar carga diligente de la `OfficeAssignment` entidades. Como se explicó anteriormente, carga diligente normalmente es más eficaz cuando necesite los datos relacionados para todas las filas recuperadas de la tabla principal. En este caso, desea mostrar las asignaciones de office para todos los instructores mostradas.
- Cuando el usuario selecciona un instructor, relacionados con `Course` se muestran las entidades. El `Instructor` y `Course` entidades se encuentran en una relación de varios a varios. Deberá usar carga diligente de la `Course` entidades y sus relacionados `Department` entidades. En este caso, la carga diferida podría ser más eficaz porque necesita cursos solo para el instructor seleccionado. Sin embargo, en este ejemplo se muestra cómo utilizar carga diligente para las propiedades de navegación dentro de las entidades que se encuentran a sí mismos en las propiedades de navegación.
- Cuando el usuario selecciona un curso, relacionadas con datos de la `Enrollments` se muestra el conjunto de entidades. El `Course` y `Enrollment` entidades se encuentran en una relación uno a varios. Agregará la carga explícita para `Enrollment` entidades y sus relacionados `Student` entidades. (Carga explícita no es necesaria porque está habilitada la carga diferida, pero esto muestra cómo realizar la carga explícita).

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Crear un modelo de vista para la vista de índice Instructor

La página de instructores muestra tres tablas diferentes. Por lo tanto, creará un modelo de vista que incluye tres propiedades, que contiene los datos de una de las tablas.

En el *ViewModels* carpeta, crear *InstructorIndexData.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Crear el controlador de Instructor y vistas

Crear un `InstructorController` (no InstructorsController) controlador con acciones de lectura/escritura EF tal como se muestra en la siguiente ilustración:

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Abra *Controllers\InstructorController.cs* y agregue un `using` instrucción para el `ViewModels` espacio de nombres:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

El código con scaffolding en el `Index` método especifica carga diligente solo para el `OfficeAssignment` propiedad de navegación:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Reemplace el `Index` método por el código siguiente para cargar adicionales relacionadas con datos y colóquelo en el modelo de vista:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

El método acepta datos de ruta opcional (`id`) y un parámetro de cadena de consulta (`courseID`) que proporcione los valores de Id. del instructor seleccionado y el curso seleccionado y pasa todos los datos necesarios a la vista. Los parámetros se proporcionan con el **seleccione** hipervínculos en la página.

El código comienza creando una instancia del modelo de vista y colocar en ella la lista de instructores. El código especifica una carga diligente de la `Instructor.OfficeAssignment` y `Instructor.Courses` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

El segundo `Include` método carga cursos y para cada curso que se ha cargado hace con carga diligente el `Course.Department` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Como se mencionó anteriormente, carga diligente no es necesaria, pero se hace para mejorar el rendimiento. Debido a que la vista siempre requiere el `OfficeAssignment` entidad, resulta más eficaz capturar que en la misma consulta. `Course`las entidades son necesarias cuando se selecciona un instructor en la página web, por lo que es mejor que la carga diferida solo si la página se muestra con más frecuencia con un curso seleccionado que sin la carga diligente.

Si se ha seleccionado un Id. de instructor, se recupera el instructor seleccionado en la lista de instructores en el modelo de vista. El modelo de vista `Courses` propiedad, a continuación, se carga con el `Course` entidades desde ese instructor `Courses` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El `Where` método devuelve una colección, pero en este caso los criterios se pasan a ese método generan solo una `Instructor` entidad que se devuelven. El `Single` método convierte la colección en una sola `Instructor` entidad, que proporciona acceso a esa entidad `Courses` propiedad.

Usa el [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) método en una colección cuando se sabe que la colección tiene un único elemento. El `Single` método produce una excepción si la colección pasada a la está vacía o si hay más de un elemento. Una alternativa es [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que devuelve un valor predeterminado (`null` en este caso) si la colección está vacía. Sin embargo, en este caso que todavía produciría una excepción (de tratar de buscar un `Courses` propiedad en un `null` referencia), y el mensaje de excepción menos claramente indicaría la causa del problema. Cuando se llama a la `Single` método, también puede pasar en el `Where` condición en lugar de llamar el `Where` método por separado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

En lugar de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

A continuación, si se ha seleccionado un curso, el curso seleccionado se recupera de la lista de cursos en el modelo de vista. A continuación, el modelo de vista `Enrollments` propiedad se carga con la `Enrollment` entidades desde dicho curso `Enrollments` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modificar la vista de índice Instructor

En *Views\Instructor\Index.cshtml*, reemplace el código de plantilla con el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Ha realizado los siguientes cambios en el código existente:

- Cambiar la clase de modelo para `InstructorIndexData`.
- Cambiar el título de la página de **índice** a **instructores**.
- Agrega un **Office** columna que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es null. (Dado que se trata de una relación de uno a cero o uno, no puede haber un relacionados `OfficeAssignment` entidad.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Código agregado que agregará dinámicamente `class="success"` a la `tr` elemento del instructor seleccionado. Esto establece el color de fondo de la fila seleccionada mediante una [arranque](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) clase. 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Agrega un nuevo `ActionLink` con la etiqueta **seleccione** inmediatamente antes de los otros vínculos en cada fila, lo que hace que el identificador de instructor seleccionado para enviarse a la `Index` método.

Ejecute la aplicación y seleccione la **instructores** ficha. La página muestra el `Location` propiedad de relacionados `OfficeAssignment` entidades y una tabla vacía de celda cuando hay no relacionados con `OfficeAssignment` entidad.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

En el *Views\Instructor\Index.cshtml* archivo, después del cierre `table` elemento (situado al final del archivo), agregue el código siguiente. Este código muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Este código lee el `Courses` propiedad del modelo de vista para mostrar una lista de cursos. También proporciona un `Select` hipervínculo que envía el Id. del curso seleccionado para la `Index` método de acción.

Ejecute la página y seleccione un instructor. Ahora verá una cuadrícula que muestra los cursos asignados al instructor seleccionado, y para cada curso verá el nombre del departamento asignado.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Después del bloque de código que acaba de agregar, agregue el código siguiente. Esto muestra una lista de los estudiantes que están inscritos en un curso cuando se selecciona dicho curso.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Este código lee el `Enrollments` propiedad del modelo de vista para mostrar una lista de los alumnos se inscribe en el curso.

Ejecute la página y seleccione un instructor. A continuación, seleccione un curso para ver la lista de estudiantes inscritos y sus calificaciones.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a>Agregar carga explícita

Abra *InstructorController.cs* y observe cómo el `Index` método obtiene la lista de las inscripciones para un curso seleccionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Al recuperar la lista de instructores, especifica una carga diligente de la `Courses` propiedad de navegación y para el `Department` propiedad de cada curso. A continuación, coloca el `Courses` colección en el modelo de vista, y ahora va a acceder a la `Enrollments` propiedad de navegación de una entidad de la colección. Dado que no especificó una carga diligente de la `Course.Enrollments` propiedad de navegación, los datos de dicha propiedad aparece en la página como resultado de la carga diferida.

Si ha deshabilitado la carga diferida sin cambiar el código de cualquier otra forma, el `Enrollments` propiedad sería null independientemente de cuántos inscripciones realmente tenía el curso. En ese caso, para cargar la `Enrollments` propiedad, tendría que especificar carga diligente o carga explícita. Ya ha visto cómo llevar a cabo la carga diligente. Para ver un ejemplo de carga explícita, reemplace la `Index` método con el código siguiente, que carga explícitamente el `Enrollments` propiedad. El código cambiado se resaltan.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Después de obtener seleccionado `Course` entidad, el nuevo código carga explícitamente ese curso `Enrollments` propiedad de navegación:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

A continuación, carga explícitamente cada `Enrollment` entidad relacionada del `Student` entidad:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Tenga en cuenta que usas el `Collection` método para cargar una propiedad de colección, pero para una propiedad que contiene una sola entidad, use la `Reference` método.

Ejecute ahora la página de índice de Instructor y no verá ninguna diferencia en lo que se muestra en la página, aunque ha cambiado la forma en que se recuperan los datos.

## <a name="summary"></a>Resumen

Ahora ha usado todas las tres maneras (diferidas, diligente y explícitas) para cargar los datos relacionados en las propiedades de navegación. En el siguiente tutorial, aprenderá cómo actualizar datos relacionados.

Vota sobre cómo le gustó este tutorial y lo que podemos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [ASP.NET Data Access: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[Siguiente](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
