---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: Lectura de datos relacionados con EF en una aplicación ASP.NET MVC'
description: En este tutorial podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework carga en las propiedades de navegación.
author: tdykstra
ms.author: riande
ms.date: 01/17/2019
ms.topic: tutorial
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8660a75655b801364cce7c4b59847c5c00562a27
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396212"
---
# <a name="tutorial-read-related-data-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: Lectura de datos relacionados con EF en una aplicación ASP.NET MVC

En el tutorial anterior se completó el modelo de datos School. En este tutorial podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework carga en las propiedades de navegación.

En las ilustraciones siguientes se muestran las páginas con las que va a trabajar.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

En este tutorial ha:

> [!div class="checklist"]
> * Obtenga información sobre cómo cargar datos relacionados
> * Crear una página de cursos
> * Crear una página de instructores

## <a name="prerequisites"></a>Requisitos previos

* [Crear un modelo de datos más complejo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="learn-how-to-load-related-data"></a>Obtenga información sobre cómo cargar datos relacionados

Hay varias maneras de que Entity Framework puede cargar datos relacionados en las propiedades de navegación de una entidad:

- *Carga diferida*. Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. Pero la primera vez que intente obtener acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Esto da como resultado varias consultas que se envían a la base de datos: uno para la propia entidad y otro cada vez que los datos de la entidad relacionados que se debe recuperar. La `DbContext` clase habilita la carga diferida de manera predeterminada.

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carga diligente*. Cuando se lee la entidad, junto a ella se recuperan datos relacionados. Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan. Carga diligente se especifica mediante el uso de la `Include` método.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carga explícita*. Esto es similar a la carga diferida, salvo que recupera explícitamente los datos relacionados en el código. no sucede automáticamente cuando tiene acceso a una propiedad de navegación. Cargar datos relacionados manualmente mediante la obtención de la entrada del Administrador de estado de objeto para una entidad y llamar a la [Collection.Load](https://msdn.microsoft.com/library/gg696220(v=vs.103).aspx) método para las colecciones o la [Reference.Load](https://msdn.microsoft.com/library/gg679166(v=vs.103).aspx) método para las propiedades que contienen un entidad única. (En el ejemplo siguiente, si desea cargar la propiedad de navegación del administrador, podría reemplazar `Collection(x => x.Courses)` con `Reference(x => x.Administrator)`.) Normalmente se usaría la carga explícita solo cuando se haya activado la carga desactivar diferida.

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Porque no recuperan inmediatamente los valores de propiedad, la carga diferida y la carga explícita son ambos conocen también como *la carga diferida*.

### <a name="performance-considerations"></a>Consideraciones sobre el rendimiento

Si sabe que necesita datos relacionados para cada entidad que se recupere, la carga diligente suele ofrecer el mejor rendimiento, dado que una sola consulta que se envía a la base de datos normalmente es más eficaz que consultas independientes para cada entidad recuperada. Por ejemplo, en los ejemplos anteriores, suponga que cada departamento tiene diez cursos relacionados. El ejemplo de la carga diligente daría como resultado en una consulta única (join) y un único viaje de ida y vuelta a la base de datos. La carga diferida y los ejemplos de la carga explícita ambos produciría once consultas y once ida y vuelta a la base de datos. Los recorridos de ida y vuelta adicionales a la base de datos afectan especialmente de forma negativa al rendimiento cuando la latencia es alta.

Por otro lado, en algunos escenarios de la carga diferida es más eficaz. La carga diligente podría provocar una combinación muy compleja para generarse, que SQL Server no puede procesar eficazmente. O bien, si es necesario tener acceso a las propiedades de navegación de una entidad solo para un subconjunto de un conjunto de las entidades que está procesando, la carga diferida puede funcionar mejor porque la carga diligente recuperaría más datos de los que necesita. Si el rendimiento es crítico, es mejor probarlo de ambas formas para elegir la mejor opción.

La carga diferida puede enmascarar el código que causa problemas de rendimiento. Por ejemplo, es posible que el código que no se especifica la carga diligente o explícita, pero procesa un gran volumen de entidades y usa varias propiedades de navegación en cada iteración puede ser muy ineficaz (debido a muchos ciclos de ida y la base de datos). Una aplicación que funcione bien en el desarrollo con un servidor SQL local podría tener problemas de rendimiento cuando se mueven a Azure SQL Database debido a la mayor latencia y la carga diferida. Las consultas de base de datos con una carga de prueba realista de generación de perfiles le ayudará a determinar si la carga diferida es adecuada. Para obtener más información, consulte [Desmitificación de estrategias de Entity Framework: Carga de datos relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) y [mediante Entity Framework para reducir la latencia de red a SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

### <a name="disable-lazy-loading-before-serialization"></a>Deshabilitar la carga diferida antes de la serialización

Si deja la carga diferida habilitada durante la serialización, puede acabar consultar datos significativamente más de lo esperado. Serialización generalmente funciona mediante el acceso a cada propiedad en una instancia de un tipo. Acceso a la propiedad desencadena la carga diferida, y se serializan las entidades de cargadas diferidas. El proceso de serialización, a continuación, tiene acceso a cada propiedad de las entidades de carga diferida, lo que puede producir aún más la carga diferida y la serialización. Para evitar esta reacción en cadena fuera de control, activar carga desactivado antes de serializar una entidad diferida.

Serialización también se complica por las clases de proxy que usa Entity Framework, como se explica en el [tutorial escenarios avanzados](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).

Es una forma de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad, como se muestra en el [usar Web API con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) tutorial.

Si no usa dto, puede deshabilitar la carga diferida y evitar problemas de proxy por [deshabilitar la creación de proxy](https://msdn.microsoft.com/data/jj592886.aspx).

Estos son algunos otros [formas de deshabilitar la carga diferida](https://msdn.microsoft.com/data/jj574232):

- Para las propiedades de navegación específicos, omita el `virtual` palabra clave cuando se declara la propiedad.
- Para todas las propiedades de navegación, establezca `LazyLoadingEnabled` a `false`, coloque el siguiente código en el constructor de la clase de contexto:

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page"></a>Crear una página de cursos

El `Course` entidad incluye una propiedad de navegación que contiene el `Department` entidad del departamento al que se asigna el curso. Para mostrar el nombre del departamento asignado en una lista de cursos, deberá obtener la `Name` propiedad desde la `Department` entidad que se encuentra en la `Course.Department` propiedad de navegación.

Crear un controlador denominado `CourseController` (no CoursesController) para el `Course` tipo de entidad, con las mismas opciones para la **controlador MVC 5 con vistas, usando Entity Framework** proveedor de scaffolding que usó anteriormente para el `Student` controlador:

| Parámetro | Valor |
| ------- | ----- |
| Clase de modelo | Seleccione **curso (ContosoUniversity.Models)**. |
| Clase de contexto de datos | Seleccione **SchoolContext (ContosoUniversity.DAL)**. |
| Nombre del controlador | Escriba *CourseController*. Nuevamente, no *CoursesController* con un *s*. Si seleccionó **curso (ContosoUniversity.Models)**, **nombre del controlador** valor se rellena automáticamente. Tendrá que cambiar el valor. |

Deje los demás valores predeterminados y agregue el controlador.

Abra *Controllers\CourseController.cs* y examine el `Index` método:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El scaffolding automático ha especificado la carga diligente para la propiedad de navegación `Department` mediante el método `Include`.

Abra *Views\Course\Index.cshtml* y reemplace el código de plantilla con el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

Ha realizado los cambios siguientes en el código con scaffolding:

- Ha cambiado el título de **índice** a **cursos**.
- Ha agregado una columna **Number** en la que se muestra el valor de propiedad `CourseID`. De forma predeterminada, las claves principales no tienen scaffolding porque normalmente son importantes para los usuarios finales. Pero en este caso, la clave principal es significativa y quiere mostrarla.
- Mueve el **departamento** columna a la derecha y cambiar su encabezado. El proveedor de scaffolding elegido mostrar correctamente el `Name` propiedad desde la `Department` entidad, pero aquí en la página del curso debe ser el encabezado de columna **departamento** en lugar de **nombre**.

Tenga en cuenta que para la columna Department, se muestra el código con scaffolding la `Name` propiedad de la `Department` entidad que se carga en el `Department` propiedad de navegación:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

Ejecute la página (seleccione la **cursos** pestaña en la página principal de Contoso University) para ver la lista con los nombres de departamento.

## <a name="create-an-instructors-page"></a>Crear una página de instructores

En esta sección creará un controlador y ver el `Instructor` entidad con el fin de mostrar la página de instructores. En esta página se leen y muestran los datos relacionados de las maneras siguientes:

- La lista de instructores muestra datos relacionados de la `OfficeAssignment` entidad. Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno. Usará la carga diligente para la `OfficeAssignment` entidades. Como se explicó anteriormente, la carga diligente normalmente es más eficaz cuando se necesitan los datos relacionados para todas las filas recuperadas de la tabla principal. En este caso, quiere mostrar las asignaciones de oficina para todos los instructores que se muestran.
- Cuando el usuario selecciona un instructor, relacionados con `Course` se muestran las entidades. Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios. Usará la carga diligente para la `Course` entidades y sus relacionados `Department` entidades. En este caso, la carga diferida podría ser más eficaz porque necesita cursos solo para el instructor seleccionado. Pero en este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación dentro de entidades que, a su vez, se encuentran en propiedades de navegación.
- Cuando el usuario selecciona un curso, relacionadas con datos desde el `Enrollments` se muestra el conjunto de entidades. Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios. Se agregará la carga explícita para `Enrollment` entidades y sus relacionados `Student` entidades. (Carga explícita no es necesaria porque la carga diferida está habilitada, pero aquí muestra cómo realizar la carga explícita.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Crear un modelo de vista para la vista de índice de instructores

En la página Instructors se muestra tres tablas diferentes. Por tanto, creará un modelo de vista que incluye tres propiedades, cada una con los datos de una de las tablas.

En el *ViewModels* carpeta, cree *InstructorIndexData.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a>Crear el controlador de Instructor y vistas

Crear un `InstructorController` (no InstructorsController) controlador con acciones de lectura/escritura EF:

| Parámetro | Valor |
| ------- | ----- |
| Clase de modelo | Seleccione **Instructor (ContosoUniversity.Models)**. |
| Clase de contexto de datos | Seleccione **SchoolContext (ContosoUniversity.DAL)**. |
| Nombre del controlador | Escriba *InstructorController*. Nuevamente, no *InstructorsController* con un *s*. Si seleccionó **curso (ContosoUniversity.Models)**, **nombre del controlador** valor se rellena automáticamente. Tendrá que cambiar el valor. |

Deje los demás valores predeterminados y agregue el controlador.

Abra *Controllers\InstructorController.cs* y agregue un `using` instrucción para el `ViewModels` espacio de nombres:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

El código con scaffolding en el `Index` método especifica la carga diligente solo para el `OfficeAssignment` propiedad de navegación:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Reemplace el `Index` método con el código siguiente para cargar adicionales relacionadas con datos y colóquelo en el modelo de vista:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

El método acepta datos de ruta opcionales (`id`) y un parámetro de cadena de consulta (`courseID`) que proporcione los valores de identificador del instructor y curso seleccionado y pasa todos los datos necesarios a la vista. Los parámetros se proporcionan mediante los hipervínculos **Select** de la página.

El código comienza creando una instancia del modelo de vista y coloca en ella la lista de instructores. El código especifica la carga diligente para la `Instructor.OfficeAssignment` y `Instructor.Courses` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

El segundo `Include` método carga cursos y para cada curso que se carga, lo hace la carga diligente la `Course.Department` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Como se mencionó anteriormente, la carga diligente no es necesaria, pero se realiza para mejorar el rendimiento. Puesto que la vista siempre requiere la `OfficeAssignment` entidad, resulta más eficaz capturarla en la misma consulta. `Course` las entidades son necesarias cuando se selecciona un instructor en la página web, por lo que es mejor que la carga diferida solo si se muestra la página más a menudo con un curso seleccionado que sin la carga diligente.

Si se ha seleccionado un Id. de instructor, se recupera el instructor seleccionado en la lista de instructores del modelo de vista. El modelo de vista `Courses` , a continuación, se carga la propiedad con el `Course` entidades de ese instructor `Courses` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El `Where` método devuelve una colección, pero en este caso los criterios que se pasan a ese método dan como resultado un solo `Instructor` entidad que se devuelven. El `Single` método convierte la colección en una sola `Instructor` entidad, que proporciona acceso a esa entidad `Courses` propiedad.

Usa el [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) método en una colección cuando se sabe que la colección tendrá un solo elemento. El `Single` método produce una excepción si la colección que se pasa está vacía o si hay más de un elemento. Una alternativa es [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que devuelve un valor predeterminado (`null` en este caso) si la colección está vacía. Sin embargo, en este caso eso seguiría iniciando una excepción (por tratar de encontrar un `Courses` propiedad en un `null` referencia), y el mensaje de excepción indicaría con menos claridad la causa del problema. Cuando se llama a la `Single` método, también puede pasar en el `Where` condición en lugar de llamar el `Where` método por separado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

En lugar de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

A continuación, si se ha seleccionado un curso, se recupera de la lista de cursos en el modelo de vista. A continuación, del modelo de vista `Enrollments` se carga la propiedad con el `Enrollment` entidades de ese curso `Enrollments` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a>Modificar la vista de índice de instructores

En *Views\Instructor\Index.cshtml*, reemplace el código de plantilla con el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

Ha realizado los cambios siguientes en el código existente:

- Ha cambiado la clase de modelo por `InstructorIndexData`.
- Ha cambiado el título de la página de **Index** a **Instructors**.
- Agrega un **Office** columna que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es null. (Porque se trata de una relación uno a cero o uno, puede que no exista un relacionados `OfficeAssignment` entity.)

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- Se ha agregado código que se agregará dinámicamente `class="success"` a la `tr` elemento del instructor seleccionado. Esto establece el color de fondo de la fila seleccionada mediante una [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) clase.

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- Agrega un nuevo `ActionLink` con la etiqueta **seleccione** inmediatamente antes de los otros vínculos de cada fila, lo que hace que el identificador del instructor seleccionado se envíen a la `Index` método.

Ejecute la aplicación y seleccione el **instructores** ficha. Muestra la página el `Location` propiedad de relacionados `OfficeAssignment` la celda cuando no hay entidades y una tabla vacía no relacionados con `OfficeAssignment` entidad.

En el *Views\Instructor\Index.cshtml* archivo después del cierre `table` elemento (al final del archivo), agregue el código siguiente. Este código muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Este código lee la propiedad `Courses` del modelo de vista para mostrar una lista de cursos. También proporciona un `Select` hipervínculo que envía el identificador del curso seleccionado a la `Index` método de acción.

Ejecute la página y seleccione un instructor. Ahora verá una cuadrícula en la que se muestran los cursos asignados al instructor seleccionado, y para cada curso, el nombre del departamento asignado.

Después del bloque de código que se acaba de agregar, agregue el código siguiente. Esto muestra una lista de los estudiantes que están inscritos en un curso cuando se selecciona ese curso.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Este código lee la `Enrollments` propiedad del modelo de vista para mostrar una lista de alumnos inscritos en el curso.

Ejecute la página y seleccione un instructor. Después, seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.

### <a name="adding-explicit-loading"></a>Adición de la carga explícita

Abra *InstructorController.cs* y mire la `Index` método obtiene la lista de las inscripciones para un curso seleccionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

Al recuperar la lista de instructores, especificó la carga diligente para la `Courses` propiedad de navegación y para el `Department` propiedad de cada curso. A continuación, coloca el `Courses` colección en el modelo de vista, y ahora que está accediendo a la `Enrollments` propiedad de navegación de una entidad de la colección. Dado que no ha especificado la carga diligente para la `Course.Enrollments` propiedad de navegación, los datos de dicha propiedad aparece en la página como resultado de la carga diferida.

Si deshabilita la carga diferida sin cambiar el código de cualquier otra forma, el `Enrollments` propiedad sería null independientemente de cuántas inscripciones tenía el curso. En ese caso, para cargar el `Enrollments` propiedad, tendría que especificar la carga diligente o carga explícita. Ya ha visto cómo realizar la carga diligente. Para ver un ejemplo de la carga explícita, reemplace el `Index` método con el código siguiente, que carga explícitamente la `Enrollments` propiedad. El código cambiado se resaltan.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

Después de obtener el texto seleccionado `Course` entidad, el nuevo código carga explícitamente ese curso `Enrollments` propiedad de navegación:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

Después, carga explícitamente cada `Enrollment` entidad relacionada con la `Student` entidad:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Tenga en cuenta que usa el `Collection` método para cargar una propiedad de colección, pero para una propiedad que contiene una sola entidad, usar el `Reference` método.

Ejecute la página de índice de instructores ahora y no verá ninguna diferencia en lo que se muestra en la página, aunque haya cambiado la forma en que se recuperan los datos.

## <a name="additional-resources"></a>Recursos adicionales

Pueden encontrar vínculos a otros recursos de Entity Framework en el [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Ha aprendido cómo cargar datos relacionados
> * Crea una página de cursos
> * Crea una página de instructores

Avance al siguiente artículo para obtener información sobre cómo actualizar datos relacionados.

> [!div class="nextstepaction"]
> [Actualización de datos relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)