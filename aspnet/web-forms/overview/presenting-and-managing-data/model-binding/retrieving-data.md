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
ms.openlocfilehash: c53c27f4852eab9813bd917315111e7cd3b04953
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396290"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Recuperar y mostrar datos con enlace de modelos y formularios web forms
====================

> Esta serie de tutoriales muestra los aspectos básicos del uso de enlace de modelos con un proyecto de formularios Web Forms de ASP.NET. Enlace de modelos permite interactuar con los datos más sencilla de tratar con datos de objetos de origen (como ObjectDataSource o SqlDataSource). Esta serie comienza con material introductorio y mueve a conceptos más avanzados en los tutoriales posteriores.
> 
>  El patrón de enlace modelo funciona con cualquier tecnología de acceso a datos. En este tutorial, va a usar Entity Framework, pero podría usar la tecnología de acceso a datos que le resulte más familiar. Desde un control de servidor enlazado a datos, como un control ListView, GridView, DetailsView o FormView, especifique los nombres de los métodos que se usará para seleccionar, actualizar, eliminar y crear datos. En este tutorial, especificará un valor para el SelectMethod. 
> 
> Dentro de ese método, proporcionan la lógica para recuperar los datos. En el siguiente tutorial, establecerá los valores para UpdateMethod, InsertMethod y la DeleteMethod.
>
> También puede [descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o Visual Basic. El código descargable funciona con Visual Studio 2012 y versiones posteriores. Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2017 que se muestra en este tutorial.
> 
> En el tutorial para ejecutar la aplicación en Visual Studio. También puede implementar la aplicación en un proveedor de hospedaje y que esté disponible a través de internet. Microsoft ofrece hospedaje web gratuito para hasta 10 sitios web en un  
>  [cuenta de evaluación de Azure gratuita](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Para obtener información acerca de cómo implementar un proyecto web de Visual Studio en Azure App Service Web Apps, consulte el [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) serie. Este tutorial también muestra cómo usar migraciones de Entity Framework Code First para implementar la base de datos de SQL Server en Azure SQL Database.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> - Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017
>   
> Este tutorial también funciona con Visual Studio 2012 y Visual Studio 2013, pero hay algunas diferencias en la plantilla de proyecto y la interfaz de usuario.


## <a name="what-youll-build"></a>¿Qué va a crear

En este tutorial, necesitará:

* Generar objetos de datos que reflejan una universidad con los estudiantes matriculados en cursos
* Crear tablas de base de datos de los objetos
* Rellenar la base de datos con datos de prueba
* Mostrar datos en un formulario web Forms

## <a name="create-the-project"></a>Crear el proyecto

1. En Visual Studio 2017, cree un **aplicación Web ASP.NET (.NET Framework)** proyecto denominado **ContosoUniversityModelBinding**.

   ![Crear proyecto](retrieving-data/_static/image19.png)

2. Seleccione **Aceptar**. Aparece el cuadro de diálogo para seleccionar una plantilla.

   ![Seleccione los formularios web forms](retrieving-data/_static/image3.png)

3. Seleccione el **formularios Web Forms** plantilla. 

4. Si es necesario, cambie a la autenticación **cuentas de usuario individuales**. 

5. Seleccione **Aceptar** para crear el proyecto.

## <a name="modify-site-appearance"></a>Modificar la apariencia del sitio

   Realizar algunos cambios para personalizar la apariencia del sitio. 
   
   1. Abra el archivo Site.Master.
   
   2. Cambie el título para mostrar **Contoso University** y no **My ASP.NET Application**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

   3. Cambiar el texto del encabezado de **nombre de la aplicación** a **Contoso University**.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

   4. Cambie los vínculos del encabezado de navegación para las columnas adecuadas del sitio. 
   
      Quitar los vínculos para **sobre** y **póngase en contacto con** y, en su lugar, vincular a un **estudiantes** página, que se va a crear.

      [!code-aspx-csharp[Main](retrieving-data/samples/sample3.aspx)]

   5. Guardar Site.Master.

## <a name="add-a-web-form-to-display-student-data"></a>Agregue un formulario web para mostrar datos de estudiante

   1. En **el Explorador de soluciones**, haga clic en el proyecto, seleccione **agregar** y, a continuación, **nuevo elemento**. 
   
   2. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **formulario Web Forms con página maestra** plantilla y asígnele el nombre **Students.aspx**.

      ![Crear página](retrieving-data/_static/image5.png)

   3. Seleccione **Agregar**.
   
   4. Página principal del formulario web Forms, seleccione **Site.Master**.
   
   5. Seleccione **Aceptar**.
   

## <a name="add-the-data-model"></a>Agregar el modelo de datos

En el **modelos** carpeta, agregue una clase denominada **UniversityModels.cs**.

   1. Haga clic en **modelos**, seleccione **agregar**y, a continuación, **nuevo elemento**. Aparecerá el cuadro de diálogo **Agregar nuevo elemento**.

   2. En el menú de navegación izquierdo, seleccione **código**, a continuación, **clase**.

      ![Crear clase de modelo](retrieving-data/_static/image20.png)

   3. Nombre de la clase **UniversityModels.cs** y seleccione **agregar**.

      En este archivo, defina el `SchoolContext`, `Student`, `Enrollment`, y `Course` clases como sigue:

      [!code-csharp[Main](retrieving-data/samples/sample4.cs)]

      El `SchoolContext` clase se deriva de `DbContext`, que administra la conexión de base de datos y los cambios en los datos.

      En el `Student` class, tenga en cuenta los atributos aplicados a la `FirstName`, `LastName`, y `Year` propiedades. Este tutorial usa estos atributos para la validación de datos. Para simplificar el código, estas propiedades se marcan con atributos de validación de datos. En un proyecto real, los atributos de validación se aplicaría a todas las propiedades que necesitan validación.

   4. Save UniversityModels.cs.

## <a name="set-up-the-database-based-on-classes"></a>Configurar la base de datos basado en clases

Este tutorial se usa [migraciones de Code First](https://docs.microsoft.com/en-us/ef/ef6/modeling/code-first/migrations/) para crear objetos y las tablas de base de datos. Estas tablas almacenan información acerca de los alumnos y sus cursos.

   1. Seleccione **herramientas** > **Administrador de paquetes de NuGet** > **Package Manager Console**.

   2. En **Package Manager Console**, ejecute este comando:  
      `enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

      Si el comando se completa correctamente, aparece un mensaje que indica que se han habilitado las migraciones.

      ![Habilitación de migraciones](retrieving-data/_static/image8.png)

      Tenga en cuenta que un archivo denominado *Configuration.cs* se ha creado. El `Configuration` clase tiene un `Seed` método, que puede rellenar previamente las tablas de base de datos con datos de prueba.

## <a name="pre-populate-the-database"></a>Rellenar previamente la base de datos

   1. Abra Configuration.cs.
   
   2. Agregue el código siguiente al método `Seed` . Además, agregue un `using` instrucción para el `ContosoUniversityModelBinding. Models` espacio de nombres.

      [!code-csharp[Main](retrieving-data/samples/sample5.cs)]

   3. Guardar Configuration.cs.

   4. En la consola de administrador de paquetes, ejecute el comando **inicial de migración agregar**.

   5. Ejecute el comando **Actualizar base de datos**.

      Si recibe una excepción al ejecutar este comando, el `StudentID` y `CourseID` valores pueden diferir de la `Seed` valores del método. Abra las tablas de base de datos y busque los valores existentes de `StudentID` y `CourseID`. Agregue esos valores en el código para la propagación del `Enrollments` tabla.

## <a name="add-a-gridview-control"></a>Agregar un control GridView

Con los datos de la base de datos rellenada, ahora está listo para recuperar los datos y mostrarlos. 

1. Abra Students.aspx.

2. Busque el `MainContent` marcador de posición. Dentro de ese marcador de posición, agregue un **GridView** control que incluye este código.

   [!code-aspx-csharp[Main](retrieving-data/samples/sample6.aspx)]

   Cosas a tener en cuenta:
   * Tenga en cuenta el valor establecido para el `SelectMethod` propiedad en el elemento GridView. Este valor especifica el método utilizado para recuperar datos de GridView, que se crean en el paso siguiente. 
   
   * El `ItemType` propiedad está establecida en el `Student` clase creada anteriormente. Esta configuración permite hacer referencia a propiedades de clase en el marcado. Por ejemplo, el `Student` clase tiene una colección denominada `Enrollments`. Puede usar `Item.Enrollments` para recuperar esa colección y, a continuación, usar [sintaxis LINQ](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) recuperar cada estudiante del inscrito suma créditos.
   
3. Guardar Students.aspx.

## <a name="add-code-to-retrieve-data"></a>Agregue código para recuperar datos

   En el archivo de código subyacente Students.aspx, agregue el método especificado para el `SelectMethod` valor. 
   
   1. Abra Students.aspx.cs.
   
   2. Agregar `using` instrucciones para la `ContosoUniversityModelBinding. Models` y `System.Data.Entity` espacios de nombres.

      [!code-csharp[Main](retrieving-data/samples/sample7.cs)]

   3. Agregue el método especificado para `SelectMethod`:

      [!code-csharp[Main](retrieving-data/samples/sample8.cs)]

      El `Include` cláusula mejora el rendimiento de las consultas, pero no es necesario. Sin el `Include` cláusula, los datos se recupera mediante [ *la carga diferida*](https://en.wikipedia.org/wiki/Lazy_loading), lo que implica enviar una consulta independiente para la base de datos cada vez relacionados se recuperan los datos. Con el `Include` cláusula, los datos se recupera mediante *carga diligente*, lo que significa que una consulta de base de datos única recupera todos los datos relacionados. Si no se usan datos relacionados, la carga diligente es menos eficiente porque se recuperan más datos. Sin embargo, en este caso, la carga diligente ofrece el mejor rendimiento porque los datos relacionados se muestran para cada registro.

      Para obtener más información acerca de las consideraciones de rendimiento al cargar los datos relacionados, consulte el **explícita de carga de datos relacionados, diligente y diferida** sección la [leer los datos relacionados con Entity Framework en ASP.NET Aplicación MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) artículo.

      De forma predeterminada, los datos se ordenan por los valores de la propiedad marcada como la clave. Puede agregar un `OrderBy` cláusula para especificar un valor de ordenación diferente. En este ejemplo, el valor predeterminado `StudentID` propiedad se utiliza para ordenar. En el [datos filtrado, paginación y ordenación](sorting-paging-and-filtering-data.md) artículo, el usuario está habilitado para seleccionar una columna para ordenar.
 
   4. Guardar Students.aspx.cs.

## <a name="run-your-application"></a>Ejecute la aplicación 

Ejecute la aplicación web (**F5**) y navegue hasta la **estudiantes** página, que muestra lo siguiente:

   ![Mostrar datos](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Generación automática de los métodos de enlace de modelos

Cuando se trabaja a través de esta serie de tutoriales, simplemente copie el código del tutorial al proyecto. Sin embargo, una desventaja de este enfoque es que es posible que no será consciente de la característica proporcionada por Visual Studio para generar automáticamente código para los métodos de enlace de modelos. Cuando se trabaja en sus propios proyectos, generación automática de código puede ahorrarle tiempo y ayudarle a obtener una idea de cómo implementar una operación. En esta sección se describe la característica de generación automática de código. Esta sección solo es informativa y no contiene ningún código que necesita para implementar en el proyecto. 

Al establecer un valor para el `SelectMethod`, `UpdateMethod`, `InsertMethod`, o `DeleteMethod` propiedades en el código de marcado, puede seleccionar la **crear un nuevo método** opción.

![Cree un método](retrieving-data/_static/image18.png)

Visual Studio no solo crea un método en el código subyacente con la firma correcta, sino que también genera código de implementación para realizar la operación. Si se establece en primer lugar el `ItemType` propiedad antes de usar la generación automática de código de características, los usos del código generado que escriban para las operaciones. Por ejemplo, al establecer el `UpdateMethod` propiedad, el código siguiente se genera automáticamente:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

De nuevo, este código no tiene que agregarse al proyecto. En el siguiente tutorial, implementará métodos para actualizar, eliminar y agregar datos nuevos.

## <a name="summary"></a>Resumen

En este tutorial, creó clases de modelo de datos y genera una base de datos de esas clases. Rellena las tablas de base de datos con datos de prueba. Usa el enlace de modelos para recuperar datos de la base de datos y, a continuación, muestra los datos en un control GridView.

En los próximos [tutorial](updating-deleting-and-creating-data.md) en esta serie, permitiremos a actualizar, eliminar y crear datos.

> [!div class="step-by-step"]
> [Siguiente](updating-deleting-and-creating-data.md)
