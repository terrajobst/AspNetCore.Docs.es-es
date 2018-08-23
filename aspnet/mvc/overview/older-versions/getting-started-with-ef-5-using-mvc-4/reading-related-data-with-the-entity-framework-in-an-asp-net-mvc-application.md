---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Leer datos relacionados con Entity Framework en una aplicación ASP.NET MVC (5 de 10) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4767b015db0bad09942802827ce54162687fcabc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836285"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a>Lectura de datos relacionados con Entity Framework en una aplicación ASP.NET MVC (5 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio de este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y comience aquí.
> 
> > [!NOTE] 
> > 
> > Si surge un problema que no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior se completó el modelo de datos School. En este tutorial podrá leer y mostrar datos relacionados, es decir, los datos que Entity Framework carga en las propiedades de navegación.

En las ilustraciones siguientes se muestran las páginas con las que va a trabajar.

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a>Carga diligente, explícita y diferida de los datos relacionados

Hay varias maneras de que Entity Framework puede cargar datos relacionados en las propiedades de navegación de una entidad:

- *Carga diferida*. Cuando la entidad se lee por primera vez, no se recuperan datos relacionados. Pero la primera vez que intente obtener acceso a una propiedad de navegación, se recuperan automáticamente los datos necesarios para esa propiedad de navegación. Esto da como resultado varias consultas que se envían a la base de datos: uno para la propia entidad y otro cada vez que los datos de la entidad relacionados que se debe recuperar. 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- *Carga diligente*. Cuando se lee la entidad, junto a ella se recuperan datos relacionados. Esto normalmente da como resultado una única consulta de combinación en la que se recuperan todos los datos que se necesitan. Carga diligente se especifica mediante el uso de la `Include` método.

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- *Carga explícita*. Esto es similar a la carga diferida, salvo que recupera explícitamente los datos relacionados en el código. no sucede automáticamente cuando tiene acceso a una propiedad de navegación. Cargar datos relacionados manualmente mediante la obtención de la entrada del Administrador de estado de objeto para una entidad y llamar a la `Collection.Load` método para las colecciones o la `Reference.Load` método para las propiedades que contienen una sola entidad. (En el ejemplo siguiente, si desea cargar la propiedad de navegación del administrador, podría reemplazar `Collection(x => x.Courses)` con `Reference(x => x.Administrator)`.)

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

Porque no recuperan inmediatamente los valores de propiedad, la carga diferida y la carga explícita son ambos conocen también como *la carga diferida*.

En general, si sabe que necesita datos relacionados para cada entidad de la carga diligente, recuperada ofrece el mejor rendimiento, porque una consulta única enviada a la base de datos es normalmente más eficaz que consultas independientes para cada entidad recuperada. Por ejemplo, en los ejemplos anteriores, suponga que cada departamento tiene diez cursos relacionados. El ejemplo de la carga diligente daría como resultado en una consulta única (join) y un único viaje de ida y vuelta a la base de datos. La carga diferida y los ejemplos de la carga explícita ambos produciría once consultas y once ida y vuelta a la base de datos. Los recorridos de ida y vuelta adicionales a la base de datos afectan especialmente de forma negativa al rendimiento cuando la latencia es alta.

Por otro lado, en algunos escenarios de la carga diferida es más eficaz. La carga diligente podría provocar una combinación muy compleja para generarse, que SQL Server no puede procesar eficazmente. O bien, si necesita tener acceso a las propiedades de navegación de una entidad solo para un subconjunto de un conjunto de entidades que está procesando, la carga diferida puede funcionar mejor porque la carga diligente recuperaría más datos de los que necesita. Si el rendimiento es crítico, es mejor probarlo de ambas formas para elegir la mejor opción.

Normalmente se usaría la carga explícita solo cuando se haya activado la carga desactivar diferida. Es un escenario cuando debe activar carga desactivar diferida durante la serialización. Serialización y la carga diferida no mezclan bien y, si no tiene cuidado que puede acabar consultar datos significativamente más de lo esperado cuando diferida está habilitada la carga. Serialización generalmente funciona mediante el acceso a cada propiedad en una instancia de un tipo. Acceso a la propiedad desencadena la carga diferida, y se serializan las entidades de cargadas diferidas. El proceso de serialización, a continuación, tiene acceso a cada propiedad de las entidades de carga diferida, lo que puede producir aún más la carga diferida y la serialización. Para evitar esta reacción en cadena fuera de control, activar carga desactivado antes de serializar una entidad diferida.

La clase de contexto de base de datos realiza la carga diferida de manera predeterminada. Hay dos formas de deshabilitar la carga diferida:

- Para las propiedades de navegación específicos, omita el `virtual` palabra clave cuando se declara la propiedad.
- Para todas las propiedades de navegación, establezca `LazyLoadingEnabled` a `false`. Por ejemplo, puede colocar el código siguiente en el constructor de la clase de contexto: 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

La carga diferida puede enmascarar el código que causa problemas de rendimiento. Por ejemplo, es posible que el código que no se especifica la carga diligente o explícita, pero procesa un gran volumen de entidades y usa varias propiedades de navegación en cada iteración puede ser muy ineficaz (debido a muchos ciclos de ida y la base de datos). Una aplicación que funcione bien en el desarrollo con un servidor SQL local podría tener problemas de rendimiento cuando se mueven a Azure SQL Database debido a la mayor latencia y la carga diferida. Las consultas de base de datos con una carga de prueba realista de generación de perfiles le ayudará a determinar si la carga diferida es adecuada. Para obtener más información, consulte [Desmitificación de estrategias de Entity Framework: cargar datos relacionados](https://msdn.microsoft.com/magazine/hh205756.aspx) y [mediante Entity Framework para reducir la latencia de red con SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).

## <a name="create-a-courses-index-page-that-displays-department-name"></a>Crear una página de índice de cursos de ese nombre de departamento de muestra

El `Course` entidad incluye una propiedad de navegación que contiene el `Department` entidad del departamento al que se asigna el curso. Para mostrar el nombre del departamento asignado en una lista de cursos, deberá obtener la `Name` propiedad desde la `Department` entidad que se encuentra en la `Course.Department` propiedad de navegación.

Crear un controlador denominado `CourseController` para el `Course` tipo de entidad, con las mismas opciones que usó anteriormente para el `Student` controlador, tal como se muestra en la siguiente ilustración (salvo que a diferencia de la imagen, la clase de contexto está en el espacio de nombres DAL no lo modelos espacio de nombres):

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

Abra *Controllers\CourseController.cs* y examine el `Index` método:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

El scaffolding automático ha especificado la carga diligente para la propiedad de navegación `Department` mediante el método `Include`.

Abra *Views\Course\Index.cshtml* y reemplace el código existente por el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

Ha realizado los cambios siguientes en el código con scaffolding:

- Ha cambiado el título de **índice** a **cursos**.
- Los vínculos de la fila se mueve a la izquierda.
- Agrega una columna en el encabezado **número** que muestra la `CourseID` valor de propiedad. (De forma predeterminada, las claves principales no tienen scaffolding porque normalmente son importantes para los usuarios finales. Sin embargo, en este caso, la clave principal es significativa y quiere mostrarla.)
- Puede cambiar el último encabezado de columna de **DepartmentID** (el nombre de la clave externa para el `Department` entidad) a **departamento**.

Tenga en cuenta que para la última columna, se muestra el código con scaffolding la `Name` propiedad de la `Department` entidad que se carga en el `Department` propiedad de navegación:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

Ejecute la página (seleccione la **cursos** pestaña en la página principal de Contoso University) para ver la lista con los nombres de departamento.

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a>Cree una página de índice de instructores que se muestra los cursos y las inscripciones

En esta sección creará un controlador y ver el `Instructor` entidad con el fin de mostrar la página de índice de instructores:

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

En esta página se leen y muestran los datos relacionados de las maneras siguientes:

- La lista de instructores muestra datos relacionados de la `OfficeAssignment` entidad. Las entidades `Instructor` y `OfficeAssignment` se encuentran en una relación de uno a cero o uno. Usará la carga diligente para la `OfficeAssignment` entidades. Como se explicó anteriormente, la carga diligente normalmente es más eficaz cuando se necesitan los datos relacionados para todas las filas recuperadas de la tabla principal. En este caso, quiere mostrar las asignaciones de oficina para todos los instructores que se muestran.
- Cuando el usuario selecciona un instructor, relacionados con `Course` se muestran las entidades. Las entidades `Instructor` y `Course` se encuentran en una relación de varios a varios. Usará la carga diligente para la `Course` entidades y sus relacionados `Department` entidades. En este caso, la carga diferida podría ser más eficaz porque necesita cursos solo para el instructor seleccionado. Pero en este ejemplo se muestra cómo usar la carga diligente para propiedades de navegación dentro de entidades que, a su vez, se encuentran en propiedades de navegación.
- Cuando el usuario selecciona un curso, relacionadas con datos desde el `Enrollments` se muestra el conjunto de entidades. Las entidades `Course` y `Enrollment` se encuentran en una relación uno a varios. Se agregará la carga explícita para `Enrollment` entidades y sus relacionados `Student` entidades. (Carga explícita no es necesaria porque la carga diferida está habilitada, pero aquí muestra cómo realizar la carga explícita.)

### <a name="create-a-view-model-for-the-instructor-index-view"></a>Crear un modelo de vista para la vista de índice de instructores

La página de índice de instructores muestra tres tablas diferentes. Por tanto, creará un modelo de vista que incluye tres propiedades, cada una con los datos de una de las tablas.

En el *ViewModels* carpeta, cree *InstructorIndexData.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a>Agregar un estilo para las filas seleccionadas

Para marcar las filas seleccionadas tiene un color de fondo diferente. Para proporcionar un estilo de esta interfaz de usuario, agregue el código resaltado siguiente a la sección `/* info and errors */` en *Content\Site.css*, tal y como se muestra a continuación:

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a>Crear el controlador de Instructor y vistas

Crear un `InstructorController` controlador tal como se muestra en la ilustración siguiente:

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

Abra *Controllers\InstructorController.cs* y agregue un `using` instrucción para el `ViewModels` espacio de nombres:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

El código con scaffolding en el `Index` método especifica la carga diligente solo para el `OfficeAssignment` propiedad de navegación:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Reemplace el `Index` método con el código siguiente para cargar adicionales relacionadas con datos y colóquelo en el modelo de vista:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

El método acepta datos de ruta opcionales (`id`) y un parámetro de cadena de consulta (`courseID`) que proporcione los valores de identificador del instructor y curso seleccionado y pasa todos los datos necesarios a la vista. Los parámetros se proporcionan mediante los hipervínculos **Select** de la página.

> [!TIP]
> 
> **Datos de ruta**
> 
> Datos de ruta son datos que el enlazador de modelos se encuentra en un segmento de dirección URL especificado en la tabla de enrutamiento. Por ejemplo, la ruta predeterminada especifica `controller`, `action`, y `id` segmentos:
> 
> rutas. MapRoute)  
>  nombre: "Default",  
>  dirección URL: "{controller} / {action} / {id}",  
>  los valores predeterminados: new {controlador = "Home", acción = "Index", Id. = UrlParameter.Optional}  
> );
> 
> En la siguiente dirección URL, la ruta predeterminada asigna `Instructor` como el `controller`, `Index` como el `action` y 1 como el `id`; estos son los valores de datos de ruta.
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> "? courseID = 2021" es un valor de cadena de consulta. El enlazador de modelos también funcionará si se pasa el `id` como un valor de cadena de consulta:
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> Las direcciones URL se crean mediante `ActionLink` instrucciones en la vista de Razor. En el código siguiente, la `id` parámetro coincide con la ruta predeterminada, por lo que `id` se agrega a los datos de ruta.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> En el código siguiente, `courseID` no coincide con un parámetro en la ruta predeterminada, por lo que se agrega como una cadena de consulta.
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


El código comienza creando una instancia del modelo de vista y coloca en ella la lista de instructores. El código especifica la carga diligente para la `Instructor.OfficeAssignment` y `Instructor.Courses` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

El segundo `Include` método carga cursos y para cada curso que se carga, lo hace la carga diligente la `Course.Department` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Como se mencionó anteriormente, la carga diligente no es necesaria, pero se realiza para mejorar el rendimiento. Puesto que la vista siempre requiere la `OfficeAssignment` entidad, resulta más eficaz capturarla en la misma consulta. `Course` las entidades son necesarias cuando se selecciona un instructor en la página web, por lo que es mejor que la carga diferida solo si se muestra la página más a menudo con un curso seleccionado que sin la carga diligente.

Si se ha seleccionado un Id. de instructor, se recupera el instructor seleccionado en la lista de instructores del modelo de vista. El modelo de vista `Courses` , a continuación, se carga la propiedad con el `Course` entidades de ese instructor `Courses` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

El `Where` método devuelve una colección, pero en este caso los criterios que se pasan a ese método dan como resultado un solo `Instructor` entidad que se devuelven. El `Single` método convierte la colección en una sola `Instructor` entidad, que proporciona acceso a esa entidad `Courses` propiedad.

Usa el [único](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) método en una colección cuando se sabe que la colección tendrá un solo elemento. El `Single` método produce una excepción si la colección que se pasa está vacía o si hay más de un elemento. Una alternativa es [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), que devuelve un valor predeterminado (`null` en este caso) si la colección está vacía. Sin embargo, en este caso eso seguiría iniciando una excepción (por tratar de encontrar un `Courses` propiedad en un `null` referencia), y el mensaje de excepción indicaría con menos claridad la causa del problema. Cuando se llama a la `Single` método, también puede pasar en el `Where` condición en lugar de llamar el `Where` método por separado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

En lugar de:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

A continuación, si se ha seleccionado un curso, se recupera de la lista de cursos en el modelo de vista. A continuación, del modelo de vista `Enrollments` se carga la propiedad con el `Enrollment` entidades de ese curso `Enrollments` propiedad de navegación.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a>Modificación de la vista de índice de instructores

En *Views\Instructor\Index.cshtml*, reemplace el código existente por el código siguiente. Se resaltan los cambios:

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

Ha realizado los cambios siguientes en el código existente:

- Ha cambiado la clase de modelo por `InstructorIndexData`.
- Ha cambiado el título de la página de **Index** a **Instructors**.
- Mover las columnas de vínculo de la fila a la izquierda.
- Quita el **FullName** columna.
- Agrega un **Office** columna que muestra `item.OfficeAssignment.Location` solo si `item.OfficeAssignment` no es null. (Porque se trata de una relación uno a cero o uno, puede que no exista un relacionados `OfficeAssignment` entity.) 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- Se ha agregado código que se agregará dinámicamente `class="selectedrow"` a la `tr` elemento del instructor seleccionado. Esto establece el color de fondo de la fila seleccionada mediante la clase CSS que creó anteriormente. (El `valign` atributo será útil en el tutorial siguiente cuando se agrega una columna de varias filas a la tabla.) 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- Agrega un nuevo `ActionLink` con la etiqueta **seleccione** inmediatamente antes de los otros vínculos de cada fila, lo que hace que el identificador del instructor seleccionado se envíen a la `Index` método.

Ejecute la aplicación y seleccione el **instructores** ficha. Muestra la página el `Location` propiedad de relacionados `OfficeAssignment` la celda cuando no hay entidades y una tabla vacía no relacionados con `OfficeAssignment` entidad.

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

En el *Views\Instructor\Index.cshtml* archivo después del cierre `table` elemento (al final del archivo), agregue el siguiente código resaltado. Esto muestra una lista de cursos relacionados con un instructor cuando se selecciona un instructor.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

Este código lee la propiedad `Courses` del modelo de vista para mostrar una lista de cursos. También proporciona un `Select` hipervínculo que envía el identificador del curso seleccionado a la `Index` método de acción.

> [!NOTE]
> El *.css* archivo se almacena en caché por los exploradores. Si no ve los cambios cuando se ejecuta la aplicación, realice una actualización de disco dura (mantenga presionada la tecla CTRL mientras hace clic en el **actualizar** botón o presione CTRL + F5).


Ejecute la página y seleccione un instructor. Ahora verá una cuadrícula en la que se muestran los cursos asignados al instructor seleccionado, y para cada curso, el nombre del departamento asignado.

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Después del bloque de código que se acaba de agregar, agregue el código siguiente. Esto muestra una lista de los estudiantes que están inscritos en un curso cuando se selecciona ese curso.

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

Este código lee la `Enrollments` propiedad del modelo de vista para mostrar una lista de alumnos inscritos en el curso.

Ejecute la página y seleccione un instructor. Después, seleccione un curso para ver la lista de los estudiantes inscritos y sus calificaciones.

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a>Adición de la carga explícita

Abra *InstructorController.cs* y mire la `Index` método obtiene la lista de las inscripciones para un curso seleccionado:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

Al recuperar la lista de instructores, especificó la carga diligente para la `Courses` propiedad de navegación y para el `Department` propiedad de cada curso. A continuación, coloca el `Courses` colección en el modelo de vista, y ahora que está accediendo a la `Enrollments` propiedad de navegación de una entidad de la colección. Dado que no ha especificado la carga diligente para la `Course.Enrollments` propiedad de navegación, los datos de dicha propiedad aparece en la página como resultado de la carga diferida.

Si deshabilita la carga diferida sin cambiar el código de cualquier otra forma, el `Enrollments` propiedad sería null independientemente de cuántas inscripciones tenía el curso. En ese caso, para cargar el `Enrollments` propiedad, tendría que especificar la carga diligente o carga explícita. Ya ha visto cómo realizar la carga diligente. Para ver un ejemplo de la carga explícita, reemplace el `Index` método con el código siguiente, que carga explícitamente la `Enrollments` propiedad. El código cambiado se resaltan.

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

Después de obtener el texto seleccionado `Course` entidad, el nuevo código carga explícitamente ese curso `Enrollments` propiedad de navegación:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Después, carga explícitamente cada `Enrollment` entidad relacionada con la `Student` entidad:

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Tenga en cuenta que usa el `Collection` método para cargar una propiedad de colección, pero para una propiedad que contiene una sola entidad, usar el `Reference` método. Puede ejecutar la página de índice de instructores ahora y no verá ninguna diferencia en lo que se muestra en la página, aunque haya cambiado la forma en que se recuperan los datos.

## <a name="summary"></a>Resumen

Ahora ha usado las tres formas (diferidas, diligente y explícitas) para cargar datos relacionados en las propiedades de navegación. En el siguiente tutorial, obtendrá información sobre cómo actualizar datos relacionados.

Pueden encontrar vínculos a otros recursos de Entity Framework en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [Siguiente](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
