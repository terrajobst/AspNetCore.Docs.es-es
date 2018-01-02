---
title: "Páginas de Razor con Entity Framework Core - Tutorial 1 de 8"
author: rick-anderson
description: "Muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core"
keywords: Tutorial de ASP.NET Core, Entity Framework Core,
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: acbd987438edeea13f29547dc471f9a211e87b04
ms.sourcegitcommit: 1de159820a572c08955ee77cc8b1caa3d7aa938c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Introducción a las páginas de Razor y Entity Framework Core con Visual Studio (1 de 8)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core 2.0 mediante 2017 de Visual Studio y los núcleos de Entity Framework (EF) 2.0.

La aplicación de ejemplo es un sitio web de una universidad ficticia de Contoso. Incluye una funcionalidad, como la admisión de estudiantes, la creación de curso y asignaciones de instructor. Esta página es el primer elemento de una serie de tutoriales que explican cómo crear la aplicación de ejemplo de la Universidad de Contoso.

[Descargar o ver la aplicación completada.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrucciones de descarga](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

Familiaridad con [páginas de Razor](xref:mvc/razor-pages/index). Deben completar los programadores nuevos [empezar a trabajar con páginas de Razor](xref:tutorials/razor-pages/razor-pages-start) antes de iniciar esta serie.

## <a name="troubleshooting"></a>Solución de problemas

Si experimenta un problema no se puede resolver, por lo general puede encontrar la solución comparando el código para el [completado fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) o [proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final). Para obtener una lista de errores comunes y cómo resolverlos, vea [la sección de solución de problemas del último tutorial de la serie](xref:data/ef-mvc/advanced#common-errors). Si no encuentra lo que necesita no existe, puede publicar una pregunta en StackOverflow.com para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [Core EF](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Esta serie de tutoriales se basa en lo que se realiza en los tutoriales anteriores. Considere la posibilidad de guardar una copia del proyecto tras cada tutorial finalizar correctamente. Si experimenta problemas, puede empezar el tutorial anterior en lugar de volver al principio. Como alternativa, puede descargar una [completado fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) y comenzar de nuevo con la fase completada.

## <a name="the-contoso-university-web-app"></a>La aplicación web de la Universidad de Contoso

La aplicación compilada en estos tutoriales es un sitio web de básicos universidad.

Los usuarios pueden ver y actualizar la información de instructor, curso y estudiantes. Aquí le ofrecemos algunas de las pantallas que creó en el tutorial.

![Página de índice de estudiantes](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

El estilo de la interfaz de usuario de este sitio es casi lo que se genera por las plantillas integradas. El tutorial es núcleo de EF con páginas de Razor, no en la interfaz de usuario.

## <a name="create-a-razor-pages-web-app"></a>Crear una aplicación web de las páginas de Razor

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core. Denomine el proyecto **ContosoUniversity**. Es importante al proyecto el nombre *ContosoUniversity* para que coincidan con los espacios de nombres al código es copiar y pegar.
 ![Nueva aplicación web de ASP.NET Core](intro/_static/np.png)
* Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.
 ![Aplicación web (páginas de Razor)](../../mvc/razor-pages/index/_static/np2.png)

Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.

## <a name="set-up-the-site-style"></a>Configurar el estilo de sitio

Algunos cambios configurar en el menú de sitio, el diseño y la página principal.

Abra *Pages/_Layout.cshtml* y realice los cambios siguientes:

* Cambiar cada aparición de "ContosoUniversity" a "Universidad de Contoso". Hay tres apariciones.

* Agregar entradas de menú para **estudiantes**, **cursos**, **instructores**, y **departamentos**y eliminar el **póngase en contacto con** entrada de menú.

Los cambios aparecen resaltados.

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Presione CTRL+F5 para ejecutar el proyecto. La página principal se muestra con las fichas creadas en los siguientes tutoriales:

![Página de inicio de la Universidad de Contoso](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Crear el modelo de datos

Crear clases de entidad para la aplicación de la Universidad de Contoso. Comience con las siguientes tres entidades:

![Diagrama del modelo de datos de inscripción de curso de estudiante](intro/_static/data-model-diagram.png)

Hay una relación de uno a varios entre `Student` y `Enrollment` entidades. Hay una relación de uno a varios entre `Course` y `Enrollment` entidades. Un estudiante puede inscribirse en cualquier número de cursos. Un curso puede tener cualquier número de alumnos inscritos en ella.

En las secciones siguientes, se crea una clase para cada una de estas entidades.

### <a name="the-student-entity"></a>La entidad Student

![Diagrama de la entidad de estudiante](intro/_static/student-entity.png)

Crear un *modelos* carpeta. En el *modelos* carpeta, cree un archivo de clase denominado *Student.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

El `ID` propiedad se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase. De forma predeterminada, el núcleo de EF interpreta una propiedad que se denomina `ID` o `classnameID` como clave principal.

El `Enrollments` es una propiedad de navegación. Propiedades de navegación se vinculan a otras entidades que están relacionados con esta entidad. En este caso, el `Enrollments` propiedad de un `Student entity` conserva toda la `Enrollment` entidades que están relacionados con dicho `Student`. Por ejemplo, si una fila de estudiante en la base de datos tiene dos inscripción filas relacionadas, el `Enrollments` propiedad de navegación contiene esos dos `Enrollment` entidades. Un relacionados `Enrollment` fila es una fila que contiene el valor de clave principal de ese estudiante en la `StudentID` columna. Por ejemplo, suponga que los estudiantes con Id. = 1 tiene dos filas en la `Enrollment` tabla. El `Enrollment` tabla tiene dos filas con `StudentID` = 1. `StudentID`es una clave externa en la `Enrollment` tabla que especifica los estudiantes en el `Student` tabla.

Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`. `ICollection<T>`se puede especificar, o un tipo como `List<T>` o `HashSet<T>`. Cuando `ICollection<T>` es usa, Core EF crea un `HashSet<T>` colección de forma predeterminada. Propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.

### <a name="the-enrollment-entity"></a>La entidad de inscripción

![Diagrama de la entidad de inscripción](intro/_static/enrollment-entity.png)

En el *modelos* carpeta, crear *Enrollment.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

El `EnrollmentID` propiedad es la clave principal. Esta entidad se utiliza la `classnameID` de patrón en lugar de `ID` como el `Student` entidad. Normalmente los programadores elegir un patrón y utilizarla en todo el modelo de datos. En un tutorial posterior, con el identificador sin classname se muestra para que resulten más fáciles de implementar la herencia en el modelo de datos.

El `Grade` propiedad es una `enum`. El signo de interrogación después de la `Grade` declaración de tipo indica que el `Grade` propiedad es que aceptan valores NULL. Un grado que sea null es diferente de una puntuación de cero, null significa una puntuación no se conoce o no se ha asignado todavía.

El `StudentID` propiedad es una clave externa y la propiedad de navegación correspondiente es `Student`. Un `Enrollment` está asociada con una entidad `Student` entidad, por lo que la propiedad contiene una sola `Student` entidad. El `Student` entidad difiere de la `Student.Enrollments` propiedad de navegación, que contiene varias `Enrollment` entidades.

El `CourseID` propiedad es una clave externa y la propiedad de navegación correspondiente es `Course`. Un `Enrollment` está asociada con una entidad `Course` entidad.

Núcleo EF interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`. Por ejemplo,`StudentID` para el `Student` propiedad de navegación, puesto que la `Student` clave principal de la entidad es `ID`. Propiedades de clave externa también pueden denominarse `<primary key property name>`. Por ejemplo, `CourseID` desde el `Course` clave principal de la entidad es `CourseID`.

### <a name="the-course-entity"></a>La entidad de curso

![Diagrama de la entidad de curso](intro/_static/course-entity.png)

En el *modelos* carpeta, crear *Course.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

El `Enrollments` es una propiedad de navegación. A `Course` entidad se puede relacionar con cualquier número de `Enrollment` entidades.

El `DatabaseGenerated` atributo permite que la aplicación especificar la clave principal en lugar de tener la base de datos generarlo.

## <a name="create-the-schoolcontext-db-context"></a>Crear el contexto de base de datos de SchoolContext

La clase principal que coordina la funcionalidad básica de EF para un modelo de datos concreto es la clase de contexto de base de datos. El contexto de datos se deriva de `Microsoft.EntityFrameworkCore.DbContext`. El contexto de datos especifica qué entidades se incluyen en el modelo de datos. En este proyecto, la clase se denomina `SchoolContext`.

En la carpeta del proyecto, cree una carpeta denominada *datos*.

En el *datos* crear carpeta *SchoolContext.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Este código crea un `DbSet` propiedad para cada conjunto de entidades. En la terminología de EF principales:

* Una entidad establecer normalmente corresponde a una tabla de base de datos.
* Una entidad corresponde a una fila de la tabla.

`DbSet<Enrollment>`y `DbSet<Course>` puede omitirse. EF Core incluye implícitamente porque la `Student` las referencias de entidad del `Enrollment` entidad y el `Enrollment` las referencias de entidad la `Course` entidad. Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.

Cuando se crea la base de datos, Core EF crea las tablas que tienen nombres de la misma que la `DbSet` nombres de propiedad. Nombres de propiedad para las colecciones son normalmente plurales (estudiantes en lugar de estudiante). Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales. Para estos tutoriales, se invalida el comportamiento predeterminado especificando los nombres de tabla singular en DbContext. Para especificar los nombres de tabla singular, agregue el código resaltado siguiente:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrar el contexto con la inserción de dependencias

ASP.NET Core incluye [inyección de dependencia](xref:fundamentals/dependency-injection). Servicios (por ejemplo, el contexto de la base de datos de EF Core) se registran con inyección de dependencia durante el inicio de la aplicación. Los componentes que requieren estos servicios (por ejemplo, las páginas de Razor) se proporcionan estos servicios a través de los parámetros del constructor. El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.

Para registrar `SchoolContext` como un servicio, abra *Startup.cs*y agregue las líneas resaltadas en la `ConfigureServices` método.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un `DbContextOptionsBuilder` objeto. Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee las cadenas de conexión desde el *appSettings.JSON que se* archivo.

Agregar `using` instrucciones para `ContosoUniversity.Data` y `Microsoft.EntityFrameworkCore` los espacios de nombres. Compile el proyecto.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Abra la *appSettings.JSON que se* de archivos y agregar una cadena de conexión tal y como se muestra en el código siguiente:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

La cadena de conexión anterior usa `ConnectRetryCount=0` para evitar [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) de que no responden.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La cadena de conexión especifica una base de datos de SQL Server LocalDB. LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está diseñada para el desarrollo de aplicaciones, no su uso en producción. LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja. De forma predeterminada, crea LocalDB *.mdf* archivos de base de datos en el `C:/Users/<user>` directory.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Agregue código para inicializar la base de datos con datos de prueba

Núcleo EF crea una base de datos vacía. En esta sección, un *inicialización* método se escribe para rellenar con datos de prueba.

En el *datos* carpeta, cree un nuevo archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

El código comprueba si hay cualquier estudiantes en la base de datos. Si no hay ningún alumno en la base de datos, la base de datos es visible con datos de prueba. Carga los datos de prueba en las matrices en lugar de `List<T>` colecciones para optimizar el rendimiento.

El `EnsureCreated` método crea automáticamente la base de datos para el contexto de base de datos. Si no existe la base de datos, `EnsureCreated` devuelve sin modificar la base de datos.

En *Program.cs*, modifique la `Main` método para hacer lo siguiente:

* Obtener una instancia de contexto de base de datos desde el contenedor de inyección de dependencia.
* Llame al método de inicialización, pasándole el contexto.
* Elimine el contexto cuando se completa el método de inicialización.

El siguiente código muestra actualizado *Program.cs* archivo.

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

La primera vez que se ejecute la aplicación, la base de datos se crea y propagado con datos de prueba. Cuando se actualiza el modelo de datos:
* Eliminar la base de datos.
* Actualizar el método de inicialización.
* Ejecutar la aplicación y se crea una nueva base de datos a la inicialización. 

En los tutoriales posteriores, la base de datos se actualiza cuando el modelo de datos los cambios, sin tener que eliminar y volver a crear la base de datos.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Agregar herramientas de scaffolding

En esta sección, se utiliza la consola de administrador de paquete (PMC) para agregar el paquete de generación de código de Visual Studio web. Este paquete es necesario para ejecutar el motor de scaffolding.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

En la consola de administrador de paquete (PMC), escriba los siguientes comandos:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

El comando anterior agrega los paquetes de NuGet para el archivo *.csproj:

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Aplicar la técnica scaffolding el modelo

* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Ejecute los siguientes comandos:


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
Si se genera el error siguiente:

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

Vuelva a ejecutar el comando y deje un comentario en la parte inferior de la página.

Si se produce un error:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).


Compile el proyecto. La compilación genera errores similar al siguiente:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Cambiar globalmente `_context.Student` a `_context.Students` (es decir, agregue una "s" para `Student`). 7 apariciones se encuentra y se actualizan. Esperamos que corregir [este error](https://github.com/aspnet/Scaffolding/issues/633) en la próxima versión.

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Prueba de la aplicación

Ejecute la aplicación y seleccione la **estudiantes** vínculo. Según el ancho del explorador, el **estudiantes** vínculo aparece en la parte superior de la página. Si el **estudiantes** vínculo no está visible, haga clic en el icono de navegación en la esquina superior derecha.

![Página de inicio de la Universidad de Contoso estrecho](intro/_static/home-page-narrow.png)

Prueba la **crear**, **editar**, y **detalles** vínculos.

## <a name="view-the-db"></a>Vista de la base de datos

Cuando se inicia la aplicación, `DbInitializer.Initialize` llamadas `EnsureCreated`. `EnsureCreated`detecta si existe la base de datos y crea uno si es necesario. Si no hay ningún alumno en la base de datos, la `Initialize` método agrega los alumnos.

Abra **Explorador de objetos de SQL Server** (SSOX) desde el **vista** menú en Visual Studio.
En SSOX, haga clic en **(localdb) \MSSQLLocalDB > bases de datos > ContosoUniversity1**.

Expanda el **tablas** nodo.

Haga clic en el **Student** de tabla y haga clic en **ver datos** para ver las columnas creadas y las filas insertadas en la tabla.

El *.mdf* y *.ldf* archivos de base de datos se encuentran en el *C:\Users\\ <yourusername>*  carpeta.

`EnsureCreated`se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguientes:

* Eliminar la base de datos.
* Cambiar el esquema de base de datos (por ejemplo, agregar un `EmailAddress` campo).
* Ejecute la aplicación.

`EnsureCreated`crea una base de datos con la`EmailAddress` columna.

## <a name="conventions"></a>Convenciones

La cantidad de código escrito en orden para EF Core crear una base de datos completa es mínima debido al uso de convenciones o suposiciones que hace que EF Core.

* Los nombres de `DbSet` propiedades se utilizan como nombres de tabla. Para las entidades que no hace referencia a un `DbSet` propiedad, clase de entidad nombres se utilizan como nombres de tabla.

* Nombres de propiedad de entidad se utilizan para los nombres de columna.

* Propiedades de la entidad que se denominan identificador o classnameID se reconocen como propiedades de clave principal.

* Una propiedad se interpreta como una propiedad de clave externa si se denomina  *<navigation property name> <primary key property name>*  (por ejemplo, `StudentID` para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`). Propiedades de clave externa pueden denominarse  *<primary key property name>*  (por ejemplo, `EnrollmentID` desde el `Enrollment` clave principal de la entidad es `EnrollmentID`).

Puede reemplazarse un comportamiento convencional. Por ejemplo, los nombres de tabla pueden especificarse explícitamente, como se muestra anteriormente en este tutorial. Los nombres de columna se pueden establecer explícitamente. Las claves principales y claves externas se pueden establecer explícitamente.

## <a name="asynchronous-code"></a>Código asincrónico

La programación asincrónica es el modo predeterminado de EF y de núcleo de ASP.NET.

Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de alta carga todos los subprocesos disponibles pueden estar en uso. Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que se liberan los subprocesos. Con código sincrónico, se pueden acumular muchos subprocesos la mientras que realmente no están realizando ningún trabajo porque está a la espera de e/s completar. Con código asincrónico, cuando un proceso está en espera de e/s completar, un subproceso se liberen para el servidor que se usará para procesando otras solicitudes. Como resultado, código asincrónico permite que los recursos de servidor que se usará de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.

Código asincrónico que introduce una pequeña cantidad de sobrecarga en tiempo de ejecución. En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras en situaciones de mucho tráfico, la mejora del rendimiento potencial es considerable.

En el código siguiente, la `async` palabra clave, `Task<T>` devolver valor, `await` (palabra clave), y `ToListAsync` método que el código se ejecutará de forma asincrónica.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* El `async` palabra clave indica al compilador que:

  * Generar las devoluciones de llamada para las partes del cuerpo del método.
  * Crear automáticamente la [tarea](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) objeto que se devuelve. Para obtener más información, consulte [tipo de valor devuelto de la tarea](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* Tipo devuelto de la parte implícita `Task` representa el trabajo en curso.

* El `await` (palabra clave) hace que el compilador dividir el método en dos partes. La primera parte se termina con la operación que se inicia de forma asincrónica. La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.

* `ToListAsync`es la versión asincrónica de la `ToList` método de extensión.

Algunos aspectos que tener en cuenta al escribir código asincrónico que usa EF principales:

* Solo las instrucciones que hacen que las consultas o comandos se envíen a la base de datos se ejecutan de forma asincrónica. Que incluye, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, y `SaveChangesAsync`. No incluye instrucciones que acaba de cambiar un `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Un contexto de núcleos de EF no es de subproceso seguro: no intentar llevar a cabo varias operaciones en paralelo. 

* Para sacar partido de las ventajas de rendimiento de código asincrónico, compruebe que paquetes de biblioteca (por ejemplo, para la paginación) usan async si llaman a métodos de EF Core que envían consultas a la base de datos.

Para obtener más información sobre la programación asincrónica en. NET, vea [información general de Async](https://docs.microsoft.com/dotnet/articles/standard/async).

En el siguiente tutorial, básica CRUD (crear, leer, actualizar y eliminar) se examinan las operaciones.

>[!div class="step-by-step"]
[Siguiente](xref:data/ef-rp/crud)
