---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Con Entity Framework 4.0 y el Control ObjectDataSource, parte 1: Introducción | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de Contoso University establecen que se crea mediante la introducción a la serie de tutoriales de Entity Framework. Si yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: ba3b65dededeca3534b9273bfd3ae48429711fce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369763"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Con Entity Framework 4.0 y el Control ObjectDataSource, parte 1: Introducción
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales se basa en la aplicación web de Contoso University creado por el [Introducción a Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie de tutoriales. Si no has completado los tutoriales anteriores, como un punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa.
> 
> La aplicación web de Contoso University muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. La aplicación de ejemplo es un sitio Web de una universidad ficticia, Contoso University. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.
> 
> El tutorial muestra ejemplos en C#. El [ejemplo descargable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contiene código en C# y Visual Basic.
> 
> ## <a name="database-first"></a>En primer lugar la base de datos
> 
> Hay tres maneras de trabajar con datos en Entity Framework: *Database First*, *Model First*, y *Code First*. Este tutorial es para la primera base de datos. Para obtener información sobre las diferencias entre estos flujos de trabajo e instrucciones sobre cómo elegir el mejor método para su escenario, consulte [flujos de trabajo de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>formularios Web Forms
> 
> Al igual que la serie de introducción, esta serie de tutoriales utiliza el modelo de formularios Web Forms de ASP.NET y se supone que sabe cómo trabajar con formularios Web Forms de ASP.NET en Visual Studio. Si no, ve [Introducción a ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si prefiere trabajar con el marco de MVC de ASP.NET, vea [Introducción a Entity Framework con ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express para Web. El tutorial no se ha probado con las versiones posteriores de Visual Studio. Existen muchas diferencias en las selecciones de menú, cuadros de diálogo y plantillas. |
> | .NET 4 | .NET 4.5 es compatible con .NET 4, pero el tutorial no se ha probado con .NET 4.5. |
> | Entity Framework 4 | El tutorial no se ha probado con las versiones posteriores de Entity Framework. A partir de Entity Framework 5, EF usa de forma predeterminada el `DbContext API` que se introdujo con EF 4.1. El control EntityDataSource se diseñó para usar el `ObjectContext` API. Para obtener información sobre cómo usar EntityDataSource control con el `DbContext` API, consulte [esta entrada de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Preguntas
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework y LINQ al foro de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).


El `EntityDataSource` control le permite crear una aplicación muy rápidamente, pero normalmente requiere mantener una cantidad significativa de lógica de negocios y la lógica de acceso a datos en su *.aspx* páginas. Si espera que su aplicación aumenta la complejidad y que requieren mantenimiento continuo, puede invertir más tiempo de desarrollo por adelantado con el fin de crear un *n niveles* o *superpuesto* estructura de la aplicación que es más fácil de mantener. Para implementar esta arquitectura, se separa la capa de presentación de la capa de lógica empresarial (BLL) y la capa de acceso a datos (DAL). Una forma de implementar esta estructura es usar el `ObjectDataSource` control en lugar de la `EntityDataSource` control. Cuando se usa el `ObjectDataSource` (control), implementar su propio código de acceso a datos y, a continuación, invóquelo en *.aspx* páginas mediante un control que tiene muchas de las mismas características que otros controles de origen de datos. Esto le permite combinar las ventajas de un enfoque de n niveles con las ventajas de usar un control de formularios Web Forms para el acceso a datos.

El `ObjectDataSource` control ofrece más flexibilidad en otras maneras. Puesto que escribir su propio código de acceso a datos, es más fácil para algo más que leer, insertar, actualizar o eliminar un tipo de entidad concreto, que son las tareas que el `EntityDataSource` control está diseñado para realizar. Por ejemplo, puede realizar el registro cada vez que se actualiza una entidad, archivar los datos cada vez que se elimina una entidad, o automáticamente comprobación y actualización de datos relacionados, según sea necesario cuando se inserta una fila con un valor de clave externa.

## <a name="business-logic-and-repository-classes"></a>Lógica de negocios y clases de repositorio

Un `ObjectDataSource` funciona mediante la invocación de una clase que crea el control. La clase incluye métodos que recuperen y actualicen los datos y proporcionar los nombres de los métodos a los `ObjectDataSource` control en el marcado. Durante la representación o procesamiento de la devolución, el `ObjectDataSource` llama a los métodos que ha especificado.

Además de operaciones CRUD básicas, la clase que se cree para usarlos con la `ObjectDataSource` control es posible que deba ejecutar lógica de negocios cuando el `ObjectDataSource` lee o actualiza datos. Por ejemplo, cuando se actualiza un departamento, es posible que deba validar que no hay otros departamentos tienen el mismo administrador dado que una persona no puede ser administrador del departamento de más de un.

En algunos `ObjectDataSource` documentación, como el [información general de clases de ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), el control llama a una clase que se conoce como un *objeto comercial* que incluye la lógica de negocios y lógica de acceso a datos . En este tutorial creará clases independientes para lógica de negocios y para la lógica de acceso a datos. La clase que encapsula la lógica de acceso a datos se denomina un *repositorio*. La clase de lógica de negocios incluye métodos de lógica de negocios y los métodos de acceso a datos, pero el repositorio para llevar a cabo tareas de acceso a datos de llamar a los métodos de acceso a datos.

También creará una capa de abstracción entre el BLL y DAL que facilita unitarias automatizadas las pruebas de la capa BLL. Esta capa de abstracción se implementa mediante la creación de una interfaz y mediante la interfaz cuando se crean instancias del repositorio en la clase de lógica de negocios. Esto permite proporcionar la clase de lógica de negocios con una referencia a cualquier objeto que implementa la interfaz del repositorio. Para el funcionamiento normal, proporcionar un objeto de repositorio que funciona con Entity Framework. Para las pruebas, proporcionar un objeto de repositorio que funciona con los datos almacenados de forma que se puede manipular fácilmente, como las variables de clase que se definen como colecciones.

La siguiente ilustración muestra la diferencia entre una clase de lógica de negocios que incluye la lógica de acceso a datos sin un repositorio y otra que usa un repositorio.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Se iniciará mediante la creación de páginas web en el que el `ObjectDataSource` control se enlaza directamente a un repositorio, ya que sólo realiza las tareas básicas de acceso a datos. En el siguiente tutorial creará una clase de lógica de negocios con lógica de validación y enlazar el `ObjectDataSource` control a esa clase en lugar de a la clase de repositorio. También creará pruebas unitarias para la lógica de validación. En el tercer tutorial de esta serie agregará ordenar y filtrar la funcionalidad a la aplicación.

Las páginas que crea en este tutorial funcionan con el `Departments` conjunto de entidades del modelo de datos que creó en el [serie de tutoriales de introducción a](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Actualización de la base de datos y el modelo de datos

Para empezar este tutorial, realizar dos cambios en la base de datos que requieren los cambios correspondientes en el modelo de datos que creó en el [Introducción a Entity Framework y formularios Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) tutoriales. En uno de estos tutoriales, ha realizado cambios en el diseñador manualmente para sincronizar el modelo de datos con la base de datos después de un cambio de base de datos. En este tutorial, usará el diseñador **actualizar modelo desde la base de datos** herramienta para actualizar automáticamente el modelo de datos.

### <a name="adding-a-relationship-to-the-database"></a>Agregar una relación a la base de datos

En Visual Studio, abra la aplicación web de Contoso University establecen que creó en el [Introducción a Entity Framework y formularios Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie de tutoriales y, a continuación, abra el `SchoolDiagram` diagrama de base de datos.

Si observa la `Department` tabla en el diagrama de base de datos, verá que tiene un `Administrator` columna. Esta columna es una clave externa para el `Person` tabla pero ninguna relación de clave externa se define en la base de datos. Deberá crear la relación y actualizar el modelo de datos para que Entity Framework puede controlar automáticamente esta relación.

En el diagrama de base de datos, haga clic en el `Department` de tabla y seleccione **relaciones**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

En el **relaciones de clave externa** clic en el cuadro **agregar**, a continuación, haga clic en el botón de puntos suspensivos para **especificación de tablas y columnas**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

En el **tablas y columnas** diálogo cuadro, establezca la tabla de clave principal y de campos para `Person` y `PersonID`y establezca la tabla de clave externa y el campo a `Department` y `Administrator`. (Al hacer esto, cambiará el nombre de la relación de `FK_Department_Department` a `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Haga clic en **Aceptar** en el **tablas y columnas** cuadro, haga clic en **cerrar** en el **relaciones de clave externa** cuadro y guarde los cambios. Si se le pregunta si desea guardar el `Person` y `Department` tablas, haga clic en **Sí**.

> [!NOTE]
> Si ha eliminado `Person` filas que corresponden a los datos que ya está en la `Administrator` columna, no podrá guardar este cambio. En ese caso, use el editor de tablas en **Explorador de servidores** para asegurarse de que el `Administrator` valor en cada `Department` fila contiene el identificador de un registro que existe realmente en el `Person` tabla.
> 
> Después de guardar el cambio, no podrá eliminar una fila de la `Person` si esa persona es un administrador del departamento de la tabla. En una aplicación de producción, proporcionaría un mensaje de error específico cuando una restricción de la base de datos evita una eliminación o se especificaría una eliminación en cascada. Para obtener un ejemplo de cómo especificar una eliminación en cascada, consulte [a Entity Framework y ASP.NET: Introducción a parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Agregar una vista a la base de datos

En el nuevo *Departments.aspx* página que se va a crear, desea proporcionar una lista desplegable de instructores, con los nombres de formato "apellido, nombre" para que los usuarios pueden seleccionar administradores de departamento. Para que sea más fácil de hacerlo, creará una vista en la base de datos. La vista consistirá simplemente los datos necesarios para la lista desplegable: el nombre completo (correctamente con formato) y la clave de registro.

En **Explorador de servidores**, expanda *School.mdf*, haga clic en el **vistas** carpeta y seleccione **agregar nueva vista**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Haga clic en **cerrar** cuando el **Agregar tabla** aparecerá el cuadro de diálogo y pegue la siguiente instrucción SQL en el panel SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Guardar la vista como `vInstructorName`.

### <a name="updating-the-data-model"></a>Actualizando el modelo de datos

En el *DAL* carpeta, abra el *SchoolModel.edmx* de archivos, haga clic en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

En el **elija los objetos de base de datos** cuadro de diálogo, seleccione el **agregar** pestaña y seleccione la vista que acaba de crear.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Haga clic en **Finalizar**.

En el diseñador, verá que la herramienta crea un `vInstructorName` entidad y una nueva asociación entre el `Department` y `Person` entidades.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> En el **salida** y **lista de errores** clave puede aparecer un mensaje de advertencia que le informa de que la herramienta crea automáticamente un elemento principal de windows para el nuevo `vInstructorName` vista. Este es el comportamiento normal.


Cuando se hace referencia a la nueva `vInstructorName` entidad en el código, no desea utilizar la convención de base de datos de un prefijo "v" en minúsculas a él. Por lo tanto, se cambiará el nombre de la entidad y un conjunto de entidades en el modelo.

Abra el **modelar Browser**. Verá `vInstructorName` aparece como un tipo de entidad y una vista.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

En **SchoolModel** (no **SchoolModel.Store**), haga clic en **vInstructorName** y seleccione **propiedades**. En el **propiedades** ventana, cambie el **nombre** propiedad a "InstructorName" y cambie el **nombre del conjunto de entidades** propiedad en "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Guarde y cierre el modelo de datos y, a continuación, recompile el proyecto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Uso de una clase de repositorio y un Control ObjectDataSource

Cree un nuevo archivo de clase en el *DAL* carpeta, asígnele el nombre *SchoolRepository.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Este código proporciona un único `GetDepartments` método que devuelve todas las entidades en el `Departments` conjunto de entidades. Dado que sabe que se accederá a la `Person` propiedad de navegación para cada fila devuelta, especificar la carga diligente de esa propiedad mediante el uso de la `Include` método. La clase también implementa el `IDisposable` interfaz para asegurarse de que la conexión de base de datos se libera cuando se desecha el objeto.

> [!NOTE]
> Una práctica común es crear una clase de repositorio para cada tipo de entidad. En este tutorial, se usa una clase de repositorio para varios tipos de entidad. Para obtener más información sobre el modelo de repositorio, consulte las publicaciones en [blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) y [blog de Julie](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


El `GetDepartments` método devuelve un `IEnumerable` objeto en lugar de un `IQueryable` objeto con el fin de asegurarse de que la colección devuelta es utilizable incluso después de desecha el propio objeto de repositorio. Un `IQueryable` objeto puede provocar el acceso de la base de datos cada vez que se tiene acceso a, pero se podría desechar el objeto de repositorio en el momento en que un control enlazado a datos que intenta presentar los datos. Podría devolver otro tipo de colección, como un `IList` en lugar del objeto un `IEnumerable` objeto. Sin embargo, devolviendo un `IEnumerable` objeto garantiza que puede realizar tareas de procesamiento de la lista típica de solo lectura, como `foreach` bucles y consultas LINQ, pero no se puede agregar a o quitar elementos de la colección, que parece indicar que dichos cambios sería se conservan en la base de datos.

Crear un *Departments.aspx* página que usa el *Site.Master* página principal y agregue el siguiente marcado en el `Content` control denominado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Este marcado crea un `ObjectDataSource` control que usa la clase de repositorio que acaba de crear, y un `GridView` control para mostrar los datos. El `GridView` control especifica **editar** y **eliminar** comandos, pero aún no ha agregado código para admitirlos aún.

Usan varias columnas `DynamicField` controles para que puede aprovechar la funcionalidad de formato y validación de datos automática. En estos casos para que funcione, debe llamar el `EnableDynamicData` método en el `Page_Init` controlador de eventos. (`DynamicControl` controles no se usan en el `Administrator` campo porque no trabajan con las propiedades de navegación.)

El `Vertical-Align="Top"` atributos cobrará importancia más adelante cuando se agrega una columna que tenga un anidada `GridView` control a la cuadrícula.

Abra el *Departments.aspx.cs* de archivo y agregue la siguiente `using` instrucción:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

A continuación, agregue el siguiente controlador de la página `Init` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

En el *DAL* carpeta, cree un nuevo archivo de clase denominado *Department.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Este código agrega metadatos al modelo de datos. Especifica que el `Budget` propiedad de la `Department` entidad representa realmente moneda aunque su tipo de datos es `Decimal`, y especifica que el valor debe estar entre 0 y 1.000.000,00 $. También especifica que el `StartDate` propiedad se debe dar formato como una fecha en formato mm/dd/aaaa.

Ejecute el *Departments.aspx* página.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Tenga en cuenta que, aunque no se especificó una cadena de formato en el *Departments.aspx* marcado de página para el **presupuesto** o **Start Date** predeterminado de columnas, moneda y fecha se ha aplicado formato a ellos mediante el `DynamicField` controles, utilizar los metadatos que se proporcionan en el *Department.cs* archivo.

## <a name="adding-insert-and-delete-functionality"></a>Agregar Insert y la funcionalidad de eliminación

Abra *SchoolRepository.cs*, agregue el código siguiente para crear un `Insert` método y un `Delete` método. El código también incluye un método denominado `GenerateDepartmentID` que calcula el siguiente valor de clave de registro disponibles para su uso por el `Insert` método. Esto es necesario porque la base de datos no está configurado para calcular automáticamente para el `Department` tabla.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>El método de conexión

El `DeleteDepartment` llamadas al método el `Attach` método con el fin de volver a establecer el vínculo que se mantiene en el Administrador de estado de objeto del contexto de objetos entre la entidad en la memoria y la base de datos de fila representa. Esto debe ocurrir antes de las llamadas al método el `SaveChanges` método.

El término *contexto del objeto* hace referencia a la clase de Entity Framework que se deriva el `ObjectContext` clase que usas para acceder a los conjuntos de entidades y entidades. En el código para este proyecto, la clase se denomina `SchoolEntities`, y siempre se denomina una instancia de ella `context`. El contexto de objeto *Administrador de estado de objeto* es una clase que deriva la `ObjectStateManager` clase. El contacto de objeto usa el Administrador de estado de objeto para almacenar objetos de entidad y realizar un seguimiento de si cada uno de ellos está sincronizado con su fila correspondiente de la tabla o las filas de la base de datos.

Al leer una entidad, el contexto del objeto almacena en el Administrador de estado de objeto y realiza un seguimiento de si dicha representación del objeto está sincronizada con la base de datos. Por ejemplo, si cambia un valor de propiedad, se establece una marca para indicar que la propiedad que ha cambiado ya no está sincronizada con la base de datos. A continuación, cuando se llama a la `SaveChanges` método, el contexto del objeto sabe qué hacer en la base de datos porque el Administrador de estado de objeto sabe exactamente cuál es la diferencia entre el estado actual de la entidad y el estado de la base de datos.

Sin embargo, este proceso normalmente no funciona en una aplicación web, porque la instancia de contexto del objeto que lee una entidad, junto con todo el contenido de su administrador de estado de objeto, se elimina después de representa una página. La instancia de contexto del objeto que se debe aplicar los cambios es una nueva que se crea una instancia para el procesamiento de devolución de datos. En el caso de los `DeleteDepartment` método, el `ObjectDataSource` control crea volver a la versión original de la entidad a partir de los valores de estado de vista, pero esto vuelve a crea `Department` entidad no existe en el Administrador de estado de objeto. Si llama a la `DeleteObject` método en esta entidad volver a crearse, la llamada produciría un error porque el contexto del objeto no sabe si la entidad está sincronizada con la base de datos. Sin embargo, una llamada a la `Attach` método restablece el seguimiento mismo entre la entidad que volver a crear y los valores en la base de datos que originalmente se hacía automáticamente cuando se lee la entidad en una instancia del contexto del objeto anterior.

Hay veces cuando no desea que el contexto del objeto para realizar el seguimiento de las entidades en el Administrador de estado de objeto, y puede establecer marcas para evitar que lo haga. En otros tutoriales de esta serie se muestran ejemplos de este.

### <a name="the-savechanges-method"></a>El método SaveChanges

Esta clase de repositorio simple ilustra los principios básicos de cómo realizar operaciones CRUD. En este ejemplo, el `SaveChanges` se llama al método inmediatamente después de cada actualización. En una aplicación de producción que desee llamar a la `SaveChanges` desde un método independiente para proporcionarle más control sobre cuándo se actualiza la base de datos. (Al final del tutorial siguiente encontrará un vínculo a notas del producto que se explica en la unidad del patrón de trabajo que es un enfoque para coordinar las actualizaciones relacionadas.) Observe también que en el ejemplo, el `DeleteDepartment` método no incluye el código para controlar los conflictos de simultaneidad, se agregará código para hacerlo en un tutorial más adelante en esta serie.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Recuperar los nombres de Instructor para seleccionar al insertar

Los usuarios deben poder seleccionar un administrador de una lista de instructores en una lista desplegable al crear nuevos departamentos. Por lo tanto, agregue el código siguiente al *SchoolRepository.cs* para crear un método para recuperar la lista de instructores con la vista que creó anteriormente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Creación de una página para insertar los departamentos de TI

Crear un *DepartmentsAdd.aspx* página que usa el *Site.Master* página y agregue el siguiente marcado en el `Content` control denominado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Este marcado crea dos `ObjectDataSource` controla, uno para insertar nuevos `Department` entidades y otra para recuperar los nombres de instructor para el `DropDownList` control que se utiliza para seleccionar los administradores de departamento. El marcado crea un `DetailsView` controlar para escribir nuevos departamentos y especifica un controlador para el control `ItemInserting` eventos para que pueda establecer el `Administrator` valor de clave externa. Al final es un `ValidationSummary` control para mostrar los mensajes de error.

Abra *DepartmentsAdd.aspx.cs* y agregue la siguiente `using` instrucción:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Agregue la siguiente variable de clase y métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

El `Page_Init` método habilita la funcionalidad de los datos dinámicos. El controlador para el `DropDownList` del control `Init` evento guarda una referencia al control y el controlador para el `DetailsView` del control `Inserting` evento utiliza esa referencia para obtener la `PersonID` valor de la actualización y el instructor seleccionado el `Administrator` propiedad de clave externa de la `Department` entidad.

Ejecute la página, agregue información de un nuevo departamento y, a continuación, haga clic en el **insertar** vínculo.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Escriba valores para otro departamento nuevo. Escriba un número mayor que 1.000.000,00 en el **presupuesto** campo y tab para el campo siguiente. Aparece un asterisco en el campo y, si mantiene el puntero del mouse sobre él, puede ver el mensaje de error que ha especificado en los metadatos para ese campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Haga clic en **insertar**, y verá el mensaje de error mostrado por el `ValidationSummary` control en la parte inferior de la página.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

A continuación, cierre el explorador y abra el *Departments.aspx* página. Agregar funcionalidad de eliminación la *Departments.aspx* página mediante la adición de un `DeleteMethod` atributo a la `ObjectDataSource` control y un `DataKeyNames` atributo a la `GridView` control. Las etiquetas de apertura para estos controles asemejará ahora en el ejemplo siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Ejecute la página.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Elimine el departamento que agregó cuando ejecutó el *DepartmentsAdd.aspx* página.

## <a name="adding-update-functionality"></a>Agregar funcionalidad de actualización

Abra *SchoolRepository.cs* y agregue la siguiente `Update` método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Al hacer clic en **actualización** en el *Departments.aspx* página, el `ObjectDataSource` control crea dos `Department` entidades que se va a pasar a la `UpdateDepartment` método. Uno contiene los valores originales que se han almacenado en el estado de vista y el otro contiene los nuevos valores que se especificaron en el `GridView` control. El código en el `UpdateDepartment` método pasa el `Department` entidad que tiene los valores originales para el `Attach` método con el fin de establecer el seguimiento entre la entidad y lo que está en la base de datos. A continuación, el código pasa el `Department` entidad que tiene los nuevos valores para el `ApplyCurrentValues` método. El contexto del objeto compara los valores antiguos y nuevos. Si un nuevo valor es diferente de un valor anterior, el contexto del objeto cambia el valor de propiedad. El `SaveChanges` método, a continuación, actualiza sólo las columnas cambiadas en la base de datos. (Sin embargo, si la función de actualización para esta entidad se asigna a un procedimiento almacenado, toda la fila se actualizaría con independencia de qué columnas se modificaron.)

Abra el *Departments.aspx* archivo y agregue los siguientes atributos para el `DepartmentsObjectDataSource` control:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Este hace que los valores anteriores para ser almacenan en la vista estado para que pueden compararse con los nuevos valores en el `Update` método.
- `OldValuesParameterFormatString="orig{0}"`   
 Esto informa al control que es el nombre del parámetro de valores original `origDepartment` .

El marcado para la etiqueta de apertura del `ObjectDataSource` control ahora es similar al siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Agregar un `OnRowUpdating="DepartmentsGridView_RowUpdating"` atributo a la `GridView` control. Se usará para establecer el `Administrator` valor de propiedad en función de la fila que el usuario selecciona en una lista desplegable. El `GridView` etiqueta de apertura ahora es similar al siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Agregar un `EditItemTemplate` controlar para el `Administrator` columna a la `GridView` controlar, inmediatamente después del `ItemTemplate` control para esa columna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Esto `EditItemTemplate` control es similar a la `InsertItemTemplate` en controlar la *DepartmentsAdd.aspx* página. La diferencia es que el valor inicial del control se establece utilizando el `SelectedValue` atributo.

Antes de la `GridView` control, agregue un `ValidationSummary` controlar como hizo en el *DepartmentsAdd.aspx* página.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Abra *Departments.aspx.cs* e inmediatamente después de la declaración de clase parcial, agregue el código siguiente para crear un campo privado para hacer referencia a la `DropDownList` control:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

A continuación, agregue controladores para la `DropDownList` del control `Init` eventos y el `GridView` del control `RowUpdating` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

El controlador para el `Init` evento guarda una referencia a la `DropDownList` control en el campo de clase. El controlador para el `RowUpdating` evento usa la referencia para obtener el valor especificado por el usuario y se aplicará la `Administrator` propiedad de la `Department` entidad.

Use la *DepartmentsAdd.aspx* página para agregar un nuevo departamento, a continuación, ejecute el *Departments.aspx* página y haga clic en **editar** en la fila que ha agregado.

> [!NOTE]
> No podrá editar las filas que no se han agregado (es decir, que ya estaban en la base de datos), debido a los datos no válidos en la base de datos; los administradores de las filas que se crearon con la base de datos son los estudiantes. Si se intenta editar uno de ellos, obtendrá una página de error que informa de un error similar a `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Si introduce un no válido **presupuesto** amount y, a continuación, haga clic en **actualización**, verá el mismo asterisco y un mensaje de error que apareció en el *Departments.aspx* página.

Cambiar un valor de campo o seleccione un administrador diferente y haga clic en **actualización**. Se muestra el cambio.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Con esto finaliza la introducción al uso de la `ObjectDataSource` control para básica CRUD (crear, leer, actualizar y eliminar) operaciones con Entity Framework. Ha creado una sencilla aplicación de n niveles, pero la capa de lógica de negocios se sigue estrechamente a la capa de acceso a datos, lo que complica las pruebas unitarias automatizadas. En el siguiente tutorial verá cómo implementar el patrón de repositorio para facilitar las pruebas unitarias.

> [!div class="step-by-step"]
> [Siguiente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
