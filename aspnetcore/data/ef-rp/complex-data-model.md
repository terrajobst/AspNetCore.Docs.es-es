---
title: "Páginas de Razor con EF Core - modelo de datos - 5 de 8"
author: rick-anderson
description: "En este tutorial agregará más entidades y relaciones y personalizar el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos."
keywords: Anotaciones de datos de ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 10/25/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: c2761f79fa4836d29541526782969bb0fd47778b
ms.sourcegitcommit: 6e46abd65973dea796d364a514de9ec2e3e1c1ed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2017
---
# <a name="creating-a-complex-data-model---ef-core-with-razor-pages-tutorial-5-of-8"></a>Crear un modelo de datos complejos - Core EF con el tutorial de las páginas de Razor (5 de 8)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

Los tutoriales anteriores ha trabajado con un modelo de datos básicos que se compone de tres entidades. En este tutorial:

* Se agregan más entidades y relaciones.
* El modelo de datos se ha personalizado mediante la especificación de formato, validación y reglas de asignación de la base de datos.

Las clases de entidad para el modelo de datos completa se muestra en la siguiente ilustración:

![Diagrama de entidad](complex-data-model/_static/diagram.png)

Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Personalizar el modelo de datos con atributos

En esta sección, el modelo de datos para personalizar el uso de atributos.

### <a name="the-datatype-attribute"></a>El atributo de tipo de datos

Las páginas de estudiante actualmente muestra la hora de la fecha de inscripción. Normalmente, los campos de fecha muestran solo la fecha y no la hora.

Actualización *Models/Student.cs* aparece resaltado este código con lo siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

El [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atributo especifica un tipo de datos que es más específico que el tipo intrínseco de base de datos. En este caso que se debe mostrar solo la fecha, no la fecha y hora. El [enumeración DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) proporciona para muchos tipos de datos, como fecha, hora, número de teléfono, moneda, EmailAddress, etcetera. El `DataType` atributo también puede permitir que la aplicación proporcionar automáticamente las características específicas del tipo. Por ejemplo:

* El `mailto:` vínculo se crea automáticamente para `DataType.EmailAddress`.
* El selector de fecha se proporciona para `DataType.Date` en la mayoría de los exploradores.

El `DataType` atributo emite HTML 5 `data-` atributos (pronunciado datos dash) que usan los exploradores HTML 5. El `DataType` atributos no proporcione la validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De forma predeterminada, el campo de fecha se muestra según los formatos predeterminados en función del servidor [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

El `ApplyFormatInEditMode` configuración especifica que el formato debe también se aplica a la interfaz de usuario de edición. Algunos campos no deben usar `ApplyFormatInEditMode`. Por ejemplo, el símbolo de moneda generalmente no debe mostrarse en un cuadro de texto de edición.

El `DisplayFormat` atributo puede usarse por sí mismo. Suele ser una buena idea usar el `DataType` atribuir a la `DisplayFormat` atributo. El `DataType` atributo transmite la semántica de los datos en lugar de cómo representar en una pantalla. El `DataType` atributo proporciona las siguientes ventajas que no están disponibles en `DisplayFormat`:

* El explorador puede habilitar características de HTML5. Por ejemplo, mostrar un control de calendario, el símbolo de moneda adecuado para la configuración regional, vínculos de correo electrónico, validación de entrada del lado cliente, etcetera.
* De forma predeterminada, el explorador representa datos con el formato correcto según la configuración regional.

Para obtener más información, consulte el [ \<entrada > documentación de la aplicación auxiliar de etiqueta](xref:mvc/views/working-with-forms#the-input-tag-helper).

Ejecute la aplicación. Navegue a la página de índice de los alumnos. Veces ya no se muestran. Todas las vistas que usa el `Student` modelo muestra la fecha sin hora.

![Página de índice de los alumnos mostrando las fechas sin veces](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>El atributo StringLength

Reglas de validación de datos y mensajes de error de validación pueden especificarse con atributos. El [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atributo especifica la longitud mínima y máxima de caracteres que se permiten en un campo de datos. El `StringLength` atributo también proporciona validación de cliente y servidor. El valor mínimo no influye en el esquema de base de datos.

Actualización de la `Student` modelo con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

El código anterior limita los nombres a no más de 50 caracteres. El `StringLength` atributo no impide que un usuario entraran en un espacio en blanco para un nombre. El [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atributo se usa para aplicar restricciones a la entrada. Por ejemplo, el siguiente código requiere el primer carácter que se va letra mayúscula y el resto de caracteres sea alfabético:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]
```

Ejecute la aplicación:

* Navegue a la página de los alumnos.
* Seleccione **crear nuevo**y escriba un nombre más de 50 caracteres.
* Seleccione **crear**, validación del lado cliente muestra un mensaje de error.

![Página que muestra los errores de longitud de cadena de índice de estudiantes](complex-data-model/_static/string-length-errors.png)

En **Explorador de objetos de SQL Server** (SSOX), abra el Diseñador de tablas de Student haciendo doble clic en el **estudiante** tabla.

![Tabla de estudiantes en SSOX antes de las migraciones](complex-data-model/_static/ssox-before-migration.png)

La imagen anterior muestra el esquema para el `Student` tabla. Los campos de nombre tengan tipo `nvarchar(MAX)` porque las migraciones no se ha ejecutado en la base de datos. Cuando se ejecutan las migraciones más adelante en este tutorial, se convierten en los campos de nombre `nvarchar(50)`.

### <a name="the-column-attribute"></a>El atributo de columna

Atributos pueden controlar cómo se asignan las clases y propiedades de la base de datos. En esta sección, el `Column` atributo se utiliza para asignar el nombre de la `FirstMidName` propiedad en "FirstName" en la base de datos.

Cuando se crea la base de datos, los nombres de propiedad en el modelo se usan para los nombres de columna (excepto cuando la `Column` atributo se usa).

El `Student` modelo utiliza `FirstMidName` para el nombre de campo ya que el campo puede contener también un segundo nombre.

Actualización de la *Student.cs* archivo con el código resaltado siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Con el cambio anterior, `Student.FirstMidName` en la aplicación se asigna a la `FirstName` columna de la `Student` tabla.

La adición de la `Column` atributo cambia la copia de seguridad de modelo la `SchoolContext`. La copia de seguridad de modelo el `SchoolContext` ya no coincide con la base de datos. Si la aplicación se ejecuta antes de aplicar las migraciones, se genera la siguiente excepción:

```SQL
SqlException: Invalid column name 'FirstName'.
```
Para actualizar la base de datos:

* Compile el proyecto.
* Abra una ventana de comandos en la carpeta del proyecto. Escriba los comandos siguientes para crear una nueva migración y actualizar la base de datos:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

El `dotnet ef migrations add ColumnFirstName` comando genera el mensaje de advertencia siguiente:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

Se genera la advertencia porque los campos de nombre ahora están limitadas a 50 caracteres. Si un nombre en la base de datos tenía más de 50 caracteres, se perderían el 51 hasta el último carácter.

* Probar la aplicación.

Abra la tabla de estudiantes en SSOX:

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

Antes de que se ha aplicado la migración, las columnas del nombre eran del tipo [nvarchar (max)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Las columnas de nombre son ahora `nvarchar(50)`. Ha cambiado el nombre de columna de `FirstMidName` a `FirstName`.

> [!Note]
> En la sección siguiente, la creación de la aplicación en algunas de las fases genera errores del compilador. Las instrucciones de especifican cuándo se deben compilar la aplicación.

## <a name="student-entity-update"></a>Actualizar la entidad Student

![Entidad Student](complex-data-model/_static/student-entity.png)

Actualización *Models/Student.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>El atributo requerido

El `Required` atributo hace que los campos obligatorios de propiedades de nombre. El `Required` atributo no es necesario para los tipos que no aceptan valores NULL, como tipos de valor (`DateTime`, `int`, `double`, etcetera.). Tipos que no pueden ser nulos se tratan automáticamente como campos obligatorios.

El `Required` atributo se podría reemplazar con un parámetro de longitud mínima en el `StringLength` atributo:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>El atributo de visualización

El `Display` atributo especifica que el título de los cuadros de texto debe ser "Nombre", "Last Name", "Nombre completo" y "Fecha de inscripción". Los títulos predeterminados que no tenían ningún espacio al dividir las palabras, por ejemplo "apellido".

### <a name="the-fullname-calculated-property"></a>La propiedad FullName calculado

`FullName`es una propiedad calculada que devuelve un valor que se crea concatenando dos otras propiedades. `FullName`no se puede establecer, tiene solo un descriptor de acceso get. Ya no `FullName` columna se crea en la base de datos.

## <a name="create-the-instructor-entity"></a>Crear la entidad Instructor

![Entidad instructor](complex-data-model/_static/instructor-entity.png)

Crear *Models/Instructor.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Tenga en cuenta que varias propiedades son los mismos en el `Student` y `Instructor` entidades. En el tutorial de herencia de implementación más adelante en esta serie, este código se refactoriza para eliminar la redundancia.

Varios atributos pueden estar en una sola línea. El `HireDate` atributos pudieron escribirse de la manera siguiente:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Las propiedades de navegación CourseAssignments y OfficeAssignment

El `CourseAssignments` y `OfficeAssignment` las propiedades son propiedades de navegación.

Un instructor puede enseñar a cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Si una propiedad de navegación contiene varias entidades:

* Debe ser un tipo de lista, donde pueden agregar, eliminar y actualizar las entradas.

Los tipos de propiedad de navegación incluyen:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Si `ICollection<T>` se especifica, Core EF crea un `HashSet<T>` colección de forma predeterminada.

El `CourseAssignment` entidad se explica en la sección acerca de las relaciones de varios a varios.

Estado que un instructor puede tener a lo sumo una oficina de reglas de negocios de la Universidad de contoso. El `OfficeAssignment` propiedad contiene un único `OfficeAssignment` entidad. `OfficeAssignment`es null si no se asigna ningún office.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Crear la entidad OfficeAssignment

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Crear *Models/OfficeAssignment.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>Key (atributo)

El `[Key]` atributo se usa para identificar una propiedad como la clave principal (PK) cuando el nombre de propiedad es algo que no sean classnameID o identificador.

Hay una relación de uno a cero o uno entre la `Instructor` y `OfficeAssignment` entidades. Solo existe una asignación de oficina en relación con el instructor se asigna a. El `OfficeAssignment` PK también es la clave externa (FK) a la `Instructor` entidad. El núcleo de EF no se reconoce automáticamente `InstructorID` como la clave principal de `OfficeAssignment` porque:

* `InstructorID`no siga la convención de nomenclatura de Id. o classnameID.

Por lo tanto, la `Key` atributo se usa para identificar `InstructorID` como la clave principal:

```csharp
[Key]
public int InstructorID { get; set; }
```

De forma predeterminada, Core EF trata la clave como no generada por base de datos porque la columna es para una relación de identificación.

### <a name="the-instructor-navigation-property"></a>La propiedad de navegación de Instructor

El `OfficeAssignment` propiedad de navegación para el `Instructor` entidad es que acepta valores NULL porque:

* Tipos de referencia (como clases admiten valores NULL).
* Un instructor podría no tener una asignación de oficina.


El `OfficeAssignment` entidad tiene un acepta valores NULL `Instructor` propiedad de navegación porque:

* `InstructorID`es que no aceptan valores NULL.
* Una asignación de office no puede existir sin un instructor.

Cuando un `Instructor` entidad tiene un relacionados `OfficeAssignment` entidad, cada entidad tiene una referencia a la otra en la propiedad de navegación.

El `[Required]` atributo puede aplicarse a la `Instructor` propiedad de navegación:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

El código anterior especifica que debe haber un instructor relacionado. El código anterior no es necesario porque el `InstructorID` clave externa (que también es la clave principal) es que no aceptan valores NULL.

## <a name="modify-the-course-entity"></a>Modificar la entidad de curso

![Entidad de curso](complex-data-model/_static/course-entity.png)

Actualización *Models/Course.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

El `Course` entidad tiene una propiedad de clave externa (FK) `DepartmentID`. `DepartmentID`apunta a relacionado `Department` entidad. El `Course` entidad tiene un `Department` propiedad de navegación.

Núcleo EF no requiere una propiedad de clave externa para un modelo de datos cuando el modelo tiene una propiedad de navegación para una entidad relacionada.

Núcleo EF crea automáticamente claves externas en la base de datos cuando son necesarias. Núcleo EF crea [sombrear propiedades](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para claves externas creadas automáticamente. Con la clave externa en el modelo de datos puede realizar actualizaciones más sencillo y más eficaz. Por ejemplo, considere la posibilidad de un modelo donde la propiedad FK `DepartmentID` es *no* incluido. Cuando se captura una entidad de curso para editar:

* El `Department` entity es null si no explícitamente está cargado.
* Para actualizar la entidad de curso, la `Department` en primer lugar debe buscarse en entidad.

Cuando la propiedad FK `DepartmentID` se incluye en el modelo de datos, no es necesario para capturar el `Department` entidad antes de una actualización.

### <a name="the-databasegenerated-attribute"></a>El atributo DatabaseGenerated

El `[DatabaseGenerated(DatabaseGeneratedOption.None)]` atributo especifica que la clave principal es proporcionada por la aplicación, en lugar de generado por la base de datos.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

De forma predeterminada, EF Core se da por supuesto que la base de datos genera valores de clave principal. Base de datos genera PK valores suele ser el mejor método. Para `Course` entidades, el usuario especifica la PK. Por ejemplo, un número de curso como una serie de 1000 departamento matemáticas, una serie de 2000 para el departamento de inglés.

El `DatabaseGenerated` atributo también se puede usar para generar valores predeterminados. Por ejemplo, la base de datos puede generar automáticamente un campo de fecha para registrar la fecha en que se crea o actualiza una fila. Para obtener más información, consulte [genera propiedades](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de navegación y de clave externa

Las propiedades de clave externa (FK) y propiedades de navegación de la `Course` entidad reflejen las relaciones siguientes:

Un curso se asigna a un departamento, por lo que hay un `DepartmentID` FK y un `Department` propiedad de navegación.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Un curso puede tener cualquier número de alumnos inscritos en él, por lo que el `Enrollments` propiedad de navegación es una colección:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Un curso puede ser imparten por varios instructores, por lo que el `CourseAssignments` propiedad de navegación es una colección:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment`se explica [más adelante](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Crear la entidad Department

![Entidad Department](complex-data-model/_static/department-entity.png)

Crear *Models/Department.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>El atributo de columna

Anteriormente la `Column` atributo se utiliza para cambiar la asignación de nombre de columna. En el código de la `Department` entidad, el `Column` atributo se utiliza para cambiar la asignación de tipos de datos SQL. La `Budget` columna se define mediante el tipo de moneda de SQL Server en la base de datos:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Asignación de columnas por lo general no es necesaria. Núcleo EF generalmente elige el tipo de datos de SQL Server apropiado en función del tipo CLR para la propiedad. El CLR `decimal` tipo se asigna a un servidor SQL Server `decimal` tipo. `Budget`es para la moneda, y el tipo de datos de moneda es más adecuado para la moneda.

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de navegación y de clave externa

Las propiedades de navegación y FK reflejan las relaciones siguientes:

* Un departamento puede tener o no un administrador.
* Un administrador es siempre un instructor. Por lo tanto, la `InstructorID` propiedad se incluye como la clave externa a la `Instructor` entidad.

La propiedad de navegación se denomina `Administrator` pero contiene un `Instructor` entidad:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

El signo de interrogación (?) en el código anterior especifica la propiedad admite valores NULL.

Un departamento puede tener varios cursos, por lo que es una propiedad de navegación de cursos:

```csharp
public ICollection<Course> Courses { get; set; }
```

Nota: Por convención, núcleo de EF permite la eliminación en cascada para claves externas que no aceptan valores NULL y para las relaciones de varios a varios. Eliminación en cascada puede dar lugar a las reglas de eliminación en cascada circular. Circular eliminación en cascada causas reglas una excepción cuando se agrega una migración.

Por ejemplo, si la `Department.InstructorID` propiedad no se definió como que acepta valores NULL:

* Núcleo EF configura una regla de eliminación en cascada para eliminar el instructor cuando se elimina el departamento.
* Eliminar el instructor cuando se elimina el departamento no es el comportamiento deseado.

Si es necesario de las reglas de negocios el `InstructorID` propiedad ser acepta valores NULL, utilice la siguiente instrucción de la API fluida:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

El código anterior deshabilita la eliminación en cascada en la relación de instructor de departamento.

## <a name="update-the-enrollment-entity"></a>Actualizar la entidad de inscripción

Es un registro de inscripción para un uno curso realizado por un alumno.

![Entidad de inscripción](complex-data-model/_static/enrollment-entity.png)

Actualización *Models/Enrollment.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de navegación y de clave externa

Las propiedades de clave externa y propiedades de navegación reflejen las relaciones siguientes:

Un registro de inscripción es para un curso, por lo que hay un `CourseID` propiedad de clave externa y un `Course` propiedad de navegación:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Es un registro de inscripción de un estudiante, por lo que hay un `StudentID` propiedad de clave externa y un `Student` propiedad de navegación:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relaciones Varios a Varios

Hay una relación de varios a varios entre la `Student` y `Course` entidades. El `Enrollment` entidad funciona como una tabla de combinación-to-many *con carga* en la base de datos. "Con una carga" significa que la `Enrollment` tabla contiene datos adicionales además de claves externas de las tablas combinadas (en este caso, la clave principal y `Grade`).

En la siguiente ilustración muestra el aspecto de estas relaciones en un diagrama de la entidad. (Este diagrama se ha generado mediante herramientas avanzadas de EF de EF 6.x. Crear el diagrama no forma parte del tutorial.)

![Curso de estudiante muchos a muchos relación](complex-data-model/_static/student-course.png)

Cada línea de relación tiene un 1 en un extremo y un asterisco (/*) en el otro, que indica una relación uno a varios.

Si el `Enrollment` tabla no incluir información de categoría, que solo sería necesario contener las claves dos externas (`CourseID` y `StudentID`). Una tabla sin carga una combinación de varios a varios se suele denominar una tabla combinada pura (PJT).

El `Instructor` y `Course` entidades tienen una relación de varios a varios con una tabla combinada pura.

Nota: EF 6.x es compatible con las tablas de unión implícita para relaciones de varios a varios, pero el núcleo de EF no lo hace. Para obtener más información, consulte [-to-many relaciones en EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).

## <a name="the-courseassignment-entity"></a>La entidad CourseAssignment

![Entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Crear *Models/CourseAssignment.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Cursos de instructor

![Instructor a cursos m:M](complex-data-model/_static/courseassignment.png)

La relación de varios a varios Instructor a cursos:

* Requiere una tabla de combinación que debe estar representada por un conjunto de entidades.
* Es una tabla combinada pura (tabla sin carga).

Es común para el nombre de una entidad de combinación `EntityName1EntityName2`. Por ejemplo, la tabla de combinación Instructor a cursos mediante este patrón es `CourseInstructor`. Sin embargo, se recomienda usar un nombre que describe la relación.

Modelos de datos empiezan de manera sencilla y crecen. No carga combinaciones (PJTs) evolucionan con frecuencia para incluir la carga. A partir de un nombre de entidad descriptivo, el nombre no tiene que cambiar cuando cambia de la tabla de combinación. Idealmente, la entidad de combinación tendrá su propio nombre (posiblemente sola palabra) natural en el dominio de negocio. Por ejemplo, libros y los clientes pueden vincularse a una entidad de combinación denominada clasificaciones. La relación de varios a varios Instructor a cursos, `CourseAssignment` es preferible `CourseInstructor`.

### <a name="composite-key"></a>Clave compuesta

Claves externas no admiten valores NULL. Las claves dos externas en `CourseAssignment` (`InstructorID` y `CourseID`) juntos identifican de forma única cada fila de la `CourseAssignment` tabla. `CourseAssignment`no requiere un PK dedicado. El `InstructorID` y `CourseID` propiedades funcionan como un PK compuesto. Es la única manera de especificar PK compuesto a EF Core con el *API fluida*. La sección siguiente muestra cómo configurar la PK compuesto.

Se asegura de la clave compuesta:

* Se permiten varias filas para un curso.
* Se permiten varias filas para un instructor.
* No se permiten varias filas para el mismo instructor y curso.

El `Enrollment` combinación entidad define su propio PK, por lo que son posibles duplicados de este tipo. Para evitar los duplicados:

* Agregar un índice único en los campos de clave externa, o
* Configurar `Enrollment` con una clave compuesta principal similar a `CourseAssignment`. Para obtener más información, consulte [índices](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Actualizar el contexto de base de datos

Agregue el código que aparece resaltado a *Data/SchoolContext.cs*:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

El código anterior agrega las nuevas entidades y configura el `CourseAssignment` PK. compuesto de la entidad

## <a name="fluent-api-alternative-to-attributes"></a>Alternativa de API fluida a atributos

El `OnModelCreating` método en el anterior código usa el *API fluida* para configurar el comportamiento básico de EF. La API se denomina "fluida" porque a menudo se usa por tender una serie de llamadas al método juntos en una única instrucción. El [sigue código](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) es un ejemplo de la API fluida de:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

En este tutorial, la API fluida se usa solo para la asignación de base de datos que no se puede realizar con atributos. Sin embargo, la API fluida puede especificar casi todo el formato, la validación y las reglas de asignación que se pueden realizar con atributos.

Algunos atributos como `MinimumLength` no se puede aplicar con la API fluida. `MinimumLength`no cambia el esquema, sólo se aplica una regla de validación de longitud mínima.

Algunos desarrolladores prefieren usar la API fluida exclusivamente para que pueden mantener las clases de entidad "limpia". Se pueden mezclar atributos y la API fluida. Hay algunas configuraciones que solo pueden realizarse con la API fluida (especificar una clave principal compuesta). Hay algunas configuraciones que solo pueden realizarse con atributos (`MinimumLength`). La práctica recomendada para el uso de atributos o API fluida:

* Elija uno de estos dos enfoques.
* Usar el método elegido constantemente lo máximo posible.

Algunos de los atributos utilizados en este tutorial se utilizan para:

* Validación solo (por ejemplo, `MinimumLength`).
* Solo la configuración de núcleo EF (por ejemplo, `HasKey`).
* Configuración de validación y EF principal (por ejemplo, `[StringLength(50)]`).

Para obtener más información acerca de los atributos frente a la API fluida, consulte [métodos de configuración](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Diagrama mostrando relaciones de entidad

En la siguiente ilustración muestra el diagrama que EF Power Tools crear para el modelo School completado.

![Diagrama de entidad](complex-data-model/_static/diagram.png)

Muestra el diagrama anterior:

* Varias líneas de relación de uno a varios (1 a \*).
* La línea de relación de uno a cero o uno (1 a 0.. 1) entre el `Instructor` y `OfficeAssignment` entidades.
* La línea de relación de cero-o-uno a varios (0.. 1 a *) entre el `Instructor` y `Department` entidades.

## <a name="seed-the-db-with-test-data"></a>Valor de inicialización de la base de datos con datos de prueba

Actualice el código en *Data/DbInitializer.cs*:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

El código anterior proporciona datos de inicialización para las nuevas entidades. La mayor parte de este código crea nuevos objetos de entidad y carga los datos de ejemplo. Los datos de ejemplo se utilizan para pruebas. El código anterior crea las siguientes relaciones de varios a varios:

* `Enrollments`
* `CourseAssignment`

Nota: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) admitirá [propagación de datos](https://github.com/aspnet/EntityFrameworkCore/issues/629).

## <a name="add-a-migration"></a>Agregar una migración

Compile el proyecto. Abra una ventana de comandos en la carpeta de proyecto y escriba el siguiente comando:

```console
dotnet ef migrations add ComplexDataModel
```

El comando anterior muestra una advertencia sobre la posible pérdida de datos.

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

Si el `database update` se ejecuta el comando, se genera el error siguiente:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Cuando se ejecutan migraciones con los datos existentes, puede haber restricciones de clave externa que no están satisfecho con los datos existentes. Para este tutorial, se crea una nueva base de datos, por lo que no hay ningún infracciones de restricción de clave externa. Vea [corregir las restricciones foreign key con datos heredados](#fk) para obtener instrucciones sobre cómo corregir las infracciones de clave externa en la base de datos actual.

## <a name="change-the-connection-string-and-update-the-db"></a>Cambie la cadena de conexión y actualizar la base de datos

El código en la sección actualizada `DbInitializer` agrega los datos de inicialización para las nuevas entidades. Para forzar el núcleo de EF para crear una nueva base de datos vacía:

* Cambiar el nombre de cadena de conexión de base de datos en *appSettings.JSON que se* a ContosoUniversity3. El nuevo nombre debe ser un nombre que no se ha usado en el equipo.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* O bien, elimine la base de datos utilizando:

    * **Explorador de objetos SQL Server** (SSOX).
    * El `database drop` comando de CLI:

   ```console
   dotnet ef database drop
   ```

Ejecute `database update` en la ventana de comandos:

```console
dotnet ef database update
```

El comando anterior ejecuta todas las migraciones.

Ejecute la aplicación. Ejecuta la aplicación se ejecuta el `DbInitializer.Initialize` método. El `DbInitializer.Initialize` rellena la base de datos nueva.

Abra la base de datos en SSOX:

* Expanda el **tablas** nodo. Se muestran las tablas creadas.
* Si SSOX se abrió previamente, haga clic en el **actualizar** botón.

![Tablas de SSOX](complex-data-model/_static/ssox-tables.png)

Examine la **CourseAssignment** tabla:

* Haga clic en el **CourseAssignment** de tabla y seleccione **ver datos**.
* Compruebe el **CourseAssignment** tabla contiene datos.

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Corregir las restricciones foreign key con datos heredados

Esta sección es opcional.

Cuando se ejecutan migraciones con los datos existentes, puede haber restricciones de clave externa que no están satisfecho con los datos existentes. Con los datos de producción, se deben realizar los pasos para migrar los datos existentes. En esta sección se proporciona un ejemplo de corrección de las infracciones de restricción de clave externa. No realice estos cambios de código sin una copia de seguridad. No realice estos cambios de código si realizó la sección anterior y actualiza la base de datos.

El *{timestamp}_ComplexDataModel.cs* archivo contiene el código siguiente:

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

El código anterior agrega un acepta valores NULL `DepartmentID` FK a la `Course` tabla. La base de datos desde el tutorial anterior contiene filas de `Course`, por lo que no se puede actualizar esa tabla por las migraciones.

Para realizar la `ComplexDataModel` trabajo de migración con los datos existentes:

* Cambiar el código para asignar a la nueva columna (`DepartmentID`) un valor predeterminado.
* Cree un departamento falso denominado "Temp" para que actúe como el departamento de forma predeterminada.

### <a name="fix-the-foreign-key-constraints"></a>Corregir las restricciones foreign key

Actualización de la `ComplexDataModel` clases `Up` método:

* Abra la *{timestamp}_ComplexDataModel.cs* archivo.
* Comentario de la línea de código que agrega el `DepartmentID` columna a la `Course` tabla.

[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Agregue el código resaltado siguiente. El nuevo código va después de la `.CreateTable( name: "Department"` bloque:[!code-csharp[Main](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Con los cambios anteriores, existente `Course` filas estarán relacionadas con el departamento "Temp" después de la `ComplexDataModel` `Up` ejecuciones de método.

Una aplicación de producción necesario lo siguiente:

* Incluir código o secuencias de comandos para agregar `Department` filas y relacionados con `Course` filas a la nueva `Department` filas.
* No se utilizaría el departamento "Temp" o el valor predeterminado de `Course.DepartmentID `.

El siguiente tutorial trata los datos relacionados.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/migrations)
[Siguiente](xref:data/ef-rp/read-related-data)