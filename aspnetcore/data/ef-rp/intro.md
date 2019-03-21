---
title: 'Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8'
author: rick-anderson
description: Se muestra cómo crear una aplicación de páginas de Razor mediante Entity Framework Core
ms.author: riande
ms.custom: seodec18
ms.date: 11/22/2018
uid: data/ef-rp/intro
ms.openlocfilehash: 7b048c8543f51a158530b8c7952b5aec7b753b42
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264698"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>Páginas de Razor con Entity Framework Core en ASP.NET Core: Tutorial 1 de 8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

En la aplicación web de ejemplo Contoso University se muestra cómo crear una aplicación web de Razor Pages de ASP.NET Core con Entity Framework (EF) Core.

La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores. Esta página es la primera de una serie de tutoriales en los que se explica cómo crear la aplicación de ejemplo Contoso University.

[Descargue o vea la aplicación completa.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [Instrucciones de descarga](xref:index#how-to-download-a-sample).

## <a name="prerequisites"></a>Requisitos previos

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

Familiaridad con las [Páginas de Razor](xref:razor-pages/index). Los programadores nuevos deben completar [Introducción a las páginas de Razor en ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) antes de empezar esta serie.

## <a name="troubleshooting"></a>Solución de problemas

Si experimenta un problema que no puede resolver, por lo general podrá encontrar la solución si compara el código con el [proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples). Una buena forma de obtener ayuda consiste en publicar una pregunta en [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).

## <a name="the-contoso-university-web-app"></a>La aplicación web Contoso University

La aplicación compilada en estos tutoriales es un sitio web básico de una universidad.

Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores. Estas son algunas de las pantallas que se crean en el tutorial.

![Página de índice de Students](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

El estilo de la interfaz de usuario de este sitio se mantiene fiel a lo que generan las plantillas integradas. El tutorial se centra en EF Core con páginas de Razor, no en la interfaz de usuario.

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>Creación de la aplicación web de Razor Pages ContosoUniversity

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el menú **Archivo** de Visual Studio, seleccione **Nuevo** > **Proyecto**.
* Cree una aplicación web de ASP.NET Core. Asigne el nombre **ContosoUniversity** al proyecto. Es importante que el nombre del proyecto sea *ContosoUniversity* para que coincidan los espacios de nombres al copiar y pegar el código.
* Seleccione **ASP.NET Core 2.1** en la lista desplegable y, luego, **Aplicación web**.

Para ver las imágenes de los pasos anteriores, consulte [Creación de una aplicación web de Razor](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).
Ejecute la aplicación.

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>Configurar el estilo del sitio

Con algunos cambios se configura el menú del sitio, el diseño y la página principal. Actualice *Pages/Shared/_Layout.cshtml* con los cambios siguientes:

* Cambie todas las repeticiones de "ContosoUniversity" por "Contoso University". Hay tres repeticiones.

* Agregue entradas de menú para **Students**, **Courses**, **Instructors** y **Departments**, y elimine la entrada de menú **Contact**.

Los cambios aparecen resaltados. (*No* se muestra todo el marcado).

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

En *Pages/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>Crear el modelo de datos

Cree las clases de entidad para la aplicación Contoso University. Comience con las tres entidades siguientes:

![Diagrama del modelo de datos Course-Enrollment-Student](intro/_static/data-model-diagram.png)

Hay una relación uno a varios entre las entidades `Student` y `Enrollment`. Hay una relación uno a varios entre las entidades `Course` y `Enrollment`. Un estudiante se puede inscribir en cualquier número de cursos. Un curso puede tener cualquier número de alumnos inscritos.

En las secciones siguientes, se crea una clase para cada una de estas entidades.

### <a name="the-student-entity"></a>La entidad Student

![Diagrama de la entidad Student](intro/_static/student-entity.png)

Cree una carpeta *Models*. En la carpeta *Models*, cree un archivo de clase denominado *Student.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

La propiedad `ID` se convierte en la columna de clave principal de la tabla de base de datos (DB) que corresponde a esta clase. De forma predeterminada, EF Core interpreta como la clave principal una propiedad que se denomine `ID` o `classnameID`. En `classnameID`, `classname` es el nombre de la clase. En el ejemplo anterior, la clave principal alternativa que se reconoce de forma automática es `StudentID`.

La propiedad `Enrollments` es una [propiedad de navegación](/ef/core/modeling/relationships). Las propiedades de navegación se vinculan a otras entidades relacionadas con esta entidad. En este caso, la propiedad `Enrollments` de una `Student entity` contiene todas las entidades `Enrollment` que están relacionadas con esa entidad `Student`. Por ejemplo, si una fila Student de la base de datos tiene dos filas Enrollment relacionadas, la propiedad de navegación `Enrollments` contiene esas dos entidades `Enrollment`. Una fila `Enrollment` relacionada es la que contiene el valor de clave principal de ese estudiante en la columna `StudentID`. Por ejemplo, suponga que el estudiante con ID=1 tiene dos filas en la tabla `Enrollment`. La tabla `Enrollment` tiene dos filas con `StudentID` = 1. `StudentID` es una clave externa en la tabla `Enrollment` que especifica el estudiante en la tabla `Student`.

Si una propiedad de navegación puede contener varias entidades, la propiedad de navegación debe ser un tipo de lista, como `ICollection<T>`. Se puede especificar `ICollection<T>`, o bien un tipo como `List<T>` o `HashSet<T>`. Cuando se usa `ICollection<T>`, EF Core crea una colección `HashSet<T>` de forma predeterminada. Las propiedades de navegación que contienen varias entidades proceden de relaciones de varios a varios y uno a varios.

### <a name="the-enrollment-entity"></a>La entidad Enrollment

![Diagrama de la entidad Enrollment](intro/_static/enrollment-entity.png)

En la carpeta *Models*, cree *Enrollment.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

La propiedad `EnrollmentID` es la clave principal. En esta entidad se usa el patrón `classnameID` en lugar de `ID` como en la entidad `Student`. Normalmente, los desarrolladores eligen un patrón y lo usan en todo el modelo de datos. En un tutorial posterior, se muestra el uso de ID sin un nombre de clase para facilitar la implementación de la herencia en el modelo de datos.

La propiedad `Grade` es una `enum`. El signo de interrogación después de la declaración de tipo `Grade` indica que la propiedad `Grade` acepta valores NULL. Una calificación que sea NULL es diferente de una calificación que sea cero; NULL significa que no se conoce una calificación o que todavía no se ha asignado.

La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`. Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad contiene una única entidad `Student`. La entidad `Student` difiere de la propiedad de navegación `Student.Enrollments`, que contiene varias entidades `Enrollment`.

La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`. Una entidad `Enrollment` está asociada con una entidad `Course`.

EF Core interpreta una propiedad como una clave externa si se denomina `<navigation property name><primary key property name>`. Por ejemplo, `StudentID` para la propiedad de navegación `Student`, puesto que la clave principal de la entidad `Student` es `ID`. Las propiedades de clave externa también se pueden denominar `<primary key property name>`. Por ejemplo `CourseID`, dado que la clave principal de la entidad `Course` es `CourseID`.

### <a name="the-course-entity"></a>La entidad Course

![Diagrama de la entidad Course](intro/_static/course-entity.png)

En la carpeta *Models*, cree *Course.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

La propiedad `Enrollments` es una propiedad de navegación. Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.

El atributo `DatabaseGenerated` permite que la aplicación especifique la clave principal en lugar de hacer que la base de datos la genere.

## <a name="scaffold-the-student-model"></a>Aplicación de scaffolding al modelo de alumnos

En esta sección, se aplica scaffolding al modelo de alumnos. Es decir, la herramienta de scaffolding genera páginas para las operaciones de creación, lectura, actualización y eliminación (CRUD) del modelo de alumnos.

* Compile el proyecto.
* Cree la carpeta *Pages/Students*.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En el **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *Pages/Students* > **Agregar** > **Nuevo elemento con scanffold**.
* En el cuadro de diálogo **Agregar scaffold**, seleccione **Razor Pages using Entity Framework (CRUD)** [Páginas de Razor Pages que usan Entity Framework (CRUD)] > **AGREGAR**.

Complete el cuadro de diálogo para **agregar páginas de Razor Pages que usan Entity Framework (CRUD)**:

* En la lista desplegable **Clase de modelo**, seleccione **Student (ContosoUniversity.Models)**.
* En la fila **Clase de contexto de datos**, haga clic en el signo **+** (más) y cambie el nombre generado por **ContosoUniversity.Models.SchoolContext**.
* En la lista desplegable **Clase de contexto de datos**, seleccione **ContosoUniversity.Models.SchoolContext**
* Seleccione **Agregar**.

![Cuadro de diálogo CRUD](intro/_static/s1.png)

Si tiene algún problema con el paso anterior, consulte [Aplicar scaffolding al modelo de película](xref:tutorials/razor-pages/model#scaffold-the-movie-model).

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute los comandos siguientes para aplicar scaffolding al modelo de alumnos.

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

------

El proceso de scaffolding ha creado y cambiado los archivos siguientes:

### <a name="files-created"></a>Archivos creados

* *Pages/Students* Create, Delete, Details, Edit, Index.
* *Data/SchoolContext.cs*

### <a name="file-updates"></a>Actualizaciones de archivos

* *Startup.cs*: en la sección siguiente se detallan los cambios realizados en este archivo.
* *appsettings.json*: se agrega la cadena de conexión que se usa para conectarse a una base de datos local.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Examinar el contexto registrado con la inserción de dependencias

ASP.NET Core integra la [inserción de dependencias](xref:fundamentals/dependency-injection). Los servicios (como el contexto de base de datos de EF Core) se registran con inserción de dependencias durante el inicio de la aplicación. Estos servicios se proporcionan a los componentes que los necesitan (como las páginas de Razor) a través de parámetros de constructor. El código de constructor que obtiene una instancia de contexto de base de datos se muestra más adelante en el tutorial.

La herramienta de scaffolding creó de forma automática un contexto de base de datos y lo registró con el contenedor de inserción de dependencias.

Examine el método `ConfigureServices` de *Startup.cs*. El proveedor de scaffolding ha agregado la línea resaltada:

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un objeto [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions). Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee la cadena de conexión desde el archivo *appsettings.json*.

## <a name="update-main"></a>Actualización de main

En *Program.cs*, modifique el método `Main` para que haga lo siguiente:

* Obtener una instancia del contexto de base de datos desde el contenedor de inserción de dependencias.
* Llame a [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).
* Elimine el contexto cuando finalice el método `EnsureCreated`.

En el código siguiente se muestra el archivo *Program.cs* actualizado.

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` garantiza la existencia de la base de datos para el contexto. Si existe, no se realiza ninguna acción. Si no existe, se crean la base de datos y todo su esquema. En `EnsureCreated` no se usan migraciones para crear la base de datos. Una base de datos que se cree con `EnsureCreated` no se podrá actualizar más adelante mediante las migraciones.

`EnsureCreated` se llama durante el inicio de la aplicación, lo que permite el flujo de trabajo siguiente:

* Se elimina la base de datos.
* Se cambia el esquema de base de datos (por ejemplo, se agrega un campo `EmailAddress`).
* Ejecute la aplicación.
* `EnsureCreated` crea una base de datos con la columna `EmailAddress`.

`EnsureCreated` es útil al principio del desarrollo, cuando el esquema evoluciona rápidamente. Más adelante, en el tutorial se elimina la base de datos y se usan las migraciones.

### <a name="test-the-app"></a>Prueba de la aplicación

Ejecute la aplicación y acepte la directiva de cookies. Esta aplicación no conserva información de carácter personal. Puede obtener más información sobre la directiva de cookies en [Compatibilidad con el Reglamento general de protección de datos (RGPD) de la UE](xref:security/gdpr).

* Haga clic en el vínculo **Students** y, después, en **Crear nuevo**.
* Pruebe los vínculos Edit, Details y Delete.

## <a name="examine-the-schoolcontext-db-context"></a>Examinar el contexto de base de datos SchoolContext

La clase principal que coordina la funcionalidad de EF Core para un modelo de datos determinado es la clase de contexto de base de datos. El contexto de datos se deriva de [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext). En el contexto de datos se especifica qué entidades se incluyen en el modelo de datos. En este proyecto, la clase se denomina `SchoolContext`.

Actualice *SchoolContext.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

El código resaltado crea una propiedad [DbSet\<TEntity >](/dotnet/api/microsoft.entityframeworkcore.dbset-1) para cada conjunto de entidades. En la terminología de EF Core:

* Un conjunto de entidades normalmente se corresponde a una tabla de base de datos.
* Una entidad se corresponde con una fila de la tabla.

`DbSet<Enrollment>` y `DbSet<Course>` se pueden omitir. EF Core las incluye implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`. Para este tutorial, conserve `DbSet<Enrollment>` y `DbSet<Course>` en el `SchoolContext`.

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La cadena de conexión especifica [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb). LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está dirigida al desarrollo de aplicaciones, no al uso en producción. LocalDB se inicia a petición y se ejecuta en modo de usuario, sin necesidad de una configuración compleja. De forma predeterminada, LocalDB crea archivos de base de datos *.mdf* en el directorio `C:/Users/<user>`.

## <a name="add-code-to-initialize-the-db-with-test-data"></a>Agregar código para inicializar la base de datos con datos de prueba

EF Core crea una base de datos vacía. En esta sección, se escribe un método `Initialize` para rellenarlo con datos de prueba.

En la carpeta *Data*, cree un archivo de clase denominado *DbInitializer.cs* y agregue el código siguiente:

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

Nota: El código anterior usa `Models` para el espacio de nombres (`namespace ContosoUniversity.Models`) en lugar de `Data`. `Models` es coherente con el código generado por el proveedor de scaffolding. Para obtener más información, consulte [este problema de scaffolding de GitHub](https://github.com/aspnet/Scaffolding/issues/822).

El código comprueba si hay estudiantes en la base de datos. Si no hay alumnos en la base de datos, se inicializa con datos de prueba. Carga los datos de prueba en matrices en lugar de colecciones `List<T>` para optimizar el rendimiento.

El método `EnsureCreated` crea automáticamente la base de datos para el contexto de base de datos. Si la base de datos existe, `EnsureCreated` vuelve sin modificarla.

En *Program.cs*, modifique el método `Main` para que llame a `Initialize`:

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

Elimine los registros de los alumnos y reinicie la aplicación. Si la base de datos no se ha inicializado, establezca un punto de interrupción en `Initialize` para diagnosticar el problema.

## <a name="view-the-db"></a>Ver la base de datos

Abra el **Explorador de objetos de SQL Server** (SSOX) desde el menú **Vista** en Visual Studio.
En SSOX, haga clic en **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.

Expanda el nodo **Tablas**.

Haga clic con el botón derecho en la tabla **Student** y haga clic en **Ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

## <a name="asynchronous-code"></a>Código asincrónico

La programación asincrónica es el modo predeterminado de ASP.NET Core y EF Core.

Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso. Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen. Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S. Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes. Como resultado, el código asincrónico permite que los recursos de servidor se usen de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.

El código asincrónico introduce una pequeña cantidad de sobrecarga en tiempo de ejecución. En situaciones de poco tráfico, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es importante.

En el código siguiente, la palabra clave [async](/dotnet/csharp/language-reference/keywords/async), el valor devuelto `Task<T>`, la palabra clave `await` y el método `ToListAsync` hacen que el código se ejecute de forma asincrónica.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* La palabra clave `async` indica al compilador que:
  * Genere devoluciones de llamada para partes del cuerpo del método.
  * Cree automáticamente el objeto [Task](/dotnet/api/system.threading.tasks.task) que se devuelve. Para más información, vea [Tipo de valor devuelto Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).

* El tipo devuelto implícito `Task` representa el trabajo en curso.
* La palabra clave `await` hace que el compilador divida el método en dos partes. La primera parte termina con la operación que se inició de forma asincrónica. La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.
* `ToListAsync` es la versión asincrónica del método de extensión `ToList`.

Algunos aspectos que tener en cuenta al escribir código asincrónico en el que se usa EF Core son los siguientes:

* Solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos. Esto incluye `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync` y `SaveChangesAsync`. No incluye las instrucciones que solo cambian una `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.
* Un contexto de EF Core no es seguro para subprocesos: no intente realizar varias operaciones en paralelo.
* Para aprovechar las ventajas de rendimiento del código asincrónico, compruebe que en los paquetes de biblioteca (por ejemplo para paginación) se usa async si llaman a métodos de EF Core que envían consultas a la base de datos.

Para obtener más información sobre la programación asincrónica en .NET, vea [Programación asincrónica](/dotnet/standard/async) y [Programación asincrónica con async y await](/dotnet/csharp/programming-guide/concepts/async/).

En el siguiente tutorial, se examinan las operaciones CRUD (crear, leer, actualizar y eliminar) básicas.

::: moniker-end

## <a name="additional-resources"></a>Recursos adicionales

* [Versión en YouTube de este tutorial](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [Siguiente](xref:data/ef-rp/crud)
