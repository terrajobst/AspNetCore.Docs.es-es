---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Recuperar y mostrar los datos con modelo de enlace y formularios web | Documentos de Microsoft
author: tfitzmac
description: Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más directa-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889100"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recuperar y mostrar los datos con el enlace de modelos y formularios web forms
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos hace interacción con los datos más sencilla de trabajar con datos de objetos de origen (por ejemplo, ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y se mueve al conceptos más avanzados en tutoriales posteriores.
> 
> El modelo de enlace funciona con cualquier tecnología de acceso a datos. En este tutorial, va a usar Entity Framework, pero puede usar la tecnología de acceso a datos que le resulte más familiar. Desde un control de servidor enlazado a datos, como un control ListView, GridView, DetailsView o FormView, especifique los nombres de los métodos que se utilizará para seleccionar, actualizar, eliminar y crear datos. En este tutorial, especifique un valor para el SelectMethod.
> 
> Dentro de ese método, proporcionar la lógica para recuperar los datos. En el siguiente tutorial, establecerá valores para UpdateMethod, DeleteMethod y InsertMethod.
> 
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB. El código que se puede descargar funciona con Visual Studio 2012 o Visual Studio 2013. Utiliza la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.
> 
> En el tutorial se ejecute la aplicación en Visual Studio. También puede hacer la aplicación disponible a través de Internet mediante la implementación en un proveedor de hospedaje. Microsoft ofrece el hospedaje web gratuito para hasta 10 sitios web en un  
>  [liberar la cuenta de prueba de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Para obtener información sobre cómo implementar un proyecto web de Visual Studio para aplicaciones de Web del servicio de aplicación de Azure, consulte la [implementación de Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie. Este tutorial también muestra cómo usar migraciones de Entity Framework Code First para implementar la base de datos de SQL Server en la base de datos de SQL Azure.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Microsoft Visual Studio 2013 o Microsoft Visual Studio Express 2013 para Web
>   
> 
> Este tutorial también funciona con Visual Studio 2012, pero habrá algunas diferencias en la plantilla de proyecto y la interfaz de usuario.


## <a name="what-youll-build"></a>Lo que vamos a compilar

En este tutorial, deberá:

1. Generar objetos de datos que reflejan una universidad con los alumnos inscritos en cursos
2. Crear tablas de base de datos de los objetos
3. Rellenar la base de datos con datos de prueba
4. Mostrar datos en un formulario web Forms

## <a name="set-up-project"></a>Configurar el proyecto

En Visual Studio 2013, cree un nuevo **aplicación Web ASP.NET** llama **ContosoUniversityModelBinding**.

![Crear proyecto](retrieving-data/_static/image2.png)

Seleccione la plantilla de formularios Web Forms y dejar el resto de opciones predeterminadas. Haga clic en Aceptar para configurar el proyecto.

![Seleccione los formularios web forms](retrieving-data/_static/image3.png)

En primer lugar, deberá tomar un par de pequeños cambios para personalizar la apariencia del sitio. Abra la **Site.Master** archivo y cambie el título para incluir la Universidad de Contoso en lugar de mi aplicación ASP.NET.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

A continuación, cambie el texto del encabezado de **nombre de la aplicación** a **Contoso Universidad**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

También en Site.Master, cambie los vínculos de navegación que aparecen en el encabezado para reflejar las páginas que son relevantes para este sitio. No necesitará cualquiera el **sobre** página o **póngase en contacto con** por lo que se pueden quitar los vínculos de página. En su lugar, necesitará un vínculo a una página denominada **estudiantes**. Esta página aún no se ha creado.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Guarde y cierre Site.Master.

Ahora, creará el formulario web Forms para mostrar los datos de los alumnos. Haga clic en el proyecto, y **agregar** una **nuevo elemento**. Seleccione el **formulario Web con página maestra** plantilla y asígnele el nombre **Students.aspx**.

![Crear página](retrieving-data/_static/image5.png)

Seleccione **Site.Master** como la página maestra para el nuevo formulario web.

## <a name="create-the-data-models-and-database"></a>Crear la base de datos y modelos de datos

Migraciones de Code First usará para crear objetos y las tablas de base de datos correspondiente. Estas tablas almacenará información acerca de los alumnos y sus cursos.

En la carpeta Models, agregue una nueva clase denominada **UniversityModels.cs**.

![Crear clase de modelo](retrieving-data/_static/image7.png)

En este archivo, defina las clases SchoolContext, estudiantes, inscripción y curso como sigue:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

La clase SchoolContext se deriva de DbContext, que administra la conexión de base de datos y los cambios en los datos.

En la clase Student, observe los atributos que se han aplicado a la **FirstName**, **LastName**, y **año** propiedades. Estos atributos se usará para la validación de datos en este tutorial. Para simplificar el código para este proyecto de demostración, estas propiedades se han marcado con atributos de validación de datos. En un proyecto real, se aplicaría atributos de validación para todas las propiedades que se deben validados datos, como propiedades en las clases de curso e inscripción.

Save UniversityModels.cs.

Utilizará las herramientas de migraciones de Code First para configurar una base de datos basado en estas clases.

En **Package Manager Console**, ejecute el comando:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Si el comando se completa correctamente, recibirá un mensaje que indica que se han habilitado las migraciones,

![habilitar las migraciones](retrieving-data/_static/image8.png)

Tenga en cuenta que un nuevo archivo denominado **archivo Configuration.cs que** se ha creado. En Visual Studio, este archivo se abre automáticamente después de crearlo. La clase de configuración contiene un **inicialización** método que permite rellenar previamente las tablas de base de datos con datos de prueba.

Agregue el código siguiente al método de inicialización. Debe agregar una **con** instrucción para el **ContosoUniversityModelBinding.Models** espacio de nombres.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Guarde el archivo Configuration.cs que.

En la consola de administrador de paquetes, ejecute el comando `add-migration initial`.

A continuación, ejecute el comando `update-database`.

Si recibe una excepción cuando se ejecuta este comando, es posible que los valores de StudentID y CourseID han variado desde los valores en el método de inicialización. Abra las tablas en la base de datos y busque los valores existentes de StudentID y CourseID. Agregue esos valores en el código para la propagación de la tabla de inscripciones.

Se ha agregado el archivo de base de datos, pero está oculto actualmente en el proyecto. Haga clic en **mostrar todos los archivos** para mostrar el archivo.

![Mostrar todos los archivos](retrieving-data/_static/image10.png)

Observe que el archivo .mdf aparece ahora en la aplicación\_carpeta de datos.

![archivo de base de datos](retrieving-data/_static/image12.png)

Haga doble clic en el archivo .mdf y abra el Explorador de servidores. Las tablas ahora existen y se rellenan con datos.

![tablas de bases de datos](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Mostrar los datos de los estudiantes y las tablas relacionadas

Con los datos en la base de datos, ya está listo para recuperar los datos y mostrarlos en una página web. Usará un **GridView** control para mostrar los datos en filas y columnas.

Abra Students.aspx y busque el **MainContent** marcador de posición. Dentro de ese marcador de posición, agregue un **GridView** control que incluye el código siguiente.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Hay un par de conceptos importantes en este código de marcado para el que debe tener en cuenta. En primer lugar, tenga en cuenta que se establece un valor para el **SelectMethod** propiedad en el elemento de GridView. Este valor especifica el nombre del método que se utiliza para recuperar los datos del control GridView. Este método creará en el paso siguiente. En segundo lugar, tenga en cuenta que la **ItemType** propiedad se establece en la clase de estudiante que creó anteriormente. Al establecer este valor, puede hacer referencia a las propiedades de dicha clase en el código de marcado. Por ejemplo, la clase Student contiene una colección con el nombre de inscripciones. Puede usar **Item.Enrollments** para recuperar esa colección y, a continuación, utilice la sintaxis LINQ para recuperar la suma de créditos inscritos para cada alumno.

En el archivo de código subyacente, debe agregar el método que se especifica para el **SelectMethod** valor. Abra **Students.aspx.cs**y agregue **con** instrucciones para la **ContosoUniversityModelBinding.Models** y **System.Data.Entity**los espacios de nombres.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

A continuación, agregue el método siguiente. Tenga en cuenta que el nombre de este método coincide con el nombre proporcionado para SelectMethod.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

El **Include** cláusula mejora el rendimiento de esta consulta, pero no es esencial para la consulta para que funcione. Sin la cláusula Include, los datos se recuperan mediante la carga diferida, lo que implica el envío de que una consulta independiente para la base de datos relacionados de cada vez que se recuperan los datos. Al proporcionar la cláusula Include, los datos se recuperan con carga diligente, lo que significa que se recuperen todos los datos relacionados mediante una única consulta de la base de datos. En casos donde la mayoría de los datos relacionados no es puede ser utilizada, diligente carga puede ser menos eficiente porque se recuperan más datos. Sin embargo, en este caso, carga diligente proporciona el mejor rendimiento porque los datos relacionados se muestran para cada registro.

Para obtener más información acerca de consideraciones sobre el rendimiento al cargar los datos relacionados, vea la sección titulada **Lazy, diligente y explícitas de carga de datos relacionados** en el [leer los datos relacionados con Entity Framework en un ASP Aplicación de MVC .NET](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) tema.

De forma predeterminada, los datos se ordenan por los valores de la propiedad marcada como la clave. Puede agregar una cláusula OrderBy para especificar un valor diferente para la ordenación. En este ejemplo, la propiedad de StudentID predeterminada se utiliza para ordenar. En el [ordenación, paginación y filtrado de datos](sorting-paging-and-filtering-data.md) tema, permitirá al usuario seleccionar una columna para ordenar.

Ejecutar la aplicación web y navegue a la página de los alumnos. La página de los estudiantes que muestra la siguiente información de estudiantes.

![Mostrar datos](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generación automática de los métodos de enlace de modelo

Cuando se trabaja a través de esta serie de tutoriales, simplemente copie el código del tutorial al proyecto. Sin embargo, una desventaja de este enfoque es que puede no conocen la existencia de la característica proporcionada por Visual Studio para generar automáticamente código para los métodos de enlace de modelos. Cuando se trabaja en sus propios proyectos, generación automática de código puede ahorrarle tiempo y ayudarle a obtener una idea de cómo implementar una operación. Esta sección describe la característica de generación automática de código. Esta sección solo es informativa y no contiene ningún código que se debe implementar en el proyecto.

Al establecer un valor para el **SelectMethod**, **UpdateMethod**, **InsertMethod**, o **DeleteMethod** propiedades en el código de marcado Puede seleccionar la **crear nuevo método** opción.

![Crear nuevo método](retrieving-data/_static/image18.png)

Visual Studio no solo crea un método en el código subyacente con la firma correcta, sino que también genera código de implementación que le ayudarán a realizar la operación. Si se establece primero la **ItemType** propiedad antes de usar la característica de generación automática de código, el código generado utilizará ese tipo para las operaciones. Por ejemplo, al establecer la propiedad UpdateMethod, automáticamente se genera el código siguiente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Una vez más, el código anterior no deben agregarse al proyecto. En el tutorial siguiente, se implementarán métodos para actualizar, eliminar y agregar datos nuevos.

## <a name="conclusion"></a>Conclusión

En este tutorial, ha creado clases del modelo de datos y genera una base de datos a partir de esas clases. Rellena las tablas de base de datos con datos de prueba. Usa el enlace de modelos para recuperar datos de la base de datos y, a continuación, muestra los datos en un control GridView.

En la siguiente [tutorial](updating-deleting-and-creating-data.md) de esta serie, le permitirá actualizar, eliminar y crear datos.

> [!div class="step-by-step"]
> [Siguiente](updating-deleting-and-creating-data.md)
