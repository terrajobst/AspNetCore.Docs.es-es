---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
title: "Crear un modelo de datos más complejo para una aplicación de ASP.NET MVC (4 de 10) | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f81f3d80-3674-4d8e-a9b1-87feed1a93c9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-a-more-complex-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 350c2e4e92c8a53d22dd2500330281b4003a05e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-more-complex-data-model-for-an-aspnet-mvc-application-4-of-10"></a>Crear un modelo de datos más complejo para una aplicación de ASP.NET MVC (4 de 10)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empiece aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, vea [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En los tutoriales anteriores ha trabajado con un modelo de datos simple que se compone de tres entidades. En este tutorial agregará más entidades y relaciones y personalizará el modelo de datos mediante la especificación de formato, validación y reglas de asignación de la base de datos. Verá dos maneras de personalizar el modelo de datos: mediante la adición de atributos a las clases de entidad y agregando código a la clase de contexto de base de datos.

Cuando haya terminado, las clases de entidad que conformarán el modelo de datos completa que se muestra en la siguiente ilustración:

![School_class_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image1.png)

## <a name="customize-the-data-model-by-using-attributes"></a>Personalizar el modelo de datos mediante el uso de atributos

En esta sección verá cómo personalizar el modelo de datos mediante el uso de atributos que especifican el formato, validación y las reglas de asignación de la base de datos. A continuación, en varias de las siguientes secciones se creará la sección completa `School` modelo de datos mediante la adición de atributos a las clases ya creadas y crear nuevas clases para los tipos de entidad restantes en el modelo.

### <a name="the-datatype-attribute"></a>El atributo de tipo de datos

Para las fechas de inscripción de estudiantes, todas las páginas web mostrar actualmente la hora junto con la fecha, aunque todo lo que le interesa para este campo es la fecha. Con los atributos de anotación de datos, puede realizar un cambio que se solucionará el formato de presentación en cada vista que muestra los datos de código. Para ver un ejemplo de cómo hacerlo, deberá agregar un atributo a la `EnrollmentDate` propiedad en la `Student` clase.

En *Models\Student.cs*, agregar un `using` instrucción para el `System.ComponentModel.DataAnnotations` espacio de nombres y agregue `DataType` y `DisplayFormat` atributos a la `EnrollmentDate` propiedad, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,13-14)]

El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo se utiliza para especificar un tipo de datos que es más específico que el tipo intrínseco de base de datos. En este caso sólo desea realizar un seguimiento de la fecha, no la fecha y hora. El [enumeración DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) proporciona para muchos tipos de datos, como *fecha, hora, número de teléfono, moneda, EmailAddress* y mucho más. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo. Por ejemplo, un `mailto:` vínculo se puede crear para [DataType.EmailAddress](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx), y no se puede proporcionar un selector de fecha para [DataType.Date](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) en exploradores que admiten [HTML5](http://html5.org/). El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos emite HTML 5 [datos -](http://ejohn.org/blog/html-5-data-attributes/) (pronunciado *guión datos*) atributos que pueden entender exploradores HTML 5. El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos no proporcionan ninguna validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De forma predeterminada, se muestra el campo de datos según los formatos predeterminados en función del servidor [CultureInfo](https://msdn.microsoft.com/en-us/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:


[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample2.cs)]


El `ApplyFormatInEditMode` configuración especifica que el formato especificado debe también se aplica cuando el valor se muestra en un cuadro de texto para su edición. (Puede no ser conveniente que para algunos campos, por ejemplo, para los valores de moneda, podría no desea que el símbolo de moneda en el cuadro de texto para su edición.)

Puede usar el [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo por sí mismo, pero suele ser una buena idea usar el [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo también. El `DataType` atributo transmite la *semántica* de los datos como contraposición a cómo representar en una pantalla y ofrece las siguientes ventajas que no obtendrá con `DisplayFormat`:

- El explorador puede habilitar características de HTML5 (por ejemplo mostrar un control de calendario, el símbolo de moneda adecuado para la configuración regional, vínculos de correo electrónico, etcetera.).
- De forma predeterminada, el explorador representará los datos con el formato correcto en función de su [configuración regional](https://msdn.microsoft.com/en-us/library/vstudio/wyzd2bce.aspx).
- El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo puede habilitar MVC elegir la plantilla de campo a la derecha para representar los datos (la [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) si usa por sí mismo utiliza la plantilla de cadena). Para obtener más información, consulte de Brad Wilson [ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Aunque redactado para MVC 2, este artículo todavía se aplica a la versión actual de ASP.NET MVC.)

Si usas el `DataType` atributo con un campo de fecha, tendrá que especificar el `DisplayFormat` atributo también con el fin de asegurarse de que el campo se representa correctamente en los exploradores de Chrome. Para obtener más información, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

Vuelva a ejecutar la página de índice de estudiantes y observe que veces ya no se muestran las fechas de inscripción. El mismo será true para cualquier vista que usa el `Student` modelo.

![Students_index_page_with_formatted_date](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image2.png)

### <a name="the-stringlengthattribute"></a>El StringLengthAttribute

También puede especificar reglas de validación de datos y mensajes de uso de atributos. Imagine que desea asegurarse de que los usuarios no escriban más de 50 caracteres para un nombre. Para agregar esta limitación, agregue [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributos a la `LastName` y `FirstMidName` propiedades, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=10,12)]

El [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributo no impedir que un usuario introducir un espacio en blanco para un nombre. Puede usar el [RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo para aplicar restricciones a la entrada. Por ejemplo, el siguiente código requiere el primer carácter que se va letra mayúscula y el resto de caracteres sea alfabético:

`[RegularExpression(@"^[A-Z]+[a-zA-Z''-'\s]*$")]`

El [MaxLength](https://msdn.microsoft.com/en-us/library/System.ComponentModel.DataAnnotations.MaxLengthAttribute.aspx) atributo proporciona una funcionalidad similar a la [StringLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributo pero no proporciona el cliente validación.

Ejecute la aplicación y haga clic en el **estudiantes** ficha. Se produce el error siguiente:

*El modelo de realizar una copia del contexto de 'SchoolContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar migraciones de Code First para actualizar la base de datos ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).*

El modelo de base de datos ha cambiado de forma que requiera un cambio en el esquema de base de datos y Entity Framework detecta. Deberá usar migraciones para actualizar el esquema sin perder los datos que agregó a la base de datos mediante el uso de la interfaz de usuario. Si cambia los datos que se creó el `Seed` método, que se cambiará a su estado original debido la [AddOrUpdate](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx) método que esté usando en el `Seed` método. ([AddOrUpdate](https://msdn.microsoft.com/en-us/library/hh846520(v=vs.103).aspx) es equivalente a una operación "upsert" de la terminología de base de datos.)

En la consola de administrador de paquete (PMC), escriba los siguientes comandos:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample4.cmd)]

El `add-migration MaxLengthOnNames` comando crea un archivo denominado  *&lt;timeStamp&gt;\_MaxLengthOnNames.cs*. Este archivo contiene código que se actualizará la base de datos para que coincida con el modelo de datos actual. La marca de tiempo que se antepone al nombre de archivo de las migraciones se usa por Entity Framework para ordenar las migraciones. Después de haber creado varias migraciones, si se coloca la base de datos, o si se implementa el proyecto mediante el uso de las migraciones, todas las migraciones se aplican en el orden en el que se crearon.

Ejecute el **crear** página y escriba nombre de más de 50 caracteres. Tan pronto como superan 50 caracteres, validación del lado cliente muestra inmediatamente un mensaje de error.

![error de val del lado del cliente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image3.png)

### <a name="the-column-attribute"></a>El atributo de columna

También puede usar atributos para controlar cómo se asignan las clases y propiedades para la base de datos. Imagine que hubiera usado el nombre `FirstMidName` para el nombre de campo ya que el campo puede contener también un nombre de medio. Pero desea que la columna de base de datos se denomine `FirstName`, ya que los usuarios que se va a escribir las consultas ad hoc en la base de datos están acostumbrados a dicho nombre. Para realizar esta asignación, puede usar el `Column` atributo.

El `Column` atributo especifica que cuando se crea la base de datos, la columna de la `Student` tabla que se asigna a la `FirstMidName` propiedad se denominará `FirstName`. En otras palabras, cuando el código hace referencia a `Student.FirstMidName`, los datos proceden o actualizados en el `FirstName` columna de la `Student` tabla. Si no especifica nombres de columna, se facilita el mismo nombre que el nombre de propiedad.

Agregar un elemento using instrucción para [System.ComponentModel.DataAnnotations.Schema](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.aspx) y el atributo de nombre de columna para el `FirstMidName` propiedad, como se muestra en el código resaltado siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample5.cs?highlight=4,14)]

La adición de la [atributo de columna](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) cambia el modelo de copia de SchoolContext, por lo que no coincida con la base de datos. Escriba los comandos siguientes en el PMC para crear otra migración:

[!code-console[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample6.cmd)]

En **Explorador de servidores** (**el Explorador de base de datos** si usas Express para Web), haga doble clic en el *estudiante* tabla.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image4.png)

La siguiente imagen muestra el nombre de la columna original, tal como estaba antes de aplicar las dos primeras migraciones. Además del nombre de columna cambia de `FirstMidName` a `FirstName`, han cambiado las dos columnas de nombre de `MAX` longitud de 50 caracteres.

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Base de datos también se puede realizar cambios de asignación mediante la [API fluida](https://msdn.microsoft.com/en-us/data/jj591617), como verá más adelante en este tutorial.

> [!NOTE]
> Si intenta compilar antes de que termine de crear todas las clases de entidad, podría obtener errores del compilador.


## <a name="create-the-instructor-entity"></a>Crear la entidad Instructor

![Instructor_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Crear *Models\Instructor.cs*, reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Tenga en cuenta que varias propiedades son los mismos en el `Student` y `Instructor` entidades. En el [implementar herencia](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie, deberá refactorizar usando la herencia para eliminar esta redundancia.

### <a name="the-required-and-display-attributes"></a>Requerido y atributos de presentación

Los atributos en el `LastName` propiedad especificar que se trata de un campo obligatorio, que el título del cuadro de texto debe ser "Last Name" (en lugar del nombre de propiedad, que sería "LastName" sin ningún espacio), y que el valor no puede tener más de 50 caracteres.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

El [StringLength atributo](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) establece la longitud máxima de la base de datos y proporciona el cliente y el lado servidor validación de ASP.NET MVC. También puede especificar la longitud mínima de la cadena en este atributo, pero el valor mínimo no influye en el esquema de base de datos. El [atributo necesario](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) no es necesario para tipos de valor como fecha y hora, int, double y float. Tipos de valor no se puede asignar un valor null, por lo que son intrínsecamente necesarios. Puede quitar el [atributo necesario](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx) y reemplazarlo con un parámetro de longitud mínima para la `StringLength` atributo:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample9.cs?highlight=2)]

Puede colocar varios atributos en una sola línea, por lo que podría escribir la clase instructor como sigue:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

### <a name="the-fullname-calculated-property"></a>FullName propiedad calculada

`FullName`es una propiedad calculada que devuelve un valor que se crea concatenando dos otras propiedades. Por lo tanto, solo tiene un `get` descriptor de acceso y no `FullName` columna se genera en la base de datos.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

### <a name="the-courses-and-officeassignment-navigation-properties"></a>Los cursos y las propiedades de navegación OfficeAssignment

El `Courses` y `OfficeAssignment` las propiedades son propiedades de navegación. Tal y como se explicó anteriormente, se definen normalmente como [virtuales](https://msdn.microsoft.com/en-us/library/9fkccyh4(v=vs.110).aspx) por lo que puede aprovechar una característica de Entity Framework llama [carga diferida](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx). Además, si una propiedad de navegación puede contener varias entidades, su tipo debe implementar la [ICollection&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/92t2ye13.aspx) interfaz. (Por ejemplo [IList&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/5y536ey6.aspx) pero no se consideran [IEnumerable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/9eekhta0.aspx) porque `IEnumerable<T>` no implementa [agregar ](https://msdn.microsoft.com/en-us/library/63ywd54z.aspx).

Un instructor puede enseñar a cualquier número de cursos, por lo que `Courses` se define como una colección de `Course` entidades. Nuestro reglas de negocios estado un instructor solo puede tener a lo sumo una oficina, por lo que `OfficeAssignment` se define como una sola `OfficeAssignment` entidad (lo que puede ser `null` si no se ha asignado ninguna oficina).

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

## <a name="create-the-officeassignment-entity"></a>Crear la entidad OfficeAssignment

![OfficeAssignment_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Crear *Models\OfficeAssignment.cs* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

Compile el proyecto, que guarda los cambios y comprueba que no ha realizado ninguna copia y pega el compilador puede detectar los errores.

### <a name="the-key-attribute"></a>El atributo clave

Hay una relación de uno a cero o uno entre la `Instructor` y `OfficeAssignment` entidades. Solo existe una asignación de oficina en relación con el instructor se asigna a, y, por tanto, su clave principal también es la clave externa para el `Instructor` entidad. Pero Entity Framework no se reconocen automáticamente `InstructorID` como el principal clave de esta entidad porque su nombre no sigue el `ID` o *classname* `ID` convención de nomenclatura. Por lo tanto, la `Key` atributo se utiliza para identificarlo como la clave:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample14.cs)]

También puede usar el `Key` atributo si la entidad tiene su propia clave principal, pero desea que la propiedad de nombre algo distinto a `classnameID` o `ID`. De forma predeterminada EF trata la clave como no generada por base de datos porque la columna es para una relación de identificación.

### <a name="the-foreignkey-attribute"></a>El atributo ForeignKey

Cuando hay una relación de uno a cero o uno o una relación uno a uno entre dos entidades (tales como entre `OfficeAssignment` y `Instructor`), EF no puede averiguar qué extremo de la relación es la entidad de seguridad y qué extremo es dependiente. Relaciones uno a uno tienen una propiedad de navegación de referencia en cada clase a la otra clase. El [ForeignKey atributo](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx) pueden aplicarse a la clase dependiente para establecer la relación. Si se omite la [ForeignKey atributo](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.foreignkeyattribute.aspx), obtendrá el siguiente error al intentar crear la migración:

No se puede determinar el extremo principal de una asociación entre los tipos 'ContosoUniversity.Models.OfficeAssignment' y 'ContosoUniversity.Models.Instructor'. El extremo principal de esta asociación debe configurarse explícitamente mediante la API fluida de relación o las anotaciones de datos.

Más adelante en el tutorial le mostraremos cómo configurar esta relación con la API fluida.

### <a name="the-instructor-navigation-property"></a>La propiedad de navegación de Instructor

El `Instructor` entidad tiene una que aceptan valores NULL `OfficeAssignment` propiedad de navegación (porque un instructor no podría tener una asignación de oficina) y el `OfficeAssignment` entidad tiene un acepta valores NULL `Instructor` propiedad de navegación (debido a una asignación de office no se puede existir sin un instructor-- `InstructorID` no acepta valores NULL). Cuando un `Instructor` entidad tiene un relacionados `OfficeAssignment` entidad, cada entidad tendrá una referencia a la otra en la propiedad de navegación.

Podría poner un `[Required]` atributo en la propiedad de navegación de Instructor para especificar que debe haber un instructor relacionado, pero no tiene que hacerlo porque la clave externa de InstructorID (que también es la clave para esta tabla) es que no aceptan valores NULL.

## <a name="modify-the-course-entity"></a>Modificar la entidad de curso

![Course_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image8.png)

En *Models\Course.cs*, reemplace el código que agregó anteriormente con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

La entidad de curso tiene una propiedad de clave externa `DepartmentID` que apunta al relacionado `Department` entidad y tiene un `Department` propiedad de navegación. Entity Framework no requiere que agregue una propiedad de clave externa para el modelo de datos cuando tenga una propiedad de navegación para una entidad relacionada. EF crea automáticamente las claves externas en la base de datos cuando son necesarias. Pero con la clave externa en el modelo de datos puede hacer que las actualizaciones más sencillo y más eficaz. Por ejemplo, cuando se captura una entidad de curso para editar, el `Department` entity es null si no lo carga, por lo que cuando se actualiza la entidad de curso, deberá primero capturar el `Department` entidad. Cuando la propiedad de clave externa `DepartmentID` se incluye en el modelo de datos, no es necesario capturar el `Department` entidad antes de actualizar.

### <a name="the-databasegenerated-attribute"></a>El atributo DatabaseGenerated

El [DatabaseGenerated atributo](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute.aspx) con el [ninguno](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.110).aspx) parámetro en el `CourseID` propiedad especifica que los valores de clave principales se proporcionado por el usuario, en lugar de generado por la base de datos.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

De forma predeterminada, Entity Framework se da por supuesto que la base de datos genera valores de clave principal. Es lo que desea en la mayoría de los escenarios. Sin embargo, para `Course` entidades, deberá utilizar un número de curso especificado por el usuario como una serie de 1000 para un departamento, una serie de 2000 para otro departamento y así sucesivamente.

### <a name="foreign-key-and-navigation-properties"></a>Clave externa y las propiedades de navegación

Las propiedades de clave externa y las propiedades de navegación de la `Course` entidad reflejen las relaciones siguientes:

- Un curso se asigna a un departamento, por lo que hay un `DepartmentID` clave externa y un `Department` propiedad de navegación por las razones mencionadas anteriormente. 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]
- Un curso puede tener cualquier número de alumnos inscritos en él, por lo que el `Enrollments` propiedad de navegación es una colección: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample18.cs)]
- Un curso puede ser imparten por varios instructores, por lo que el `Instructors` propiedad de navegación es una colección: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample19.cs)]

## <a name="creating-the-department-entity"></a>Crear la entidad Department

![Department_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image9.png)

Crear *Models\Department.cs* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample20.cs)]

### <a name="the-column-attribute"></a>El atributo de columna

Versiones anteriores utilizan el [atributo de columna](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.columnattribute.aspx) para cambiar la asignación de nombres de columna. En el código de la `Department` entidad, el `Column` atributo está usándola para cambiar SQL asignar tipos de datos para que la columna se definirá con el servidor SQL Server [dinero](https://msdn.microsoft.com/en-us/library/ms179882.aspx) tipo en la base de datos:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample21.cs)]

Asignación de columnas por lo general no es necesaria, dado que Entity Framework normalmente elige el tipo de datos de SQL Server adecuado en función del tipo CLR que se define para la propiedad. El CLR `decimal` tipo se asigna a un servidor SQL Server `decimal` tipo. Pero en este caso se sabe que la columna se contener cantidades de moneda y el [dinero](https://msdn.microsoft.com/en-us/library/ms179882.aspx) es más adecuado para ese tipo de datos.

### <a name="foreign-key-and-navigation-properties"></a>Clave externa y las propiedades de navegación

Las propiedades de navegación y de clave externas reflejen las relaciones siguientes:

- Un departamento puede tener o no un administrador, y un administrador es siempre un instructor. Por lo tanto, la `InstructorID` propiedad se incluye como la clave externa para la `Instructor` se agrega la entidad y un signo de interrogación después de la `int` escriba designación para marcar la propiedad como que acepta valores NULL. La propiedad de navegación se denomina `Administrator` pero contiene un `Instructor` entidad: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample22.cs)]
- Un departamento puede tener varios cursos, por lo que hay un `Courses` propiedad de navegación: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample23.cs)]

 > [!NOTE]
 > Por convención, Entity Framework permite la eliminación en cascada para las claves externas que no aceptan valores NULL y para las relaciones de varios a varios. Esto puede dar lugar a eliminar reglas de cascada circular, lo que producirán una excepción cuando se ejecuta el código de inicializador. Por ejemplo, si no define el `Department.InstructorID` propiedad como que acepta valores NULL, se obtendrá el siguiente mensaje de excepción cuando se ejecuta el inicializador: "la relación referencial dará como resultado una referencia cíclica que no está permitida." Si es necesario sus reglas de negocios `InstructorID` propiedad como no acepta valores NULL, tendría que usar la API fluida siguiente para deshabilitar la eliminación en cascada en la relación: 

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample24.cs)]


## <a name="modifying-the-student-entity"></a>Modificar la entidad Student

![Student_entity](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image10.png)

En *Models\Student.cs*, reemplace el código que agregó anteriormente con el código siguiente. Los cambios aparecen resaltados.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample25.cs?highlight=12,15,24-27)]

## <a name="the-enrollment-entity"></a>La entidad de inscripción

 En *Models\Enrollment.cs*, reemplace el código que agregó anteriormente con el código siguiente

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample26.cs?highlight=16)]

### <a name="foreign-key-and-navigation-properties"></a>Clave externa y las propiedades de navegación

Las propiedades de clave externa y propiedades de navegación reflejan las relaciones siguientes:

- Un registro de inscripción es para un solo curso, por lo que hay un `CourseID` propiedad de clave externa y un `Course` propiedad de navegación: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample27.cs)]
- Un registro de inscripción es para un estudiante único, por lo que hay un `StudentID` propiedad de clave externa y un `Student` propiedad de navegación: 

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample28.cs)]

### <a name="many-to-many-relationships"></a>Relaciones Varios a Varios

Hay una relación de varios a varios entre la `Student` y `Course` entidades y el `Enrollment` entidad funciona como una tabla de combinación-to-many *con carga* en la base de datos. Esto significa que la `Enrollment` tabla contiene datos adicionales además de las claves externas de las tablas combinadas (en este caso, una clave principal y un `Grade` propiedad).

En la siguiente ilustración muestra el aspecto de estas relaciones en un diagrama de la entidad. (Este diagrama se ha generado mediante la [herramientas de Entity Framework Power](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d); crear el diagrama no forma parte del tutorial, se utilizan simplemente aquí como una ilustración.)

![Many_relationship Course_many de estudiante](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image11.png)

Cada línea de relación tiene un 1 en un extremo y un asterisco (\*) en el otro, que indica una relación uno a varios.

Si el `Enrollment` tabla no incluir información de categoría, que solo sería necesario contener las dos claves externas `CourseID` y `StudentID`. En ese caso, se corresponde a una tabla de la combinación de varios a varios *sin carga* (o *tabla combinada pura*) en la base de datos, y no tiene que crear una clase de modelo para él en absoluto. El `Instructor` y `Course` entidades tienen este tipo de relación de varios a varios y, como puede ver, no hay ninguna clase de entidad entre ellos:

![Course_many instructor de many_relationship](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image12.png)

Se requiere una tabla de combinación en la base de datos, sin embargo, como se muestra en el siguiente diagrama de base de datos:

![Course_many instructor de many_relationship_tables](https://asp.net/media/2577802/Windows-Live-Writer_Creating-a.NET-MVC-Application-4-of-10h1_B662_Instructor-Course_many-to-many_relationship_tables_03e042cf-db89-4b4c-985a-e458351ada76.png)

Entity Framework crea automáticamente el `CourseInstructor` tabla y leer y actualizar indirectamente si se lee y actualiza el `Instructor.Courses` y `Course.Instructors` propiedades de navegación.

## <a name="entity-diagram-showing-relationships"></a>Diagrama mostrando relaciones de entidad

En la siguiente ilustración muestra el diagrama creados por las herramientas de alimentación de Entity Framework para el modelo School completado.

![School_data_model_diagram](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image13.png)

Además de las líneas de relación de varios a varios (\* a \*) y las líneas de relación de uno a varios (1 a \*), aquí se puede ver la línea de relación de uno a cero o uno (1 a 0.. 1) entre el `Instructor` y `OfficeAssignment` las entidades y la línea de relación de cero-o-uno a varios (0.. 1 a \*) entre las entidades Instructor y departamento.

## <a name="customize-the-data-model-by-adding-code-to-the-database-context"></a>Personalizar el modelo de datos mediante la adición de código en el contexto de base de datos

A continuación agregará las nuevas entidades para la `SchoolContext` clase y personalizar algunos de la asignación mediante [API fluida](https://msdn.microsoft.com/en-us/data/jj591617) llamadas. (La API es "fluida" porque se utiliza a menudo, sin espacios, una serie de llamadas al método juntos en una sola instrucción).

En este tutorial usará la API fluida solo para la asignación de base de datos que no se puede realizar con atributos. Sin embargo, también puede utilizar la API fluida para especificar casi todo el formato, la validación y las reglas de asignación que se pueden realizar mediante el uso de atributos. Algunos atributos como `MinimumLength` no se puede aplicar con la API fluida. Como se mencionó anteriormente, `MinimumLength` no cambia el esquema, sólo se aplica una regla de validación del lado cliente y servidor

Algunos desarrolladores prefieren usar la API fluida exclusivamente para que pueden mantener las clases de entidad "limpia". Puede mezclar atributos y API fluida si quieres, hay algunas personalizaciones que solo pueden realizarse mediante el uso de la API fluida, pero en general la práctica recomendada es elegir uno de estos dos enfoques y utilizar ese constantemente lo máximo posible.

Para agregar las nuevas entidades para los datos de modelo y realizan la asignación de la base de datos que no se hace uso de atributos, reemplace el código de *DAL\SchoolContext.cs* con el código siguiente:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample29.cs)]

La nueva instrucción en el [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método configura la tabla de combinación de varios a varios:

- Para la relación de varios a varios entre la `Instructor` y `Course` entidades, el código especifica los nombres de tabla y columna de la tabla de combinación. Código puede configurar la relación de varios a varios para sin este código, pero si no lo llame, obtendrá nombres predeterminados como `InstructorInstructorID` para la `InstructorID` columna.

    [!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample30.cs)]

El código siguiente proporciona un ejemplo de cómo se podría haber utilizado la API fluida en lugar de atributos para especificar la relación entre el `Instructor` y `OfficeAssignment` entidades:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample31.cs)]

Para obtener información sobre lo que hacen las instrucciones de "API fluida" en segundo plano, consulte la [API fluida](https://blogs.msdn.com/b/aspnetue/archive/2011/05/04/entity-framework-code-first-tutorial-supplement-what-is-going-on-in-a-fluent-api-call.aspx) entrada de blog.

## <a name="seed-the-database-with-test-data"></a>Valor de inicialización de la base de datos con datos de prueba

Reemplace el código de la *Migrations\Configuration.cs* archivo con el código siguiente con el fin de proporcionar datos de inicialización para las nuevas entidades que ha creado.

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample32.cs)]

Como vimos en el primer tutorial, la mayor parte de este código simplemente actualiza crea nuevos objetos de entidad y carga los datos de ejemplo en propiedades según sea necesario para las pruebas. Sin embargo, tenga en cuenta cómo el `Course` entidad, que tiene una relación de varios a varios con el `Instructor` entidad, se controla:

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample33.cs)]

Cuando se crea un `Course` objeto, inicializar el `Instructors` propiedad de navegación como una colección vacía con el código `Instructors = new List<Instructor>()`. Esto permite agregar `Instructor` entidades que están relacionados con este `Course` utilizando el `Instructors.Add` método. Si no creó una lista vacía, no podrá agregar estas relaciones, porque la `Instructors` propiedad sería null y no tiene un `Add` método. También puede agregar la inicialización de la lista al constructor.

## <a name="add-a-migration-and-update-the-database"></a>Agregar una migración y actualizar la base de datos

En el PMC, escriba la `add-migration` comando:

`PM> add-Migration Chap4`

Si intenta actualizar la base de datos en este momento, obtendrá el siguiente error:

*La instrucción ALTER TABLE entran en conflicto con la restricción FOREIGN KEY "FK\_dbo. Curso\_dbo. Departamento\_DepartmentID ". Se produjo el conflicto en la tabla base de datos "ContosoUniversity", "dbo. Departamento", columna 'DepartmentID'.*

Editar la &lt; *timestamp&gt;\_Chap4.cs* de archivos y realizar los siguientes cambios de código (agregará una instrucción SQL y modificar un `AddColumn` instrucción):

[!code-csharp[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample34.cs?highlight=14-18)]

(Asegúrese de que se convierta en comentario o eliminar las existentes `AddColumn` línea cuando se agrega una nueva o, obtendrá un error al escribir el `update-database` comando.)

En ocasiones, al ejecutar migraciones con los datos existentes, debe insertar datos de código auxiliar en la base de datos para satisfacer las restricciones foreign key y, es lo que está haciendo ahora. El código generado agrega un acepta valores NULL `DepartmentID` clave externa para el `Course` tabla. Si ya hay filas en la `Course` tabla cuando se ejecuta el código, el `AddColumn` se producirán un error en la operación porque SQL Server no conoce el valor que se incluirán en la columna que no puede ser nula. Por lo tanto, ha cambiado el código para asignar un valor predeterminado de la nueva columna, y ha creado un departamento de recibos denominado "Temp" para que actúe como el departamento de forma predeterminada. Como resultado, si existen `Course` filas cuando se ejecuta este código, estarán todos relacionados con el departamento "Temp".

Cuando el `Seed` método se ejecuta, va a insertar filas en la `Department` tabla y relacionan existente `Course` filas para los nuevos `Department` filas. Si no ha agregado ningún curso en la interfaz de usuario, a continuación, ya no necesitará el departamento "Temp" o el valor predeterminado en la `Course.DepartmentID` columna. Para permitir la posibilidad de que, alguien podría haber agregado cursos mediante la aplicación, ¿desea actualizar el `Seed` código del método para asegurarse de que todos los `Course` filas (no solo los inserta en ejecuciones anteriores de la `Seed` método) tienen válido `DepartmentID` valores antes de quitar el valor predeterminado de la columna de valor y elimine el departamento "Temp".

Cuando haya terminado de editar el &lt; *timestamp&gt;\_Chap4.cs* de archivo, escriba la `update-database` comando el PMC para realizar la migración.

> [!NOTE]
> Es posible obtener otros errores durante la migración de datos y realizar cambios de esquema. Si se producen errores de migración no se puede resolver, puede cambiar la cadena de conexión en el *Web.config* de archivo o eliminar la base de datos. El enfoque más sencillo consiste en cambiar el nombre de la base de datos *Web.config* archivo. Por ejemplo, cambiar el nombre de la base de datos a CU\_probar tal y como se muestra en la siguiente:
> 
> [!code-xml[Main](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/samples/sample35.xml?highlight=1-2)]
> 
>  Con una base de datos, no hay ningún dato para migrar y el `update-database` comando es mucho más probable que se completan sin errores. Para obtener instrucciones sobre cómo eliminar la base de datos, vea [cómo quitar una base de datos de Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/).


Abra la base de datos **Explorador de servidores** tal y como hizo anteriormente y expanda el **tablas** nodo para ver que todas las tablas se han creado. (Si todavía están **Explorador de servidores** abrir desde la hora anterior, haga clic en el **actualizar** botón.)

![](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image14.png)

No cree una clase de modelo para el `CourseInstructor` tabla. Como se explicó anteriormente, se trata de una tabla de combinación para la relación de varios a varios entre la `Instructor` y `Course` entidades.

Haga clic en el `CourseInstructor` de tabla y seleccione **mostrar datos de tabla** para comprobar que tiene datos en él como resultado de la `Instructor` entidades que agregó a la `Course.Instructors` propiedad de navegación.

![Table_data_in_CourseInstructor_table](creating-a-more-complex-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="summary"></a>Resumen

Ahora tiene un modelo de datos más compleja y una base de datos correspondiente. En el tutorial siguiente aprenderá más acerca de diferentes maneras de obtener acceso a datos relacionados.

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Siguiente](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
