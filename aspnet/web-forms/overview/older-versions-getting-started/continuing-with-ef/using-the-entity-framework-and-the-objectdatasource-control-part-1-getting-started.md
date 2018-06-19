---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Con Entity Framework 4.0 y el Control ObjectDataSource, parte 1: Introducción | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web de la Universidad de Contoso que se crea mediante la introducción a la serie de tutoriales de Entity Framework. Si yo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 6584767418c898913777b3b1549a816679c8430d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892087"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Con Entity Framework 4.0 y el Control ObjectDataSource, parte 1: Introducción
====================
por [Tom Dykstra](https://github.com/tdykstra)

> Esta serie de tutoriales que se basa en la aplicación web de la Universidad de Contoso que se crea mediante la [Getting Started with the Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie de tutoriales. Si no has completado los tutoriales anteriores, como punto de partida para este tutorial puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que puede haberla creado. También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) creado por la serie de tutoriales completa.
> 
> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de formularios Web Forms de ASP.NET con el Entity Framework 4.0 y Visual Studio 2010. La aplicación de ejemplo es un sitio Web de una universidad ficticia de Contoso. Incluye funciones como la admisión de estudiantes, la creación de cursos y asignaciones de instructores.
> 
> El tutorial muestra ejemplos en C#. El [ejemplo descargable](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) contiene código en C# y Visual Basic.
> 
> ## <a name="database-first"></a>En primer lugar la base de datos
> 
> Hay tres formas de trabajar con datos en Entity Framework: *Database First*, *Model First*, y *Code First*. Este tutorial es para la primera base de datos. Para obtener información sobre las diferencias entre estos flujos de trabajo e instrucciones sobre cómo elegir la mejor para su escenario, consulte [flujos de trabajo de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>formularios Web Forms
> 
> Al igual que la serie Getting Started, esta serie de tutoriales utiliza el modelo de formularios Web Forms de ASP.NET y se supone que sabe cómo trabajar con formularios Web Forms de ASP.NET en Visual Studio. Si no, ve [Introducción a ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Si prefiere trabajar con el marco de MVC de ASP.NET, consulte [Introducción a Entity Framework mediante ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express para Web. El tutorial no se ha probado con las versiones posteriores de Visual Studio. Existen muchas diferencias en las selecciones de menús, cuadros de diálogo y las plantillas. |
> | .NET 4 | .NET 4.5 es compatible con .NET 4, pero no se ha probado el tutorial con .NET 4.5. |
> | Entity Framework 4 | El tutorial no se ha probado con las versiones posteriores de Entity Framework. A partir de Entity Framework 5, EF usa de forma predeterminada el `DbContext API` que se introdujo con EF 4.1. El control EntityDataSource se diseñó para usar el `ObjectContext` API. Para obtener información sobre cómo usar el EntityDataSource control con el `DbContext` API, consulte [esta entrada de blog](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Preguntas
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET Entity Framework](https://forums.asp.net/1227.aspx), [Entity Framework y LINQ to foro de entidades](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), o [ StackOverflow.com](http://stackoverflow.com/).


El `EntityDataSource` control le permite crear rápidamente una aplicación, pero normalmente requiere mantener una gran cantidad de lógica de negocios y la lógica de acceso a datos en su *.aspx* páginas. Si espera que la aplicación que se va a aumentar en complejidad como requieren mantenimiento continuado, puede invertir más tiempo de desarrollo por adelantado para crear un *con n niveles* o *superpuesto* estructura de aplicación que es más fácil de mantener. Para implementar esta arquitectura, se separa la capa de presentación de la capa de lógica empresarial (BLL) y la capa de acceso a datos (DAL). Una manera de implementar esta estructura es usar el `ObjectDataSource` controlar en lugar de la `EntityDataSource` control. Cuando se usa el `ObjectDataSource` (control), que implementa su propio código de acceso a datos y, a continuación, invóquelo en *.aspx* páginas mediante un control que tiene muchas de estas mismas características que otros controles de origen de datos. Esto le permite combinar las ventajas de un enfoque de n niveles con las ventajas de usar un control de formularios Web Forms para acceso a datos.

El `ObjectDataSource` control proporciona una mayor flexibilidad de otras maneras. Puesto que escribir su propio código de acceso a datos, es más fácil algo más que simplemente leer, insertar, actualizar o eliminar un tipo de entidad concreto, que se indican las tareas que el `EntityDataSource` control está diseñado para realizar. Por ejemplo, puede realizar el registro cada vez que se actualiza una entidad, archivar datos cada vez que se elimina una entidad, o automáticamente comprobación y actualizar los datos relacionados con según sea necesario al insertar una fila con un valor de clave externa.

## <a name="business-logic-and-repository-classes"></a>Lógica de negocios y clases de repositorio

Un `ObjectDataSource` funciona el control mediante la invocación de una clase que cree. La clase incluye métodos que permiten recuperar y actualizarán los datos y proporcionar los nombres de los métodos para el `ObjectDataSource` control en el marcado. Durante la representación o procesamiento de devolución de datos, la `ObjectDataSource` llama a los métodos que ha especificado.

Además de las operaciones CRUD básicas, la clase que se cree para usarlos con el `ObjectDataSource` control posible que deba ejecutar lógica de negocios cuando la `ObjectDataSource` lee o actualiza datos. Por ejemplo, cuando se actualiza un departamento, deberá comprobar que no hay otros departamentos tienen el mismo administrador porque una persona no puede ser administrador del departamento de más de un.

En algunos `ObjectDataSource` documentación, como el [general sobre la clase ObjectDataSource](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), el control llama a una clase que se conoce como un *objeto comercial* que incluye lógica de negocios y la lógica de acceso a datos . En este tutorial creará clases independientes para la lógica de negocios y para la lógica de acceso a datos. La clase que encapsula la lógica de acceso a datos se denomina un *repositorio*. La clase de la lógica de negocios incluye métodos de lógica de negocios y los métodos de acceso a datos, pero el repositorio para llevar a cabo tareas de acceso a datos de llamar a los métodos de acceso a datos.

También creará una capa de abstracción entre el BLL y DAL que facilita unitarias automatizadas pruebas de la capa BLL. Esta capa de abstracción se implementa mediante la creación de una interfaz y mediante la interfaz cuando se crea una instancia del repositorio en la clase de la lógica de negocios. Esto posibilita la proporciona la clase de la lógica de negocios con una referencia a cualquier objeto que implementa la interfaz del repositorio. Para un funcionamiento normal, es proporcionar un objeto de repositorio que funciona con Entity Framework. Para las pruebas, proporcionar un objeto de repositorio que funciona con datos almacenados en una forma que se puede manipular fácilmente, como las variables de clase definidas como colecciones.

En la siguiente ilustración muestra la diferencia entre una clase de lógica de negocios que incluye la lógica de acceso a datos sin un repositorio y otro que usa un repositorio.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Se iniciará mediante la creación de páginas web en el que el `ObjectDataSource` control se enlaza directamente a un repositorio, ya que solo realiza tareas de acceso a datos básica. En el siguiente tutorial creará una clase de la lógica de negocios con lógica de validación y enlazar el `ObjectDataSource` control a esa clase en lugar de a la clase de repositorio. También creará pruebas unitarias para la lógica de validación. En el tercer tutorial de esta serie agregará ordenar y filtrar la funcionalidad a la aplicación.

Las páginas que se creará en este tutorial funcionan con el `Departments` conjunto de entidades del modelo de datos que creó en el [serie de tutoriales de introducción a](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Actualización de la base de datos y el modelo de datos

Puede empezar este tutorial si se realizan dos cambios en la base de datos que requieren los cambios correspondientes en el modelo de datos que creó en el [Introducción a Entity Framework como en formularios Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) tutoriales. En uno de los tutoriales, ha realizado cambios en el diseñador manualmente para sincronizar el modelo de datos con la base de datos después de un cambio de base de datos. En este tutorial, utilizará el diseñador **actualizar modelo desde la base de datos** herramienta para actualizar automáticamente el modelo de datos.

### <a name="adding-a-relationship-to-the-database"></a>Agregar una relación a la base de datos

En Visual Studio, abra la aplicación web de la Universidad de Contoso que creó en el [Introducción a Entity Framework como en formularios Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) serie de tutoriales y, a continuación, abra el `SchoolDiagram` diagrama de base de datos.

Si observa la `Department` tabla en el diagrama de base de datos, verá que tiene un `Administrator` columna. Esta columna es una clave externa a la `Person` tabla pero ninguna relación de clave externa se define en la base de datos. Debe crear la relación y actualizar el modelo de datos para que Entity Framework puede controlar automáticamente esta relación.

En el diagrama de base de datos, haga clic en el `Department` de tabla y seleccione **relaciones**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

En el **relaciones de clave externa** cuadro haga clic en **agregar**, a continuación, haga clic en el botón de puntos suspensivos para **especificaciones de tablas y columnas**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

En el **tablas y columnas** diálogo cuadro, establezca la tabla de clave principal y de campos para `Person` y `PersonID`y establece la tabla de clave externa y campo a `Department` y `Administrator`. (Si hace esto, cambiará el nombre de la relación de `FK_Department_Department` a `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Haga clic en **Aceptar** en el **tablas y columnas** cuadro, haga clic en **cerrar** en el **relaciones de clave externa** cuadro y guarde los cambios. Si se le pregunta si desea guardar la `Person` y `Department` tablas, haga clic en **Sí**.

> [!NOTE]
> Si se ha eliminado `Person` filas que corresponden a los datos que ya está en la `Administrator` columna, no podrá guardar este cambio. En ese caso, use el editor de tablas en **Explorador de servidores** para asegurarse de que el `Administrator` valor en cada `Department` fila contiene el identificador de un registro que existe realmente en el `Person` tabla.
> 
> Después de guardar el cambio, no podrá eliminar una fila de la `Person` si esa persona es un administrador del departamento de la tabla. En una aplicación de producción, proporcionaría un mensaje de error específico cuando una restricción de la base de datos impide que una operación de eliminación, o se debe especificar una eliminación en cascada. Para obtener un ejemplo de cómo especificar una eliminación en cascada, consulte [Entity Framework y ASP.NET: Introducción iniciado parte 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Agregar una vista a la base de datos

En el nuevo *Departments.aspx* página que va a crear, desea proporcionar una lista desplegable de instructores, con los nombres de formato "apellido, nombre" para que los usuarios pueden seleccionar los administradores de departamento. Para que sea más fácil de hacerlo, creará una vista en la base de datos. La vista se compondrá de solamente los datos necesarios para la lista desplegable: el nombre completo (correctamente con formato) y la clave de registro.

En **Explorador de servidores**, expanda *School.mdf*, haga clic en el **vistas** carpeta y seleccione **agregar nueva vista**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Haga clic en **cerrar** cuando el **Agregar tabla** aparecerá el cuadro de diálogo y pegue la siguiente instrucción SQL en el panel SQL:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Guarde la vista como `vInstructorName`.

### <a name="updating-the-data-model"></a>Si actualiza el modelo de datos

En el *DAL* carpeta, abra el *SchoolModel.edmx* de archivos, haga clic en la superficie de diseño y seleccione **actualizar modelo desde base de datos**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

En el **elija los objetos de base de datos** cuadro de diálogo, seleccione la **agregar** pestaña y seleccione la vista que acaba de crear.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Haga clic en **Finalizar**.

En el diseñador, verá que la herramienta crea un `vInstructorName` entidad y una nueva asociación entre la `Department` y `Person` entidades.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> En el **salida** y **lista de errores** de teclas de windows puede aparecer un mensaje de advertencia que le informa de que la herramienta crea automáticamente un elemento principal para el nuevo `vInstructorName` vista. Este es el comportamiento normal.


Cuando haga referencia a la nueva `vInstructorName` entidad en el código, no desea que use la convención de base de datos de un prefijo de una "v" minúscula a él. Por lo tanto, se cambiará el nombre de la entidad y un conjunto de entidades en el modelo.

Abra la **modelo explorador**. Verá `vInstructorName` aparece como un tipo de entidad y una vista.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

En **SchoolModel** (no **SchoolModel.Store**), haga clic en **vInstructorName** y seleccione **propiedades**. En el **propiedades** ventana, cambiar la **nombre** propiedad "InstructorName" y cambiar el **establecer nombre de entidad** propiedad a "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Guarde y cierre el modelo de datos y, a continuación, volver a generar el proyecto.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Usar una clase de repositorio y un Control ObjectDataSource

Crear un nuevo archivo de clase en el *DAL* carpeta, asígnele el nombre *SchoolRepository.cs*y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Este código proporciona un único `GetDepartments` método que devuelve todas las entidades en el `Departments` conjunto de entidades. Dado que sabe que va a tener acceso el `Person` propiedad de navegación para cada fila devuelta, especificar diligente cargar para esa propiedad mediante la `Include` método. La clase también implementa el `IDisposable` interfaz para asegurarse de que se libera la conexión de base de datos cuando se elimina el objeto.

> [!NOTE]
> Una práctica común consiste en crear una clase de repositorio para cada tipo de entidad. En este tutorial, se usa una clase de repositorio para varios tipos de entidad. Para obtener más información sobre el modelo de repositorio, vea las entradas en [blog del equipo de Entity Framework](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) y [blog de Julie Lerman](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


El `GetDepartments` método devuelve un `IEnumerable` objeto en lugar de una `IQueryable` objeto con el fin de asegurarse de que la colección devuelta es utilizable incluso después de que se elimine el propio objeto de repositorio. Un `IQueryable` objeto puede provocar el acceso de la base de datos cada vez que se tiene acceso, pero podría ser eliminado el objeto de repositorio en el momento en que un control de enlace de datos intenta presentar los datos. Podría devolver otro tipo de colección, como un `IList` en lugar del objeto un `IEnumerable` objeto. Sin embargo, devolver un `IEnumerable` objeto garantiza que puede realizar tareas de procesamiento de la lista de solo lectura típico como `foreach` bucles y las consultas LINQ, pero no se puede agregar o quitar elementos en la colección, que podría implicar que dichos cambios sería se conservan en la base de datos.

Crear un *Departments.aspx* página que usa el *Site.Master* página principal y agregue el siguiente marcado en el `Content` control denominado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Este marcado crea un `ObjectDataSource` control que usa la clase de repositorio que acaba de crear, y un `GridView` control para mostrar los datos. El `GridView` control especifica **editar** y **eliminar** comandos, pero no agregó código para admitirlas todavía.

Usan varias columnas `DynamicField` controla para que puede aprovechar la funcionalidad de formato y validación automática de los datos. En estos casos para que funcione, tendrá que llamar a la `EnableDynamicData` método en el `Page_Init` controlador de eventos. (`DynamicControl` controles no se utilizan en los `Administrator` campo porque no trabajan con propiedades de navegación.)

El `Vertical-Align="Top"` atributos cobrará importancia más adelante cuando se agrega una columna que tenga una anidada `GridView` control a la cuadrícula.

Abra la *Departments.aspx.cs* de archivos y agregue las siguientes `using` instrucción:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

A continuación, agregue el siguiente controlador de la página `Init` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

En el *DAL* carpeta, cree un nuevo archivo de clase denominado *Department.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Este código agrega metadatos al modelo de datos. Especifica que la `Budget` propiedad de la `Department` entidad representa realmente moneda aunque su tipo de datos es `Decimal`, y especifica que el valor debe estar entre 0 y 1.000.000,00 $. También especifica que el `StartDate` propiedad debe tener el formato como una fecha en formato mm/dd/aaaa.

Ejecute el *Departments.aspx* página.

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Tenga en cuenta que, aunque no se especificó una cadena de formato en el *Departments.aspx* marcado de la página para la **presupuesto** o **Start Date** predeterminado de columnas, moneda y fecha formato se ha aplicado a ellos mediante el `DynamicField` controla, usando los metadatos que proporcionan en el *Department.cs* archivo.

## <a name="adding-insert-and-delete-functionality"></a>Agregar Insert y la funcionalidad de eliminación

Abra *SchoolRepository.cs*, agregue el código siguiente para crear un `Insert` método y un `Delete` método. El código también incluye un método denominado `GenerateDepartmentID` que calcula el siguiente valor de clave de registro disponibles para su uso por el `Insert` método. Esto es necesario porque la base de datos no está configurado para calcular automáticamente para el `Department` tabla.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>El Attach (método)

El `DeleteDepartment` llamadas al método el `Attach` método con el fin de volver a establecer el vínculo que se mantiene en el Administrador de estado de objeto del contexto de objeto entre la entidad en la memoria y la base de datos de fila representa. Esto debe ocurrir antes de las llamadas al método el `SaveChanges` método.

El término *contexto del objeto* hace referencia a la clase de Entity Framework que se deriva de la `ObjectContext` clase que usa para tener acceso a los conjuntos de entidades y entidades. En el código para este proyecto, la clase se denomina `SchoolEntities`, y una instancia de dicho siempre se denomina `context`. El contexto de objeto *Administrador de estado de objeto* es una clase que deriva de la `ObjectStateManager` clase. El contacto de objeto usa el Administrador de estado de objeto para almacenar objetos de entidad y realizar un seguimiento de si cada uno de ellos esté sincronizado con su fila correspondiente de la tabla o las filas en la base de datos.

Al leer una entidad, el contexto del objeto almacena en el Administrador de estado de objeto y realiza un seguimiento de si esa representación del objeto está sincronizada con la base de datos. Por ejemplo, si cambia un valor de propiedad, se establece una marca para indicar que la propiedad que ha cambiado ya no está sincronizada con la base de datos. A continuación, cuando se llama a la `SaveChanges` método, el contexto del objeto sabe qué hacer en la base de datos porque el Administrador de estado de objeto sabe exactamente lo que es diferente entre el estado actual de la entidad y el estado de la base de datos.

Sin embargo, este proceso normalmente no funciona en una aplicación web, porque la instancia de contexto de objeto que lee una entidad, junto con todo el contenido de su administrador de estado de objeto, se elimina una vez que se representa una página. Instancia del objeto de contexto que debe aplicar los cambios es uno nuevo que se crea una instancia para el procesamiento de devolución de datos. En el caso de la `DeleteDepartment` método, el `ObjectDataSource` control crea volver a la versión original de la entidad a partir de valores de estado de vista, pero esto recreando `Department` entidad no existe en el Administrador de estado de objeto. Si llama a la `DeleteObject` método en esta entidad vuelve a crear, la llamada produciría un error porque el contexto del objeto no sabe si la entidad está sincronizada con la base de datos. Sin embargo, al llamar a la `Attach` método restablece el mismo seguimiento entre la entidad vuelve a crear y los valores en la base de datos que originalmente se hacía automáticamente cuando la entidad se leyó en una instancia anterior del contexto del objeto.

Hay momentos en que no desea que el contexto del objeto para realizar el seguimiento de las entidades en el Administrador de estado de objeto, y puede establecer marcas para evitar que lo hace. Se muestran ejemplos de esto en tutoriales posteriores de esta serie.

### <a name="the-savechanges-method"></a>El método SaveChanges

Esta clase de repositorio simple muestra los principios básicos de cómo realizar operaciones CRUD. En este ejemplo, el `SaveChanges` método se llama inmediatamente después de cada actualización. En una aplicación de producción puede llamar a la `SaveChanges` desde un método independiente para proporcionarle más control sobre cuándo se actualiza la base de datos. (Al final del tutorial siguiente encontrará un vínculo a notas del producto que se explica en la unidad de patrón de trabajo que es un enfoque para coordinar las actualizaciones relacionadas.) Observe también que en el ejemplo, el `DeleteDepartment` método no incluye código para controlar conflictos de simultaneidad; se agregará código para hacerlo en un tutorial posterior de esta serie.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Recuperar nombres de Instructor para seleccionar al insertar

Los usuarios deben poder seleccionar un administrador de una lista de instructores en una lista desplegable al crear nuevos departamentos. Por lo tanto, agregue el código siguiente a *SchoolRepository.cs* para crear un método para recuperar la lista de instructores mediante la vista que creó anteriormente:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Crear una página para insertar departamentos

Crear un *DepartmentsAdd.aspx* página que usa el *Site.Master* página y agregue el siguiente marcado en el `Content` control denominado `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Este marcado crea dos `ObjectDataSource` controla, uno para insertar nuevos `Department` entidades y otra para recuperar los nombres de instructor para el `DropDownList` control que se utiliza para seleccionar los administradores de departamento. El marcado crea un `DetailsView` controlar para nuevos departamentos y, si se especifica un controlador para el control `ItemInserting` eventos para que pueda establecer el `Administrator` valor de clave externa. Al final es un `ValidationSummary` control para mostrar mensajes de error.

Abra *DepartmentsAdd.aspx.cs* y agregue el siguiente `using` instrucción:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Agregue la siguiente variable de clase y métodos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

El `Page_Init` método habilita la funcionalidad de datos dinámicos. El controlador de la `DropDownList` del control `Init` evento guarda una referencia al control y el controlador para el `DetailsView` del control `Inserting` evento usa esa referencia para obtener la `PersonID` valor del instructor seleccionado y actualice el `Administrator` propiedad de clave externa de la `Department` entidad.

Ejecute la página, agregue información de un departamento de nuevo y, a continuación, haga clic en el **insertar** vínculo.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Especifique valores para otro departamento nuevo. Escriba un número mayor que 1.000.000,00 en la **presupuesto** campo y ficha al campo siguiente. Aparece un asterisco en el campo y, si mantiene el puntero del mouse sobre él, puede ver el mensaje de error que ha especificado en los metadatos para ese campo.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Haga clic en **insertar**, y verá el mensaje de error mostrado por el `ValidationSummary` control en la parte inferior de la página.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

A continuación, cierre el explorador y abra el *Departments.aspx* página. Agregar capacidad de eliminar el *Departments.aspx* página mediante la adición de un `DeleteMethod` atribuir a la `ObjectDataSource` (control) y un `DataKeyNames` atribuir a la `GridView` control. Las etiquetas de apertura de estos controles ahora parecerán al ejemplo siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Ejecute la página.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Eliminar el departamento agregó cuando ejecutó el *DepartmentsAdd.aspx* página.

## <a name="adding-update-functionality"></a>Agregar funcionalidad de actualización

Abra *SchoolRepository.cs* y agregue el siguiente `Update` método:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Al hacer clic en **actualización** en el *Departments.aspx* página, el `ObjectDataSource` control crea dos `Department` entidades para pasar a la `UpdateDepartment` método. Uno contiene los valores originales que se han almacenado en estado de vista y el otro contiene los nuevos valores que se especificaron en el `GridView` control. El código en el `UpdateDepartment` método pasa el `Department` entidad que tiene los valores originales para el `Attach` método para establecer el seguimiento entre la entidad y lo que aparece en la base de datos. A continuación, el código pasa la `Department` entidad que tiene los nuevos valores para el `ApplyCurrentValues` (método). El contexto del objeto compara los valores antiguos y nuevos. Si un nuevo valor es diferente de un valor anterior, el contexto del objeto cambia el valor de propiedad. El `SaveChanges` método, a continuación, actualiza sólo las columnas cambiadas en la base de datos. (Sin embargo, si la función de actualización para esta entidad se asigna a un procedimiento almacenado, toda la fila se actualizaría con independencia de qué columnas se modificaron.)

Abra la *Departments.aspx* de archivos y agregue los siguientes atributos para el `DepartmentsObjectDataSource` control:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Este causas los valores anteriores para ser almacenan en estado de vista para que pueden compararse con los nuevos valores en el `Update` método.
- `OldValuesParameterFormatString="orig{0}"`   
 Esto informa al control que es el nombre del parámetro de valores originales `origDepartment` .

El marcado para la etiqueta de apertura de la `ObjectDataSource` control ahora es similar al siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Agregar un `OnRowUpdating="DepartmentsGridView_RowUpdating"` atribuir a la `GridView` control. Se utilizará para establecer el `Administrator` valor de propiedad basándose en la fila que el usuario selecciona de una lista desplegable. La `GridView` etiqueta de apertura ahora es similar al siguiente:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Agregar un `EditItemTemplate` controlar para la `Administrator` columna a la `GridView` controlar, inmediatamente después del `ItemTemplate` control para esa columna:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Esto `EditItemTemplate` control es similar a la `InsertItemTemplate` controlar en el *DepartmentsAdd.aspx* página. La diferencia es que el valor inicial del control se establece utilizando la `SelectedValue` atributo.

Antes de la `GridView` de control, agregue un `ValidationSummary` controlar como hizo en el *DepartmentsAdd.aspx* página.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Abra *Departments.aspx.cs* e inmediatamente después de la declaración de clase parcial, agregue el código siguiente para crear un campo privado para hacer referencia a la `DropDownList` control:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

A continuación, agregar controladores para la `DropDownList` del control `Init` eventos y la `GridView` del control `RowUpdating` eventos:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

El controlador para el `Init` evento guarda una referencia a la `DropDownList` control en el campo de clase. El controlador para el `RowUpdating` evento usa la referencia para obtener el valor especificado por el usuario y aplicarlos a la `Administrator` propiedad de la `Department` entidad.

Use la *DepartmentsAdd.aspx* página para agregar un nuevo departamento y vuelva a ejecutar la *Departments.aspx* página y haga clic en **editar** en la fila que ha agregado.

> [!NOTE]
> No podrá editar las filas que no se agregó (es decir, que ya estaban en la base de datos), debido a los datos no válidos en la base de datos; los administradores de las filas que se crearon con la base de datos son los alumnos. Si se intenta modificar uno de ellos, obtendrá una página de error que informa de un error como `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Si escribe un válido **presupuesto** importe y, a continuación, haga clic en **actualización**, verá el mismo asterisco y el mensaje de error que se vio en el *Departments.aspx* página.

Cambiar un valor de campo o seleccione un administrador diferente y haga clic en **actualización**. Se muestra el cambio.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Con esto finaliza la introducción al uso de la `ObjectDataSource` control para básica CRUD (crear, leer, actualizar y eliminar) operaciones con Entity Framework. Crear una sencilla aplicación de n niveles, pero la capa de lógica de negocios se sigue estrechamente a la capa de acceso a datos, lo que complica pruebas unitarias automatizadas. En el tutorial siguiente verá cómo implementar el modelo de repositorio para facilitar la prueba unitaria.

> [!div class="step-by-step"]
> [Siguiente](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
