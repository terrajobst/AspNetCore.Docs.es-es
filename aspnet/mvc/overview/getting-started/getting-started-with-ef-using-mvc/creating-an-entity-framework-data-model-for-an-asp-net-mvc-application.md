---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 'Tutorial: Introducción a Entity Framework 6 Code First con MVC 5 | Microsoft Docs'
description: En esta serie de tutoriales, aprenderá a crear una aplicación de ASP.NET MVC 5 que usa Entity Framework 6 para el acceso a datos.
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889956"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>Tutorial: Introducción a Entity Framework 6 Code First con MVC 5

> [!NOTE]
> Se recomienda para nuevo desarrollo [páginas de Razor de ASP.NET Core](/aspnet/core/razor-pages) a través de controladores y vistas MVC de ASP.NET. Para una serie de tutoriales similar a ésta mediante las páginas de Razor, consulte [Tutorial: Introducción a las páginas de Razor en ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start). El nuevo tutorial:
> * Es más fácil de seguir.
> * Proporciona más procedimientos recomendados de EF Core.
> * Usa consultas más eficaces.
> * Es más actual en relación con la API más reciente.
> * Abarca más características.
> * Es el método preferido para el desarrollo de nuevas aplicaciones.

En esta serie de tutoriales, aprenderá a crear una aplicación de ASP.NET MVC 5 que usa Entity Framework 6 para el acceso a datos. Este tutorial usa el flujo de trabajo Code First. Para obtener información sobre cómo elegir entre Code First, Database First y Model First, consulte [crear un modelo](/ef/ef6/modeling/).

Esta serie de tutoriales explica cómo crear la aplicación de ejemplo Contoso University. La aplicación de ejemplo es un sitio Web de la Universidad simple. Con ella, puede ver y actualizar los estudiantes, cursos e información del instructor. Estos son dos de las pantallas que cree:

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Editar alumno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

En este tutorial ha:

> [!div class="checklist"]
> * Crear una aplicación web MVC
> * Configurar el estilo del sitio
> * Instalación de Entity Framework 6
> * Crear el modelo de datos
> * Crear el contexto de base de datos
> * Inicializar la base de datos con datos de prueba
> * Configuración de EF 6 para usar LocalDB
> * Crear el controlador y vistas
> * Vista de la base de datos

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>Crear una aplicación web MVC

1. Abra Visual Studio y cree un C# web proyecto mediante el **aplicación Web ASP.NET (.NET Framework)** plantilla. Denomine el proyecto *ContosoUniversity* y seleccione **Aceptar**.

   ![Cuadro de diálogo nuevo proyecto en Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. En **nueva aplicación Web de ASP.NET - ContosoUniversity**, seleccione **MVC**.

   ![Cuadro de diálogo nuevo de aplicación web en Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > De forma predeterminada, el **autenticación** opción está establecida en **sin autenticación**. Para este tutorial, la aplicación web no requiere los usuarios inicien sesión. Además, no restringir el acceso en función de quién haya iniciado sesión.

1. Seleccione **Aceptar** para crear el proyecto.

## <a name="set-up-the-site-style"></a>Configurar el estilo del sitio

Con algunos cambios sencillos se configura el menú del sitio, el diseño y la página principal.

1. Abra *Views\Shared\\_Layout.cshtml*y realice los cambios siguientes:

   - Cambie todas las repeticiones de "My ASP.NET Application" y "Application name" a "Contoso University".
   - Agregar entradas de menú para los estudiantes, cursos, instructores y departamentos y elimine la entrada de contacto.

   Se resaltan los cambios en el siguiente fragmento de código:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. En *Views\Home\Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. Presione Ctrl + F5 para ejecutar el sitio web. Consulte la página principal con el menú principal.

## <a name="install-entity-framework-6"></a>Instalación de Entity Framework 6

1. Desde el **herramientas** menú, elija **Administrador de paquetes de NuGet**y, a continuación, elija **Package Manager Console**.

2. En el **Package Manager Console** ventana, escriba el siguiente comando:

   ```text
   Install-Package EntityFramework
   ```

Este paso es uno de unos pocos pasos que este tutorial tiene hacer manualmente, pero que podrían haberse realizado automáticamente con la característica de scaffolding de ASP.NET MVC. Está realizando manualmente para que puedan ver los pasos necesarios para usar Entity Framework (EF). Más adelante usará scaffolding para crear el controlador de MVC y vistas. Una alternativa es dejar que la técnica scaffolding instale el paquete NuGet de EF, crear la clase de contexto de base de datos y crear la cadena de conexión automáticamente. Cuando esté listo para hacerlo así, lo único que debe hacer es omitir esos pasos y aplicar la técnica scaffolding el controlador de MVC después de crear las clases de entidad.

## <a name="create-the-data-model"></a>Crear el modelo de datos

A continuación podrá crear las clases de entidad para la aplicación Contoso University. Empezará con las tres entidades siguientes:

**Curso** <-> **inscripción** <-> **para estudiantes**

| Entidades | Relación |
| -------- | ------------ |
| Curso para la inscripción | Uno a varios |
| Estudiante a la inscripción | Uno a varios |

Hay una relación uno a varios entre las entidades `Student` y `Enrollment`, y también entre las entidades `Course` y `Enrollment`. En otras palabras, un estudiante se puede inscribir en cualquier número de cursos y un curso puede tener cualquier número de alumnos inscritos.

En las secciones siguientes, creará una clase para cada una de estas entidades.

> [!NOTE]
> Si intenta compilar el proyecto antes de finalizar la creación de todas estas clases de entidad, obtendrá errores del compilador.

### <a name="the-student-entity"></a>La entidad Student

- En el *modelos* carpeta, cree un archivo de clase denominado *Student.cs* con el botón secundario en la carpeta **el Explorador de soluciones** y eligiendo **agregar**  >  **Clase**. Reemplace el código de plantilla por el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La propiedad `ID` se convertirá en la columna de clave principal de la tabla de base de datos que corresponde a esta clase. De forma predeterminada, Entity Framework interpreta una propiedad que se denomina `ID` o *classname* `ID` como clave principal.

La propiedad `Enrollments` es una *propiedad de navegación*. Las propiedades de navegación contienen otras entidades relacionadas con esta entidad. En este caso, el `Enrollments` propiedad de un `Student` entidad contendrá todas las `Enrollment` entidades que están relacionadas con esa `Student` entidad. En otras palabras, si un determinado `Student` fila en la base de datos tiene dos relacionados con `Enrollment` filas (valor de las filas que contienen la clave principal de ese estudiante en sus `StudentID` columna de clave externa), que `Student` la entidad `Enrollments` propiedad de navegación contendrá esas dos `Enrollment` entidades.

Las propiedades de navegación se definen normalmente como `virtual` por lo que puede sacar provecho de cierta funcionalidad de Entity Framework como *la carga diferida*. (La carga diferida se explicará más adelante, en la [leer datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie.)

Si una propiedad de navegación puede contener varias entidades (como en las relaciones de varios a varios o uno a varios), su tipo debe ser una lista a la que se puedan agregar las entradas, eliminarlas y actualizarlas, como `ICollection`.

### <a name="the-enrollment-entity"></a>La entidad Enrollment

- En la carpeta *Models*, cree *Enrollment.cs* y reemplace el código existente con el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

El `EnrollmentID` propiedad será la clave principal; esta entidad se usa el *classname* `ID` de patrones en lugar de `ID` por sí mismo como se vio en el `Student` entidad. Normalmente debería elegir un patrón y usarlo en todo el modelo de datos. En este caso, la variación muestra que se puede usar cualquiera de los patrones. En un tutorial posterior, podrá ver cómo, al utilizar `ID` sin `classname` resulta más fácil implementar la herencia en el modelo de datos.

El `Grade` propiedad es una [enum](/ef/ef6/modeling/code-first/data-types/enums). El signo de interrogación después de la `Grade` declaración de tipos que indica la `Grade` propiedad es [que acepta valores NULL](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types). Una calificación que sea null es diferente de una calificación de cero; null significa que una calificación no se conoce o no se ha asignado todavía.

La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`. Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad solo puede contener un única entidad `Student` (a diferencia de la propiedad de navegación `Student.Enrollments` que se vio anteriormente, que puede contener varias entidades `Enrollment`).

La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`. Una entidad `Enrollment` está asociada con una entidad `Course`.

Entity Framework interpreta una propiedad como propiedad de clave externa si se denomina *&lt;nombre de la propiedad de navegación&gt;&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `StudentID`para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`). Propiedades de clave externa también se puede denominar el mismo simplemente *&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `CourseID` puesto que la `Course` clave principal de la entidad es `CourseID`).

### <a name="the-course-entity"></a>La entidad Course

- En el *modelos* carpeta, cree *Course.cs*, reemplace el código de plantilla con el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propiedad `Enrollments` es una propiedad de navegación. Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.

Se incluirá más información sobre la <xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute> atributo en un tutorial más adelante en esta serie. Básicamente, este atributo permite escribir la clave principal para el curso en lugar de hacer que la base de datos lo genere.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

La clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado es el *contexto de base de datos* clase. Esta clase se crea mediante la derivación de la [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx) clase. En el código, especifique las entidades que se incluyen en el modelo de datos. También se puede personalizar determinado comportamiento de Entity Framework. En este proyecto, la clase se denomina `SchoolContext`.

- Para crear una carpeta en el proyecto ContosoUniversity, haga clic en el proyecto en **el Explorador de soluciones** y haga clic en **agregar**y, a continuación, haga clic en **nueva carpeta**. Nombre de la nueva carpeta *DAL* (para la capa de acceso a datos). En esa carpeta, cree un nuevo archivo de clase denominado *SchoolContext.cs*y reemplace el código de plantilla con el código siguiente:

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>Especificar conjuntos de entidades

Este código crea un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx) propiedad para cada conjunto de entidades. En la terminología de Entity Framework, un *conjunto de entidades* normalmente corresponde a una tabla de base de datos y un *entidad* corresponde a una fila de la tabla.

> [!NOTE]
>
> Puede omitir el `DbSet<Enrollment>` y `DbSet<Course>` instrucciones y el funcionamiento sería el mismo. Entity Framework las incluiría implícitamente porque la `Student` las referencias de entidad la `Enrollment` entidad y el `Enrollment` las referencias de entidad la `Course` entidad.

### <a name="specify-the-connection-string"></a>Especifique la cadena de conexión

El nombre de la cadena de conexión (que agregará más adelante en el archivo Web.config) se pasa al constructor.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

También podría pasar en la cadena de conexión en lugar del nombre de uno de los que se almacena en el archivo Web.config. Para obtener más información acerca de las opciones para especificar la base de datos para usar, consulte [modelos y las cadenas de conexión](/ef/ef6/fundamentals/configuring/connection-strings).

Si no especifica explícitamente una cadena de conexión o el nombre de uno, Entity Framework se da por supuesto que el nombre de la cadena de conexión es el mismo que el nombre de clase. El nombre de cadena de conexión predeterminado en este ejemplo, a continuación, sería `SchoolContext`, igual que lo que debe especificar explícitamente.

### <a name="specify-singular-table-names"></a>Especifique los nombres de tabla en singular

El `modelBuilder.Conventions.Remove` instrucción en el [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método impide que se pluralizan los nombres de tabla. Si no lo hace, las tablas generadas en la base de datos se denominará `Students`, `Courses`, y `Enrollments`. En su lugar, los nombres de tabla serán `Student`, `Course`, y `Enrollment`. Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales o no. Este tutorial usa la forma singular, pero lo importante es que puede seleccionar cualquier formato que prefiera mediante la inclusión o si se omite esta línea de código.

## <a name="initialize-db-with-test-data"></a>Inicializar la base de datos con datos de prueba

Entity Framework puede automáticamente (o quitar y volver a crear) una base de datos cuando se ejecuta la aplicación. Puede especificar que se debe realizar esto cada vez que se ejecuta la aplicación o solo cuando el modelo no está sincronizado con la base de datos existente. También puede escribir un `Seed` que Entity Framework método automáticamente se llama después de crear la base de datos para rellenarla con datos de prueba.

El comportamiento predeterminado consiste en crear una base de datos solo si no existe (y producir una excepción si el modelo ha cambiado y ya existe la base de datos). En esta sección, especificará que debe quitarse y volver a crear cada vez que cambia el modelo de la base de datos. Si se quita la base de datos, la pérdida de todos los datos. Esto suele ser bien durante el desarrollo, porque el `Seed` método se ejecutará cuando se vuelve a crear la base de datos y se volverá a crear los datos de prueba. Pero en producción que normalmente no desea perder todos los datos cada vez que necesite cambiar el esquema de base de datos. Más adelante verá cómo controlar los cambios de modelo mediante migraciones de Code First para cambiar el esquema de base de datos en lugar de quitar y volver a crear la base de datos.

1. En la carpeta de la capa DAL, cree un nuevo archivo de clase denominado *SchoolInitializer.cs* y reemplace el código de plantilla con el código siguiente, lo que hace que una base de datos se crea si es necesario y carga los datos de prueba en la nueva base de datos.

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   El `Seed` método toma el objeto de contexto de base de datos como un parámetro de entrada y el código en el método utiliza ese objeto para agregar nuevas entidades a la base de datos. Para cada tipo de entidad, el código crea una colección de nuevas entidades, los agrega a la correspondiente `DbSet` propiedad y, a continuación, guarda los cambios en la base de datos. No es necesario llamar a la `SaveChanges` método después de cada grupo de entidades, como se hizo aquí, pero hacerlo le ayuda a localizar el origen de un problema si se produce una excepción mientras se está escribiendo el código en la base de datos.

2. Para indicar a Entity Framework para utilizar su inicializador de clase, agregue un elemento a la `entityFramework` elemento en la aplicación *Web.config* archivo (uno en la carpeta raíz del proyecto), tal como se muestra en el ejemplo siguiente:

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   El `context type` especifica el nombre de clase de contexto completo y el ensamblado esté, y el `databaseinitializer type` especifica el nombre completo de la clase de inicializador y el ensamblado se encuentra en. (Cuando no desee EF que use el inicializador, puede establecer un atributo en el `context` elemento: `disableDatabaseInitialization="true"`.) Para obtener más información, consulte [archivo de configuración](/ef/ef6/fundamentals/configuring/config-file).

   Una alternativa a establecer el inicializador de la *Web.config* archivo es hacerlo en código mediante la adición de un `Database.SetInitializer` instrucción a la `Application_Start` método en el *Global.asax.cs* archivo. Para obtener más información, consulte [descripción inicializadores de base de datos en Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

La aplicación ahora está configurada para que cuando tiene acceso a la base de datos por primera vez en una ejecución determinada de la aplicación, Entity Framework compara el modelo de la base de datos (su `SchoolContext` y clases de entidad). Si hay una diferencia, la aplicación se quita y vuelve a crea la base de datos.

> [!NOTE]
> Al implementar una aplicación en un servidor web de producción, debe quitar o deshabilitar el código que se quita y vuelve a crea la base de datos. Se hará en un tutorial más adelante en esta serie.

## <a name="set-up-ef-6-to-use-localdb"></a>Configuración de EF 6 para usar LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) es una versión ligera del motor de base de datos de SQL Server Express. Es fácil de instalar y configurar, se inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo de ejecución especiales de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos. Puede colocar archivos de base de datos LocalDB en la *aplicación\_datos* carpeta de un proyecto web si desea poder copiar la base de datos con el proyecto. La característica de instancia de usuario en SQL Server Express también le permite trabajar con *.mdf* archivos, pero la característica de instancia de usuario está en desuso; por lo tanto, se recomienda LocalDB para trabajar con *.mdf* archivos. LocalDB se instala de forma predeterminada con Visual Studio.

Normalmente, SQL Server Express no se utiliza para las aplicaciones web de producción. En particular LocalDB no se recomienda para su uso en producción con una aplicación web porque no se ha diseñado para trabajar con IIS.

- En este tutorial, trabajará con LocalDB. Abra la aplicación *Web.config* archivo y agregue un `connectionStrings` elemento anterior del `appSettings` elemento, como se muestra en el ejemplo siguiente. (Asegúrese de actualizar el *Web.config* archivo en la carpeta raíz del proyecto. También hay un *Web.config* de archivos en el *vistas* subcarpeta que no es necesario actualizar.)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Ha agregado la cadena de conexión especifica que Entity Framework usará una base de datos de LocalDB denominada *ContosoUniversity1.mdf*. (La base de datos que no exista, pero EF lo creará). Si desea crear la base de datos en su *aplicación\_datos* carpeta, puede agregar `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` a la cadena de conexión. Para obtener más información acerca de las cadenas de conexión, consulte [cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](/previous-versions/aspnet/jj653752(v=vs.110)).

Realmente no necesita una cadena de conexión en el *Web.config* archivo. Si no se proporciona una cadena de conexión, Entity Framework usa una cadena de conexión predeterminado en función de la clase de contexto. Para obtener más información, consulte [Code First para una base de datos](/ef/ef6/modeling/code-first/workflows/new-database).

## <a name="create-controller-and-views"></a>Crear el controlador y vistas

Ahora creará una página web para mostrar los datos. El proceso de solicitar los datos automáticamente, desencadena la creación de la base de datos. Comenzará creando un nuevo controlador. Pero antes de hacerlo, compile el proyecto para que las clases de modelo y contexto esté disponible para scaffolding de controlador MVC.

1. Haga clic en el **controladores** carpeta **el Explorador de soluciones**, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento de scaffolding**.
2. En el **agregar Scaffold** cuadro de diálogo, seleccione **controlador MVC 5 con vistas, usando Entity Framework**y, a continuación, elija **agregar**.

     ![Agregar cuadro de diálogo Scaffold en Visual Studio](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. En el **Agregar controlador** cuadro de diálogo, realice las selecciones siguientes y, a continuación, elija **agregar**:

   - Clase de modelo: **Estudiante (ContosoUniversity.Models)**. (Si no ve esta opción en la lista desplegable, compile el proyecto e inténtelo de nuevo.)
   - Clase de contexto de datos: **SchoolContext (ContosoUniversity.DAL)**.
   - Nombre del controlador: **StudentController** (no StudentsController).
   - Deje los valores predeterminados para los demás campos.

     Al hacer clic en **agregar**, el proveedor de scaffolding crea un *StudentController.cs* archivo y un conjunto de vistas (*.cshtml* archivos) que funcionan con el controlador. En el futuro al crear los proyectos que usan Entity Framework, también puede beneficiarse de algunas funciones adicionales del proveedor de scaffolding: crear la primera clase de modelo, no cree una cadena de conexión y, a continuación, en el **Agregar controlador** cuadro Especifique **nuevo contexto de datos** seleccionando el **+** situado junto a **clase de contexto de datos**. El proveedor de scaffolding creará su `DbContext` clase y su cadena de conexión, así como el controlador y vistas.
4. Visual Studio abre el *Controllers\StudentController.cs* archivo. Verá que se ha creado una variable de clase que crea una instancia de un objeto de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     El `Index` método de acción obtiene una lista de estudiantes de la *estudiantes* entidad establece leyendo la `Students` propiedad de la instancia de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     El *Student\Index.cshtml* vista muestra esta lista en una tabla:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Presione Ctrl + F5 para ejecutar el proyecto. (Si se produce un error "No se puede crear la instantánea", cierre el explorador y vuelva a intentarlo.)

     Haga clic en el **estudiantes** pestaña para ver los datos de prueba que el `Seed` método insertado. Según cómo estrecho es la ventana del explorador, verá el vínculo de la pestaña de estudiante en la barra de herramientas o tendrá que hacer clic en la esquina superior derecha para ver el vínculo.

     ![Botón de menú](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>Vista de la base de datos

Cuando se ejecutó la página Students y la aplicación intentó obtener acceso a la base de datos, EF detecta que hay no era ninguna base de datos y crear uno. EF, a continuación, ejecutó el método de inicialización para rellenar la base de datos con datos.

Puede usar **Explorador de servidores** o **Explorador de objetos de SQL Server** (SSOX) para ver la base de datos en Visual Studio. Para este tutorial, va a usar **Explorador de servidores**.

1. Cierre el explorador.
2. En **Explorador de servidores**, expanda **conexiones de datos** (es posible que deba seleccionar el botón Actualizar en primer lugar), expanda **contexto School (ContosoUniversity)** y, a continuación, expanda  **Las tablas** para ver las tablas de la base de datos.

3. Haga clic en el **estudiante** de tabla y haga clic en **mostrar datos de tabla** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

4. Cerrar la **Explorador de servidores** conexión.

El *ContosoUniversity1.mdf* y *.ldf* archivos de base de datos se encuentran en el *% USERPROFILE %* carpeta.

Dado que usa el `DropCreateDatabaseIfModelChanges` inicializador, ahora podría realizar un cambio en el `Student` clase, ejecute de nuevo la aplicación y la base de datos sería volver a crearse para que coincidan con el cambio. Por ejemplo, si agrega un `EmailAddress` propiedad a la `Student` de clases, vuelva a ejecutar la página Students y, a continuación, busque en la tabla de nuevo, verá una nueva `EmailAddress` columna.

## <a name="conventions"></a>Convenciones

La cantidad de código que tendría que escribir en el orden de Entity Framework poder crear una base de datos completa para usted es mínima debido *convenciones*, o las suposiciones que hace Entity Framework. Algunos de ellos ya se han anotado o se han utilizado sin que sea consciente de ellos:

- Los formularios pluralizados de nombres de clase de entidad se utilizan como nombres de tabla.
- Los nombres de propiedad de entidad se usan para los nombres de columna.
- Las propiedades de entidad se denominan `ID` o *classname* `ID` se reconocen como propiedades de clave principal.
- Una propiedad se interpreta como una propiedad de clave externa si se denomina *&lt;nombre de la propiedad de navegación&gt;&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `StudentID` para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`). Propiedades de clave externa también se puede denominar el mismo simplemente &lt;nombre de propiedad de clave principal&gt; (por ejemplo, `EnrollmentID` puesto que la `Enrollment` clave principal de la entidad es `EnrollmentID`).

Hemos visto que se pueden reemplazar las convenciones. Por ejemplo, especifica que los nombres de tabla no deben estar en plural y más adelante verá cómo marcar explícitamente una propiedad como propiedad de clave externa.

## <a name="get-the-code"></a>Obtención del código

[Descargue el proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Para obtener más información sobre EF 6, consulte estos artículos:

* [Acceso a los datos de ASP.NET: Recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md)

* [Convenciones de Code First](/ef/ef6/modeling/code-first/conventions/built-in)

* [Crear un modelo de datos más complejo](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha:

> [!div class="checklist"]
> * Crea una aplicación web MVC
> * Configurar el estilo del sitio
> * Instalado Entity Framework 6
> * Crear el modelo de datos
> * Crea el contexto de base de datos
> * Base de datos inicializado con los datos de prueba
> * Configuración de EF 6 para usar LocalDB
> * Las vistas y el controlador creado
> * Ver la base de datos

Avance al siguiente artículo para obtener información sobre cómo revisar y personalizar el crear, leer, actualizar, eliminar (CRUD) de código en los controladores y vistas.
> [!div class="nextstepaction"]
> [Implementar la funcionalidad CRUD básica](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)