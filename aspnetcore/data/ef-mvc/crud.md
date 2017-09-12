---
title: "Núcleo de ASP.NET MVC con EF Core - CRUD - 2 de 10"
author: tdykstra
description: 
keywords: ASP.NET Core, Entity Framework Core, CRUD, crear, leer, actualizar, eliminar
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 6e1cd570-40f1-4b24-8b6e-7d2d27758f18
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: 87aa7e63b1a08e457c5fdcbc052bfa039b8d2175
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a>Crear, leer, actualizar y eliminar - Core EF con el tutorial de MVC de ASP.NET Core (2 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En el tutorial anterior, creó una aplicación MVC que almacena y muestra los datos con Entity Framework y SQL Server LocalDB. En este tutorial, podrá revisar y personalizar el CRUD (crear, leer, actualizar y eliminar) código que el scaffolding de MVC crea automáticamente para usted en controladores y vistas.

> [!NOTE] 
> Es una práctica común para implementar el modelo de repositorio con el fin de crear una capa de abstracción entre el controlador y la capa de acceso a datos. Para mantener estos tutoriales sencilla y se centra en enseña a usar el marco de trabajo de entidad, no usan repositorios. Para obtener información acerca de los repositorios con EF, consulte [del último tutorial de esta serie](advanced.md).

En este tutorial, trabajará con las páginas web siguientes:

![Página de detalles de estudiante](crud/_static/student-details.png)

![Página de creación de estudiante](crud/_static/student-create.png)

![Página de edición de estudiante](crud/_static/student-edit.png)

![Página de borrado de estudiante](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a>Personalizar la página de detalles

El código con scaffolding de la página de índice de los alumnos se deja fuera del `Enrollments` propiedad, porque esta propiedad contiene una colección. En el **detalles** página, se mostrará el contenido de la colección en una tabla HTML.

En *Controllers/StudentsController.cs*, el método de acción para los detalles de la vista utiliza la `SingleOrDefaultAsync` método para recuperar un único `Student` entidad. Agregue código que llama a `Include`. `ThenInclude`, y `AsNoTracking` métodos, como se muestra en el código que aparece resaltado.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

El `Include` y `ThenInclude` métodos hacen que el contexto cargar la `Student.Enrollments` propiedad de navegación y dentro de cada inscripción el `Enrollment.Course` propiedad de navegación.  Aprenderá más acerca de estos métodos en el [leer datos relacionados](read-related-data.md) tutorial.

El `AsNoTracking` método mejora el rendimiento en escenarios donde no se actualizará las entidades devueltas en la duración del contexto actual. Aprenderá más acerca de `AsNoTracking` al final de este tutorial.

### <a name="route-data"></a>Datos de ruta

El valor de clave que se pasa a la `Details` método procede de *enrutar datos*. Datos de ruta son datos que el enlazador de modelos que se encuentra en un segmento de la dirección URL. Por ejemplo, la ruta predeterminada especifica segmentos controlador, acción e Id.:

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

En la siguiente dirección URL, la ruta predeterminada se asigna Instructor como el controlador, el índice como la acción y 1 como el identificador; Éstos son valores de datos de ruta.

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

La última parte de la dirección URL ("? courseID = 2021") es un valor de cadena de consulta. El enlazador de modelos también pasará el valor de identificador para la `Details` método `id` parámetro si se pasa como un valor de cadena de consulta:

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

En la página de índice, se crean direcciones URL del hipervínculo por las instrucciones de la aplicación auxiliar de etiqueta en la vista de Razor. En el siguiente código Razor, el `id` parámetro coincide con la ruta predeterminada, por lo que `id` se agrega a los datos de ruta.

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

Esto genera el siguiente código HTML cuando `item.ID` es 6:

```html
<a href="/Students/Edit/6">Edit</a>
```

En el siguiente código Razor, `studentID` no coincide con un parámetro en la ruta predeterminada, por lo que se agrega como una cadena de consulta.

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

Esto genera el siguiente código HTML cuando `item.ID` es 6:

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

Para obtener más información acerca de aplicaciones auxiliares de etiquetas, consulte [aplicaciones auxiliares de etiquetas en ASP.NET Core](xref:mvc/views/tag-helpers/intro).

### <a name="add-enrollments-to-the-details-view"></a>Agregue las inscripciones a la vista de detalles

Abra *Views/Students/Details.cshtml*. Cada campo se muestra mediante `DisplayNameFor` y `DisplayFor` aplicaciones auxiliares, como se muestra en el ejemplo siguiente:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

Después del último campo e inmediatamente antes del cierre `</dl>` etiqueta, agregue el código siguiente para mostrar una lista de las inscripciones:

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

Si no es correcta la sangría del código después de pegar el código, presione CTRL-K-D para corregirlo.

Este código recorre las entidades en el `Enrollments` propiedad de navegación. Para cada inscripción muestra el título del curso y el grado de. Se recupera el título del curso de la entidad de curso que se almacena en la `Course` propiedad de navegación de la entidad de inscripciones.

Ejecute la aplicación, seleccione la **estudiantes** ficha y haga clic en el **detalles** vínculo acerca de un estudiante. Ver la lista de cursos y sus notas para el alumno seleccionado:

![Página de detalles de estudiante](crud/_static/student-details.png)

## <a name="update-the-create-page"></a>Actualizar la página Crear

En *StudentsController.cs*, modifique el HttpPost `Create` método agregando un bloque try-catch y quitar Id. de la `Bind` atributo.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

Este código agrega la entidad de estudiantes creada por el enlazador de modelos de ASP.NET MVC a la entidad de los alumnos establece y, a continuación, guarda los cambios en la base de datos. (Enlazador de modelos se refiere a la funcionalidad de ASP.NET MVC que resulta más sencillo para trabajar con datos enviados por un formulario; un enlazador de modelos convierte los valores de formulario expuesto a tipos de CLR y los pasa al método de acción en parámetros. En este caso, el enlazador de modelos crea instancias de una entidad de estudiante automáticamente con valores de propiedad de la colección de formulario.)

Que se han suprimido `ID` desde el `Bind` atributo porque Id. es el valor de clave principal que SQL Server establecerá automáticamente cuando se inserta la fila. Entrada del usuario no establece el valor de identificador.

Distinto de la `Bind` atributo, el bloque try-catch es el único cambio que haya realizado en el código con scaffolding. Si una excepción que se deriva de `DbUpdateException` es detecta mientras se guardan los cambios, se muestra un mensaje de error genérico. `DbUpdateException`excepciones se deben a veces por algo externo a la aplicación en lugar de un error de programación, por lo que el usuario se recomienda que vuelva a intentarlo. Aunque no ha implementado en este ejemplo, una aplicación de calidad de producción debería registrar la excepción. Para obtener más información, consulte el **registro para obtener información** sección [supervisión y telemetría (creación reales en la nube de aplicaciones con Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).

El `ValidateAntiForgeryToken` atributo ayuda a evitar ataques de falsificación (CSRF) de solicitud entre sitios. El token se inserta automáticamente en la vista de la [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) y se incluye cuando se envía el formulario por el usuario. El token se valida mediante el `ValidateAntiForgeryToken` atributo. Para obtener más información acerca de CSRF, consulte [falsificación de Anti-solicitudes](../../security/anti-request-forgery.md).

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a>Nota de seguridad sobre overposting

El `Bind` atributo que incluye el código con scaffolding en el `Create` método es una manera de proteger contra overposting en crear escenarios. Por ejemplo, suponga que la entidad Student incluye un `Secret` propiedad que no desea que esta página web para establecer.

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

Incluso si no tienes un `Secret` campo en la página web, un pirata informático podría usar una herramienta como Fiddler o escribir código de JavaScript, para registrar un `Secret` formar el valor. Sin el `Bind` atributo limitar los campos que el enlazador de modelos que se utiliza cuando crea una instancia de estudiantes, el enlazador de modelos seleccionará que `Secret` formato de valor y usarlo para crear la instancia de entidad de estudiante. A continuación, cualquier valor el pirata especificado para el `Secret` campo de formulario se actualizarán en la base de datos. La siguiente imagen muestra la adición de la herramienta de Fiddler el `Secret` campo (con el valor "OverPost") a los valores de formulario expuesto.

![Campo de secreto adición de Fiddler](crud/_static/fiddler.png)

El valor "OverPost", a continuación, se agregaría correctamente a la `Secret` propiedad de las filas insertadas de fila, aunque se habían previsto que la página web pueda establece esta propiedad.

Puede evitar overposting en escenarios de edición leer primero la entidad desde la base de datos y, a continuación, llamando a `TryUpdateModel`, pasando una lista de propiedades permitidas explícita. Que es el método utilizado en estos tutoriales.

Es una manera alternativa para evitar que overposting que muchos desarrolladores prefieren usar modelos de vista en lugar de las clases de entidad con el enlace de modelos. Incluir solo las propiedades que desea actualizar en el modelo de vista. Una vez que haya finalizado el enlazador de modelos MVC, copie las propiedades del modelo de vista a la instancia de entidad, opcionalmente mediante una herramienta como AutoMapper. Use `_context.Entry` en la instancia de entidad para establecer su estado en `Unchanged`y, a continuación, establezca `Property("PropertyName").IsModified` en true en cada propiedad de entidad que se incluye en el modelo de vista. Este método da tanto en Editar y crear escenarios.

### <a name="test-the-create-page"></a>Probar la página Crear

El código en *Views/Students/Create.cshtml* utiliza `label`, `input`, y `span` (para los mensajes de validación) aplicaciones auxiliares para cada campo de etiquetas.

Ejecute la página seleccionando el **estudiantes** ficha y haga clic en **crear nuevo**.

Especifique una fecha y nombres. Pruebe a escribir una fecha no válida si el explorador le permite hacerlo. (Algunos exploradores obligan a utilizar un selector de fecha.) A continuación, haga clic en **crear** para ver el mensaje de error.

![Error de validación de fecha](crud/_static/date-error.png)

Se trata de validación del lado servidor que obtendrá de forma predeterminada; en un tutorial posterior verá cómo agregar atributos que se generarán código para la validación del lado cliente también. El siguiente código resaltado muestra la comprobación de validación del modelo en el `Create` método.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

Cambie la fecha a un valor válido y haga clic en **crear** para ver el nuevo alumno aparecen en la **índice** página.

## <a name="update-the-edit-page"></a>Actualizar la página de edición

En *StudentController.cs*, el HttpGet `Edit` método (uno sin el `HttpPost` atributo) usa el `SingleOrDefaultAsync` método para recuperar la entidad seleccionada de estudiantes, tal y como se vio en el `Details` método. No es necesario cambiar este método.

### <a name="recommended-httppost-edit-code-read-and-update"></a>Recomienda HttpPost editar código: lectura y actualización

Reemplace el método de acción HttpPost editar con el código siguiente.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

Estos cambios implementan una práctica recomendada de seguridad para evitar overposting. El scaffolder genera un `Bind` de atributo y agrega la entidad creada por el enlazador de modelos a la entidad establecida con un `Modified` marca. Que no se recomienda código para muchos escenarios porque la `Bind` atributo borra los datos previamente existentes en los campos no aparecen en la `Include` parámetro.

El nuevo código lee la entidad existente y llama `TryUpdateModel` para actualizar los campos en la entidad recuperada [en función de la entrada del usuario en los datos de formulario expuesto](xref:mvc/models/model-binding#how-model-binding-works). Seguimiento de conjuntos de cambios automático de Entity Framework la `Modified` marca en los campos que se cambian mediante la entrada de formulario. Cuando el `SaveChanges` se llama al método, Entity Framework crea instrucciones SQL para actualizar la fila de la base de datos. Conflictos de simultaneidad se omiten y las columnas de tabla que se actualizaron por el usuario se actualizan en la base de datos. (Un tutorial posterior muestra cómo controlar los conflictos de simultaneidad).

Como práctica recomendada para evitar la publicación excesiva, los campos que desea que se pueda actualizar por la **editar** en la lista de permitidos son los `TryUpdateModel` parámetros. (La cadena vacía anterior a la lista de campos en la lista de parámetros es para que un prefijo para utilizarlo con los nombres de campos de formulario). Actualmente no hay ningún campo adicional que va a proteger, pero mostrar los campos que desea que el enlazador de modelos para enlazar garantiza que si agrega campos para el modelo de datos en el futuro, que están automáticamente protegidos hasta que se agreguen de forma explícita aquí.

Como resultado de estos cambios, la firma del método de la HttpPost `Edit` método es el mismo que el HttpGet `Edit` método; por lo tanto, se ha cambiado el nombre del método `EditPost`.

### <a name="alternative-httppost-edit-code-create-and-attach"></a>Código de edición HttpPost alternativo: crear y adjuntar

El código para modificar HttpPost recomendado garantiza que solo las columnas cambiadas se actualizan y conserva los datos de las propiedades que no desea que incluyen para el enlace de modelos. Sin embargo, el enfoque basado principalmente de lectura requiere una base de datos adicional de lectura y puede dar lugar a un código más complejo para tratar los conflictos de simultaneidad. Una alternativa consiste en adjuntar una entidad creada por el enlazador de modelos en el contexto EF y se marca como modificada. (No se actualiza el proyecto con este código, solo se muestra para ilustrar un enfoque opcional.) 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

Puede usar este enfoque cuando la interfaz de usuario de la página web incluye todos los campos de la entidad y se puede actualizar cualquiera de ellos.

El código con scaffolding utiliza el enfoque de crear y adjuntar, sino solo detecta `DbUpdateConcurrencyException` excepciones y devuelve códigos de error 404.  El ejemplo mostrado detecta cualquier excepción de actualización de base de datos y muestra un mensaje de error.

### <a name="entity-states"></a>Estados de entidad

El contexto de base de datos realiza un seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos, y esta información determina lo que ocurre cuando se llama a la `SaveChanges` método. Por ejemplo, cuando se pasa una nueva entidad a la `Add` método, que el estado de la entidad se establece en `Added`. A continuación, cuando se llama a la `SaveChanges` método, el contexto de base de datos emite un comando INSERT de SQL.

Una entidad puede estar en uno de los siguientes estados:

* `Added`. La entidad no existe todavía en la base de datos. El `SaveChanges` método emite una instrucción INSERT.

* `Unchanged`. Nada debe hacerse con esta entidad mediante el `SaveChanges` método. Al leer una entidad de la base de datos, la entidad empieza con este estado.

* `Modified`. Se han modificado algunos o todos los valores de propiedad de la entidad. El `SaveChanges` método emite una instrucción UPDATE.

* `Deleted`. La entidad se ha marcado para su eliminación. El `SaveChanges` método emite una instrucción DELETE.

* `Detached`. La entidad no sometida a seguimiento por el contexto de base de datos.

En una aplicación de escritorio, normalmente se establecen automáticamente los cambios de estado. Leer una entidad y realizar cambios en algunas de sus valores de propiedad. Esto hace que su estado de entidad que se cambie automáticamente al `Modified`. A continuación, cuando se llama a `SaveChanges`, Entity Framework genera una instrucción UPDATE de SQL que actualiza solo las propiedades reales que se cambió.

En una aplicación web, el `DbContext` que inicialmente lee una entidad y muestra sus datos que se pueda editar se eliminan una vez que se representa una página. Cuando el HttpPost `Edit` se llama al método de acción, se realiza una nueva solicitud de web y tiene una nueva instancia de la `DbContext`. Si vuelve a leer la entidad en ese contexto nuevo, simular procesamiento escritorio.

Sin embargo, si no quiere realizar adicional de la operación de lectura, tiene que usar el objeto de entidad creado por el enlazador de modelos.  La manera más sencilla de hacerlo es establecer el estado de entidad en Modified tal y como se hace en el código de edición HttpPost alternativo mostrado anteriormente. A continuación, cuando se llama a `SaveChanges`, Entity Framework actualiza todas las columnas de la fila de la base de datos, porque el contexto no tiene ninguna manera de saber qué propiedades ha cambiado.

Si desea evitar el enfoque basado principalmente de lectura, pero también desea que la instrucción UPDATE de SQL para actualizar solo los campos que el usuario cambia realmente, el código es más complejo. Debe guardar los valores originales de alguna manera (como mediante el uso de los campos ocultos) para que estén disponibles cuando el HttpPost `Edit` se llama al método. A continuación, puede crear una entidad de Student con los valores originales, llamada la `Attach` método con esa versión original de la entidad, actualizar los valores de la entidad a los nuevos valores y, a continuación, llamar a `SaveChanges`.

### <a name="test-the-edit-page"></a>Probar la página de edición

Ejecute la aplicación y seleccione la **estudiantes** ficha, a continuación, haga clic en un **editar** hipervínculo.

![Página de edición de estudiantes](crud/_static/student-edit.png)

Cambiar algunos de los datos y haga clic en **guardar**. El **índice** se abre la página y verá los datos modificados.

## <a name="update-the-delete-page"></a>Actualizar la página de borrado

En *StudentController.cs*, el código de plantilla para el HttpGet `Delete` método usa la `SingleOrDefaultAsync` método para recuperar la entidad seleccionada de estudiantes, tal y como se vieron en los detalles y modificar métodos. Sin embargo, para implementar un mensaje de error personalizado cuando la llamada a `SaveChanges` se produce un error, agregará algunas funciones a este método y su vista correspondiente.

Como hemos visto de actualización y crea las operaciones, las operaciones de eliminación requieren dos métodos de acción. El método que se llama en respuesta a una solicitud GET muestra una vista que proporciona al usuario una oportunidad para aprobar o cancelar la operación de eliminación. Si el usuario lo aprueba, se crea una solicitud POST. Cuando esto ocurre, el HttpPost `Delete` se llama al método y, a continuación, este método realiza la operación de eliminación.

Agregará un bloque try-catch para el HttpPost `Delete` método para controlar los errores que pueden producirse cuando se actualiza la base de datos. Si se produce un error, el método HttpPost Delete llama al método de eliminación de HttpGet, pasando un parámetro que indica que se ha producido un error. El método HttpGet Delete, a continuación, vuelve a mostrar la página de confirmación junto con el mensaje de error, dando al usuario la oportunidad de cancelar o vuelva a intentarlo.

Reemplace el HttpGet `Delete` método de acción con el código siguiente, que administra el informe de errores.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

Este código acepta un parámetro opcional que indica si se llamó al método después de un error al guardar los cambios. Este parámetro es false cuando el HttpGet `Delete` método se llama sin un error anterior. Cuando se llama por la HttpPost `Delete` método en respuesta a un error de actualización de base de datos, el parámetro es true y un mensaje de error se pasa a la vista.

### <a name="the-read-first-approach-to-httppost-delete"></a>El enfoque basado principalmente de lectura para HttpPost Delete

Reemplace el HttpPost `Delete` método de acción (denominado `DeleteConfirmed`) con el código siguiente, que realiza la operación de eliminación real y captura los errores de actualización de base de datos.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

Este código recupera la entidad seleccionada, a continuación, llama el `Remove` método para establecer el estado de la entidad como `Deleted`. Cuando `SaveChanges` se denomina un DELETE de SQL se genere un comando.

### <a name="the-create-and-attach-approach-to-httppost-delete"></a>El enfoque de crear y adjuntar a HttpPost Delete

Si mejora el rendimiento en una aplicación de gran volumen es una prioridad, podría impedir una consulta SQL innecesaria creando instancias de una entidad de estudiante con solo el principal valor y, a continuación, establecer el estado de entidad en la clave `Deleted`. Eso es todo lo que necesita de Entity Framework para eliminar la entidad. (No coloque este código en el proyecto; aquí es únicamente para ilustrar una alternativa).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

Si la entidad con datos relacionados que también deben eliminarse, asegúrese de que la eliminación en cascada se configura en la base de datos. Con este enfoque a la eliminación de entidad, EF no podría saber que hay entidades relacionadas va a eliminar.

### <a name="update-the-delete-view"></a>Actualizar la vista de eliminación

En *Views/Student/Delete.cshtml*, agregar un mensaje de error entre el encabezado h2 y el título h3, tal como se muestra en el ejemplo siguiente:

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

Ejecute la página seleccionando el **estudiantes** ficha y haga clic en un **eliminar** hipervínculo:

![Eliminar página de confirmación](crud/_static/student-delete.png)

Haga clic en **eliminar**. Se abrirá la página de índice sin los estudiantes eliminado. (Verá un ejemplo de código de acción en el tutorial de simultaneidad de control de errores).

## <a name="closing-database-connections"></a>Cerrar las conexiones de base de datos

Para liberar los recursos que contiene una conexión de base de datos, la instancia de contexto debe eliminarse tan pronto como sea posible cuando haya terminado con él. Integrado ASP.NET Core [inyección de dependencia](../../fundamentals/dependency-injection.md) se encarga de esa tarea.

En *Startup.cs*, se llama a la [método de extensión AddDbContext](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) para aprovisionar el `DbContext` clase en el contenedor de DI de ASP.NET. Que el método establece la duración del servicio en `Scoped` de forma predeterminada. `Scoped`significa que la duración del objeto de contexto coincide con el tiempo de vida de solicitud web, y el `Dispose` método llamará automáticamente al final de la solicitud web.

## <a name="handling-transactions"></a>Controlar transacciones

De forma predeterminada el Entity Framework implementa de manera implícita las transacciones. En escenarios donde realizar cambios en varias filas o tablas y, a continuación, llamar a `SaveChanges`, Entity Framework automáticamente se asegura que todos los cambios se realizan correctamente o producirá un error en todas ellas. Si se realizan algunos cambios en primer lugar y, a continuación, se produce un error, los cambios se deshacen automáticamente. Para escenarios donde necesita más control, por ejemplo, si van a incluir operaciones realizadas fuera de Entity Framework en una transacción, consulte [transacciones](https://docs.microsoft.com/ef/core/saving/transactions).

## <a name="no-tracking-queries"></a>Consultas de seguimiento no

Cuando un contexto de base de datos recupera las filas de tabla y crea objetos de entidad que tienen tipos de archivo, de forma predeterminada realiza un seguimiento de si las entidades en memoria están sincronizadas con el contenido de la base de datos. Los datos en memoria actúa como una memoria caché y se utilizan cuando se actualiza una entidad. Este almacenamiento en caché a menudo es necesario en una aplicación web porque el contexto de instancias son normalmente de corta duración (un nuevo objeto uno se crea y se elimina para cada solicitud) y el contexto que lee una entidad normalmente se elimina antes de esa entidad se vuelve a usar.

Puede deshabilitar el seguimiento de los objetos de entidad en la memoria mediante una llamada a la `AsNoTracking` método. En el que se puede hacer que los escenarios típicos son los siguientes:

* Durante la vigencia de contexto no es necesario actualizar todas las entidades y no necesita EF a [cargar automáticamente las propiedades de navegación con entidades recuperadas por consultas independientes](read-related-data.md). Con frecuencia se cumplen estas condiciones en los métodos de acción del controlador HttpGet.

* Se ejecuta una consulta que recupera un gran volumen de datos, y se actualizará solo una pequeña parte de los datos devueltos. Puede ser más eficaz para desactivar el seguimiento de la consulta de gran tamaño y ejecutar una consulta más adelante para las entidades pocos que deben actualizarse.

* Que desea asociar una entidad con el fin de actualizar, pero antes de recuperar la misma entidad para un propósito diferente. Dado que la entidad ya está sometida a seguimiento por el contexto de base de datos, no se puede adjuntar la entidad que desea cambiar. Una manera de controlar esta situación es llamar a `AsNoTracking` en la consulta anterior.

Para obtener más información, vea [seguimiento vs. Seguimiento de ningún](https://docs.microsoft.com/ef/core/querying/tracking).

## <a name="summary"></a>Resumen

Ahora tiene un conjunto completo de páginas que realizan operaciones CRUD sencillas para entidades de estudiante. En el siguiente tutorial se podrá expandir la funcionalidad de la **índice** página mediante la adición de ordenación, filtrado y paginación.

>[!div class="step-by-step"]
[Anterior](intro.md)
[Siguiente](sort-filter-page.md)  
