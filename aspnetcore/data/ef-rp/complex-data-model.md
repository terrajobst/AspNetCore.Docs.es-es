---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Modelo de datos (5 de 8)'
author: rick-anderson
description: En este tutorial agregará más entidades y relaciones, y personalizará el modelo de datos especificando reglas de formato, validación y asignación.
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: 2cec45afbf08e5dd379a54e780e4218bfc86d13f
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/03/2018
ms.locfileid: "32741282"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a>Páginas de Razor con EF Core en ASP.NET Core: Modelo de datos (5 de 8)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

En los tutoriales anteriores se trabajaba con un modelo de datos básico que se componía de tres entidades. En este tutorial:

* Se agregan más entidades y relaciones.
* Se personaliza el modelo de datos especificando el formato, la validación y las reglas de asignación de la base de datos.

Las clases de entidad para el modelo de datos completo se muestran en la siguiente ilustración:

![Diagrama de entidades](complex-data-model/_static/diagram.png)

Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).

## <a name="customize-the-data-model-with-attributes"></a>Personalizar el modelo de datos con atributos

En esta sección, se personaliza el modelo de datos mediante atributos.

### <a name="the-datatype-attribute"></a>El atributo DataType

Las páginas de alumno actualmente muestran la hora de la fecha de inscripción. Normalmente, los campos de fecha muestran solo la fecha y no la hora.

Actualice *Models/Student.cs* con el siguiente código resaltado:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

El atributo [DataType](/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) especifica un tipo de datos más específico que el tipo intrínseco de base de datos. En este caso solo se debe mostrar la fecha, no la fecha y hora. La [enumeración DataType](/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) proporciona muchos tipos de datos, como Date (Fecha), Time (Hora), PhoneNumber (Número de teléfono), Currency (Divisa), EmailAddress (Dirección de correo electrónico), etc. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo. Por ejemplo:

* El vínculo `mailto:` se crea automáticamente para `DataType.EmailAddress`.
* El selector de fecha se proporciona para `DataType.Date` en la mayoría de los exploradores.

El atributo `DataType` emite atributos HTML 5 `data-` (se pronuncia "datos dash") para su uso por parte de los exploradores HTML 5. Los atributos `DataType` no proporcionan validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De manera predeterminada, el campo de fecha se muestra según los formatos predeterminados basados en el elemento [CultureInfo](xref:fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support) del servidor.

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

La configuración `ApplyFormatInEditMode` especifica que el formato también debe aplicarse a la interfaz de usuario de edición. Algunos campos no deben usar `ApplyFormatInEditMode`. Por ejemplo, el símbolo de divisa generalmente no debe mostrarse en un cuadro de texto de edición.

El atributo `DisplayFormat` puede usarse por sí solo. Normalmente se recomienda usar el atributo `DataType` con el atributo `DisplayFormat`. El atributo `DataType` transmite la semántica de los datos en lugar de cómo se representan en una pantalla. El atributo `DataType` proporciona las siguientes ventajas que no están disponibles en `DisplayFormat`:

* El explorador puede habilitar características de HTML5. Por ejemplo, mostrar un control de calendario, el símbolo de divisa adecuado según la configuración regional, vínculos de correo electrónico, validación de entradas del lado cliente, etc.
* De manera predeterminada, el explorador representa los datos con el formato correcto según la configuración regional.

Para obtener más información, vea la [documentación de la aplicación auxiliar de etiquetas \<entrada>](xref:mvc/views/working-with-forms#the-input-tag-helper).

Ejecute la aplicación. Vaya a la página de índice de Students. Ya no se muestran las horas. Todas las vistas que usa el modelo `Student` muestran la fecha sin hora.

![Página de índice de estudiantes en la que se muestran las fechas sin horas](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a>El atributo StringLength

Las reglas de validación de datos y los mensajes de error de validación se pueden especificar con atributos. El atributo [StringLength](/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) especifica la longitud mínima y máxima de caracteres que se permite en un campo de datos. El atributo `StringLength` también proporciona validación del lado cliente y del lado servidor. El valor mínimo no influye en el esquema de base de datos.

Actualice el modelo `Student` con el código siguiente:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

El código anterior limita los nombres a no más de 50 caracteres. El atributo `StringLength` no impide que un usuario escriba un espacio en blanco para un nombre. El atributo [RegularExpression](/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) se usa para aplicar restricciones a la entrada. Por ejemplo, el código siguiente requiere que el primer carácter sea una letra mayúscula y el resto de caracteres sean alfabéticos:

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

Ejecute la aplicación:

* Vaya a la página Students.
* Seleccione **Create New** y escriba un nombre más de 50 caracteres.
* Seleccione **Create**, la validación del lado cliente muestra un mensaje de error.

![Página de índice de estudiantes en la que se muestran errores de longitud de cadena](complex-data-model/_static/string-length-errors.png)

En el **Explorador de objetos de SQL Server**, (SSOX) abra el diseñador de tablas de Student haciendo doble clic en la tabla **Student**.

![Tabla de estudiantes en SSOX antes de las migraciones](complex-data-model/_static/ssox-before-migration.png)

La imagen anterior muestra el esquema para la tabla `Student`. Los campos de nombre tienen tipo `nvarchar(MAX)` porque las migraciones no se han ejecutado en la base de datos. Cuando se ejecutan las migraciones más adelante en este tutorial, los campos de nombre se convierten en `nvarchar(50)`.

### <a name="the-column-attribute"></a>El atributo Column

Los atributos pueden controlar cómo se asignan las clases y propiedades a la base de datos. En esta sección, el atributo `Column` se usa para asignar el nombre de la propiedad `FirstMidName` a "FirstName" en la base de datos.

Cuando se crea la base de datos, los nombres de propiedad en el modelo se usan para los nombres de columna (excepto cuando se usa el atributo `Column`).

El modelo `Student` usa `FirstMidName` para el nombre de campo por la posibilidad de que el campo contenga también un segundo nombre.

Actualice el archivo *Models/Student.cs* con el siguiente código resaltado:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

Con el cambio anterior, `Student.FirstMidName` en la aplicación se asigna a la columna `FirstName` de la tabla `Student`.

La adición del atributo `Column` cambia el modelo de respaldo de `SchoolContext`. El modelo que está haciendo la copia de seguridad de `SchoolContext` ya no coincide con la base de datos. Si la aplicación se ejecuta antes de aplicar las migraciones, se genera la siguiente excepción:

```SQL
SqlException: Invalid column name 'FirstName'.
```
Para actualizar la base de datos:

* Compile el proyecto.
* Abra una ventana de comandos en la carpeta del proyecto. Escriba los comandos siguientes para crear una migración y actualizar la base de datos:

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

El comando `dotnet ef migrations add ColumnFirstName` genera el siguiente mensaje de advertencia:

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

La advertencia se genera porque los campos de nombre ahora están limitados a 50 caracteres. Si un nombre en la base de datos tenía más de 50 caracteres, se perderían desde el 51 hasta el último carácter.

* Pruebe la aplicación.

Abra la tabla de estudiantes en SSOX:

![Tabla de estudiantes en SSOX después de las migraciones](complex-data-model/_static/ssox-after-migration.png)

Antes de aplicar la migración, las columnas de nombre eran de tipo [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql). Las columnas de nombre ahora son `nvarchar(50)`. El nombre de columna ha cambiado de `FirstMidName` a `FirstName`.

> [!Note]
> En la sección siguiente, la creación de la aplicación en algunas de las fases genera errores del compilador. Las instrucciones especifican cuándo se debe compilar la aplicación.

## <a name="student-entity-update"></a>Actualizar la entidad Student

![Entidad Student](complex-data-model/_static/student-entity.png)

Actualice *Models/Student.cs* con el siguiente código:

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a>El atributo Required

El atributo `Required` hace que las propiedades de nombre sean campos obligatorios. El atributo `Required` no es necesario para los tipos que no aceptan valores NULL, como los tipos de valor (`DateTime`, `int`, `double`, etc.). Los tipos que no aceptan valores NULL se tratan automáticamente como campos obligatorios.

El atributo `Required` se podría reemplazar con un parámetro de longitud mínima en el atributo `StringLength`:

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a>El atributo Display

El atributo `Display` especifica que el título de los cuadros de texto debe ser "First Name", "Last Name", "Full Name" y "Enrollment Date". Los títulos predeterminados no tenían ningún espacio de división de palabras, por ejemplo "Lastname".

### <a name="the-fullname-calculated-property"></a>La propiedad calculada FullName

`FullName` es una propiedad calculada que devuelve un valor que se crea mediante la concatenación de otras dos propiedades. No se puede establecer `FullName`, tiene solo un descriptor de acceso get. No se crea ninguna columna `FullName` en la base de datos.

## <a name="create-the-instructor-entity"></a>Crear la entidad Instructor

![La entidad Instructor](complex-data-model/_static/instructor-entity.png)

Cree *Models/Instructor.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

Tenga en cuenta que varias propiedades son las mismas en las entidades `Student` y `Instructor`. En el tutorial Implementación de la herencia, más adelante en esta serie, se refactoriza este código para eliminar la redundancia.

En una sola línea puede haber varios atributos. Los atributos `HireDate` pudieron escribirse de la manera siguiente:

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a>Las propiedades de navegación CourseAssignments y OfficeAssignment

`CourseAssignments` y `OfficeAssignment` son propiedades de navegación.

Un instructor puede impartir cualquier número de cursos, por lo que `CourseAssignments` se define como una colección.

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

Si una propiedad de navegación contiene varias entidades:

* Debe ser un tipo de lista, donde se pueden agregar, eliminar y actualizar las entradas.

Los tipos de propiedad de navegación incluyen:

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

Si se especifica `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada.

La entidad `CourseAssignment` se explica en la sección sobre las relaciones de varios a varios.

Las reglas de negocio de Contoso University establecen que un instructor puede tener, a lo sumo, una oficina. La propiedad `OfficeAssignment` contiene una única instancia de `OfficeAssignment`. `OfficeAssignment` es NULL si no se asigna ninguna oficina.

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a>Crear la entidad OfficeAssignment

![Entidad OfficeAssignment](complex-data-model/_static/officeassignment-entity.png)

Cree *Models/OfficeAssignment.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a>El atributo Key

El atributo `[Key]` se usa para identificar una propiedad como la clave principal (PK) cuando el nombre de propiedad es diferente de classnameID o ID.

Hay una relación de uno a cero o uno entre las entidades `Instructor` y `OfficeAssignment`. Solo existe una asignación de oficina en relación con el instructor a la que está asignada. La clave principal de `OfficeAssignment` también es la clave externa (FK) para la entidad `Instructor`. EF Core no reconoce automáticamente `InstructorID` como la clave principal de `OfficeAssignment` porque:

* `InstructorID` no sigue la convención de nomenclatura de ID o classnameID.

Por tanto, se usa el atributo `Key` para identificar `InstructorID` como la clave principal:

```csharp
[Key]
public int InstructorID { get; set; }
```

De forma predeterminada, EF Core trata la clave como no generada por la base de datos porque la columna es para una relación de identificación.

### <a name="the-instructor-navigation-property"></a>La propiedad de navegación Instructor

La propiedad de navegación `OfficeAssignment` para la entidad `Instructor` acepta valores NULL porque:

* Los tipos de referencia, como las clases, aceptan valores NULL.
* Un instructor podría no tener una asignación de oficina.


La entidad `OfficeAssignment` tiene una propiedad de navegación `Instructor` que no acepta valores NULL porque:

* `InstructorID` no acepta valores NULL.
* Una asignación de oficina no puede existir sin un instructor.

Cuando una entidad `Instructor` tiene una entidad `OfficeAssignment` relacionada, cada entidad tiene una referencia a la otra en su propiedad de navegación.

El atributo `[Required]` puede aplicarse a la propiedad de navegación `Instructor`:

```csharp
[Required]
public Instructor Instructor { get; set; }
```

El código anterior especifica que debe haber un instructor relacionado. El código anterior no es necesario porque la clave externa `InstructorID`, que también es la clave principal, no acepta valores NULL.

## <a name="modify-the-course-entity"></a>Modificar la entidad Course

![La entidad Course](complex-data-model/_static/course-entity.png)

Actualice *Models/Course.cs* con el siguiente código:

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

La entidad `Course` tiene una propiedad de clave externa (FK) `DepartmentID`. `DepartmentID` apunta a la entidad relacionada `Department`. La entidad `Course` tiene una propiedad de navegación `Department`.

EF Core no requiere una propiedad de clave externa para un modelo de datos cuando el modelo tiene una propiedad de navegación para una entidad relacionada.

EF Core crea automáticamente claves externas en la base de datos siempre que se necesiten. EF Core crea [propiedades paralelas](https://docs.microsoft.com/ef/core/modeling/shadow-properties) para las claves externas creadas automáticamente. Tener la clave externa en el modelo de datos puede hacer que las actualizaciones sean más sencillas y eficaces. Por ejemplo, considere la posibilidad de un modelo donde la propiedad de la clave externa `DepartmentID` *no* está incluida. Cuando se captura una entidad de curso para editar:

* La entidad `Department` es NULL si no se carga explícitamente.
* Para actualizar la entidad Course, la entidad `Department` debe capturarse en primer lugar.

Cuando se incluye la propiedad de clave externa `DepartmentID` en el modelo de datos, no es necesario capturar la entidad `Department` antes de una actualización.

### <a name="the-databasegenerated-attribute"></a>El atributo DatabaseGenerated

El atributo `[DatabaseGenerated(DatabaseGeneratedOption.None)]` especifica que la aplicación proporciona la clave principal, en lugar de generarla la base de datos.

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

De forma predeterminada, EF Core da por supuesto que la base de datos genera valores de clave principal. Los valores de clave principal generados por la base de datos suelen ser el mejor método. Para las entidades `Course`, el usuario especifica la clave principal. Por ejemplo, un número de curso como una serie de 1000 para el departamento de matemáticas, una serie de 2000 para el departamento de inglés.

También se puede usar el atributo `DatabaseGenerated` para generar valores predeterminados. Por ejemplo, la base de datos puede generar automáticamente un campo de fecha para registrar la fecha en que se crea o actualiza una fila. Para obtener más información, vea [Propiedades generadas](https://docs.microsoft.com/ef/core/modeling/generated-properties).

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de clave externa y de navegación

Las propiedades de clave externa (FK) y las de navegación de la entidad `Course` reflejan las relaciones siguientes:

Un curso se asigna a un departamento, por lo que hay una clave externa `DepartmentID` y una propiedad de navegación `Department`.

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

Un curso puede tener cualquier número de alumnos inscritos en él, por lo que la propiedad de navegación `Enrollments` es una colección:

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

Un curso puede ser impartido por varios instructores, por lo que la propiedad de navegación `CourseAssignments` es una colección:

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

`CourseAssignment` se explica [más adelante](#many-to-many-relationships).

## <a name="create-the-department-entity"></a>Crear la entidad Department

![La entidad Department](complex-data-model/_static/department-entity.png)

Cree *Models/Department.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a>El atributo Column

Anteriormente se usó el atributo `Column` para cambiar la asignación de nombres de columna. En el código de la entidad `Department`, se usó el atributo `Column` para cambiar la asignación de tipos de datos de SQL. La columna `Budget` se define mediante el tipo money de SQL Server en la base de datos:

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

Por lo general, la asignación de columnas no es necesaria. EF Core generalmente elige el tipo de datos de SQL Server apropiado en función del tipo CLR para la propiedad. El tipo CLR `decimal` se asigna a un tipo `decimal` de SQL Server. `Budget` es para la divisa, y el tipo de datos money es más adecuado para la divisa.

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de clave externa y de navegación

Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:

* Un departamento puede tener o no un administrador.
* Un administrador siempre es un instructor. Por lo tanto, la propiedad `InstructorID` se incluye como la clave externa para la entidad `Instructor`.

La propiedad de navegación se denomina `Administrator` pero contiene una entidad `Instructor`:

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

El signo de interrogación (?) en el código anterior especifica que la propiedad acepta valores NULL.

Un departamento puede tener varios cursos, por lo que hay una propiedad de navegación Courses:

```csharp
public ICollection<Course> Courses { get; set; }
```

Nota: Por convención, EF Core permite la eliminación en cascada para las claves externas que no aceptan valores NULL y para las relaciones de varios a varios. La eliminación en cascada puede dar lugar a reglas de eliminación en cascada circular. Las reglas de eliminación en cascada circular provocan una excepción cuando se agrega una migración.

Por ejemplo, si no se ha definido que la propiedad `Department.InstructorID` acepta valores NULL:

* EF Core configura una regla de eliminación en cascada para eliminar el instructor cuando se elimine el departamento.
* Eliminar el instructor cuando se elimine el departamento no es el comportamiento previsto.

Si las reglas de negocios requieren que la propiedad `InstructorID` no acepte valores NULL, use la siguiente instrucción de API fluida:

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

El código anterior deshabilita la eliminación en cascada en la relación de instructor y departamento.

## <a name="update-the-enrollment-entity"></a>Actualizar la entidad Enrollment

Un registro de inscripción es para un curso realizado por un alumno.

![La entidad Enrollment](complex-data-model/_static/enrollment-entity.png)

Actualice *Models/Enrollment.cs* con el siguiente código:

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a>Propiedades de clave externa y de navegación

Las propiedades de clave externa y de navegación reflejan las relaciones siguientes:

Un registro de inscripción es para un curso, por lo que hay una propiedad de clave externa `CourseID` y una propiedad de navegación `Course`:

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

Un registro de inscripción es para un alumno, por lo que hay una propiedad de clave externa `StudentID` y una propiedad de navegación `Student`:

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a>Relaciones Varios a Varios

Hay una relación de varios a varios entre las entidades `Student` y `Course`. La entidad `Enrollment` funciona como una tabla combinada varios a varios *con carga útil* en la base de datos. "Con carga útil" significa que la tabla `Enrollment` contiene datos adicionales, además de claves externas de las tablas combinadas (en este caso, la clave principal y `Grade`).

En la ilustración siguiente se muestra el aspecto de estas relaciones en un diagrama de entidades. (Este diagrama se ha generado mediante EF Power Tools para EF 6.x. Crear el diagrama no forma parte del tutorial).

![Relación de varios a varios entre estudiantes y cursos](complex-data-model/_static/student-course.png)

Cada línea de relación tiene un 1 en un extremo y un asterisco (*) en el otro, para indicar una relación uno a varios.

Si la tabla `Enrollment` no incluyera información de calificaciones, solo tendría que contener las dos claves externas (`CourseID` y `StudentID`). Una tabla combinada de varios a varios sin carga útil se suele denominar una tabla combinada pura (PJT).

Las entidades `Instructor` y `Course` tienen una relación de varios a varios con una tabla combinada pura.

Nota: EF 6.x es compatible con las tablas de combinación implícitas para relaciones de varios a varios, pero EF Core no. Para obtener más información, consulte [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/) (Relaciones de varios a varios en EF Core 2.0).

## <a name="the-courseassignment-entity"></a>La entidad CourseAssignment

![La entidad CourseAssignment](complex-data-model/_static/courseassignment-entity.png)

Cree *Models/CourseAssignment.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a>Relación Instructor-to-Courses

![Relación de varios a varios Instructor-to-Courses](complex-data-model/_static/courseassignment.png)

La relación de varios a varios Instructor-to-Courses:

* Requiere una tabla de combinación que debe estar representada por un conjunto de entidades.
* Es una tabla combinada pura (tabla sin carga útil).

Es común denominar una entidad de combinación `EntityName1EntityName2`. Por ejemplo, la tabla de combinación Instructor-to-Courses usando este patrón es `CourseInstructor`. Pero se recomienda usar un nombre que describa la relación.

Los modelos de datos empiezan de manera sencilla y crecen. Las tablas combinadas sin carga útil (PJT) evolucionan con frecuencia para incluir la carga útil. A partir de un nombre de entidad descriptivo, no es necesario cambiar el nombre cuando la tabla combinada cambia. Idealmente, la entidad de combinación tendrá su propio nombre natural (posiblemente una sola palabra) en el dominio de empresa. Por ejemplo, Books y Customers podrían vincularse a través de una entidad combinada denominada Ratings. Para la relación de varios a varios Instructor-to-Courses, se prefiere `CourseAssignment` a `CourseInstructor`.

### <a name="composite-key"></a>Clave compuesta

Las claves externas no aceptan valores NULL. Las dos claves externas en `CourseAssignment` (`InstructorID` y `CourseID`) juntas identifican de forma única cada fila de la tabla `CourseAssignment`. `CourseAssignment` no requiere una clave principal dedicada. Las propiedades `InstructorID` y `CourseID` funcionan como una clave principal compuesta. La única manera de especificar claves principales compuestas en EF Core es con la *API fluida*. La sección siguiente muestra cómo configurar la clave principal compuesta.

La clave compuesta asegura que:

* Se permiten varias filas para un curso.
* Se permiten varias filas para un instructor.
* No se permiten varias filas para el mismo instructor y curso.

La entidad de combinación `Enrollment` define su propia clave principal, por lo que este tipo de duplicados son posibles. Para evitar los duplicados:

* Agregue un índice único en los campos de clave externa, o
* Configure `Enrollment` con una clave compuesta principal similar a `CourseAssignment`. Para obtener más información, vea [Índices](https://docs.microsoft.com/ef/core/modeling/indexes).

## <a name="update-the-db-context"></a>Actualizar el contexto de la base de datos

Agregue el código resaltado siguiente a *Data/SchoolContext.cs*:

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

El código anterior agrega las nuevas entidades y configura la clave principal compuesta de la entidad `CourseAssignment`.

## <a name="fluent-api-alternative-to-attributes"></a>Alternativa de la API fluida a los atributos

El método `OnModelCreating` del código anterior usa la *API fluida* para configurar el comportamiento de EF Core. La API se denomina "fluida" porque a menudo se usa para encadenar una serie de llamadas de método en una única instrucción. El [código siguiente](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) es un ejemplo de la API fluida:

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

En este tutorial, la API fluida se usa solo para la asignación de base de datos que no se puede realizar con atributos. Pero la API fluida puede especificar casi todas las reglas de formato, validación y asignación que se pueden realizar mediante el uso de atributos.

Algunos atributos como `MinimumLength` no se pueden aplicar con la API fluida. `MinimumLength` no cambia el esquema, solo aplica una regla de validación de longitud mínima.

Algunos desarrolladores prefieren usar la API fluida exclusivamente para mantener "limpias" las clases de entidad. Se pueden mezclar atributos y la API fluida. Hay algunas configuraciones que solo se pueden realizar con la API fluida (especificando una clave principal compuesta). Hay algunas configuraciones que solo se pueden realizar con atributos (`MinimumLength`). La práctica recomendada para el uso de atributos o API fluida:

* Elija uno de estos dos enfoques.
* Use el enfoque elegido de forma tan coherente como sea posible.

Algunos de los atributos utilizados en este tutorial se usan para:

* Solo validación (por ejemplo, `MinimumLength`).
* Solo configuración de EF Core (por ejemplo, `HasKey`).
* Validación y configuración de EF Core (por ejemplo, `[StringLength(50)]`).

Para obtener más información sobre la diferencia entre los atributos y la API fluida, vea [Métodos de configuración](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).

## <a name="entity-diagram-showing-relationships"></a>Diagrama de entidades en el que se muestran las relaciones

En la siguiente ilustración se muestra el diagrama creado por EF Power Tools para el modelo School completado.

![Diagrama de entidades](complex-data-model/_static/diagram.png)

El diagrama anterior muestra:

* Varias líneas de relación uno a varios (1 a \*).
* La línea de relación de uno a cero o uno (1 a 0..1) entre las entidades `Instructor` y `OfficeAssignment`.
* La línea de relación de cero o uno o varios (0..1 a *) entre las entidades `Instructor` y `Department`.

## <a name="seed-the-db-with-test-data"></a>Inicializar la base de datos con datos de prueba

Actualice el código en *Data/DbInitializer.cs*:

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

El código anterior proporciona datos de inicialización para las nuevas entidades. La mayor parte de este código crea objetos de entidad y carga los datos de ejemplo. Los datos de ejemplo se usan para pruebas. El código anterior crea las siguientes relaciones de varios a varios:

* `Enrollments`
* `CourseAssignment`

Nota: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) admitirá la [propagación de datos](https://github.com/aspnet/EntityFrameworkCore/issues/629).

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

Si se ejecuta el comando `database update`, se genera el error siguiente:

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

Cuando se ejecutan migraciones con datos existentes, puede haber restricciones de clave externa que no se cumplen con los datos existentes. Para este tutorial, se crea una base de datos, por lo que no hay ninguna infracción de restricciones de clave externa. Vea la sección [Corregir las restricciones de claves externas con datos heredados](#fk) para obtener instrucciones sobre cómo corregir las infracciones de clave externa en la base de datos actual.

## <a name="change-the-connection-string-and-update-the-db"></a>Cambiar la cadena de conexión y actualizar la base de datos

El código en la `DbInitializer` actualizada agrega los datos de inicialización para las nuevas entidades. Para obligar a EF Core a crear una base de datos vacía:

* Cambie el nombre de la cadena de conexión de la base de datos en *appSettings.JSON* a ContosoUniversity3. El nuevo nombre debe ser un nombre que no se haya usado en el equipo.

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* O bien, elimine la base de datos mediante:

  * **Explorador de objetos de SQL Server** (SSOX).
  * El comando de la CLI `database drop`:

    ```console
    dotnet ef database drop
    ```

Ejecute `database update` en la ventana de comandos:

```console
dotnet ef database update
```

El comando anterior ejecuta todas las migraciones.

Ejecute la aplicación. Ejecutar la aplicación ejecuta el método `DbInitializer.Initialize`. `DbInitializer.Initialize` rellena la base de datos nueva.

Abra la base de datos en SSOX:

* Expanda el nodo **Tablas**. Se muestran las tablas creadas.
* Si anteriormente se abrió SSOX, haga clic en el botón **Actualizar**.

![Tablas en SSOX](complex-data-model/_static/ssox-tables.png)

Examine la tabla **CourseAssignment**:

* Haga clic con el botón derecho en la tabla **CourseAssignment** y seleccione **Ver datos**.
* Compruebe que la tabla **CourseAssignment** contiene datos.

![Datos de CourseAssignment en SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a>Corregir las restricciones de claves externas con datos heredados

Esta sección es opcional.

Cuando se ejecutan migraciones con datos existentes, puede haber restricciones de clave externa que no se cumplen con los datos existentes. Con los datos de producción, se deben realizar algunos pasos para migrar los datos existentes. En esta sección se proporciona un ejemplo de corrección de las infracciones de restricción de clave externa. No realice estos cambios de código sin hacer una copia de seguridad. No realice estos cambios de código si realizó la sección anterior y actualizó la base de datos.

El archivo *{marca_de_tiempo}_ComplexDataModel.cs* contiene el código siguiente:

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

El código anterior agrega una clave externa `DepartmentID` que acepta valores NULL a la tabla `Course`. La base de datos del tutorial anterior contiene filas en `Course`, por lo que no se puede actualizar esa tabla mediante migraciones.

Para realizar la migración de `ComplexDataModel`, trabaje con los datos existentes:

* Cambie el código para asignar a la nueva columna (`DepartmentID`) un valor predeterminado.
* Cree un departamento falso denominado "Temp" para que actúe como el departamento predeterminado.

### <a name="fix-the-foreign-key-constraints"></a>Corregir las restricciones de clave externa

Actualice el método `Up` de las clases `ComplexDataModel`:

* Abra el archivo *{marca_de_tiempo}_ComplexDataModel.cs*.
* Convierta en comentario la línea de código que agrega la columna `DepartmentID` a la tabla `Course`.

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

Agregue el código resaltado siguiente. El nuevo código va después del bloque `.CreateTable( name: "Department"`: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

Con los cambios anteriores, las filas `Course` existentes estarán relacionadas con el departamento "Temp" después de ejecutar el método `ComplexDataModel` de `Up`.

Una aplicación de producción debería:

* Incluir código o scripts para agregar filas de `Department` y filas de `Course` relacionadas a las nuevas filas de `Department`.
* No use el departamento "Temp" o el valor predeterminado de `Course.DepartmentID`.

El siguiente tutorial trata los datos relacionados.

> [!div class="step-by-step"]
> [Anterior](xref:data/ef-rp/migrations)
> [Siguiente](xref:data/ef-rp/read-related-data)
