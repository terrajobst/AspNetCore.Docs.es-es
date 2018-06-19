---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC (1 de 10) | Documentos de Microsoft
author: tdykstra
description: Una versión más reciente de esta serie de tutoriales está disponible, para Visual Studio 2013, Entity Framework 6 y 5 de MVC. El Alemania de aplicación web de ejemplo Contoso universidad...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a963f26b408f2a54bd9cd3e852bc1e368f86c41f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877936"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Crear un modelo de datos de Entity Framework para una aplicación de ASP.NET MVC (1 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A [versión más reciente de esta serie de tutoriales](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) está disponible para Visual Studio 2013, Entity Framework 6 y 5 de MVC.
> 
> 
> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con el Entity Framework 5 y Visual Studio 2012. La aplicación de ejemplo es un sitio web de una universidad ficticia, Contoso University. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores. Esta serie de tutoriales explica cómo compilar la aplicación de ejemplo de la Universidad de Contoso. También puede [descargar la aplicación completa](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Hay tres formas de trabajar con datos en Entity Framework: *Database First*, *Model First*, y *Code First*. Este tutorial está destinado Code First. Para obtener información sobre las diferencias entre estos flujos de trabajo e instrucciones sobre cómo elegir la mejor para su escenario, consulte [flujos de trabajo de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> La aplicación de ejemplo se basa en [ASP.NET MVC](../../../index.md). Si prefiere trabajar con el modelo de formularios Web Forms de ASP.NET, consulte el [enlace de modelos y formularios Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) serie de tutoriales y [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express para Web. Esto instala automáticamente el SDK de Windows Azure si aún no tiene VS 2012 o VS 2012 Express for Web. Visual Studio 2013 deberían funcionar, pero el tutorial no se ha probado con él y algunas selecciones de menú y los cuadros de diálogo son diferentes. El [versión de VS 2013 de Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) es necesario para la implementación de Windows Azure. |
> | .NET 4.5 | La mayoría de las características mostradas funcionará en .NET 4, pero algunas no. Por ejemplo, la compatibilidad de enumeraciones en EF requiere .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Si omite los pasos de implementación de Windows Azure, no necesita el SDK. Cuando se lanza una nueva versión del SDK, el vínculo instalará la versión más reciente. En ese caso, tendrá que adaptar algunos de las instrucciones para la nueva interfaz de usuario y funciones. |
> 
> ## <a name="questions"></a>Preguntas
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework y LINQ to foro de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Agradecimientos
> 
> Vea el último tutorial de la serie para [confirmaciones y una nota sobre VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Versión original del tutorial
> 
> La versión original del tutorial está disponible en la [4.1 EF / libro electrónico MVC 3](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>La aplicación Web de la Universidad de Contoso

La aplicación que se va a compilar en estos tutoriales es un sitio web sencillo de una universidad.

Los usuarios pueden ver y actualizar la información de estudiantes, cursos e instructores. A continuación se muestran algunas de las pantallas que se van a crear.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

El estilo de la interfaz de usuario de este sitio se ha mantenido fiel a lo que generan las plantillas integradas, para que el tutorial se pueda centrar principalmente en cómo usar Entity Framework.

## <a name="prerequisites"></a>Requisitos previos

Las direcciones y capturas de pantalla en este tutorial se suponen que está usando [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) o [Visual Studio 2012 Express para Web](https://go.microsoft.com/fwlink/?LinkID=275131), con las últimas actualizaciones y Azure SDK para .NET instalado a partir de julio, 2013. Puede obtener todo esto con el siguiente vínculo:

[Azure SDK para .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Si tiene instalado Visual Studio, el vínculo anterior instalará los componentes que faltan. Si no tiene Visual Studio, el vínculo instalará Visual Studio 2012 Express para Web. Puede usar Visual Studio 2013, pero algunos de los procedimientos requeridos y pantallas serán diferentes.

## <a name="create-an-mvc-web-application"></a>Crear una aplicación Web MVC

Abra Visual Studio y cree un nuevo proyecto de C# denominado "ContosoUniversity" utilizando el **aplicación Web de ASP.NET MVC 4** plantilla. Asegúrese de que el destino **.NET Framework 4.5** (que vamos a usar [ `enum` propiedades](https://msdn.microsoft.com/data/hh859576.aspx), y que requiere .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla.

Deje el **Razor** ver motor seleccionado y dejar el **crear un proyecto de prueba unitaria** casilla está desactivada.

Haga clic en **Aceptar**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Configurar el estilo de sitio

Con algunos cambios sencillos se configura el menú del sitio, el diseño y la página principal.

Abra *Views\Shared\\_Layout.cshtml*y reemplace el contenido del archivo con el código siguiente. Los cambios aparecen resaltados.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Este código realiza los cambios siguientes:

- Reemplaza las instancias de la plantilla de "Mi aplicación de MVC de ASP.NET" y "su logotipo aquí" por "Contoso Universidad".
- Agrega varios vínculos de acción que se utilizará más adelante en el tutorial.

En *Views\Home\Index.cshtml*, reemplace el contenido del archivo con el código siguiente para eliminar los párrafos plantilla acerca de ASP.NET y MVC:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

En *controllers\homecontroller*, cambie el valor de `ViewBag.Message` en el `Index` método de acción para "Bienvenido a la Universidad de Contoso", tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Presione CTRL + F5 para ejecutar el sitio. Vea la página principal con el menú principal.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Crear el modelo de datos

A continuación podrá crear las clases de entidad para la aplicación Contoso University. Se empieza por las siguientes tres entidades:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Hay una relación uno a varios entre las entidades `Student` y `Enrollment`, y también entre las entidades `Course` y `Enrollment`. En otras palabras, un estudiante se puede inscribir en cualquier número de cursos y un curso puede tener cualquier número de alumnos inscritos.

En las secciones siguientes creará una clase para cada una de estas entidades.

> [!NOTE]
> Si intenta compilar el proyecto antes de que termine de crear todas las clases de entidad, obtendrá errores del compilador.


### <a name="the-student-entity"></a>La entidad Student

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

En el *modelos* carpeta, crear *Student.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

La propiedad `StudentID` se convertirá en la columna de clave principal de la tabla de base de datos que corresponde a esta clase. De forma predeterminada, Entity Framework interpreta una propiedad que se denomina `ID` o *classname* `ID` como clave principal.

El `Enrollments` propiedad es una *propiedad de navegación*. Las propiedades de navegación contienen otras entidades relacionadas con esta entidad. En este caso, el `Enrollments` propiedad de un `Student` entidad contendrá todos los `Enrollment` entidades que están relacionados con dicho `Student` entidad. En otras palabras, si una determinada `Student` fila en la base de datos tiene dos relacionados con `Enrollment` filas (filas que contienen la clave principal de ese estudiante valor en sus `StudentID` columna de clave externa), que `Student` la entidad `Enrollments` propiedad de navegación contendrá esos dos `Enrollment` entidades.

Propiedades de navegación se definen normalmente como `virtual` por lo que puede sacar provecho de ciertas funciones de Entity Framework como *carga diferida*. (La carga diferida se explicará más adelante, en la [datos relacionados de lectura](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial más adelante en esta serie.

Si una propiedad de navegación puede contener varias entidades (como en las relaciones de varios a varios o uno a varios), su tipo debe ser una lista a la que se puedan agregar las entradas, eliminarlas y actualizarlas, como `ICollection`.

### <a name="the-enrollment-entity"></a>La entidad de inscripción

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

En la carpeta *Models*, cree *Enrollment.cs* y reemplace el código existente con el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

La propiedad grado es un [enum](https://msdn.microsoft.com/data/hh859576.aspx). El signo de interrogación después de la `Grade` declaración de tipo indica que la `Grade` propiedad es [que aceptan valores NULL](https://msdn.microsoft.com/library/2cf62fcy.aspx). Un grado que sea null es diferente de una puntuación de cero: null significa que una puntuación no se conoce o no se ha asignado todavía.

La propiedad `StudentID` es una clave externa y la propiedad de navegación correspondiente es `Student`. Una entidad `Enrollment` está asociada con una entidad `Student`, por lo que la propiedad solo puede contener un única entidad `Student` (a diferencia de la propiedad de navegación `Student.Enrollments` que se vio anteriormente, que puede contener varias entidades `Enrollment`).

La propiedad `CourseID` es una clave externa y la propiedad de navegación correspondiente es `Course`. Una entidad `Enrollment` está asociada con una entidad `Course`.

### <a name="the-course-entity"></a>La entidad de curso

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

En el *modelos* carpeta, crear *Course.cs*, sustituyendo el código existente por el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

La propiedad `Enrollments` es una propiedad de navegación. Una entidad `Course` puede estar relacionada con cualquier número de entidades `Enrollment`.

Digamos más sobre los [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Ninguno)] el atributo en el tutorial siguiente. Básicamente, este atributo permite escribir la clave principal para el curso en lugar de hacer que la base de datos lo genere.

## <a name="create-the-database-context"></a>Crear el contexto de base de datos

La clase principal que coordina la funcionalidad de Entity Framework para un modelo de datos concreto es el *contexto de base de datos* clase. Crear esta clase derivando de la [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) clase. En el código se especifica qué entidades se incluyen en el modelo de datos. También se puede personalizar determinado comportamiento de Entity Framework. En este proyecto, la clase se denomina `SchoolContext`.

Cree una carpeta denominada *DAL* (para la capa de acceso a datos). En esa carpeta, cree un nuevo archivo de clase denominado *SchoolContext.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Este código crea un [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) propiedad para cada conjunto de entidades. En la terminología de Entity Framework, un *conjunto de entidades* normalmente corresponde a una tabla de base de datos y un *entidad* corresponde a una fila de la tabla.

El `modelBuilder.Conventions.Remove` instrucción en el [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) método impide que los nombres de tabla se pluralizan. Si no hace esto, las tablas generadas se denominará `Students`, `Courses`, y `Enrollments`. En su lugar, los nombres de tabla serán `Student`, `Course`, y `Enrollment`. Los desarrolladores están en desacuerdo sobre si los nombres de tabla deben ser plurales o no. Este tutorial usa la forma singular, pero lo importante es que puede seleccionar cualquier formato que prefiera mediante la inclusión o la omisión de esta línea de código.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) es una versión ligera de SQL Server Express Database Engine que se inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo especial de ejecución de SQL Server Express que le permite trabajar con bases de datos como *.mdf* archivos. Normalmente, los archivos de base de datos de LocalDB se mantienen en la *aplicación\_datos* carpeta de un proyecto web. La característica de instancia de usuario en SQL Server Express también le permite trabajar con *.mdf* archivos, pero la característica de instancia de usuario está en desuso; por lo tanto, se recomienda LocalDB para trabajar con *.mdf* archivos.

Normalmente no se utiliza SQL Server Express para las aplicaciones web de producción. En particular LocalDB no se recomienda para su uso en producción con una aplicación web porque no está diseñado para trabajar con IIS.

En Visual Studio 2012 y versiones posteriores, LocalDB se instala de forma predeterminada con Visual Studio. En Visual Studio 2010 y versiones anteriores, SQL Server Express (sin LocalDB) se instala de forma predeterminada con Visual Studio; tendrá que instalarla manualmente si usa Visual Studio 2010.

En este tutorial trabajará con LocalDB para que la base de datos puede almacenarse en el *aplicación\_datos* carpeta como un *.mdf* archivo. Abrir la raíz *Web.config* de archivos y agregar una nueva cadena de conexión para el `connectionStrings` colección, como se muestra en el ejemplo siguiente. (Asegúrese de actualizar la *Web.config* archivo en la carpeta raíz del proyecto. También hay un *Web.config* archivo se encuentra en la *vistas* subcarpeta que no es necesario actualizar.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

De forma predeterminada, Entity Framework buscará en una cadena de conexión que el mismo nombre que el `DbContext` clase (`SchoolContext` para este proyecto). Ha agregado la cadena de conexión especifica una base de datos de LocalDB denominado *ContosoUniversity.mdf* ubicado en el *aplicación\_datos* carpeta. Para obtener más información, consulte [cadenas de conexión de SQL Server para las aplicaciones Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

No necesita realmente especificar la cadena de conexión. Si no proporciona una cadena de conexión, Entity Framework creará uno automáticamente; Sin embargo, la base de datos no estén en el *aplicación\_datos* carpeta de la aplicación. Para obtener información sobre dónde se creará la base de datos, vea [Code First para una base de datos](https://msdn.microsoft.com/data/jj193542).

El `connectionStrings` colección tiene también una cadena de conexión denominada `DefaultConnection` que se utiliza para la base de datos de pertenencia. No va a usar la base de datos de pertenencia de este tutorial. La única diferencia entre las dos cadenas de conexión es el nombre de la base de datos y el valor del atributo name.

## <a name="set-up-and-execute-a-code-first-migration"></a>Configurar y ejecutar una migración de primer código

Cuando se inicia por primera vez desarrollar una aplicación, los datos de cambios en el modelo con frecuencia y cada vez que los cambios del modelo obtiene sincronizada con la base de datos. Puede configurar el Entity Framework para quitar y volver a crear la base de datos cada vez que cambie el modelo de datos automáticamente. Esto no es un problema al principio de desarrollo porque están volver a crear fácilmente datos de prueba, pero una vez que haya implementado en producción normalmente es conveniente actualizar el esquema de base de datos sin quitar la base de datos. La característica de migraciones permite Code First actualizar la base de datos sin quitar y volver a crearla. Al principio del ciclo de desarrollo de un nuevo proyecto que desee usar [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) para quitar, volver a crear y volver a inicializar la base de datos cada vez que los cambios del modelo. Uno prepararse implementar la aplicación, puede convertir en el enfoque de migraciones. En este tutorial usará sólo las migraciones. Para obtener más información, consulte [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) y [serie de presentación en pantalla de migraciones](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Habilitar migraciones de Code First

1. Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. En el `PM>` símbolo del sistema escriba el siguiente comando:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![comando Enable-migrations](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Este comando crea un *migraciones* carpeta en el proyecto ContosoUniversity y lo coloca en esa carpeta una *archivo Configuration.cs que* archivos que se pueden editar para configurar las migraciones.

    ![Carpeta de migraciones](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    El `Configuration` clase incluye una `Seed` método al que se llama cuando se crea la base de datos y cada vez que se actualiza después de un modelo de datos modificados.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    El propósito de este `Seed` método es que le permite insertar datos de prueba en la base de datos después de que lo crea o actualiza su Code First.

### <a name="set-up-the-seed-method"></a>Configurar el método de inicialización

El [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método se ejecuta cuando migraciones de Code First crea la base de datos y cada vez que actualiza la base de datos para la migración más reciente. El propósito del método de inicialización es que le permite insertar datos en las tablas antes de que la aplicación tiene acceso a la base de datos por primera vez.

En versiones anteriores de Code First, antes del lanzamiento de las migraciones, era habitual que `Seed` métodos para insertar datos de prueba, porque con cada cambio de modelo durante el desarrollo de la base de datos no tiene que eliminar y volver a crear desde cero completamente. Con migraciones de Code First, prueba los datos se conservan después de realizar cambios de base de datos, por lo que incluso los datos de prueba de la [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método normalmente no es necesario. De hecho, no desea que el `Seed` método para insertar datos de prueba si va a usar migraciones para implementar la base de datos en producción, porque el `Seed` método se ejecutará en producción. En ese caso desea la `Seed` método para insertar en la base de datos únicamente los datos que desea insertar en producción. Por ejemplo, puede incluir nombres de departamento real en la base de datos la `Department` tabla cuando la aplicación esté disponible en producción.

Para este tutorial, usará las migraciones para la implementación, pero su `Seed` método insertará los datos de prueba de todos modos para que resulten más fáciles de ver cómo funciona la funcionalidad de la aplicación sin tener que insertar manualmente una gran cantidad de datos.

1. Reemplace el contenido de la *archivo Configuration.cs que* archivos con el código siguiente, que cargará los datos de prueba en la nueva base de datos. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    El [inicialización](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método toma el objeto de contexto de base de datos como un parámetro de entrada y el código en el método usa dicho objeto para agregar nuevas entidades a la base de datos. Para cada tipo de entidad, el código crea una colección de nuevas entidades, los agrega a la correspondiente [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) propiedad y, a continuación, guarda los cambios realizados en la base de datos. No es necesario llamar a la [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) método después de cada grupo de entidades, como se hace aquí, pero hacerlo le ayuda a localizar el origen de un problema si se produce una excepción mientras se está escribiendo el código en la base de datos.

    Algunas de las instrucciones que insertan datos utilizan el [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método para realizar una operación "upsert". Dado que el `Seed` método se ejecuta con cada migración, simplemente no se puede insertar datos, porque las filas que se está intentando agregar ya estarán ahí después de la primera migración que crea la base de datos. La operación "upsert" evita los errores que sucedería si se intenta insertar una fila que ya existe, pero ***invalida*** los cambios a los datos que haya podido realizar durante la comprobación de la aplicación. Con datos de prueba en algunas tablas puede no ser conveniente que ocurra esto: en algunos casos si cambia los datos durante las pruebas se desea que permanecen después de las actualizaciones de base de datos los cambios. En ese caso en el que desea realizar una operación de inserción condicional: insertar una fila sólo si aún no existe. El método de inicialización utiliza ambos enfoques.

    El primer parámetro pasado a la [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método especifica la propiedad que se va a usar para comprobar si ya existe una fila. Para los datos de estudiante de prueba que se va a proporcionar, el `LastName` propiedad puede utilizarse para este propósito, puesto que cada apellido en la lista es único:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Este código supone que los últimos nombres sean únicos. Si agrega manualmente un alumno con apellidos duplicados, obtendrá la excepción siguiente la próxima vez que se realiza una migración.

    La secuencia contiene más de un elemento

    Para obtener más información sobre la `AddOrUpdate` método, consulte [tener cuidado con el método de EF 4.3 AddOrUpdate](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) en el blog de Julie Lerman.

    El código que agrega `Enrollment` entidades no usan el `AddOrUpdate` método. Comprueba si una entidad ya existe y que inserta la entidad si no existe. Este enfoque, conservará los cambios realizados en un grado de inscripción cuando ejecutan las migraciones. El código recorre cada miembro de la `Enrollment` [lista](https://msdn.microsoft.com/library/6sh2ey19.aspx) y si la inscripción no se encuentra en la base de datos, agrega la inscripción a la base de datos. La primera vez que se actualice la base de datos, la base de datos estará vacío, por lo que agrega cada inscripción.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Para obtener información sobre cómo depurar el `Seed` método y cómo controlar los datos redundantes, como dos estudiantes denominados "Alexander Carson", consulte [Seeding y bases de datos de depuración de Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) en el blog de Rick Anderson.
2. Compile el proyecto.

### <a name="create-and-execute-the-first-migration"></a>Crear y ejecutar la primera migración

1. En la ventana de la consola de administrador de paquetes, escriba los siguientes comandos: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    El `add-migration` comando agrega a la carpeta Migrations un *[DateStamp]\_InitialCreate.cs* archivo que contiene código que crea la base de datos. El primer parámetro (`InitialCreate)` se utiliza para el archivo de nombre y puede ser todo lo que desees; normalmente elegir una palabra o frase que resume lo que se hace en la migración. Por ejemplo, podría denominar una migración posterior &quot;AddDepartmentTable&quot;.

    ![Carpeta de migraciones con migración inicial](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    El `Up` método de la `InitialCreate` clase crea las tablas de base de datos que corresponden a los conjuntos de entidades del modelo de datos, y el `Down` método elimina. Las migraciones llaman al método `Up` para implementar los cambios del modelo de datos para una migración. Cuando se escribe un comando para revertir la actualización, las migraciones llaman al método `Down`. El código siguiente muestra el contenido de la `InitialCreate` archivo:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    El `update-database` comando se ejecuta la `Up` método para crear la base de datos y, a continuación, se ejecuta el `Seed` método para rellenar la base de datos.

Una base de datos de SQL Server ahora se ha creado para el modelo de datos. El nombre de la base de datos es *ContosoUniversity*y el *.mdf* archivo se encuentra en el proyecto *aplicación\_datos* carpeta ya que es lo que especifique en la cadena de conexión.

Puede usar **Explorador de servidores** o **Explorador de objetos de SQL Server** (SSOX) para ver la base de datos en Visual Studio. En este tutorial usará **Explorador de servidores**. En Visual Studio Express 2012 para Web, **Explorador de servidores** se denomina **el Explorador de base de datos**.

1. Desde el **vista** menú, haga clic en **Explorador de servidores**.
2. Haga clic en el **Agregar conexión** icono.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Si se le presenta la **Elegir origen de datos** cuadro de diálogo, haga clic en **Microsoft SQL Server**y, a continuación, haga clic en **continuar**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. En el **Agregar conexión** diálogo cuadro, escriba **(localdb) \v11.0** para el **nombre del servidor**. En **seleccione o escriba un nombre de base de datos**, seleccione **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Haga clic en **Aceptar.**
6. Expanda **SchoolContext** y, a continuación, expanda **tablas**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Haga clic en el **Student** de tabla y haga clic en **mostrar datos de tabla** para ver las columnas que se crearon y las filas que se insertaron en la tabla.

    ![Tabla de estudiantes](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Crear un controlador de estudiantes y vistas

El siguiente paso es crear un ASP.NET MVC controlador y vistas en la aplicación que puede trabajar con una de estas tablas.

1. Para crear un `Student` controlador, haga clic en el **controladores** carpeta **el Explorador de soluciones**, seleccione **agregar**y, a continuación, haga clic en **controlador** . En el **Agregar controlador** diálogo cuadro, realice las selecciones siguientes y, a continuación, haga clic en **agregar**: 

   - Nombre del controlador: **StudentController**.
   - Plantilla: **controlador de MVC con acciones de lectura/escritura y vistas, mediante Entity Framework**.
   - Clase de modelo: **estudiante (ContosoUniversity.Models)**. (Si no ve esta opción en la lista desplegable, compile el proyecto e inténtelo de nuevo.)
   - Clase de contexto de datos: **SchoolContext (ContosoUniversity.Models)**.
   - Vistas: **Razor (CSHTML)**. (El valor predeterminado.)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio abre el *Controllers\StudentController.cs* archivo. Verá que se ha creado una variable de clase que crea una instancia de un objeto de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     El `Index` método de acción obtiene una lista de estudiantes desde el *estudiantes* conjunto mediante la lectura de entidades la `Students` propiedad de la instancia de contexto de base de datos:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     El *Student\Index.cshtml* vista muestra esta lista en una tabla:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Presione CTRL+F5 para ejecutar el proyecto.

     Haga clic en el **estudiantes** pestaña para ver los datos de prueba que el `Seed` método insertado.

     ![Página de índice de estudiante](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Convenciones

La cantidad de código que tenía que escribir en orden para Entity Framework poder crear una base de datos completa para usted es mínima debido al uso de *convenciones*, o suposiciones que hace de Entity Framework. Algunos de ellos ya se han anotado:

- Las formas pluralized de nombres de clase de entidad se utilizan como nombres de tabla.
- Los nombres de propiedad de entidad se usan para los nombres de columna.
- Propiedades de la entidad que se denominan `ID` o *classname* `ID` se reconocen como propiedades de clave principal.

Ha visto que pueden reemplazarse convenciones (por ejemplo, se especifica que los nombres de tabla no deben ser pluralizan), y aprenderá más acerca de las convenciones y cómo invalidar en la [crear un modelo de datos más complejos](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) tutorial más adelante en esta serie. Para obtener más información, consulte [convenciones de código de primera](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Resumen

Ahora ha creado una aplicación simple que usa Entity Framework y SQL Server Express para almacenar y mostrar los datos. En el tutorial siguiente aprenderá cómo realizar básica CRUD (crear, leer, actualizar y eliminar) operaciones. Puede dejar comentarios en la parte inferior de esta página. Cuéntenos cómo le gustó esta parte del tutorial y cómo podemos mejorarla.

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Siguiente](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
