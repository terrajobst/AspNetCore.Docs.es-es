---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Introducción a Entity Framework 6 Code First con MVC 5 | Microsoft Docs
author: tdykstra
description: 'Hay disponible una versión más reciente de esta serie de tutoriales: Introducción a ASP.NET Core y Entity Framework Core con Visual Studio 2015. El Contoso Universi...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1b8d78954746cd6908f9ca9c2a51591f45fa01f7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403088"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Introducción a Entity Framework 6 Code First con MVC 5
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descargar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Hay disponible una versión más reciente de esta serie de tutoriales: [empezar a trabajar con ASP.NET Core y Entity Framework Core con Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con el Entity Framework 6 y Visual Studio 2013. Este tutorial usa el flujo de trabajo Code First. Para obtener información sobre cómo elegir entre Code First, Database First y Model First, consulte [flujos de trabajo de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores. Esta serie de tutoriales explica cómo crear la aplicación de ejemplo Contoso University. También puede [descargue la aplicación terminada](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Hay disponible una versión de Visual Basic traducida Mike Brind: [MVC 5 con EF 6 en Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) en el sitio Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (paquete de NuGet de EntityFramework 6.1.0)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (opcional)
>   
> 
> El tutorial también debería funcionar con [Visual Studio 2013 Express para Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) o Visual Studio 2012. El [versión VS 2012 del SDK de Windows Azure](https://go.microsoft.com/fwlink/p/?linkid=323511) es necesario para la implementación de Windows Azure con Visual Studio 2012.
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para versiones anteriores de este tutorial, vea [el EF 4.1 o e-book de MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) y [Introducción a EF 5 mediante MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework y LINQ al foro de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).
> 
> Si experimenta un problema que no se puede resolver, por lo general puede encontrar la solución al problema comparando el código para el proyecto completado que se puede descargar. Para que algunos errores comunes y cómo resolverlos, consulte [errores comunes así como las soluciones para ellos.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>La aplicación Web de Contoso University

La aplicación que se va a compilar en estos tutoriales es un sitio web sencillo de una universidad.

Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores. A continuación se muestran algunas de las pantallas que se van a crear.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Editar alumno](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

El estilo de la interfaz de usuario de este sitio se ha mantenido fiel a lo que generan las plantillas integradas, para que el tutorial se pueda centrar principalmente en cómo usar Entity Framework.

## <a name="prerequisites"></a>Requisitos previos

Consulte **versiones de Software** en la parte superior de la página. Entity Framework 6 no es un requisito previo, puesto que el paquete NuGet de EF se instala como parte del tutorial.

## <a name="create-an-mvc-web-application"></a>Crear una aplicación Web MVC

Abra Visual Studio y cree un nuevo proyecto de C# Web denominado "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **MVC** plantilla.

Si el **Host en la nube** casilla de verificación en la **Microsoft Azure** sección está activada, desactívela.

Haga clic en **Cambiar autenticación**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

En el **Cambiar autenticación** cuadro de diálogo, seleccione **sin autenticación**y, a continuación, haga clic en **Aceptar**. En este tutorial no requerir que los usuarios inicien sesión o restringir el acceso en función de quién ha iniciado sesión.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

En el cuadro de diálogo nuevo proyecto de ASP.NET, haga clic en **Aceptar** para crear el proyecto.

## <a name="set-up-the-site-style"></a>Configurar el estilo del sitio

Con algunos cambios sencillos se configura el menú del sitio, el diseño y la página principal.

Abra *Views\Shared\\_Layout.cshtml*y realice los cambios siguientes:

- Cambie todas las repeticiones de "My ASP.NET Application" y "Application name" a "Contoso University".
- Agregar entradas de menú para los estudiantes, cursos, instructores y departamentos y elimine la entrada de contacto.

Los cambios aparecen resaltados.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

En *Views\Home\Index.cshtml*, reemplace el contenido del archivo con el código siguiente para reemplazar el texto sobre ASP.NET y MVC con texto sobre esta aplicación:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Presione CTRL + F5 para ejecutar el sitio. Consulte la página principal con el menú principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Instalación de Entity Framework 6

Desde el **herramientas** menú haga clic en **Administrador de paquetes de NuGet** y, a continuación, haga clic en **Package Manager Console**.

En el **Package Manager Console** ventana escriba el siguiente comando:

`Install-Package EntityFramework`

![EF instalado](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

La imagen muestra 6.0.0 está instalado, pero NuGet instalará la versión más reciente de Entity Framework (excepto las versiones preliminares), que a partir de la actualización más reciente para el tutorial es 6.1.1.

Este paso es uno de unos pocos pasos que este tutorial tiene hacer manualmente, pero que podrían haberse realizado automáticamente con la característica de scaffolding de ASP.NET MVC. Está realizando manualmente para que puedan ver los pasos necesarios para usar Entity Framework. Más adelante usará scaffolding para crear el controlador de MVC y vistas. Una alternativa es dejar que la técnica scaffolding instale el paquete NuGet de EF, crear la clase de contexto de base de datos y crear la cadena de conexión automáticamente. Cuando esté listo para hacerlo así, lo único que debe hacer es omitir esos pasos y aplicar la técnica scaffolding el controlador de MVC después de crear las clases de entidad.

## <a name="create-the-data-model"></a>Crear el modelo de datos

A continuación podrá crear las clases de entidad para la aplicación Contoso University. Empezará con las tres entidades siguientes:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Hay una relación uno a varios entre las entidades `Student` y `Enrollment`, y también entre las entidades `Course` y `Enrollment`. En otras palabras, un estudiante se puede inscribir en cualquier número de cursos y un curso puede tener cualquier número de alumnos inscritos.

En las secciones siguientes creará una clase para cada una de estas entidades.

> [!NOTE]
> Si intenta compilar el proyecto antes de finalizar la creación de todas estas clases de entidad, obtendrá errores del compilador.


### <a name="the-student-entity"></a>La entidad Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

En el *modelos* carpeta, cree un archivo de clase denominado *Student.cs* y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

La propiedad `ID` se convertirá en la columna de clave principal de la tabla de base de datos que corresponde a esta clase. De forma predeterminada, Entity Framework interpreta una propiedad que se denomina `ID` o *classname* `ID` como clave principal.

El `Enrollments` propiedad es un *propiedad de navegación*. Las propiedades de navegación contienen otras entidades relacionadas con esta entidad. En este caso, el `Enrollments` propiedad de un `Student` entidad contendrá todas las `Enrollment` entidades que están relacionadas con esa `Student` entidad. En otras palabras, si un determinado `Student` fila en la base de datos tiene dos relacionados con `Enrollment` filas (valor de las filas que contienen la clave principal de ese estudiante en sus `StudentID` columna de clave externa), que `Student` la entidad `Enrollments` propiedad de navegación contendrá esas dos `Enrollment` entidades.

Las propiedades de navegación se definen normalmente como `virtual` por lo que puede sacar provecho de cierta funcionalidad de Entity Framework como *la carga diferida*. (La carga diferida se explicará más adelante, en la [leer datos relacionados](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie.)

Si una propiedad de navegación puede contener varias entidades (como en las relaciones de varios a varios o uno a varios), su tipo debe ser una lista a la que se puedan agregar las entradas, eliminarlas y actualizarlas, como `ICollection`.

### <a name="the-enrollment-entity"></a>La entidad Enrollment

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

En la carpeta *Models*, cree *Enrollment.cs* y reemplace el código existente con el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

El `EnrollmentID` propiedad será la clave principal; esta entidad se usa el *classname* `ID` de patrones en lugar de `ID` por sí mismo como se vio en el `Student` entidad. Normalmente debería elegir un patrón y usarlo en todo el modelo de datos. En este caso, la variación muestra que se puede usar cualquiera de los patrones. En un tutorial posterior, podrá ver cómo, al utilizar `ID` sin `classname` resulta más fácil implementar la herencia en el modelo de datos.

El `Grade` propiedad es una [enum](https://msdn.microsoft.com/data/hh859576.aspx). El signo de interrogación después de la `Grade` declaración de tipos que indica la `Grade` propiedad es [que acepta valores NULL](https://msdn.microsoft.com/library/2cf62fcy.aspx). Una calificación que sea null es diferente de una calificación de cero; null significa que una calificación no se conoce o no se ha asignado todavía.

La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`. Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad solo puede contener un única entidad `Student` (a diferencia de la propiedad de navegación `Student.Enrollments` que se vio anteriormente, que puede contener varias entidades `Enrollment`).

La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`. Una entidad `Enrollment` está asociada con una entidad `Course`.

Entity Framework interpreta una propiedad como propiedad de clave externa si se denomina *&lt;nombre de la propiedad de navegación&gt;&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `StudentID`para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`). Propiedades de clave externa también se puede denominar el mismo simplemente *&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `CourseID` puesto que la `Course` clave principal de la entidad es `CourseID`).

### <a name="the-course-entity"></a>La entidad Course

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

En el *modelos* carpeta, cree *Course.cs*, reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propiedad `Enrollments` es una propiedad de navegación. Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.

Se incluirá más información sobre la [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) atributo en un tutorial más adelante en esta serie. Básicamente, este atributo permite escribir la clave principal para el curso en lugar de hacer que la base de datos lo genere.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

La clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos determinado es el *contexto de base de datos* clase. Esta clase se crea mediante la derivación de la [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) clase. En el código se especifica qué entidades se incluyen en el modelo de datos. También se puede personalizar determinado comportamiento de Entity Framework. En este proyecto, la clase se denomina `SchoolContext`.

Para crear una carpeta en el proyecto ContosoUniversity, haga clic en el proyecto en **el Explorador de soluciones** y haga clic en **agregar**y, a continuación, haga clic en **nueva carpeta**. Nombre de la nueva carpeta *DAL* (para la capa de acceso a datos). En esa carpeta, cree un nuevo archivo de clase denominado *SchoolContext.cs*y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Especificar conjuntos de entidades

Este código crea un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) propiedad para cada conjunto de entidades. En la terminología de Entity Framework, un *conjunto de entidades* normalmente corresponde a una tabla de base de datos y un *entidad* corresponde a una fila de la tabla.

> [!NOTE] 
> 
> Se podría omitir la `DbSet<Enrollment>` y `DbSet<Course>` instrucciones y el funcionamiento sería el mismo. Entity Framework las incluiría implícitamente porque la entidad `Student` hace referencia a la entidad `Enrollment` y la entidad `Enrollment` hace referencia a la entidad `Course`.


### <a name="specifying-the-connection-string"></a>Especifica la cadena de conexión

El nombre de la cadena de conexión (que agregará más adelante en el archivo Web.config) se pasa al constructor.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

También podría pasar en la cadena de conexión en lugar del nombre de uno de los que se almacena en el archivo Web.config. Para obtener más información acerca de las opciones para especificar la base de datos para usar, consulte [Entity Framework: modelos y las conexiones](https://msdn.microsoft.com/data/jj592674).

Si no especifica explícitamente una cadena de conexión o el nombre de uno, Entity Framework se da por supuesto que el nombre de la cadena de conexión es el mismo que el nombre de clase. El nombre de cadena de conexión predeterminado en este ejemplo, a continuación, sería `SchoolContext`, igual que lo que debe especificar explícitamente.

### <a name="specifying-singular-table-names"></a>Especificar nombres de tabla en singular

El `modelBuilder.Conventions.Remove` instrucción en el [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método impide que se pluralizan los nombres de tabla. Si no lo hace, las tablas generadas en la base de datos se denominará `Students`, `Courses`, y `Enrollments`. En su lugar, los nombres de tabla serán `Student`, `Course`, y `Enrollment`. Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales o no. Este tutorial usa la forma singular, pero lo importante es que puede seleccionar cualquier formato que prefiera mediante la inclusión o si se omite esta línea de código.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Configuración de EF para inicializar la base de datos con datos de prueba

Entity Framework puede automáticamente crear (o quitar y volver a crear) una base de datos cuando se ejecuta la aplicación. Puede especificar que se debe realizar esto cada vez que se ejecuta la aplicación o solo cuando el modelo no está sincronizado con la base de datos existente. También puede escribir un `Seed` método que Entity Framework llama automáticamente después de crear la base de datos para rellenarla con datos de prueba.

El comportamiento predeterminado consiste en crear una base de datos solo si no existe (y producir una excepción si el modelo ha cambiado y ya existe la base de datos). En esta sección especificará que debe quitarse y volver a crear cada vez que cambia el modelo de la base de datos. Si se quita la base de datos, la pérdida de todos los datos. Esto es generalmente Aceptar durante el desarrollo, porque el `Seed` método se ejecutará cuando se vuelve a crear la base de datos y se volverá a crear los datos de prueba. Pero en producción que normalmente no desea perder todos los datos cada vez que necesite cambiar el esquema de base de datos. Más adelante verá cómo controlar los cambios de modelo mediante migraciones de Code First para cambiar el esquema de base de datos en lugar de quitar y volver a crear la base de datos.

En la carpeta de la capa DAL, cree un nuevo archivo de clase denominado *SchoolInitializer.cs* y reemplace el código de plantilla con el  
siguiendo el código, lo que hace que una base de datos se crea cuando sea necesario y carga los datos de prueba en la nueva base de datos.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

El `Seed` método toma el objeto de contexto de base de datos como un parámetro de entrada y el código en el método utiliza ese objeto para agregar nuevas entidades a la base de datos. Para cada tipo de entidad, el código crea una colección de nuevas entidades, los agrega a la correspondiente `DbSet` propiedad y, a continuación, guarda los cambios en la base de datos. No es necesario llamar a la `SaveChanges` método después de cada grupo de entidades, como se hizo aquí, pero hacerlo le ayuda a localizar el origen de un problema si se produce una excepción mientras se está escribiendo el código en la base de datos.

Para indicar a Entity Framework para utilizar su inicializador de clase, agregue un elemento a la `entityFramework` elemento en la aplicación *Web.config* archivo (uno en la carpeta raíz del proyecto), tal como se muestra en el ejemplo siguiente:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

El `context type` especifica el nombre de clase de contexto completo y el ensamblado esté, y el `databaseinitializer type` especifica el nombre completo de la clase de inicializador y el ensamblado se encuentra en. (Cuando no desee EF que use el inicializador, puede establecer un atributo en el `context` elemento: `disableDatabaseInitialization="true"`.) Para obtener más información, consulte [Entity Framework - archivo de configuración](https://msdn.microsoft.com/data/jj556606).

Como alternativa a establecer el inicializador de la *Web.config* archivo es hacerlo en código mediante la adición de un `Database.SetInitializer` instrucción a la `Application_Start` método en el *Global.asax.cs* archivo. Para obtener más información, consulte [descripción inicializadores de base de datos en Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

La aplicación ahora está establecida de seguridad para que cuando tiene acceso a la base de datos por primera vez en una ejecución determinada de la  
aplicación, Entity Framework compara el modelo de la base de datos (su `SchoolContext` y clases de entidad). Si hay una diferencia, la aplicación se quita y vuelve a crea la base de datos.

> [!NOTE]
> Al implementar una aplicación en un servidor web de producción, debe quitar o deshabilitar el código que se quita y vuelve a crea la base de datos. Se hará en un tutorial más adelante en esta serie.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Configuración de EF para usar una base de datos de SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) es una versión ligera del motor de base de datos de SQL Server Express. Es fácil de instalar y configurar, se inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo de ejecución especiales de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos. Puede colocar archivos de base de datos LocalDB en la *aplicación\_datos* carpeta de un proyecto web si desea poder copiar la base de datos con el proyecto. La característica de instancia de usuario en SQL Server Express también le permite trabajar con *.mdf* archivos, pero la característica de instancia de usuario está en desuso; por lo tanto, se recomienda LocalDB para trabajar con *.mdf* archivos. En Visual Studio 2012 y versiones posteriores, LocalDB está instalado de forma predeterminada con Visual Studio.

Normalmente, SQL Server Express no se utiliza para las aplicaciones web de producción. En particular LocalDB no se recomienda para su uso en producción con una aplicación web porque no está diseñado para trabajar con IIS.

En este tutorial, trabajará con LocalDB. Abra la aplicación *Web.config* archivo y agregue un `connectionStrings` elemento anterior del `appSettings` elemento, como se muestra en el ejemplo siguiente. (Asegúrese de actualizar el *Web.config* archivo en la carpeta raíz del proyecto. También hay un *Web.config* archivo se encuentra en la *vistas* subcarpeta que no es necesario actualizar.)

Si utiliza Visual Studio 2015, reemplace "v11.0" en la cadena de conexión por "MSSQLLocalDB", que ha cambiado el nombre de instancia de SQL Server de forma predeterminada.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Ha agregado la cadena de conexión especifica que Entity Framework usará una base de datos de LocalDB denominada *ContosoUniversity1.mdf*. (La base de datos que no exista; EF lo creará.) Si desea que la base de datos que se creen en su *aplicación\_datos* carpeta, puede agregar `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` a la cadena de conexión. Para obtener más información acerca de las cadenas de conexión, consulte [cadenas de conexión de SQL Server para aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Realmente no debe tener una cadena de conexión en el *Web.config* archivo. Si no se proporciona una cadena de conexión, Entity Framework usará el valor predeterminado es uno basado en la clase de contexto. Para obtener más información, consulte [Code First para una base de datos](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Creación de un controlador de estudiante y vistas

Ahora creará una página web para mostrar los datos y, automáticamente se desencadenará el proceso de solicitar los datos  
la creación de la base de datos. Comenzará creando un nuevo controlador. Pero antes de hacerlo, compile el proyecto para que las clases de modelo y contexto esté disponible para scaffolding de controlador MVC.

1. Haga clic en el **controladores** carpeta **el Explorador de soluciones**, seleccione **agregar**y, a continuación, haga clic en **nuevo elemento de scaffolding**.
2. En el **agregar Scaffold** cuadro de diálogo, seleccione **controlador MVC 5 con vistas, usando Entity Framework**.

     ![Agregar Scaffold](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. En el cuadro de diálogo Agregar controlador, realice las selecciones siguientes y, a continuación, haga clic en **agregar**:

   - Clase de modelo: **Student (ContosoUniversity.Models)**. (Si no ve esta opción en la lista desplegable, compile el proyecto e inténtelo de nuevo.)
   - Clase de contexto de datos: **SchoolContext (ContosoUniversity.DAL)**.
   - Nombre del controlador: **StudentController** (no StudentsController).
   - Deje los valores predeterminados para los demás campos.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Al hacer clic en **agregar**, el proveedor de scaffolding crea un archivo StudentController.cs y un conjunto de vistas (archivos .cshtml) que funcionan con el controlador. En el futuro al crear los proyectos que usan Entity Framework también se puede sacar provecho de algunas funciones adicionales del proveedor de scaffolding: simplemente cree la primera clase de modelo, no cree una cadena de conexión y, a continuación, en el **Agregar controlador** cuadro Especifique la nueva clase de contexto. El proveedor de scaffolding creará su `DbContext` clase y su cadena de conexión, así como el controlador y vistas.
4. Visual Studio abre el *Controllers\StudentController.cs* archivo. Verá que se ha creado una variable de clase que crea una instancia de un objeto de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     El `Index` método de acción obtiene una lista de estudiantes de la *estudiantes* entidad establece leyendo la `Students` propiedad de la instancia de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     El *Student\Index.cshtml* vista muestra esta lista en una tabla:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Presione CTRL+F5 para ejecutar el proyecto. (Si se produce un error "No se puede crear la instantánea", cierre el explorador y vuelva a intentarlo.)

     Haga clic en el **estudiantes** pestaña para ver los datos de prueba que el `Seed` método insertado. Según cómo estrecho es la ventana del explorador, verá el vínculo de la pestaña de estudiante en la barra de herramientas o tendrá que hacer clic en la esquina superior derecha para ver el vínculo.

     ![Botón de menú](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Página de índice de Student](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Ver la base de datos

Cuando se ejecutó la página Students y la aplicación intentó obtener acceso a la base de datos, EF comprobó que se ha producido ninguna base de datos y por lo que ha creado uno, a continuación, ejecutó el método de inicialización para rellenar la base de datos con datos.

Puede usar **Explorador de servidores** o **Explorador de objetos de SQL Server** (SSOX) para ver la base de datos en Visual Studio. En este tutorial usará **Explorador de servidores**. (En las ediciones Express de Visual Studio anteriores a 2013, **Explorador de servidores** se denomina **Database Explorer**.)

1. Cierre el explorador.
2. En **Explorador de servidores**, expanda **conexiones de datos**, expanda **contexto School (ContosoUniversity)** y, a continuación, expanda **tablas** para ver las tablas de la base de datos.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Haga clic en el **estudiante** de tabla y haga clic en **mostrar datos de tabla** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

    ![Tabla de estudiantes](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Cerrar la **Explorador de servidores** conexión.

El *ContosoUniversity1.mdf* y *.ldf* archivos de base de datos se encuentran en el `C:\Users\<yourusername>` carpeta.

Dado que usa el `DropCreateDatabaseIfModelChanges` inicializador, ahora podría realizar un cambio en el `Student` clase, ejecute de nuevo la aplicación y la base de datos sería volver a crearse para que coincidan con el cambio. Por ejemplo, si agrega un `EmailAddress` propiedad a la `Student` de clases, vuelva a ejecutar la página Students y, a continuación, busque en la tabla de nuevo, verá un nuevo `EmailAddress` columna.

## <a name="conventions"></a>Convenciones

La cantidad de código que tendría que escribir en el orden de Entity Framework poder crear una base de datos completa para usted es mínima debido al uso de *convenciones*, o las suposiciones que hace Entity Framework. Algunos de ellos ya se han anotado o se han utilizado sin que sea consciente de ellos:

- Los formularios pluralizados de nombres de clase de entidad se utilizan como nombres de tabla.
- Los nombres de propiedad de entidad se usan para los nombres de columna.
- Las propiedades de entidad se denominan `ID` o *classname* `ID` se reconocen como propiedades de clave principal.
- Una propiedad se interpreta como una propiedad de clave externa si se denomina *&lt;nombre de la propiedad de navegación&gt;&lt;nombre de propiedad de clave principal&gt;* (por ejemplo, `StudentID` para el `Student` propiedad de navegación desde el `Student` clave principal de la entidad es `ID`). Propiedades de clave externa también se puede denominar el mismo simplemente &lt;nombre de propiedad de clave principal&gt; (por ejemplo, `EnrollmentID` puesto que la `Enrollment` clave principal de la entidad es `EnrollmentID`).

Hemos visto que se pueden reemplazar las convenciones. Por ejemplo, especifica que los nombres de tabla no deben estar en plural y más adelante verá cómo marcar explícitamente una propiedad como propiedad de clave externa. Aprenderá más acerca de las convenciones y cómo reemplazar en el [crear un modelo de datos más complejos](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial más adelante en esta serie. Para obtener más información sobre las convenciones, vea [convenciones de Code First](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Resumen

Ahora ha creado una aplicación simple que usa Entity Framework y SQL Server Express LocalDB para almacenar y mostrar los datos. En el siguiente tutorial obtendrá información sobre cómo realizar básica CRUD (crear, leer, actualizar y eliminar) las operaciones.

Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Pueden encontrar vínculos a otros recursos de Entity Framework en [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Siguiente](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
