---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Advanced escenarios de Entity Framework para una aplicación Web MVC (10 de 10) | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: d58a745896b29317c1d1049e3bf1a5ec2e628820
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Escenarios de avanzada de Entity Framework para una aplicación Web MVC (10 de 10)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empiece aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, vea [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior implementa el repositorio y una unidad de patrones de trabajo. Este tutorial trata los temas siguientes:

- Realización de consultas SQL sin formato.
- Realizar consultas de seguimiento no.
- Examen de las consultas se envía a la base de datos.
- Trabajar con clases de proxy.
- Deshabilitar la detección automática de los cambios.
- Cómo deshabilitar la validación al guardar los cambios.
- [Errores y alternativas de trabajo](#errors)

Para la mayoría de estos temas, trabajará con páginas que se han crean previamente. Para usar SQL sin formato para realizar actualizaciones masivas creará una nueva página que actualiza el número de créditos de todos los cursos en la base de datos:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Y utilizar una consulta de seguimiento no que se agregará la nueva lógica de validación a la página Editar departamento:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Realizar consultas SQL sin formato

La API First de Entity Framework código incluye métodos que permiten pasar comandos SQL directamente a la base de datos. Tiene las siguientes opciones:

- Use la `DbSet.SqlQuery` método para las consultas que devuelven tipos de entidad. Los objetos devueltos deben ser del tipo esperado por la `DbSet` objeto y se realiza automáticamente un seguimiento por el contexto de base de datos a menos que desactive seguimiento. (Consulte la sección siguiente la `AsNoTracking` método.)
- Use la `Database.SqlQuery` método para las consultas que devuelven tipos que no son entidades. No realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si utiliza este método para recuperar tipos de entidad.
- Use la [Database.ExecuteSqlCommand](https://msdn.microsoft.com/en-us/library/gg679456(v=vs.103).aspx) para comandos de consulta no.

Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado ajustado a un método concreto de almacenamiento de datos. Hace esto mediante la generación de consultas SQL y comandos para usted, que también se evita tener que escribir usted mismo. Pero hay situaciones excepcionales cuando se necesita ejecutar consultas específicas de SQL que ha creado manualmente, y estos métodos permiten controlar dichas excepciones.

Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques de inyección de SQL. Una manera de hacerlo es utilizar consultas parametrizadas para asegurarse de que no se puede interpretar cadenas enviadas por una página web que los comandos SQL. En este tutorial usará las consultas con parámetros cuando se integra proporcionados por el usuario en una consulta.

### <a name="calling-a-query-that-returns-entities"></a>Llamar a una consulta que devuelve las entidades

Suponga que desea la `GenericRepository` clase para proporcionar funciones adicionales de filtrado y ordenación flexibilidad sin necesidad de crear una clase derivada con métodos adicionales. Una forma de lograr que sería agregar un método que acepta una consulta SQL. Puede especificar cualquier tipo de filtro u ordenación desea en el controlador, como un `Where` cláusula que depende de una de las combinaciones o una subconsulta. En esta sección verá cómo implementar este tipo de método.

Crear el `GetWithRawSql` método agregando el código siguiente a *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

En *CourseController.cs*, llame al método nuevo desde el `Details` método, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

En este caso podría haber utilizado el `GetByID` método, pero se está usando el `GetWithRawSql` método para comprobar que la `GetWithRawSQL` funciona de método.

Ejecute la página de detalles para comprobar que la instrucción select consulta works (seleccione la **curso** ficha y, a continuación, **detalles** para un curso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Llamar a una consulta que devuelve otros tipos de objetos

Anteriormente, se crea una cuadrícula de estadísticas de estudiante de la página acerca de que el número de alumnos se ha explicado para cada fecha de inscripción. El código que lo haga en *HomeController.cs* utiliza LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Suponga que desea escribir el código que permita recuperar estos datos directamente en SQL, en lugar de usar LINQ. Para ello necesita ejecutar una consulta que devuelve un valor distinto de objetos entidad, lo que significa que debe usar el `Database.SqlQuery` método.

En *HomeController.cs*, reemplace la instrucción LINQ en el `About` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Ejecute la página acerca de. Muestra los mismos datos que lo hacía anteriormente.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Llamar a una consulta Update

Imagine que desean que los administradores de la Universidad de Contoso poder realizar cambios masivos en la base de datos, como cambiar el número de créditos para cada curso. Si la Universidad tiene un gran número de cursos, sería ineficaz para recuperarlos todos como entidades y cambiarlos de forma individual. En esta sección implementaremos una página web que permite al usuario especificar un factor por el que se va a cambiar el número de créditos para todos los cursos y podrá realizar el cambio mediante la ejecución de una instancia de SQL `UPDATE` instrucción. La página web tendrá un aspecto similar de la ilustración siguiente:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

En el tutorial anterior, ha utilizado el repositorio genérico para leer y actualizar `Course` entidades en el `Course` controlador. Para esta operación de actualización masiva, debe crear un nuevo método de repositorio que no se encuentra en el repositorio genérico. Para ello, creará una dedicado `CourseRepository` clase que deriva de la `GenericRepository` clase.

En el *DAL* carpeta, crear *CourseRepository.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

En *UnitOfWork.cs*, cambie la `Course` tipo de repositorio de `GenericRepository<Course>` a`CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

En *CourseContoller.cs*, agregue un `UpdateCourseCredits` método:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Este método se utilizará para ambos `HttpGet` y `HttpPost`. Cuando el `HttpGet` `UpdateCourseCredits` método se ejecuta, el `multiplier` variable será null y la vista mostrará un cuadro de texto vacío y un botón de envío, tal como se muestra en la ilustración anterior.

Cuando el **actualización** se hace clic en el botón y el `HttpPost` método se ejecuta, `multiplier` tendrá el valor especificado en el cuadro de texto. El código, a continuación, llama al repositorio `UpdateCourseCredits` método, que devuelve el número de filas afectadas, y ese valor se almacena en la `ViewBag` objeto. Cuando la vista recibe el número de filas afectadas en la `ViewBag` del objeto, muestra ese número en lugar del cuadro de texto y enviar botón, tal como se muestra en la siguiente ilustración:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Crear una vista en la *Views\Course* carpeta para la página de actualización créditos del curso:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

En *Views\Course\UpdateCourseCredits.cshtml*, reemplace el código de plantilla con el código siguiente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Ejecute el `UpdateCourseCredits` método seleccionando la **cursos** ficha, a continuación, agregar "/ UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Escriba un número en el cuadro de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Haga clic en **Actualizar**. Ver el número de filas afectadas:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Haga clic en **volver a la lista** para ver la lista de cursos con el número de créditos revisado.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obtener más información acerca de las consultas SQL sin formato, vea [sin procesar consultas SQL](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) en el blog del equipo de Entity Framework.

## <a name="no-tracking-queries"></a>Consultas de seguimiento no

Cuando un contexto de base de datos recupera las filas de la base de datos y crea objetos de entidad que tienen tipos de archivo, de forma predeterminada realiza un seguimiento de si las entidades en memoria están sincronizadas con el contenido de la base de datos. Los datos en memoria actúa como una memoria caché y se utilizan cuando se actualiza una entidad. Este almacenamiento en caché a menudo es necesario en una aplicación web porque el contexto de instancias son normalmente de corta duración (un nuevo objeto uno se crea y se elimina para cada solicitud) y el contexto que lee una entidad normalmente se elimina antes de esa entidad se vuelve a usar.

Puede especificar si el contexto realiza un seguimiento de los objetos de entidad para una consulta mediante el uso de la `AsNoTracking` método. En el que se puede hacer que los escenarios típicos son los siguientes:

- La consulta recupera un gran volumen de datos que pueden mejorar considerablemente el rendimiento al desactivar el seguimiento.
- Que desea asociar una entidad para actualizarla, pero anteriormente se recupera la misma entidad para un propósito diferente. Dado que la entidad ya está sometida a seguimiento por el contexto de base de datos, no se puede adjuntar la entidad que desea cambiar. Una manera de evitar que esto suceda es usar el `AsNoTracking` opción con la consulta anterior.

En esta sección podrá implementar lógica de negocios que se muestra al segundo de estos escenarios. En concreto, podrá aplicar una regla de negocios que dice que un instructor no puede ser el Administrador de más de un departamento.

En *DepartmentController.cs*, agregar un nuevo método que se puede llamar desde el `Edit` y `Create` métodos para asegurarse de que no hay dos departamentos tienen el mismo administrador:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Agregue código en el `try` bloquear de la `HttpPost` `Edit` método para llamar a este método nuevo si no hay ningún error de validación. El `try` bloque ahora es similar al ejemplo siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Ejecute la página Editar departamento e intente cambiar el administrador del departamento a un instructor que ya es el Administrador de un departamento diferente. Obtiene el mensaje de error esperado:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ahora ejecute volver a la página Editar departamento y este cambio de hora la **presupuesto** cantidad. Al hacer clic en **guardar**, verá una página de error:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

El mensaje de error de excepción es "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Esto ha ocurrido debido a la siguiente secuencia de eventos:

- El `Edit` llamadas al método el `ValidateOneAdministratorAssignmentPerInstructor` método, que recupera todos los departamentos que tengan Kim Abercrombie como su administrador. Que hace que el departamento de inglés que debe leerse. Dado que es el departamento que se está editando, no se notifica ningún error. Como resultado de esta operación de lectura, sin embargo, la entidad de departamento inglés que se leyó desde la base de datos está ahora sometida a seguimiento por el contexto de base de datos.
- El `Edit` método intenta establecer el `Modified` marca en inglés entidad department creado por el enlazador de modelos MVC, pero eso no es posible porque el contexto ya está realizando un seguimiento de una entidad para el departamento de inglés.

Una solución a este problema consiste en mantener el contexto de un seguimiento de las entidades de departamento en memoria recuperadas por la consulta de validación. No hay ningún inconveniente haga esto, porque no puede actualizar esta entidad o que lo lean de nuevo de forma que se beneficiaría de la que se está almacenando en caché en memoria.

En *DepartmentController.cs*, en la `ValidateOneAdministratorAssignmentPerInstructor` (método), no especificar ningún seguimiento, tal y como se muestra en la siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Repita el intento de editar el **presupuesto** cantidad de un departamento. Este tiempo de la operación es correcta y el sitio devuelve previsto que vaya a la página de índice de departamentos, que muestra el valor de presupuesto revisado.

## <a name="examining-queries-sent-to-the-database"></a>Examen de las consultas enviadas a la base de datos

A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos. Para ello, puede examinar una variable de consulta en el depurador o llamar a la consulta `ToString` método. Para probar esto, podrá ver una consulta simple y, a continuación, examine qué ocurre con los a medida que agregue opciones tal diligente cargar, filtrado y ordenación.

En *controladores/CourseController*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Ahora establecer un punto de interrupción en *GenericRepository.cs* en el `return query.ToList();` y `return orderBy(query).ToList();` las instrucciones de la `Get` método. Ejecute el proyecto en modo de depuración y seleccione la página de índice de curso. Cuando el código alcanza el punto de interrupción, examine el `query` variable. Vea la consulta que se envía a SQL Server. Es una sencilla `Select` instrucción:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Las consultas pueden ser demasiado largos para mostrarse en las ventanas de depuración en Visual Studio. Para ver toda la consulta, puede copiar el valor de la variable y péguelo en un editor de texto:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Ahora agregará una lista desplegable a la página de índice de curso para que los usuarios pueden filtrar para un departamento concreto. Ordenará los cursos por título, y especificará carga diligente de la `Department` propiedad de navegación. En *CourseController.cs*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

El método recibe el valor seleccionado de la lista desplegable en el `SelectedDepartment` parámetro. Si se selecciona nada, este parámetro será null.

Un `SelectList` colección que contiene todos los departamentos se pasa a la vista de la lista desplegable. Los parámetros se pasan a la `SelectList` constructor especifica el nombre del campo de valor, el nombre del campo de texto y el elemento seleccionado.

Para el `Get` método de la `Course` repositorio, el código especifica una expresión de filtro, un criterio de ordenación y diligente para cargar la `Department` propiedad de navegación. Devuelve la expresión de filtro siempre `true` si se selecciona nada en la lista desplegable (es decir, `SelectedDepartment` es null).

En *Views\Course\Index.cshtml*, inmediatamente antes de la apertura `table` etiqueta, agregue el código siguiente para crear la lista desplegable y un botón de envío:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Con los puntos de interrupción sigue establecidos el `GenericRepository` (clase), ejecute la página de índice de curso. Continúe con las dos primeras horas que el código alcanza un punto de interrupción para que la página se muestre en el explorador. Seleccione un departamento en la lista desplegable y haga clic en **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Este tiempo será el primer punto de interrupción de la lista desplegable de la consulta de departamentos. Omitir que y ver el `query` variable la próxima vez que el código alcanza el punto de interrupción para ver qué la `Course` consultar ahora parece que. Verá algo parecido a lo siguiente:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Puede ver que la consulta es ahora un `JOIN` consulta que carga `Department` datos junto con el `Course` datos y que incluye un `WHERE` cláusula.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Trabajar con clases de Proxy

Cuando Entity Framework crea instancias de la entidad (por ejemplo, cuando se ejecuta una consulta), a menudo crea como instancias de un tipo derivado generada dinámicamente que actúa como un proxy para la entidad. Este proxy reemplaza algunas propiedades virtuales de la entidad que se va a insertar enlaces para realizar automáticamente acciones cuando se tiene acceso a la propiedad. Por ejemplo, este mecanismo se usa para admitir la carga diferida de las relaciones.

La mayoría de las veces no es necesario tener en cuenta este uso de los servidores proxy, pero hay excepciones:

- En algunos escenarios puede evitar que Entity Framework puedan crear instancias de proxy. Por ejemplo, serializar instancias de proxy no podría ser más eficaz que serializar instancias de proxy.
- Cuando cree instancias de una clase de entidad con la `new` , no obtiene una instancia del proxy. Esto significa que no aparece la funcionalidad, como el seguimiento de cambios automático y la carga diferida. Esto suele ser muy bien; por lo general no necesita la carga diferida, porque va a crear una nueva entidad que no se encuentra en la base de datos, y generalmente no es necesario si va a marcar explícitamente la entidad como el seguimiento de cambios `Added`. Sin embargo, si necesita la carga diferida y necesita seguimiento de cambios, puede crear nuevas instancias de entidad con servidores proxy mediante la `Create` método de la `DbSet` clase.
- Puede obtener un tipo de entidad real de un tipo de proxy. Puede usar el `GetObjectType` método de la `ObjectContext` clase para obtener el tipo de entidad real de una instancia de tipo de proxy.

Para obtener más información, consulte [trabajar con servidores proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) en el blog del equipo de Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Deshabilitar la detección automática de cambios

Entity Framework determina cómo ha cambiado una entidad (y, por tanto, las actualizaciones que necesitan para enviarse a la base de datos) mediante la comparación de los valores actuales de una entidad con los valores originales. Los valores originales se almacenan si la entidad se consultan o adjunta. Algunos de los métodos que provocan la detección de cambios automático son los siguientes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si está realizando el seguimiento de un gran número de entidades y se llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas desactivando temporalmente detección de cambios automático mediante el [AutoDetectChangesEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) propiedad. Para obtener más información, consulte [detectar cambios automáticamente](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Cómo deshabilitar la validación al guardar los cambios

Cuando se llama a la `SaveChanges` método, de forma predeterminada, Entity Framework valida los datos de todas las propiedades de todas las entidades modificadas antes de actualizar la base de datos. Si ha actualizado un gran número de entidades y ya ha validado los datos, este trabajo no es necesario y podría hacer que el proceso de guardar los cambios tardan menos tiempo temporalmente si se desactiva la validación. Puede hacer que el uso de la [ValidateOnSaveEnabled](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) propiedad. Para obtener más información, consulte [validación](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Resumen

Con esto finaliza la siguiente serie de tutoriales sobre el uso de Entity Framework en una aplicación ASP.NET MVC. Vínculos a otros recursos de Entity Framework pueden encontrarse en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obtener más información acerca de cómo implementar la aplicación web después de que haya creado, vea [ASP.NET Deployment Content Map](https://msdn.microsoft.com/en-us/library/bb386521.aspx) en MSDN Library.

Para obtener información acerca de otros temas relacionados con MVC, como la autenticación y autorización, consulte la [MVC recomienda recursos](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Confirmaciones

- Tom Dykstra la versión original de este tutorial se escribió y es un sistema de escritura de programación senior en el equipo de contenido de herramientas y plataforma Web de Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) coautor de este tutorial y ha sido la mayoría del trabajo de actualización para EF 5 y MVC 4. Rick es un escritor de programación senior de Microsoft que se centran en Azure y MVC.
- [Rowan Miller](http://www.romiller.com) y otros miembros del equipo de Entity Framework asistida con revisiones de código y ayudaron a muchos problemas de las migraciones que se produjo mientras se estábamos actualizando el tutorial de EF 5 de depuración.

## <a name="vb"></a>VB

Cuando se generó originalmente el tutorial, se proporcionan versiones de C# y VB del proyecto haya finalizado la descarga. Con esta actualización proporcionamos un proyecto descargable C# para cada capítulo para que sea más fácil empezar a trabajar en cualquier parte de la serie, pero debido a las limitaciones de tiempo y otras prioridades se no hizo que para VB. Si compila un proyecto VB con estos tutoriales y ¿estaría dispuesto a compartir con otras personas, háganoslo saber.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Errores y soluciones alternativas

### <a name="cannot-createshadow-copy"></a>No se puede crear/ocultación el copia

Mensaje de error:

*No se puede crear/ocultación copia 'DotNetOpenAuth.OpenId' cuando ese archivo ya existe.*

Solución:

Espere unos segundos y actualice la página.

### <a name="update-database-not-recognized"></a>Actualizar base de datos no reconocida

Mensaje de error:

*El término 'Update-Database' no se reconoce como el nombre de un cmdlet, función, archivo de script o un programa ejecutable. Compruebe la ortografía del nombre, o si incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta e inténtelo de nuevo.* (Desde el  *`Update-Database`*  comando el PMC.)

Solución:

Salga de Visual Studio. Vuelva a Abrir proyecto y vuelva a intentarlo.

### <a name="validation-failed"></a>Error de validación

Mensaje de error:

*Error de validación de una o más entidades. Vea la propiedad 'EntityValidationErrors' para obtener más detalles.* (Desde el  *`Update-Database`*  comando el PMC.)

Solución:

Una causa de este problema es que los errores de validación cuando el `Seed` ejecuciones de método. Vea [Seeding y bases de datos de depuración de Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obtener sugerencias sobre cómo depurar el `Seed` método.

### <a name="http-50019-error"></a>HTTP 500.19 error

Mensaje de error:

*No se puede tener acceso a HTTP Error 500.19 - Error interno del servidor la página solicitada porque los datos de configuración relacionados de la página no están válidos.*

Solución:

Una manera que se puede obtener este error es de varias copias de la solución, cada uno de ellos con el mismo número de puerto. Normalmente puede resolver este problema, salir de todas las instancias de Visual Studio y luego reiniciar el proyecto de su trabajo en. Si esto no funciona, pruebe a cambiar el número de puerto. Haga clic con el botón secundario en el archivo de proyecto y, a continuación, haga clic en Propiedades. Seleccione el **Web** ficha y, a continuación, cambie el número de puerto en el **dirección Url del proyecto** cuadro de texto.

### <a name="error-locating-sql-server-instance"></a>Instancia de SQL Server de localización de error

Mensaje de error:

*Se ha producido un error relacionado con la red o específico de la instancia al establecer una conexión a SQL Server. No se encontró el servidor o éste no estaba accesible. Compruebe que el nombre de instancia es correcto y que SQL Server está configurado para permitir conexiones remotas. (proveedor: Interfaces de red SQL, error: 26 - Error Buscar servidor/instancia especificado)*

Solución:

Compruebe la cadena de conexión. Si se ha eliminado manualmente la base de datos, cambiar el nombre de la base de datos en la cadena de construcción.

>[!div class="step-by-step"]
[Anterior](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
[Siguiente](building-the-ef5-mvc4-chapter-downloads.md)
