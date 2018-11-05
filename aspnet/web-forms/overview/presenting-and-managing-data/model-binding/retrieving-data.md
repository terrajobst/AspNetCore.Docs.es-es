---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperar y mostrar datos con formularios web y el enlace de modelos | Microsoft Docs
author: Rick-Anderson
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: b05f3780d7c4e4734b35c0d9377a89d6f3edb0f8
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021344"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recuperar y mostrar datos con enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.
> 
> El patrón de enlace modelo funciona con cualquier tecnología de acceso a datos. En este tutorial, va a usar Entity Framework, pero podría usar la tecnología de acceso a datos que le resulte más familiar. Desde un control de servidor enlazado a datos, como un control ListView, GridView, DetailsView o FormView, especifique los nombres de los métodos que se usará para seleccionar, actualizar, eliminar y crear datos. En este tutorial, especificará un valor para el SelectMethod.
> 
> Dentro de ese método, proporcionan la lógica para recuperar los datos. En el siguiente tutorial, establecerá los valores para UpdateMethod, InsertMethod y la DeleteMethod.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código descargable funciona con Visual Studio 2012 o Visual Studio 2013. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.
> 
> En el tutorial para ejecutar la aplicación en Visual Studio. Se puede también que la aplicación esté disponible a través de Internet mediante su implementación en un proveedor de hospedaje. Microsoft ofrece hospedaje web gratuito para hasta 10 sitios web en un  
>  [cuenta de evaluación de Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Para obtener información acerca de cómo implementar un proyecto web de Visual Studio en Azure App Service Web Apps, consulte el [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie. Este tutorial también muestra cómo usar migraciones de Entity Framework Code First para implementar la base de datos de SQL Server en Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Microsoft Visual Studio 2013 o Microsoft Visual Studio Express 2013 para Web
>   
> 
> Este tutorial también funciona con Visual Studio 2012, pero habrá algunas diferencias en la plantilla de proyecto y la interfaz de usuario.


## <a name="what-youll-build"></a>¿Qué va a crear

En este tutorial, necesitará:

1. Generar objetos de datos que reflejan una universidad con los estudiantes matriculados en cursos
2. Crear tablas de base de datos de los objetos
3. Rellenar la base de datos con datos de prueba
4. Mostrar datos en un formulario web Forms

## <a name="set-up-project"></a>Configuración de proyecto

En Visual Studio 2013, cree un nuevo **aplicación Web ASP.NET** llamado **ContosoUniversityModelBinding**.

![Crear proyecto](retrieving-data/_static/image2.png)

Seleccione la plantilla de formularios Web Forms y deje las demás opciones de forma predeterminada. Haga clic en Aceptar para el proyecto de instalación.

![Seleccione los formularios web forms](retrieving-data/_static/image3.png)

En primer lugar, hará que un par de pequeños cambios para personalizar la apariencia del sitio. Abra el **Site.Master** de archivo y cambie el título para incluir Contoso University en lugar de mi aplicación ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

A continuación, cambie el texto del encabezado de **nombre de la aplicación** a **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

También en Site.Master, cambie los vínculos de navegación que aparecen en el encabezado para reflejar las páginas que son relevantes para este sitio. No necesitará cualquiera el **sobre** página o el **póngase en contacto con** para esos vínculos se pueden quitar de la página. En su lugar, tendrá un vínculo a una página denominada **estudiantes**. Esta página aún no se ha creado.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Guarde y cierre Site.Master.

Ahora, creará el formulario web Forms para mostrar los datos de estudiante. Haga clic en el proyecto, y **agregar** un **nuevo elemento**. Seleccione el **formulario Web Forms con página maestra** plantilla y asígnele el nombre **Students.aspx**.

![Crear página](retrieving-data/_static/image5.png)

Seleccione **Site.Master** como la página maestra para el nuevo formulario web.

## <a name="create-the-data-models-and-database"></a>Crear la base de datos y modelos de datos

Usará migraciones de Code First para crear objetos y las tablas de base de datos correspondiente. Estas tablas almacena información acerca de los alumnos y sus cursos.

En la carpeta Models, agregue una nueva clase denominada **UniversityModels.cs**.

![Crear clase de modelo](retrieving-data/_static/image7.png)

En este archivo, defina las clases SchoolContext, estudiantes, inscripción y curso como sigue:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

La clase SchoolContext se deriva de DbContext, que administra la conexión de base de datos y los cambios en los datos.

En la clase Student, observe los atributos que se han aplicado a la **FirstName**, **LastName**, y **año** propiedades. Estos atributos se usará para la validación de datos en este tutorial. Para simplificar el código para este proyecto de demostración, estas propiedades se marcaron con atributos de validación de datos. En un proyecto real, los atributos de validación se aplicaría a todas las propiedades que necesitan los datos validados, como las propiedades en las clases de inscripción y curso.

Guardar UniversityModels.cs.

Utilizará las herramientas para migraciones de Code First para configurar una base de datos basado en estas clases.

En **Package Manager Console**, ejecute el comando:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Si el comando se completa correctamente, recibirá un mensaje que indica que se han habilitado las migraciones,

![Habilitación de migraciones](retrieving-data/_static/image8.png)

Tenga en cuenta que un nuevo archivo denominado **Configuration.cs** se ha creado. En Visual Studio, este archivo se abre automáticamente después de crearlo. La clase de configuración contiene un **inicialización** método que permite rellenar previamente las tablas de base de datos con datos de prueba.

Agregue el código siguiente al método de inicialización. Deberá agregar un **mediante** instrucción para el **ContosoUniversityModelBinding.Models** espacio de nombres.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Guardar Configuration.cs.

En la consola de administrador de paquetes, ejecute el comando `add-migration initial`.

A continuación, ejecute el comando `update-database`.

Si recibe una excepción al ejecutar este comando, es posible que los valores de StudentID y CourseID han variado desde los valores en el método de inicialización. Abra esas tablas en la base de datos y busque los valores existentes de StudentID y CourseID. Agregue esos valores en el código para la propagación de la tabla de inscripciones.

Se ha agregado el archivo de base de datos, pero actualmente está oculto en el proyecto. Haga clic en **mostrar todos los archivos** para mostrar el archivo.

![Mostrar todos los archivos](retrieving-data/_static/image10.png)

Observe que el archivo .mdf aparece ahora en la aplicación\_carpeta de datos.

![archivo de base de datos](retrieving-data/_static/image12.png)

Haga doble clic en el archivo .mdf y abra el Explorador de servidores. Las tablas ya existen y se rellenan con datos.

![tablas de bases de datos](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Mostrar los datos de los estudiantes y las tablas relacionadas

Con datos de la base de datos, ahora está listo para recuperar los datos y mostrarlos en una página web. Usará un **GridView** control para mostrar los datos en filas y columnas.

Abra Students.aspx y busque el **MainContent** marcador de posición. Dentro de ese marcador de posición, agregue un **GridView** control que incluye el código siguiente.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Hay un par de conceptos importantes en este código de marcado para que tenga en cuenta. En primer lugar, tenga en cuenta que se establece un valor para el **SelectMethod** propiedad en el elemento GridView. Este valor especifica el nombre del método que se usa para recuperar datos del control GridView. Este método se creará en el paso siguiente. En segundo lugar, tenga en cuenta que el **ItemType** propiedad está establecida en la clase Student que creó anteriormente. Al establecer este valor, se puede hacer referencia a las propiedades de dicha clase en el código de marcado. Por ejemplo, la clase Student contiene una colección denominada inscripciones. Puede usar **Item.Enrollments** para recuperar esa colección y, a continuación, utilice la sintaxis LINQ para recuperar la suma de créditos inscritos para cada alumno.

En el archivo de código subyacente, deberá agregar el método que se ha especificado para el **SelectMethod** valor. Abra **Students.aspx.cs**y agregue **mediante** instrucciones para la **ContosoUniversityModelBinding.Models** y **System.Data.Entity**espacios de nombres.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

A continuación, agregue el siguiente método. Tenga en cuenta que el nombre de este método coincide con el nombre proporcionado para SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

El **Include** cláusula mejora el rendimiento de esta consulta, pero no es esencial para la consulta para que funcione. Sin la cláusula Include, se recuperan los datos mediante la carga diferida, lo que implica el envío de que una consulta independiente para la base de datos cada vez relacionados se recuperan los datos. Al proporcionar la cláusula Include, los datos se recuperan mediante la carga diligente, lo que significa que todos los datos relacionados se recuperan a través de una sola consulta de la base de datos. En casos donde la mayoría de los datos relacionados es que no sea la carga diligente, utilizada puede ser menos eficiente porque se recuperan más datos. Sin embargo, en este caso, la carga diligente proporciona el mejor rendimiento porque los datos relacionados se muestran para cada registro.

Para obtener más información acerca de la consideración de rendimiento al cargar los datos relacionados, vea la sección titulada **explícita de carga de datos relacionados, diligente y diferida** en el [leer los datos relacionados con Entity Framework en un ASP Aplicación de MVC de .NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tema.

De forma predeterminada, los datos se ordenan por los valores de la propiedad marcada como la clave. Puede agregar una cláusula OrderBy para especificar un valor diferente para la ordenación. En este ejemplo, la propiedad de StudentID predeterminada se utiliza para ordenar. En el [datos filtrado, paginación y ordenación](sorting-paging-and-filtering-data.md) tema, permitirá al usuario seleccionar una columna para ordenar.

Ejecutar la aplicación web y vaya a la página Students. La página Students muestra la siguiente información de estudiantes.

![Mostrar datos](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generación automática de los métodos de enlace de modelos

Cuando se trabaja a través de esta serie de tutoriales, simplemente copie el código del tutorial al proyecto. Sin embargo, una desventaja de este enfoque es que es posible que no será consciente de la característica proporcionada por Visual Studio para generar automáticamente código para los métodos de enlace de modelos. Cuando se trabaja en sus propios proyectos, generación automática de código puede ahorrarle tiempo y ayudarle a obtener una idea de cómo implementar una operación. En esta sección se describe la característica de generación automática de código. Esta sección solo es informativa y no contiene ningún código que necesita para implementar en el proyecto.

Al establecer un valor para el **SelectMethod**, **UpdateMethod**, **InsertMethod**, o **DeleteMethod** propiedades en el código de marcado Puede seleccionar la **crear un nuevo método** opción.

![Crear nuevo método](retrieving-data/_static/image18.png)

Visual Studio no solo crea un método en el código subyacente con la firma correcta, sino que también genera código de implementación que le ayuden a realizar la operación. Si se establece en primer lugar el **ItemType** propiedad antes de usar la característica de generación automática de código, el código generado utilizará ese tipo para las operaciones. Por ejemplo, al establecer la propiedad UpdateMethod, automáticamente se genera el código siguiente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

De nuevo, el código anterior no deben agregarse al proyecto. En el tutorial siguiente, se implementarán métodos para actualizar, eliminar y agregar datos nuevos.

## <a name="conclusion"></a>Conclusión

En este tutorial, creó clases de modelo de datos y genera una base de datos de esas clases. Rellena las tablas de base de datos con datos de prueba. Usa el enlace de modelos para recuperar datos de la base de datos y, a continuación, muestra los datos en un control GridView.

En los próximos [tutorial](updating-deleting-and-creating-data.md) en esta serie, permitirá actualizar, eliminar y crear datos.

> [!div class="step-by-step"]
> [Siguiente](updating-deleting-and-creating-data.md)
