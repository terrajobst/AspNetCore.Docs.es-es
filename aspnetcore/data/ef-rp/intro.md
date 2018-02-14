---
title: "Páginas de Razor con Entity Framework Core: Tutorial 1 de 8"
author: rick-anderson
description: "Se muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>Introducción a las páginas de Razor y Entity Framework Core con Visual Studio (1 de 8)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

En la aplicación web de ejemplo Contoso University se muestra cómo crear aplicaciones web ASP.NET Core 2.0 MVC con Entity Framework (EF) Core 2.0 y Visual Studio 2017.

La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores. Esta página es la primera de una serie de tutoriales en los que se explica cómo crear la aplicación de ejemplo Contoso University.

[Descargue o vea la aplicación completa.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrucciones de descarga](xref:tutorials/index#how-to-download-a-sample).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

Familiaridad con las [Páginas de Razor](xref:mvc/razor-pages/index). Los programadores nuevos deben completar [Introducción a las páginas de Razor en ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) antes de empezar esta serie.

## <a name="troubleshooting"></a>Solución de problemas

Si experimenta un problema que no puede resolver, por lo general podrá encontrar la solución si compara el código con la [fase completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) o el [proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final). Para obtener una lista de errores comunes y cómo resolverlos, vea [la sección de solución de problemas del último tutorial de la serie](xref:data/ef-mvc/advanced#common-errors). Si ahí no encuentra lo que necesita, puede publicar una pregunta en [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP]
> Esta serie de tutoriales se basa en lo que se realiza en los tutoriales anteriores. Considere la posibilidad de guardar una copia del proyecto después de completar correctamente cada tutorial. Si experimenta problemas, puede empezar desde el tutorial anterior en lugar de volver al principio. Como alternativa, puede descargar una [fase completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) y empezar de nuevo con la fase completada.

## <a name="the-contoso-university-web-app"></a>La aplicación web Contoso University

La aplicación compilada en estos tutoriales es un sitio web básico de una universidad.

Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores. Estas son algunas de las pantallas que se crean en el tutorial.

![Página de índice de Students](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

El estilo de la interfaz de usuario de este sitio se mantiene fiel a lo que generan las plantillas integradas. El tutorial se centra en EF Core con páginas de Razor, no en la interfaz de usuario.

## <a name="create-a-razor-pages-web-app"></a>Creación de una aplicación web de páginas de Razor

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core. Asigne el nombre **ContosoUniversity** al proyecto. Es importante que el nombre del proyecto sea *ContosoUniversity* para que coincidan los espacios de nombres al copiar y pegar el código.
 ![Nueva aplicación web de ASP.NET Core](intro/_static/np.png)
* Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.
 ![Aplicación web (páginas de Razor)](../../mvc/razor-pages/index/_static/np2.png)

Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.

## <a name="set-up-the-site-style"></a>Configurar el estilo del sitio

Con algunos cambios se configura el menú del sitio, el diseño y la página principal.

Abra *Pages/_Layout.cshtml* y realice los cambios siguientes:

* Cambie todas las repeticiones de "ContosoUniversity" por "Contoso University". Hay tres repeticiones.

* Agregue entradas de menú para **Students**, **Courses**, **Instructors** y **Departments**, y elimine la entrada de menú **Contact**.

Los cambios aparecen resaltados. (*No* se muestra todo el marcado).

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

Presione CTRL+F5 para ejecutar el proyecto. La página principal se muestra con las pestañas creadas en los tutoriales siguientes:

![Página de inicio de Contoso University](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>Crear el modelo de datos

Cree las clases de entidad para la aplicación Contoso University. Comience con las tres entidades siguientes:

![Diagrama del modelo de datos Course-Enrollment-Student](intro/_static/data-model-diagram.png)

Hay una relación uno a varios entre las entidades `Student` y `Enrollment`. Hay una relación uno a varios entre las entidades `Course` y `Enrollment`. Un estudiante se puede inscribir en cualquier número de cursos. Un curso puede tener cualquier número de alumnos inscritos.

En las secciones siguientes, se crea una clase para cada una de estas entidades.

### <a name="the-student-entity"></a>La entidad Student

![Diagrama de la entidad Student](intro/_static/student-entity.png)

Cree una carpeta *Models*. En la carpeta *Models*, cree un archivo de clase denominado *Student.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

La propiedad `ID` se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase. De forma predeterminada, EF Core interpreta como la clave principal una propiedad que se denomine `ID` o `classnameID`.

La propiedad `Enrollments` es una propiedad de navegación. Las propiedades de navegación se vinculan a otras entidades relacionadas con esta entidad. En este caso, la propiedad `Enrollments` de una `Student entity` contiene todas las entidades `Enrollment` que están relacionadas con esa entidad `Student`. Por ejemplo, si una fila Student de la base de datos tiene dos filas Enrollment relacionadas, la propiedad de navegación `Enrollments` contiene esas dos entidades `Enrollment`. Una fila `Enrollment` relacionada es la que contiene el valor de clave principal de ese estudiante en la columna `StudentID`. Por ejemplo, suponga que el estudiante con ID=1 tiene dos filas en la tabla `Enrollment`. La tabla `Enrollment` tiene dos filas con `StudentID` = 1. `StudentID` es una clave externa en la tabla `Enrollment` que especifica el estudiante en la tabla `Student`.

Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`. Se puede especificar `ICollection<T>`, o bien un tipo como `List<T>` o `HashSet<T>`. Cuando se usa `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada. Las propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.

### <a name="the-enrollment-entity"></a>La entidad Enrollment

![Diagrama de la entidad Enrollment](intro/_static/enrollment-entity.png)

En la carpeta *Models*, cree *Enrollment.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

La propiedad `EnrollmentID` es la clave principal. En esta entidad se usa el patrón `classnameID` en lugar de `ID` como en la entidad `Student`. Normalmente, los desarrolladores eligen un patrón y lo usan en todo el modelo de datos. En un tutorial posterior, se muestra el uso de ID sin un nombre de clase para facilitar la implementación de la herencia en el modelo de datos.

La propiedad `Grade` es una `enum`. El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` acepta valores NULL. Una calificación que sea NULL es diferente de una calificación que sea cero; NULL significa que no se conoce una calificación o que todavía no se ha asignado.

La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`. Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad contiene una única entidad `Student`. La entidad `Student` difiere de la propiedad de navegación `Student.Enrollments`, que contiene varias entidades `Enrollment`.

La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`. Una entidad `Enrollment` está asociada con una entidad `Course`.

EF Core interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`. Por ejemplo, `StudentID` para la propiedad de navegación `Student`, puesto que la clave principal de la entidad `Student` es `ID`. Las propiedades de clave externa también se pueden denominar `<primary key property name>`. Por ejemplo `CourseID`, dado que la clave principal de la entidad `Course` es `CourseID`.

### <a name="the-course-entity"></a>La entidad Course

![Diagrama de la entidad Course](intro/_static/course-entity.png)

En la carpeta *Models*, cree *Course.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

La propiedad `Enrollments` es una propiedad de navegación. Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.

El atributo `DatabaseGenerated` permite que la aplicación especifique la clave principal en lugar de hacer que la base de datos la genere.

## <a name="create-the-schoolcontext-db-context"></a>Crear el contexto de base de datos SchoolContext

La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos. El contexto de datos se deriva de `Microsoft.EntityFrameworkCore.DbContext`. En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos. En este proyecto, la clase se denomina `SchoolContext`.

En la carpeta del proyecto, cree una carpeta denominada *Data*.

En la carpeta *Data*, cree *SchoolContext.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Este código crea una propiedad `DbSet` para cada conjunto de entidades. En la terminología de EF Core:

* Un conjunto de entidades normalmente se corresponde a una tabla de base de datos.
* Una entidad se corresponde con una fila de la tabla.

`DbSet<Enrollment>` y `DbSet<Course>` se pueden omitir. EF Core las incluye implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`. Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.

Cuando se crea la base de datos, EF Core crea las tablas con los mismos nombres que los nombres de propiedad `DbSet`. Los nombres de propiedad para las colecciones normalmente están en plural (Students en lugar de Student). Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben estar en plural. Para estos tutoriales, se invalida el comportamiento predeterminado mediante la especificación de nombres de tabla en singular en DbContext. Para especificar los nombres de tabla en singular, agregue el código resaltado siguiente:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registro del contexto con inserción de dependencias

ASP.NET Core incluye la [inserción de dependencias](xref:fundamentals/dependency-injection). Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación. Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor. El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.

Para registrar `SchoolContext` como servicio, abra *Startup.cs* y agregue las líneas resaltadas al método `ConfigureServices`.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto `DbContextOptionsBuilder`. Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.

Agregue instrucciones `using` para los espacios de nombres `ContosoUniversity.Data` y `Microsoft.EntityFrameworkCore`. Compile el proyecto.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Abra el archivo *appsettings.json* y agregue una cadena de conexión como se muestra en el código siguiente:

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

En la cadena de conexión anterior se usa `ConnectRetryCount=0` para evitar que [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) se bloquee.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La cadena de conexión especifica una base de datos de SQL Server LocalDB. LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está dirigida al desarrollo de aplicaciones, no al uso en producción. LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja. De forma predeterminada, LocalDB crea archivos de base de datos *.mdf* en el directorio `C:/Users/<user>`.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Agregar código para inicializar la base de datos con datos de prueba

EF Core crea una base de datos vacía. En esta sección, se escribe un método *Seed* para rellenarla con datos de prueba.

En la carpeta *Data*, cree un archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

El código comprueba si hay estudiantes en la base de datos. Si no hay ningún estudiante en la base de datos, se inicializa con datos de prueba. Carga los datos de prueba en matrices en lugar de colecciones `List<T>` para optimizar el rendimiento.

El método `EnsureCreated` crea automáticamente la base de datos para el contexto de base de datos. Si la base de datos existe, `EnsureCreated` vuelve sin modificarla.

En *Program.cs*, modifique el método `Main` para que haga lo siguiente:

* Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.
* Llamar al método de inicialización, pasándolo al contexto.
* Eliminar el contexto cuando el método de inicialización finalice.

En el código siguiente se muestra el archivo *Program.cs* actualizado.

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

La primera vez que se ejecuta la aplicación, se crea la base de datos y se inicializa con datos de prueba. Cuando se actualice el modelo de datos:
* Se elimina la base de datos.
* Se actualiza el método de inicialización.
* Se ejecuta la aplicación y se crea una base de datos inicializada. 

En los tutoriales posteriores, la base de datos se actualiza cuando cambia el modelo de datos, sin tener que eliminarla y volver a crearla.

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>Agregar herramientas de scaffolding

En esta sección, se usa la Consola del Administrador de paquetes (PMC) para agregar el paquete de generación de código web de Visual Studio. Este paquete es necesario para ejecutar el motor de scaffolding.

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

En la Consola del Administrador de paquetes (PMC), escriba los comandos siguientes:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

El comando anterior agrega los paquetes NuGet al archivo *.csproj:

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Aplicar scaffolding al modelo

* Abra una ventana de comandos en el directorio del proyecto (el directorio que contiene los archivos *Program.cs*, *Startup.cs* y *.csproj*).
* Ejecute los comandos siguientes:


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


Compile el proyecto. La compilación genera errores similares a los siguientes:

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 Cambie globalmente `_context.Student` por `_context.Students` (es decir, agregue una "s" a `Student`). Se encuentran y actualizan siete repeticiones. Esperamos solucionar [este problema](https://github.com/aspnet/Scaffolding/issues/633) en la próxima versión.

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>Prueba de la aplicación

Ejecute la aplicación y haga clic en el vínculo **Students**. Según el ancho del explorador, el vínculo **Students** aparece en la parte superior de la página. Si el vínculo **Students** no se ve, haga clic en el icono de navegación en la esquina superior derecha.

![Página de inicio estrecha de Contoso University](intro/_static/home-page-narrow.png)

Pruebe los vínculos **Create**, **Edit** y **Details**.

## <a name="view-the-db"></a>Ver la base de datos

Cuando se inicia la aplicación, `DbInitializer.Initialize` llama a `EnsureCreated`. `EnsureCreated` detecta si la base de datos existe y crea una si es necesario. Si no hay ningún estudiante en la base de datos, el método `Initialize` los agrega.

Abra el **Explorador de objetos de SQL Server** (SSOX) desde el menú **Vista** en Visual Studio.
En SSOX, haga clic en **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.

Expanda el nodo **Tablas**.

Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

Los archivos de base de datos *.mdf* y *.ldf* se encuentran en la carpeta *C:\Usuarios\\<yourusername>*.

`EnsureCreated` se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguiente:

* Se elimina la base de datos.
* Se cambia el esquema de base de datos (por ejemplo, se agrega un campo `EmailAddress`).
* Ejecute la aplicación.

`EnsureCreated` crea una base de datos con la columna `EmailAddress`.

## <a name="conventions"></a>Convenciones

La cantidad de código que se escribe para que EF Core cree una base de datos completa es mínima debido al uso de convenciones o a las suposiciones que hace EF Core.

* Los nombres de las propiedades `DbSet` se usan como nombres de tabla. Para las entidades a las que no se hace referencia con una propiedad `DbSet`, los nombres de clase de entidad se usan como nombres de tabla.

* Los nombres de propiedad de entidad se usan para los nombres de columna.

* Las propiedades de entidad que se denominan ID o classnameID se reconocen como propiedades de clave principal.

* Una propiedad se interpreta como propiedad de clave externa si se denomina *<navigation property name><primary key property name>* (por ejemplo, `StudentID` para la propiedad de navegación `Student`, dado que la clave principal de la entidad `Student` es `ID`). Las propiedades de clave externa también se pueden denominar *<primary key property name>* (por ejemplo `EnrollmentID`, dado que la clave principal de la entidad `Enrollment` es `EnrollmentID`).

El comportamiento de las convenciones se puede reemplazar. Por ejemplo, los nombres de tabla se pueden especificar explícitamente, como se muestra anteriormente en este tutorial. Los nombres de columna se pueden establecer explícitamente. Las claves principales y las claves externas se pueden establecer explícitamente.

## <a name="asynchronous-code"></a>Código asincrónico

La programación asincrónica es el modo predeterminado de ASP.NET Core y EF Core.

Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso. Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen. Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S. Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes. Como resultado, el código asincrónico permite que los recursos de servidor se usen de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.

El código asincrónico introduce una pequeña cantidad de sobrecarga en tiempo de ejecución. En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es importante.

En el código siguiente, la palabra clave `async`, el valor devuelto `Task<T>`, la palabra clave `await` y el método `ToListAsync` hacen que el código se ejecute de forma asincrónica.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* La palabra clave `async` indica al compilador que:

  * Genere devoluciones de llamada para partes del cuerpo del método.
  * Cree automáticamente el objeto [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) que se devuelve. Para más información, vea [Tipo de valor devuelto Task](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* El tipo devuelto implícito `Task` representa el trabajo en curso.

* La palabra clave `await` hace que el compilador divida el método en dos partes. La primera parte termina con la operación que se inició de forma asincrónica. La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.

* `ToListAsync` es la versión asincrónica del método de extensión `ToList`.

Algunos aspectos que tener en cuenta al escribir código asincrónico en el que se usa EF Core son los siguientes:

* Solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos. Esto incluye `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` y `SaveChangesAsync`. No incluye las instrucciones que solo cambian una `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Un contexto de EF Core no es seguro para subprocesos: no intente realizar varias operaciones en paralelo. 

* Para aprovechar las ventajas de rendimiento del código asincrónico, compruebe que en los paquetes de biblioteca (por ejemplo para paginación) se usa async si llaman a métodos de EF Core que envían consultas a la base de datos.

Para obtener más información sobre la programación asincrónica en .NET, vea [Información general de Async](https://docs.microsoft.com/dotnet/articles/standard/async).

En el siguiente tutorial, se examinan las operaciones CRUD (crear, leer, actualizar y eliminar) básicas.

>[!div class="step-by-step"]
[Siguiente](xref:data/ef-rp/crud)
