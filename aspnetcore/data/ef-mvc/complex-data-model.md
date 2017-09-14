---
title: "Núcleo de ASP.NET MVC con EF Core - modelo de datos: 5 de 10"
author: tdykstra
description: "En este tutorial agregará más entidades y relaciones y personalizar el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos."
keywords: Anotaciones de datos de ASP.NET Core, Entity Framework Core,
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 0dd63913-a041-48b6-96a4-3aeaedbdf5d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: a9e255040c300bc5ce55a356e17e6912dbaeaf88
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-aspnet-core-mvc-tutorial-5-of-10"></a>Crear un modelo de datos complejos - Core EF con el tutorial de MVC de ASP.NET Core (5 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En los tutoriales anteriores, ha trabajado con un modelo de datos simple que se compone de tres entidades. En este tutorial, agregará más entidades y relaciones y personalizará el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos.

Cuando haya terminado, las clases de entidad que conformarán el modelo de datos completa que se muestra en la siguiente ilustración:

![Diagrama de entidad](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizar el modelo de datos mediante el uso de atributos

En esta sección verá cómo personalizar el modelo de datos mediante el uso de atributos que especifican el formato, validación y las reglas de asignación de la base de datos. A continuación, en varias de las siguientes secciones que creará el modelo de datos School completando mediante la adición de atributos a las clases ya ha creado y crear clases nuevas para los demás tipos de entidad en el modelo.

### <a name="the-datatype-attribute"></a>El atributo de tipo de datos

Para las fechas de inscripción de estudiantes, todas las páginas web mostrar actualmente la hora junto con la fecha, aunque todo lo que le interesa para este campo es la fecha. Con los atributos de anotación de datos, puede realizar un cambio que se solucionará el formato de presentación en cada vista que muestra los datos de código. Para ver un ejemplo de cómo hacerlo, deberá agregar un atributo a la `EnrollmentDate` propiedad en la `Student` clase.

En *Models/Student.cs*, agregar un `using` instrucción para el `System.ComponentModel.DataAnnotations` espacio de nombres y agregue `DataType` y `DisplayFormat` atributos a la `EnrollmentDate` propiedad, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

El `DataType` atributo se utiliza para especificar un tipo de datos que es más específico que el tipo intrínseco de base de datos. En este caso sólo desea realizar un seguimiento de la fecha, no la fecha y hora. El `DataType` enumeración proporciona para muchos tipos de datos, como fecha, hora, número de teléfono, moneda, EmailAddress y mucho más. El `DataType` atributo también puede habilitar la aplicación proporcionar automáticamente las características específicas del tipo. Por ejemplo, un `mailto:` vínculo se puede crear para `DataType.EmailAddress`, y no se puede proporcionar un selector de fecha para `DataType.Date` en exploradores compatibles con HTML5. El `DataType` atributo emite HTML 5 `data-` atributos (pronunciado datos dash) que pueden entender exploradores HTML 5. El `DataType` atributos no proporcionan ninguna validación.

`DataType.Date`no especifica el formato de la fecha en que se muestra. De forma predeterminada, se muestra el campo de datos según los formatos predeterminados según CultureInfo del servidor.

El `DisplayFormat` atributo se usa para especificar explícitamente el formato de fecha:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

El `ApplyFormatInEditMode` configuración especifica que la aplicación de formato también se debe aplicar cuando el valor se muestra en un cuadro de texto para su edición. (No conviene que para algunos campos, por ejemplo, para los valores de moneda, no puede el símbolo de moneda en el cuadro de texto para su edición.)

Puede usar el `DisplayFormat` atributo por sí mismo, pero suele ser una buena idea usar el `DataType` atributo también. El `DataType` atributo transmite la semántica de los datos en lugar de cómo representar en una pantalla y ofrece las siguientes ventajas que no obtendrá con `DisplayFormat`:

* El explorador puede habilitar características de HTML5 (por ejemplo para mostrar un control de calendario, el símbolo de moneda adecuado para la configuración regional, los vínculos de correo electrónico, algunos comandos de cliente de entrada de validación, etcetera.).

* De forma predeterminada, el explorador representará los datos con el formato correcto en función de la configuración regional.

Para obtener más información, consulte el [ \<entrada > documentación de la aplicación auxiliar de etiquetas](../../mvc/views/working-with-forms.md#the-input-tag-helper).

Vuelva a ejecutar la página de índice de los alumnos y observe que veces ya no se muestran las fechas de inscripción. El mismo será true para cualquier vista que usa el modelo de estudiante.

![Página de índice de los alumnos mostrando las fechas sin veces](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>El atributo StringLength

También puede especificar reglas de validación de datos y mensajes de error de validación utilizando atributos. El `StringLength` atributo establece la longitud máxima de la base de datos y proporciona el cliente y el lado servidor validación de ASP.NET MVC. También puede especificar la longitud mínima de la cadena en este atributo, pero el valor mínimo no influye en el esquema de base de datos.

Imagine que desea asegurarse de que los usuarios no escriban más de 50 caracteres para un nombre. Para agregar esta limitación, agregue `StringLength` atributos a la `LastName` y `FirstMidName` propiedades, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

El `StringLength` atributo no impedir que un usuario introducir un espacio en blanco para un nombre. Puede usar el `RegularExpression` atributo para aplicar restricciones a la entrada. Por ejemplo, el siguiente código requiere el primer carácter que se va letra mayúscula y el resto de caracteres sea alfabético:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

El `MaxLength` atributo proporciona una funcionalidad similar a la `StringLength` atributo pero no proporciona el cliente validación.

Ahora ha cambiado el modelo de base de datos de forma que requiera un cambio en el esquema de base de datos. Deberá usar migraciones para actualizar el esquema sin perder los datos que ha agregado a la base de datos mediante el interfaz de usuario de la aplicación.

Guarde los cambios y compile el proyecto. A continuación, abra la ventana de comandos en la carpeta de proyecto y escriba los siguientes comandos:

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

El `migrations add` comando advierte de que se puede producir pérdida de datos, porque hace que el cambio de la longitud máxima más corta de dos columnas.  Migraciones crea un archivo denominado * \<timeStamp > _MaxLengthOnNames.cs*. Este archivo contiene código en el `Up` método que actualizará la base de datos para que coincida con el modelo de datos actual. El `database update` que el código ejecutó el comando.

La marca de tiempo como precedida el nombre de archivo de las migraciones se usa por Entity Framework para ordenar las migraciones. Puede crear varias migraciones antes de ejecutar el comando de actualización de bases de datos y, a continuación, todas las migraciones se aplican en el orden en el que se crearon.

Ejecute la página Crear y escriba cualquier nombre de más de 50 caracteres. Al hacer clic en crear, validación del lado cliente muestra un mensaje de error.

![Página que muestra los errores de longitud de cadena de índice de estudiantes](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a>El atributo de columna

También puede usar atributos para controlar cómo se asignan las clases y propiedades para la base de datos. Imagine que hubiera usado el nombre `FirstMidName` para el nombre de campo ya que el campo puede contener también un nombre de medio. Pero desea que la columna de base de datos se denomine `FirstName`, ya que los usuarios que se va a escribir las consultas ad hoc en la base de datos están acostumbrados a dicho nombre. Para realizar esta asignación, puede usar el `Column` atributo.

El `Column` atributo especifica que cuando se crea la base de datos, la columna de la `Student` tabla que se asigna a la `FirstMidName` propiedad se denominará `FirstName`. En otras palabras, cuando el código hace referencia a `Student.FirstMidName`, los datos proceden o actualizados en el `FirstName` columna de la `Student` tabla. Si no especifica nombres de columna, se facilita el mismo nombre que el nombre de propiedad.

En el *Student.cs* , agregue un `using` instrucción para `System.ComponentModel.DataAnnotations.Schema` y agregue el atributo de nombre de columna para el `FirstMidName` propiedad, como se muestra en el código resaltado siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

La adición de la `Column` atributo cambia la copia de seguridad de modelo la `SchoolContext`, por lo que no coincida con la base de datos.

Guarde los cambios y compile el proyecto. A continuación, abra la ventana de comandos en la carpeta de proyecto y escriba los comandos siguientes para crear otra migración:

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

En **Explorador de objetos de SQL Server**, abra el Diseñador de tablas de Student haciendo doble clic en el **estudiante** tabla.

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

Antes de aplicar las dos primeras migraciones, eran las columnas del nombre de tipo nvarchar (max). Ahora son nvarchar (50) y el nombre de la columna ha cambiado de FirstMidName a FirstName.

> [!Note]
> Si intenta compilar antes de que termine de crear todas las clases de entidad en las secciones siguientes, podría obtener errores del compilador.

## <a name="final-changes-to-the-student-entity"></a>Cambios finales a la entidad Student

![Entidad Student](complex-data-model/_static/student-entity.png)

En *Models/Student.cs*, reemplace el código que agregó anteriormente con el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>El atributo requerido

El `Required` atributo hace que los campos obligatorios de propiedades de nombre. El `Required` atributo no es necesario para los tipos que no aceptan valores NULL, como tipos de valor (fecha y hora, int, double, float, etcetera.). Tipos que no pueden ser nulos se tratan automáticamente como campos obligatorios.

Puede quitar el `Required` de atributo y reemplácelo por un parámetro de longitud mínima para la `StringLength` atributo:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>El atributo de visualización

El `Display` atributo especifica que el título de los cuadros de texto debe ser "Nombre", "Last Name", "Nombre completo" y "Fecha de inscripción" en lugar del nombre de propiedad en cada instancia (que no tiene ningún espacio al dividir las palabras).

### <a name="the-fullname-calculated-property"></a>La propiedad FullName calculado

`FullName`es una propiedad calculada que devuelve un valor que se crea concatenando dos otras propiedades. Por lo tanto, tiene solo un descriptor de acceso get y no `FullName` columna se genera en la base de datos.

## <a name="create-the-instructor-entity"></a>Crear la entidad Instructor

![Entidad instructor](complex-data-model/_static/instructor-entity.png)

Crear *Models/Instructor.cs*, reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Tenga en cuenta que varias propiedades son los mismos en las entidades de Student y Instructor. En el [implementar herencia](inheritance.md) tutorial más adelante en esta serie, deberá refactorizar este código para eliminar la redundancia.

Puede colocar varios atributos en una sola línea, por lo que también puede escribir el `HireDate` atributos como se indica a continuación:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Las propiedades de navegación CourseAssignments y OfficeAssignment

El `CourseAssignments` y `OfficeAssignment` las propiedades son propiedades de navegación.

Un instructor puede enseñar a cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Si una propiedad de navegación puede contener varias entidades, su tipo debe ser una lista en el que se pueden se agrega, elimina y actualiza las entradas.  Puede especificar `ICollection<T>` o un tipo como `List<T>` o `HashSet<T>`. Si especifica `ICollection<T>`, EF crea un `HashSet<T>` colección de forma predeterminada.

El motivo por qué se trata `CourseAssignment` entidades se explica a continuación en la sección acerca de las relaciones de varios a varios.

Estado de reglas de negocios de la Universidad de Contoso que un instructor solo puede tener a lo sumo una oficina, por lo que el `OfficeAssignment` propiedad contiene una única entidad OfficeAssignment (que puede ser null si no se asigna ningún office).

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Crear la entidad OfficeAssignment

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Crear *Models/OfficeAssignment.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Key (atributo)

Hay una relación de uno a cero o uno entre el Instructor y las entidades OfficeAssignment. Solo existe una asignación de oficina en relación con el instructor se asigna a y, por lo tanto, su clave principal también es la clave externa para la entidad Instructor. Pero Entity Framework no se reconoce automáticamente InstructorID como la clave principal de esta entidad porque su nombre no sigue la convención de nomenclatura de Id. o classnameID. Por lo tanto, la `Key` atributo se utiliza para identificarlo como la clave:

```csharp
[Key]
public int InstructorID { get; set; }
```

También puede usar el `Key` atributo si la entidad tiene su propia clave principal, pero desea asignar nombre a la propiedad algo que no sean classnameID o identificador.

De forma predeterminada, EF trata la clave como no generada por base de datos porque la columna es para una relación de identificación.

### <a name="the-instructor-navigation-property"></a>La propiedad de navegación de Instructor

La entidad Instructor tiene una que aceptan valores NULL `OfficeAssignment` propiedad de navegación (porque un instructor no podría tener una asignación de oficina), y la entidad OfficeAssignment tiene un acepta valores NULL `Instructor` propiedad de navegación (debido a una asignación de office no se puede existir sin un instructor-- `InstructorID` no acepta valores NULL). Cuando una entidad Instructor tiene una entidad OfficeAssignment relacionada, cada entidad tendrá una referencia a la otra en la propiedad de navegación.

Podría poner un `[Required]` atributo en la propiedad de navegación de Instructor para especificar que debe haber un instructor relacionado, pero no tiene que hacerlo porque el `InstructorID` clave externa (que también es la clave para esta tabla) es que no aceptan valores NULL.

## <a name="modify-the-course-entity"></a>Modificar la entidad de curso

![Entidad de curso](complex-data-model/_static/course-entity.png)

En *Models/Course.cs*, reemplace el código que agregó anteriormente con el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

La entidad de curso tiene una propiedad de clave externa `DepartmentID` que señala a la entidad relacionada de departamento y tiene un `Department` propiedad de navegación.

Entity Framework no requiere que agregue una propiedad de clave externa para el modelo de datos cuando tenga una propiedad de navegación para una entidad relacionada.  EF crea claves externas en la base de datos cuando son necesarias y crea automáticamente [sombrear propiedades](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para ellos. Pero con la clave externa en el modelo de datos puede hacer que las actualizaciones más sencillo y más eficaz. Por ejemplo, al recuperar una entidad de curso para editar, la entidad Department es null si no lo carga, por lo que cuando se actualiza la entidad de curso, deberá primero capturar la entidad Department. Cuando la propiedad de clave externa `DepartmentID` se incluye en el modelo de datos, no es necesario capturar la entidad Department antes de actualizar.

### <a name="the-databasegenerated-attribute"></a>El atributo DatabaseGenerated

El `DatabaseGenerated` atribuir a la `None` parámetro en el `CourseID` propiedad especifica que los valores de clave principales se proporcionado por el usuario, en lugar de generado por la base de datos.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

De forma predeterminada, Entity Framework se da por supuesto que la base de datos genera valores de clave principal. Es lo que desea en la mayoría de los escenarios. Sin embargo, para las entidades de curso, podrá utilizar un número de curso especificado por el usuario como una serie de 1000 para un departamento, una serie de 2000 para otro departamento y así sucesivamente.

El `DatabaseGenerated` atributo también se puede usar para generar valores predeterminados, como en el caso de columnas de base de datos que se usan para registrar la fecha se crea o actualiza una fila.  Para obtener más información, consulte [genera propiedades](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de navegación y de clave externa

Las propiedades de clave externa y propiedades de navegación de la entidad de curso reflejan las relaciones siguientes:

Un curso se asigna a un departamento, por lo que hay un `DepartmentID` clave externa y un `Department` propiedad de navegación por las razones mencionadas anteriormente.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Un curso puede tener cualquier número de alumnos inscritos en él, por lo que el `Enrollments` propiedad de navegación es una colección:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Un curso puede ser imparten por varios instructores, por lo que la `CourseAssignments` propiedad de navegación es una colección (el tipo `CourseAssignment` se explica [más adelante](#many-to-many-relationships)):

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a>Crear la entidad Department

![Entidad Department](complex-data-model/_static/department-entity.png)


Crear *Models/Department.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>El atributo de columna

Versiones anteriores utilizan el `Column` atributo para cambiar la asignación de nombres de columna. En el código de la entidad Department, el `Column` atributo es que se va a utilizar para cambiar SQL asignar tipos de datos para que la columna se definirá con el tipo de moneda de SQL Server en la base de datos:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Asignación de columnas por lo general no es necesaria, dado que Entity Framework elige el tipo de datos de SQL Server adecuado en función del tipo CLR que se define para la propiedad. El CLR `decimal` tipo se asigna a un servidor SQL Server `decimal` tipo. Pero en este caso se sabe que la columna se pueden contener cantidades de moneda, y es más adecuado para el tipo de datos de moneda.

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de navegación y de clave externa

Las propiedades de navegación y de clave externas reflejen las relaciones siguientes:

Un departamento puede tener o no un administrador, y un administrador es siempre un instructor. Por lo tanto, la `InstructorID` propiedad se incluye como la clave externa para la entidad Instructor y se agrega un signo de interrogación después de la `int` escriba designación para marcar la propiedad como que acepta valores NULL. La propiedad de navegación se denomina `Administrator` pero contiene una entidad Instructor:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

Un departamento puede tener varios cursos, por lo que es una propiedad de navegación de cursos:

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> Por convención, Entity Framework permite la eliminación en cascada para las claves externas que no aceptan valores NULL y para las relaciones de varios a varios. Esto puede dar lugar a eliminar reglas de cascada circular, lo que producirán una excepción al intentar agregar una migración. Por ejemplo, si no define la propiedad Department.InstructorID como que acepta valores NULL, EF podría configurar una regla de eliminación en cascada para eliminar el instructor cuando se elimina el departamento, que no es lo que desea que ocurra. Si es necesario sus reglas de negocios el `InstructorID` propiedad no aceptan valores NULL, se tendría que usar la siguiente instrucción de la API fluida para deshabilitar la eliminación en cascada en la relación:
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a>Modificar la entidad de inscripción

![Entidad de inscripción](complex-data-model/_static/enrollment-entity.png)

En *Models/Enrollment.cs*, reemplace el código que agregó anteriormente con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de navegación y de clave externa

Las propiedades de clave externa y propiedades de navegación reflejan las relaciones siguientes:

Un registro de inscripción es para un solo curso, por lo que hay un `CourseID` propiedad de clave externa y un `Course` propiedad de navegación:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Un registro de inscripción es para un estudiante único, por lo que hay un `StudentID` propiedad de clave externa y un `Student` propiedad de navegación:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relaciones Varios a Varios

Hay una relación de varios a varios entre las entidades de estudiante y curso y la entidad de inscripción funciona como una tabla de combinación-to-many *con carga* en la base de datos. "Con una carga" significa que la tabla Enrollment contiene datos adicionales además de las claves externas de las tablas combinadas (en este caso, una clave principal y una propiedad de grado).

En la siguiente ilustración muestra el aspecto de estas relaciones en un diagrama de la entidad. (Este diagrama se ha generado mediante las herramientas de alimentación de Entity Framework para EF 6.x; crear el diagrama no forma parte del tutorial, se utilizan simplemente aquí como una ilustración.)

![Curso de estudiante muchos a muchos relación](complex-data-model/_static/student-course.png)

Cada línea de relación tiene un 1 en un extremo y un asterisco (*) en el otro, que indica una relación uno a varios.

Si la tabla Enrollment no incluye la información de categoría, que solo sería necesario contener las dos claves externas CourseID y StudentID. En ese caso, sería una tabla sin carga una combinación de varios a varios (o una tabla combinada pura) en la base de datos. Las entidades Instructor y el curso tienen ese tipo de relación de varios a varios, y el paso siguiente consiste en crear una clase de entidad para que funcione como una tabla de combinación sin carga.

(EF 6.x es compatible con las tablas de unión implícita para relaciones de varios a varios, pero el núcleo de EF no lo hace. Para obtener más información, consulte el [explicación en el repositorio de GitHub de núcleo de EF](https://github.com/aspnet/EntityFramework/issues/1368).) 

## <a name="the-courseassignment-entity"></a>La entidad CourseAssignment

![Entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Crear *Models/CourseAssignment.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a>Unirse a nombres de entidad

Se requiere una tabla de combinación en la base de datos para la relación de varios a varios Instructor a cursos y tiene que ser representado por un conjunto de entidades. Es común para el nombre de una entidad de combinación `EntityName1EntityName2`, que en este caso sería `CourseInstructor`. Sin embargo, se recomienda que elija un nombre que describa la relación. Modelos de datos empiezan de manera sencilla y crecen con combinaciones de carga no obtener con frecuencia cargas más tarde. Si empieza con un nombre descriptivo de entidad, no tendrás que cambiar el nombre más adelante. Idealmente, la entidad de combinación tendrá su propio nombre (posiblemente sola palabra) natural en el dominio de negocio. Por ejemplo, podrían vincularse a través de las clasificaciones de libros y los clientes. Para esta relación, `CourseAssignment` es una opción mejor que `CourseInstructor`.

### <a name="composite-key"></a>Clave compuesta

Puesto que las claves externas no son que aceptan valores NULL y juntos forma única identifican cada fila de la tabla, no es necesario para una clave principal independiente. El *InstructorID* y *CourseID* propiedades deben funcionar como una clave principal compuesta. Es la única manera de identificar las claves principales compuestas a EF mediante la *API fluida* (no se puede realizar mediante el uso de atributos). Verá cómo configurar la clave principal compuesta en la sección siguiente.

La clave compuesta garantiza que, aunque es posible tener varias filas para un curso y varias filas para un instructor, no puede tener varias filas para el mismo instructor y el curso. El `Enrollment` combinación entidad define su propia clave principal, por lo que son posibles duplicados de este tipo. Para evitar los duplicados, podría agregar un índice único en los campos de clave externos o configurar `Enrollment` con una clave compuesta principal similar a `CourseAssignment`. Para obtener más información, consulte [índices](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-database-context"></a>Actualizar el contexto de base de datos

Agregue el código resaltado siguiente a la *Data/SchoolContext.cs* archivo:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

Este código agrega las nuevas entidades y configura la clave principal compuesta de la entidad CourseAssignment.

## <a name="fluent-api-alternative-to-attributes"></a>Alternativa de API fluida a atributos

El código en el `OnModelCreating` método de la `DbContext` clase utiliza el *API fluida* para configurar el comportamiento EF. La API se denomina "fluida" porque a menudo se usa por tender una serie de llamadas al método juntas en una única instrucción, como en este ejemplo desde la [documentación principal de EF](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

En este tutorial, que está usando la API fluida solo para la asignación de base de datos que no se puede realizar con atributos. Sin embargo, también puede utilizar la API fluida para especificar casi todo el formato, la validación y las reglas de asignación que se pueden realizar mediante el uso de atributos. Algunos atributos como `MinimumLength` no se puede aplicar con la API fluida. Como se mencionó anteriormente, `MinimumLength` no cambia el esquema, sólo se aplica una regla de validación del lado cliente y el servidor.

Algunos desarrolladores prefieren usar la API fluida exclusivamente para que pueden mantener las clases de entidad "limpia". Puede mezclar atributos y API fluida si quieres, hay algunas personalizaciones que solo pueden realizarse mediante el uso de la API fluida, pero en general la práctica recomendada es elegir uno de estos dos enfoques y utilizar ese constantemente lo máximo posible. Si utiliza ambos, tenga en cuenta que siempre que hay un conflicto, API fluida de invalidaciones de atributos.

Para obtener más información acerca de los atributos frente a la API fluida, consulte [métodos de configuración](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Diagrama mostrando relaciones de entidad

En la siguiente ilustración muestra el diagrama creados por las herramientas de alimentación de Entity Framework para el modelo School completado.

![Diagrama de entidad](complex-data-model/_static/diagram.png)

Además de las líneas de relación de uno a varios (1 a \*), aquí se puede ver la línea de relación de uno a cero o uno (1 a 0.. 1) entre las entidades Instructor y OfficeAssignment y la línea de relación de cero-o-uno a varios (0.. 1 a *) entre el Entidades de instructor y departamento.

## <a name="seed-the-database-with-test-data"></a>Valor de inicialización de la base de datos con datos de prueba

Reemplace el código de la *Data/DbInitializer.cs* archivo con el código siguiente con el fin de proporcionar datos de inicialización para las nuevas entidades que ha creado.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

Como vimos en el primer tutorial, la mayor parte de este código simplemente crea nuevos objetos de entidad y carga los datos de ejemplo en propiedades según sea necesario para las pruebas. Tenga en cuenta cómo se administran las relaciones de varios a varios: el código crea relaciones mediante la creación de entidades en el `Enrollments` y `CourseAssignment` combinar conjuntos de entidades.

## <a name="add-a-migration"></a>Agregar una migración

Guarde los cambios y compile el proyecto. A continuación, abra la ventana de comandos en la carpeta del proyecto y escriba el `migrations add` comando (no el comando update-database aún):

```console
dotnet ef migrations add ComplexDataModel
```

Recibe una advertencia acerca de la posible pérdida de datos.

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Si intenta ejecutar la `database update` de comandos en este momento (no lo hace aún), se obtendrá el error siguiente:

> La instrucción ALTER TABLE entran en conflicto con la restricción FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID". Se produjo el conflicto en la tabla base de datos "ContosoUniversity", "dbo. Departamento", columna 'DepartmentID'.

En ocasiones, al ejecutar migraciones con los datos existentes, debe insertar los datos de código auxiliar en la base de datos para satisfacer las restricciones foreign key. El código generado en el `Up` método agrega una clave externa de DepartmentID no acepta valores NULL a la tabla Course. Si ya hay filas en la tabla Course cuando se ejecuta el código, el `AddColumn` se produce un error en la operación porque SQL Server no conoce el valor que se incluirán en la columna que no puede ser nula. En este tutorial se va a ejecutar la migración en una base de datos, pero en una aplicación de producción tendría que realizar la migración a administrar los datos existentes, por lo que las instrucciones siguientes muestran un ejemplo de cómo hacerlo.

Para realizar esta migración trabajar con datos existentes, que tiene que cambiar el código para asignar un valor predeterminado de la nueva columna y crear un departamento de recibos denominado "Temp" para que actúe como el departamento de forma predeterminada. Como resultado, existente filas curso estarán todos relacionados con el departamento "Temp" después de la `Up` ejecuciones de método.

* Abra la *{timestamp}_ComplexDataModel.cs* archivo. 

* Convertir en comentario la línea de código que agrega la columna DepartmentID a la tabla Course.

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* Agregue el código resaltado siguiente después del código que crea la tabla de departamento:

  [!code-csharp[Main](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

En una aplicación de producción, debería escribir código o secuencias de comandos para agregar filas de departamento y relacionados con filas de curso para las nuevas filas de departamento. A continuación, ya no tendría el departamento "Temp" o el valor predeterminado en la columna Course.DepartmentID.

Guarde los cambios y compile el proyecto.

## <a name="change-the-connection-string-and-update-the-database"></a>Cambie la cadena de conexión y actualizar la base de datos

Ahora tienes código nuevo el `DbInitializer` clase que agrega datos de inicialización para las nuevas entidades a una base de datos vacía. Para asegurarse de EF crear una nueva base de datos vacía, cambia el nombre de la base de datos en la cadena de conexión en *appSettings.JSON que se* a ContosoUniversity3 o algún otro nombre que no has utilizado en el equipo que está utilizando.

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

Guarde el cambio a *appSettings.JSON que se*.

> [!NOTE]
> Como alternativa a cambiar el nombre de la base de datos, puede eliminar la base de datos. Use **Explorador de objetos de SQL Server** (SSOX) o `database drop` comando de CLI:
> ```console
> dotnet ef database drop
> ```

Después de haber cambiado el nombre de la base de datos o eliminar la base de datos, ejecute el `database update` comando en la ventana de comandos para ejecutar las migraciones.

```console
dotnet ef database update
```

Ejecutar la aplicación para hacer que el `DbInitializer.Initialize` método para ejecutar y rellenar la base de datos nueva.

Abra la base de datos de SSOX como lo hizo anteriormente y expanda el **tablas** nodo para ver que todas las tablas se han creado. (Si todavía tiene SSOX abrir desde la hora anterior, haga clic en el botón Actualizar).

![Tablas de SSOX](complex-data-model/_static/ssox-tables.png)

Ejecute la aplicación para desencadenar el código de inicializador que propaga la base de datos.

Haga clic en el **CourseAssignment** de tabla y seleccione **ver datos** para comprobar que tiene datos en ella.

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a>Resumen

Ahora tiene un modelo de datos más compleja y una base de datos correspondiente. En el siguiente tutorial, aprenderá más acerca de cómo obtener acceso a datos relacionados.

>[!div class="step-by-step"]
[Anterior](migrations.md)
[Siguiente](read-related-data.md)  
