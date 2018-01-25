---
title: "Núcleo de ASP.NET MVC con Entity Framework Core - Tutorial 1 de 10"
author: tdykstra
description: 
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: c30556368ba24fb38cf3347dd49f171b5246514c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a>Introducción a ASP.NET MVC de núcleo y Entity Framework Core con Visual Studio (1 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Hay disponible una versión de las páginas de Razor de este tutorial [aquí](xref:data/ef-rp/intro). La versión de las páginas de Razor es más fácil de seguir y abarca más características de EF. Le recomendamos que siga el [versión de las páginas de Razor de este tutorial](xref:data/ef-rp/intro).

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core 2.0 mediante 2017 de Visual Studio y los núcleos de Entity Framework (EF) 2.0.

La aplicación de ejemplo es un sitio web de una universidad ficticia de Contoso. Incluye una funcionalidad, como la admisión de estudiantes, la creación de curso y asignaciones de instructor. Éste es el primero de una serie de tutoriales que explican cómo crear la aplicación de ejemplo Contoso Universidad desde el principio.

[Descargar o ver la aplicación completa.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

Núcleo EF 2.0 es la versión más reciente de EF pero aún no dispone de todas las características de EF 6.x. Para obtener información sobre cómo elegir entre EF 6.x y EF Core, vea [frente a EF Core. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/). Si elige EF 6.x, consulte [la versión anterior de esta serie de tutoriales](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

> [!NOTE]
> * Para la versión 1.1 de ASP.NET Core de este tutorial, vea el [versión de VS de 2017 actualización 2 de este tutorial en formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).
> * Para la versión de Visual Studio 2015 de este tutorial, vea la [versión de VS 2015 de la documentación de ASP.NET Core en formato PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>Solución de problemas

Si experimenta un problema no se puede resolver, por lo general puede encontrar la solución comparando el código para el [proyecto completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final). Para obtener una lista de errores comunes y cómo resolverlos, vea [la sección de solución de problemas del último tutorial de la serie](advanced.md#common-errors). Si no encuentra lo que necesita no existe, puede publicar una pregunta en StackOverflow.com para [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) o [Core EF](https://stackoverflow.com/questions/tagged/entity-framework-core).

> [!TIP] 
> Se trata de una serie de tutoriales de 10, cada uno de los cuales se basa en lo que se realiza en los tutoriales anteriores. Considere la posibilidad de guardar una copia del proyecto tras cada tutorial finalizar correctamente. A continuación, si experimenta problemas, puede iniciar desde el tutorial anterior en lugar de volver al principio de la serie completa.

## <a name="the-contoso-university-web-application"></a>La aplicación web de la Universidad de Contoso

La aplicación que se va a compilar en estos tutoriales es un sitio web de la Universidad simple.

Los usuarios pueden ver y actualizar la información de instructor, curso y estudiantes. Aquí le ofrecemos algunas de las pantallas que se va a crear.

![Página de índice de estudiantes](intro/_static/students-index.png)

![Página de edición de estudiantes](intro/_static/student-edit.png)

El estilo de la interfaz de usuario de este sitio se ha conservado a lo que se genera por las plantillas integradas, para que pueda centrarse el tutorial principalmente en cómo usar Entity Framework.

## <a name="create-an-aspnet-core-mvc-web-application"></a>Crear una aplicación web de MVC de ASP.NET Core

Abra Visual Studio y cree un nuevo proyecto de web en ASP.NET Core C# con el nombre con el nombre "ContosoUniversity".

* Desde el **archivo** menú, seleccione **nuevo > proyecto**.

* En el panel izquierdo, seleccione **instalado > Visual C# > Web**.

* Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core**.

* Escriba **ContosoUniversity** como el nombre y haga clic en **Aceptar**.

  ![Cuadro de diálogo Nuevo proyecto](intro/_static/new-project.png)

* Espere a que el **ASP.NET Core aplicación Web (.NET Core)** aparece el cuadro de diálogo

* Seleccione **ASP.NET Core 2.0** y **aplicación Web (Model-View-Controller)** plantilla.

  **Nota:** este tutorial requiere ASP.NET 2.0 de núcleo y Core EF 2.0 o posterior, asegúrese de que **ASP.NET Core 1.1** no está seleccionada.

* Asegúrese de que **autenticación** está establecido en **sin autenticación**.

* Haga clic en **Aceptar**

  ![Cuadro de diálogo Nuevo proyecto ASP.NET](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>Configurar el estilo de sitio

Algunos cambios sencillos a configurar en el menú de sitio, el diseño y la página principal.

Abra *Views/Shared/_Layout.cshtml* y realice los cambios siguientes:

* Cambiar cada aparición de "ContosoUniversity" a "Universidad de Contoso". Hay tres apariciones.

* Agregar entradas de menú para **estudiantes**, **cursos**, **instructores**, y **departamentos**y eliminar el **póngase en contacto con** entrada de menú.

Los cambios aparecen resaltados.

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

En *Views/Home/Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

Presione CTRL + F5 para ejecutar el proyecto o elija **Depurar > Iniciar sin depurar** en el menú. Vea la página principal con pestañas para las páginas que se creará en estos tutoriales.

![Página de inicio de la Universidad de Contoso](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Paquetes de Entity Framework Core NuGet

Para agregar compatibilidad con EF Core a un proyecto, instale al proveedor de base de datos que desea tener como destino. Este tutorial utiliza SQL Server y el paquete de proveedor es [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/). Este paquete se incluye en el [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, por lo que no tiene que instalarlo.

Este paquete y sus dependencias (`Microsoft.EntityFrameworkCore` y `Microsoft.EntityFrameworkCore.Relational`) proporcionan compatibilidad en tiempo de ejecución para EF. Va a agregar un paquete de herramientas más adelante, en la [migraciones](migrations.md) tutorial. 

Para obtener información acerca de otros proveedores de base de datos que están disponibles para Entity Framework Core, vea [proveedores de la base de datos](https://docs.microsoft.com/ef/core/providers/).

## <a name="create-the-data-model"></a>Crear el modelo de datos

A continuación podrá crear las clases de entidad para la aplicación de la Universidad de Contoso. Se empieza por las siguientes tres entidades.

![Diagrama del modelo de datos de inscripción de curso de estudiante](intro/_static/data-model-diagram.png)

Hay una relación de uno a varios entre `Student` y `Enrollment` entidades, y hay una relación de uno a varios entre `Course` y `Enrollment` entidades. En otras palabras, se puede inscribir un estudiante en cualquier número de cursos y un curso puede tener cualquier número de alumnos inscritos en ella.

En las secciones siguientes creará una clase para cada una de estas entidades.

### <a name="the-student-entity"></a>La entidad Student

![Diagrama de la entidad de estudiante](intro/_static/student-entity.png)

En el *modelos* carpeta, cree un archivo de clase denominado *Student.cs* y reemplace el código de plantilla con el código siguiente.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

El `ID` propiedad se convertirá en la columna de clave principal de la tabla de base de datos que corresponde a esta clase. De forma predeterminada, Entity Framework interpreta una propiedad que se denomina `ID` o `classnameID` como clave principal.

El `Enrollments` es una propiedad de navegación. Propiedades de navegación contienen otras entidades que están relacionados con esta entidad. En este caso, el `Enrollments` propiedad de un `Student entity` contendrá todos los `Enrollment` entidades que están relacionados con dicho `Student` entidad. En otras palabras, si una fila determinada de estudiante en la base de datos tiene dos inscripción filas relacionadas (filas que contienen el valor de clave principal de ese estudiante en la columna de clave externa de StudentID), que `Student` la entidad `Enrollments` va a contenerlas propiedades de navegación dos `Enrollment` entidades.

Si una propiedad de navegación puede contener varias entidades (como en las relaciones de varios a varios o de uno a varios), su tipo debe ser una lista en la que las entradas pueden agregar, eliminar y actualizar, como `ICollection<T>`. Puede especificar `ICollection<T>` o un tipo como `List<T>` o `HashSet<T>`. Si especifica `ICollection<T>`, EF crea un `HashSet<T>` colección de forma predeterminada.

### <a name="the-enrollment-entity"></a>La entidad de inscripción

![Diagrama de la entidad de inscripción](intro/_static/enrollment-entity.png)

En el *modelos* carpeta, crear *Enrollment.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

El `EnrollmentID` propiedad será la clave principal; esta entidad se usa el `classnameID` de patrón en lugar de `ID` por sí solo como se vio en el `Student` entidad. Normalmente debería elegir un patrón y utilizarla en todo el modelo de datos. En este caso, la variación muestra que puede usar cualquier patrón. En un [tutorial posterior](inheritance.md), verá cómo usar identificador sin classname resulta más fácil implementar la herencia en el modelo de datos.

El `Grade` propiedad es una `enum`. El signo de interrogación después de la `Grade` declaración de tipo indica que el `Grade` propiedad es que aceptan valores NULL. Un grado que sea null es diferente de una puntuación de cero, null significa una puntuación no se conoce o no se ha asignado todavía.

El `StudentID` propiedad es una clave externa y la propiedad de navegación correspondiente es `Student`. Un `Enrollment` está asociada con una entidad `Student` entidad, por lo que la propiedad solo puede contener un único `Student` entidad (a diferencia de la `Student.Enrollments` propiedad de navegación que vio anteriormente, que puede contener varios `Enrollment` entidades).

El `CourseID` propiedad es una clave externa y la propiedad de navegación correspondiente es `Course`. Un `Enrollment` está asociada con una entidad `Course` entidad.

Entity Framework interpreta una propiedad como una propiedad de clave externa si se denomina `<navigation property name><primary key property name>` (por ejemplo, `StudentID` para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`). Propiedades de clave externa también se pueden denominar simplemente `<primary key property name>` (por ejemplo, `CourseID` desde el `Course` clave principal de la entidad es `CourseID`).

### <a name="the-course-entity"></a>La entidad de curso

![Diagrama de la entidad de curso](intro/_static/course-entity.png)

En el *modelos* carpeta, crear *Course.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

El `Enrollments` es una propiedad de navegación. A `Course` entidad se puede relacionar con cualquier número de `Enrollment` entidades.

Digamos más sobre la `DatabaseGenerated` de atributo en un [tutorial posterior](complex-data-model.md) de esta serie. Básicamente, este atributo le permite introducir la clave principal para el curso en lugar de tener la base de datos generarlo.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

La clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos especificada es la clase de contexto de base de datos. Esta clase se crea al derivar de la clase `Microsoft.EntityFrameworkCore.DbContext`. En el código se especifica qué entidades se incluyen en el modelo de datos. También puede personalizar cierto comportamiento de Entity Framework. En este proyecto, la clase se denomina `SchoolContext`.

En la carpeta del proyecto, cree una carpeta denominada *datos*.

En el *datos* carpeta crear un nuevo archivo de clase denominado *SchoolContext.cs*y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

Este código crea un `DbSet` propiedad para cada conjunto de entidades. En la terminología de Entity Framework, un conjunto de entidades suele corresponderse con una tabla de base de datos, mientras que una entidad lo hace con una fila de la tabla.

Se podría ha omitido el `DbSet<Enrollment>` y `DbSet<Course>` instrucciones y funcionará la misma. Entity Framework incluiría ellos implícitamente porque el `Student` las referencias de entidad del `Enrollment` entidad y la `Enrollment` las referencias de entidad la `Course` entidad.

Cuando se crea la base de datos, EF crea las tablas que tienen nombres de la misma que la `DbSet` nombres de propiedad. Nombres de propiedad para las colecciones son normalmente plural (estudiantes en lugar de estudiante), pero los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser pluralizan o no. Para estos tutoriales obtendrá invalidar el comportamiento predeterminado especificando los nombres de tabla singular en DbContext. Para ello, agregue el código resaltado siguiente después de la última propiedad DbSet.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>Registrar el contexto con la inserción de dependencias

Implementa ASP.NET Core [inyección de dependencia](../../fundamentals/dependency-injection.md) de forma predeterminada. Servicios (por ejemplo, el contexto de base de datos EF) se registran con inyección de dependencia durante el inicio de la aplicación. Los componentes que requieren estos servicios (por ejemplo, controladores MVC) se proporcionan estos servicios a través de los parámetros del constructor. Podrá ver el código del constructor de controlador que obtiene una instancia de contexto más adelante en este tutorial.

Para registrar `SchoolContext` como un servicio, abra *Startup.cs*y agregue las líneas resaltadas en la `ConfigureServices` método.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

El nombre de la cadena de conexión se pasa al contexto mediante una llamada a un método en un `DbContextOptionsBuilder` objeto. Para el desarrollo local, el [sistema de configuración de ASP.NET Core](xref:fundamentals/configuration/index) lee las cadenas de conexión desde el *appSettings.JSON que se* archivo.

Agregar `using` instrucciones para `ContosoUniversity.Data` y `Microsoft.EntityFrameworkCore` espacios de nombres y, a continuación, compile el proyecto.

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

Abra la *appSettings.JSON que se* de archivos y agregar una cadena de conexión tal y como se muestra en el ejemplo siguiente.

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

La cadena de conexión especifica una base de datos de SQL Server LocalDB. LocalDB es una versión ligera del motor de base de datos de SQL Server Express y está diseñada para el desarrollo de aplicaciones, no su uso en producción. LocalDB a petición se inicia y se ejecuta en modo de usuario, así que no hay ninguna configuración compleja. De forma predeterminada, crea LocalDB *.mdf* archivos en la base de datos la `C:/Users/<user>` directory.

## <a name="add-code-to-initialize-the-database-with-test-data"></a>Agregue código para inicializar la base de datos con datos de prueba

Entity Framework creará una base de datos vacía por usted. En esta sección, tienes que escribir un método que se llama una vez creada la base de datos para rellenarlo con datos de prueba.

Aquí usará el `EnsureCreated` método para crear automáticamente la base de datos. En un [tutorial posterior](migrations.md) verá cómo controlar los cambios en el modelo mediante migraciones de Code First para cambiar el esquema de base de datos en lugar de quitar y volver a crear la base de datos.

En el *datos* carpeta, cree un nuevo archivo de clase denominado *DbInitializer.cs* y reemplace el código de plantilla con el código siguiente, que hace que una base de datos se crean cuando es necesario y datos en el nuevo de pruebas de carga base de datos.

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

El código comprueba si hay cualquier estudiantes en la base de datos, y si no es así, se supone la base de datos es nuevo y debe ser propagado con datos de prueba. Carga los datos de prueba en las matrices en lugar de `List<T>` colecciones para optimizar el rendimiento.

En *Program.cs*, modifique la `Main` método para hacer lo siguiente al iniciar la aplicación:

* Obtener una instancia de contexto de base de datos desde el contenedor de inyección de dependencia.
* Llame al método de inicialización, pasándole el contexto.
* Elimine el contexto cuando se realiza el método de inicialización.

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

Agregar `using` instrucciones:

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

En los tutoriales anteriores, puede ver un código similar en el `Configure` método *Startup.cs*. Se recomienda que realice la `Configure` método sólo para configurar la canalización de solicitudes. Código de inicio de la aplicación pertenece a la `Main` método.

Ahora la primera vez que ejecute la aplicación, la base de datos se creará y propagado con datos de prueba. Cada vez que cambie el modelo de datos, puede eliminar la base de datos, actualizar el método de inicialización y comience de nuevo con una nueva base de datos del mismo modo. En los tutoriales posteriores, verá cómo modificar la base de datos cuando el modelo de datos los cambios, sin tener que eliminar y volver a crearla.

## <a name="create-a-controller-and-views"></a>Crear un controlador y vistas

A continuación, usará el motor de la técnica scaffolding en Visual Studio para agregar un controlador MVC y vistas que se va a utilizar EF para consultar y guardar los datos.

La creación automática de los métodos de acción CRUD y vistas se conoce como la técnica scaffolding. Scaffolding difiere de la generación de código en que el código con scaffolding es un punto de partida que se puede modificar para satisfacer sus propias necesidades, mientras que normalmente no modifique código generado. Cuando necesite personalizar código generado, utiliza clases parciales o regenerar el código cuando se produzcan cambios.

* Haga clic en el **controladores** carpeta **el Explorador de soluciones** y seleccione **Agregar > nuevo elemento de scaffolding**.

Si aparece el cuadro de diálogo **Agregar dependencias de MVC**:

* [Actualice Visual Studio a la última versión](https://www.visualstudio.com/downloads/). La versiones de Visual Studio anteriores a la 15.5 muestran este cuadro de diálogo.
* Si no puede actualizar, seleccione **AGREGAR** y luego siga los pasos para agregar el controlador de nuevo.

* En el **agregar scaffolding** cuadro de diálogo:

  * Seleccione **controlador de MVC con vistas, mediante Entity Framework**.

  * Haga clic en **Agregar**.

* En el **Agregar controlador** cuadro de diálogo:

  * En **clase modelo** seleccione **estudiante**.

  * En **clase de contexto de datos** seleccione **SchoolContext**.

  * Acepte el valor predeterminado **StudentsController** como el nombre.

  * Haga clic en **Agregar**.

  ![Scaffold Student](intro/_static/scaffold-student.png)

  Al hacer clic en **agregar**, el motor de scaffolding de Visual Studio crea un *StudentsController.cs* archivo y un conjunto de vistas (*.cshtml* archivos) que funcionan con el controlador.

(El motor de scaffolding también puede crear el contexto de base de datos automáticamente si no crea manualmente en primer lugar como lo hizo anteriormente en este tutorial. Puede especificar una nueva clase de contexto en el **Agregar controlador** cuadro haciendo clic en el signo más a la derecha del **clase de contexto de datos**.  Visual Studio, a continuación, creará el `DbContext` , así como las vistas y el controlador de clase.)

Observará que el controlador toma un `SchoolContext` como un parámetro de constructor.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

Inserción de dependencias ASP.NET se encargará de pasar una instancia de `SchoolContext` en el controlador. Ha configurado en el *Startup.cs* versiones anteriores de archivos.

El controlador contiene un `Index` método de acción, que muestra todos los alumnos en la base de datos. El método obtiene una lista de estudiantes de la entidad de los alumnos establecida leyendo el `Students` propiedad de la instancia de contexto de base de datos:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

Obtendrá información acerca de los elementos de programación asincrónicos en este código más adelante en el tutorial.

El *Views/Students/Index.cshtml* vista muestra esta lista en una tabla:

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

Presione CTRL + F5 para ejecutar el proyecto o elija **Depurar > Iniciar sin depurar** en el menú.

Haga clic en la pestaña de estudiantes para ver los datos de prueba que el `DbInitializer.Initialize` método insertado. Dependiendo de cómo estrecha la ventana del explorador es, verá el `Student` vínculo de ficha en la parte superior de la página o tendrá que hacer clic en el icono de navegación en la esquina superior derecha para ver el vínculo.

![Página de inicio de la Universidad de Contoso estrecho](intro/_static/home-page-narrow.png)

![Página de índice de estudiantes](intro/_static/students-index.png)

## <a name="view-the-database"></a>Vista de la base de datos

Cuando se inicia la aplicación, el `DbInitializer.Initialize` llamadas al método `EnsureCreated`. EF vio que se ha producido ninguna base de datos y por lo que crea uno, a continuación, el resto de la `Initialize` código del método rellena la base de datos con datos. Puede usar **Explorador de objetos de SQL Server** (SSOX) para ver la base de datos en Visual Studio.

Cierre el explorador.

Si la ventana SSOX no está abierta, seleccione la plantilla en el **vista** menú en Visual Studio.

En SSOX, haga clic en **(localdb) \MSSQLLocalDB > bases de datos**y, a continuación, haga clic en la entrada para el nombre de base de datos que se encuentra en la cadena de conexión en su *appSettings.JSON que se* archivo.

Expanda el **tablas** nodo para ver las tablas de la base de datos.

![Tablas de SSOX](intro/_static/ssox-tables.png)

Haga clic en el **Student** de tabla y haga clic en **ver datos** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

![Tabla de estudiantes en SSOX](intro/_static/ssox-student-table.png)

El *.mdf* y *.ldf* archivos de base de datos se encuentran en el *C:\Users\<suNombreDeUsuario >* carpeta.

Dado que se está llamando a `EnsureCreated` en el método de inicializador que se ejecuta en el inicio de aplicación, ahora se puede realizar un cambio en la `Student` clase, elimine la base de datos, ejecute de nuevo la aplicación y la base de datos sería volver a crear para que coincida con el cambio. Por ejemplo, si agrega un `EmailAddress` propiedad a la `Student` (clase), verá un nuevo `EmailAddress` columna en la tabla que se vuelven a crear.

## <a name="conventions"></a>Convenciones

La cantidad de código que tenía que escribir en orden para Entity Framework poder crear una base de datos completa para usted es mínima debido al uso de convenciones o suposiciones que hace de Entity Framework.

* Los nombres de `DbSet` propiedades se utilizan como nombres de tabla. Para las entidades que no hace referencia a un `DbSet` propiedad, clase de entidad nombres se utilizan como nombres de tabla.

* Nombres de propiedad de entidad se utilizan para los nombres de columna.

* Propiedades de la entidad que se denominan identificador o classnameID se reconocen como propiedades de clave principal.

* Una propiedad se interpreta como una propiedad de clave externa si se denomina  *<navigation property name> <primary key property name>*  (por ejemplo, `StudentID` para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`). Propiedades de clave externa también se pueden denominar simplemente  *<primary key property name>*  (por ejemplo, `EnrollmentID` desde el `Enrollment` clave principal de la entidad es `EnrollmentID`).

Puede reemplazarse un comportamiento convencional. Por ejemplo, puede especificar explícitamente los nombres de tabla, tal y como se vio anteriormente en este tutorial. Y puede establecer los nombres de columna y establecer cualquier propiedad como clave principal o clave externa, como verá en un [tutorial posterior](complex-data-model.md) de esta serie.

## <a name="asynchronous-code"></a>Código asincrónico

La programación asincrónica es el modo predeterminado de EF y de núcleo de ASP.NET.

Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de alta carga todos los subprocesos disponibles pueden estar en uso. Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que se liberan los subprocesos. Con código sincrónico, se pueden acumular muchos subprocesos la mientras que realmente no están realizando ningún trabajo porque está a la espera de e/s completar. Con código asincrónico, cuando un proceso está en espera de e/s completar, un subproceso se liberen para el servidor que se usará para procesando otras solicitudes. Como resultado, código asincrónico permite que los recursos de servidor que se usará de forma más eficaz, y el servidor está habilitado para administrar más tráfico sin retrasos.

Código asincrónico que introduce una pequeña cantidad de sobrecarga en tiempo de ejecución, pero para situaciones de poco tráfico la disminución del rendimiento es insignificante, mientras en situaciones de tráfico elevado, es importante la mejora del rendimiento potencial.

En el código siguiente, la `async` palabra clave, `Task<T>` devolver valor, `await` (palabra clave), y `ToListAsync` método que el código se ejecutará de forma asincrónica.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* El `async` palabra clave indica al compilador que se va a generar las devoluciones de llamada para las partes del cuerpo del método como crear automáticamente la `Task<IActionResult>` objeto que se devuelve.

* El tipo de valor devuelto `Task<IActionResult>` representa el trabajo en curso con un resultado de tipo `IActionResult`.

* El `await` (palabra clave) hace que el compilador dividir el método en dos partes. La primera parte se termina con la operación que se inicia de forma asincrónica. La segunda parte se coloca en un método de devolución de llamada que se llama cuando finaliza la operación.

* `ToListAsync`es la versión asincrónica de la `ToList` método de extensión.

Algunos aspectos que tener en cuenta cuando se escribe código asincrónico que utiliza Entity Framework:

* Solo las instrucciones que hacen que las consultas o comandos se envíen a la base de datos se ejecutan de forma asincrónica. Que incluye, por ejemplo, `ToListAsync`, `SingleOrDefaultAsync`, y `SaveChangesAsync`. No incluye, por ejemplo, las instrucciones que acaba de cambiar un `IQueryable`, como `var students = context.Students.Where(s => s.LastName == "Davolio")`.

* Un contexto EF no es seguro para subprocesos: no intentar llevar a cabo varias operaciones en paralelo. Cuando se llama a cualquier método EF asincrónico, use siempre el `await` palabra clave.

* Si desea aprovechar las ventajas de rendimiento de código asincrónico, asegúrese de que cualquier biblioteca de paquetes que está usando (por ejemplo de paginación), también usan asincronía si llama a los métodos de Entity Framework que hacer que las consultas se envían a la base de datos.

Para obtener más información sobre la programación asincrónica en. NET, vea [información general de Async](https://docs.microsoft.com/dotnet/articles/standard/async).

## <a name="summary"></a>Resumen

Ahora ha creado una aplicación simple que usa el núcleo de Entity Framework y SQL Server Express LocalDB para almacenar y mostrar los datos. En el siguiente tutorial, obtendrá información sobre cómo realizar básica CRUD (crear, leer, actualizar y eliminar) operaciones.

>[!div class="step-by-step"]
[Siguiente](crud.md)
