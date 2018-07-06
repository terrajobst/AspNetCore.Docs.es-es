---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Advanced escenarios de Entity Framework para una aplicación Web MVC (10 de 10) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5a75d85140a40660314ab267fdd74a8058d791fc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832680"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>Escenarios de opciones avanzadas de Entity Framework para una aplicación Web MVC (10 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio de este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y comience aquí.
> 
> > [!NOTE] 
> > 
> > Si surge un problema que no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior implementa el repositorio y unidad de patrones de trabajo. En este tutorial se trata los temas siguientes:

- Realización de consultas SQL sin procesar.
- Realización de consultas de no realizar un seguimiento.
- Examinar las consultas se envía a la base de datos.
- Trabajar con clases de proxy.
- Deshabilitar la detección automática de los cambios.
- Deshabilitar la validación al guardar los cambios.
- [Errores y soluciones](#errors)

Para la mayoría de estos temas, trabajará con las páginas que ya ha creado. Para usar SQL sin procesar para realizar actualizaciones masivas creará una nueva página que actualiza el número de créditos de todos los cursos de la base de datos:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

Y para usar una consulta de seguimiento no agregará la nueva lógica de validación a la página Edit Department:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>Realizar consultas SQL sin formato

La API de Entity Framework Code First incluye métodos que permiten pasar comandos SQL directamente a la base de datos. Tiene las siguientes opciones:

- Use el método `DbSet.SqlQuery` para las consultas que devuelven tipos de entidad. Los objetos devueltos deben ser del tipo esperado por la `DbSet` objeto y se realiza el seguimiento automático del contexto de base de datos a menos que desactive el seguimiento. (Consulte la sección siguiente la `AsNoTracking` método.)
- Use el `Database.SqlQuery` método para las consultas que devuelven tipos que no son entidades. No se realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si usa este método para recuperar tipos de entidad.
- Use la [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx) para comandos sin consulta.

Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado estrechamente a un método concreto de almacenamiento de datos. Lo consigue mediante la generación de consultas SQL y comandos, lo que también le evita tener que escribirlos usted mismo. Pero hay situaciones excepcionales cuando se necesita ejecutar consultas específicas de SQL que ha creado manualmente, y estos métodos permiten controlar dichas excepciones.

Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques por inyección de código SQL. Una manera de hacerlo es mediante consultas parametrizadas para asegurarse de que las cadenas enviadas por una página web no se pueden interpretar como comandos SQL. En este tutorial usará las consultas con parámetros al integrar la entrada de usuario en una consulta.

### <a name="calling-a-query-that-returns-entities"></a>Llamar a una consulta que devuelve las entidades

Suponga que desea la `GenericRepository` clase para proporcionar funciones adicionales de filtrado y ordenación flexibilidad sin necesidad de crear una clase derivada con métodos adicionales. Una manera de hacerlo sería agregar un método que acepta una consulta SQL. A continuación, puede especificar cualquier tipo de filtrado u ordenación desea en el controlador, como un `Where` cláusula que depende un combinaciones o una subconsulta. En esta sección verá cómo implementar este tipo de método.

Crear el `GetWithRawSql` método agregando el código siguiente al *GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

En *CourseController.cs*, llamar al método nuevo desde el `Details` método, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

En este caso se podría haber usado el `GetByID` método, pero se usa el `GetWithRawSql` método para comprobar que la `GetWithRawSQL` funciona el método.

Ejecute la página de detalles para comprobar que las consultas select (seleccionar la **curso** ficha y, a continuación, **detalles** para un curso).

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Llamar a una consulta que devuelve otros tipos de objetos

Anteriormente creó una cuadrícula de estadísticas de alumno de la página About que mostraba el número de alumnos para cada fecha de inscripción. El código que hace esto en *HomeController.cs* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

Suponga que desea escribir el código que recupera estos datos directamente en SQL, en lugar de mediante LINQ. Para ello, necesita ejecutar una consulta que devuelve un valor distinto de objetos entidad, lo que significa deba usar el `Database.SqlQuery` método.

En *HomeController.cs*, reemplace la instrucción LINQ en el `About` método con el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Ejecute la página About. Muestra los mismos datos que anteriormente.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>Llamar a una consulta Update

Supongamos que los administradores de Contoso University quieren realizar cambios masivos en la base de datos, como cambiar el número de créditos para cada curso. Si la universidad tiene un gran número de cursos, sería poco eficaz recuperarlos todos como entidades y cambiarlos de forma individual. En esta sección implementará una página web que permite al usuario especificar un factor por el que se va a cambiar el número de créditos para todos los cursos y podrá realizar el cambio mediante la ejecución de una instancia de SQL `UPDATE` instrucción. La página web tendrá el mismo aspecto que la ilustración siguiente:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

En el tutorial anterior, ha utilizado el repositorio genérico para leer y actualizar `Course` entidades en el `Course` controlador. Para esta operación de actualización masiva, deberá crear un nuevo método de repositorio que no se encuentra en el repositorio genérico. Para ello, creará una dedicado `CourseRepository` clase que deriva la `GenericRepository` clase.

En el *DAL* carpeta, cree *CourseRepository.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

En *UnitOfWork.cs*, cambie el `Course` tipo de repositorio de `GenericRepository<Course>` a `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

En *CourseContoller.cs*, agregue un `UpdateCourseCredits` método:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Este método se utilizará para ambos `HttpGet` y `HttpPost`. Cuando el `HttpGet` `UpdateCourseCredits` ejecuciones de método, el `multiplier` variable será null y la vista mostrará un cuadro de texto vacío y un botón de envío, tal como se muestra en la ilustración anterior.

Cuando el **actualización** se hace clic en el botón y el `HttpPost` método ejecuciones, `multiplier` tendrá el valor especificado en el cuadro de texto. El código llama a continuación, el repositorio `UpdateCourseCredits` método, que devuelve el número de filas afectadas, y ese valor se almacena en la `ViewBag` objeto. Cuando la vista recibe el número de filas afectadas en la `ViewBag` de objeto, muestra ese número en lugar del cuadro de texto y enviar el botón, como se muestra en la siguiente ilustración:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

Crear una vista en el *Views\Course* carpeta para la página Update Course Credits:

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

En *Views\Course\UpdateCourseCredits.cshtml*, reemplace el código de plantilla con el código siguiente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Ejecute el método `UpdateCourseCredits` seleccionando la pestaña **Courses**, después, agregue "/UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Escriba un número en el cuadro de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Haga clic en **Actualizar**. Verá el número de filas afectadas:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Haga clic en **Volver a la lista** para ver la lista de cursos con el número de créditos revisado.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obtener más información sobre consultas SQL sin formato, vea [consultas SQL sin procesar](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx) en el blog del equipo de Entity Framework.

## <a name="no-tracking-queries"></a>consultas de no seguimiento

Cuando un contexto de base de datos recupera filas de la base de datos y crea objetos de entidad que representan a ellos, de forma predeterminada realiza el seguimiento de si las entidades en memoria están sincronizadas con el contenido de la base de datos. Los datos en memoria actúan como una caché y se usan cuando se actualiza una entidad. Este almacenamiento en caché suele ser necesario en una aplicación web porque las instancias de contexto normalmente son de corta duración (para cada solicitud se crea una y se elimina) y el contexto que lee una entidad normalmente se elimina antes de volver a usar esa entidad.

Puede especificar si el contexto realiza un seguimiento de los objetos de entidad para una consulta mediante el `AsNoTracking` método. Los siguientes son escenarios típicos en los que es posible que quiera hacer esto:

- La consulta recupera un gran volumen de datos que pueden mejorar considerablemente el rendimiento al desactivar el seguimiento.
- Desea adjuntar una entidad para actualizarla, pero que recuperó anteriormente la misma entidad para un propósito diferente. Como el contexto de base de datos ya está realizando el seguimiento de la entidad, no se puede adjuntar la entidad que se quiere cambiar. Una manera de evitar que esto suceda es usar el `AsNoTracking` opción con la consulta anterior.

En esta sección se implementa lógica de negocios que se muestra al segundo de estos escenarios. En concreto, deberá aplicar una regla de negocios que dice que un instructor no puede ser el Administrador de más de un departamento.

En *DepartmentController.cs*, agregue un nuevo método que se puede llamar desde el `Edit` y `Create` métodos para asegurarse de que no hay dos departamentos tienen el mismo administrador:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

Agregue código en el `try` bloquear de la `HttpPost` `Edit` método se llama a este nuevo método si no hay ningún error de validación. El `try` bloque es ahora similar al ejemplo siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

Ejecute la página Edit Department y vuelva a cambiar el Administrador de un departamento a un instructor que ya es el Administrador de un departamento diferente. Obtenga el mensaje de error esperado:

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ahora ejecute volver a la página Edit Department y este cambio de horario de la **presupuesto** cantidad. Al hacer clic en **guardar**, verá una página de error:

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

El mensaje de error de excepción es "`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`" Esto sucedió debido a la siguiente secuencia de eventos:

- El `Edit` llamadas al método el `ValidateOneAdministratorAssignmentPerInstructor` método, que recupera todos los departamentos que tengan Kim Abercrombie como su administrador. Que hace que el departamento de inglés a leerse. Dado que es el departamento que se está editando, se notifica ningún error. Como resultado de esta operación de lectura, sin embargo, la entidad de departamento de inglés que se leyó desde la base de datos está ahora realizando el seguimiento del contexto de base de datos.
- El `Edit` método intenta establecer el `Modified` marca en el inglés entidad department creada por el enlazador de modelo MVC, pero se produce un error porque el contexto ya está realizando el seguimiento de una entidad del departamento de inglés.

Una solución a este problema es mantener el contexto de seguimiento de las entidades de departamento en memoria recuperadas por la consulta de validación. No hay ningún inconveniente de hacerlo, porque no se puede actualizar esta entidad o leerlo nuevo de forma que se beneficiaría de los que se almacenen en caché en memoria.

En *DepartmentController.cs*, en el `ValidateOneAdministratorAssignmentPerInstructor` método, no especificar ningún seguimiento, como se muestra en la siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

Repita el intento de modificar el **presupuesto** cantidad de un departamento. Esta vez la operación es correcta y el sitio devuelve según lo previsto en la página de índice de Departments, que muestra el valor de presupuesto revisado.

## <a name="examining-queries-sent-to-the-database"></a>Examen de las consultas enviadas a la base de datos

A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos. Para ello, puede examinar una variable de consulta en el depurador o llamar a la consulta `ToString` método. Para probar esto, examinará una consulta simple y, a continuación, ver lo que ocurre al mismo a medida que agrega opciones tal diligente cargar, filtrado y ordenación.

En *controladores/CourseController*, reemplace el `Index` método con el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

Ahora establezca un punto de interrupción *GenericRepository.cs* en el `return query.ToList();` y `return orderBy(query).ToList();` instrucciones de la `Get` método. Ejecute el proyecto en modo de depuración y seleccione la página de índice del curso. Cuando el código alcanza el punto de interrupción, examinar el `query` variable. Vea la consulta que se envía a SQL Server. Es una sencilla `Select` instrucción:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

Las consultas pueden ser demasiado largos para mostrarse en las ventanas de depuración en Visual Studio. Para ver toda la consulta, puede copiar el valor de la variable y péguelo en un editor de texto:

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

Ahora agregará una lista desplegable a la página de índice de curso para que los usuarios pueden filtrar para un departamento determinado. Los cursos se ordena por título, y especificará la carga diligente para la `Department` propiedad de navegación. En *CourseController.cs*, reemplace el `Index` método con el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

El método recibe el valor seleccionado de la lista desplegable en el `SelectedDepartment` parámetro. Si se selecciona nada, este parámetro será nulo.

Un `SelectList` colección que contiene todos los departamentos se pasa a la vista de la lista desplegable. Los parámetros pasados a la `SelectList` constructor especifica el nombre del campo de valor, el nombre del campo de texto y el elemento seleccionado.

Para el `Get` método de la `Course` repositorio, el código especifica una expresión de filtro, un criterio de ordenación y la carga diligente para la `Department` propiedad de navegación. Devuelve la expresión de filtro siempre `true` si se selecciona nada en la lista desplegable (es decir, `SelectedDepartment` es null).

En *Views\Course\Index.cshtml*, inmediatamente antes de la apertura `table` etiqueta, agregue el código siguiente para crear la lista desplegable y un botón de envío:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

Con los puntos de interrupción sigue establecidos el `GenericRepository` (clase), ejecute la página de índice del curso. Continúe con los dos primeros tiempos de que el código alcanza un punto de interrupción para que se muestre la página en el explorador. Seleccionar un departamento de la lista desplegable y haga clic en **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Esta vez será el primer punto de interrupción de la lista desplegable de la consulta de los departamentos de TI. Omitir ese paso y ver el `query` variable la próxima vez que el código alcanza el punto de interrupción con el fin de ver qué la `Course` ahora el aspecto de la consulta. Verá algo parecido a lo siguiente:

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

Puede ver que la consulta es ahora un `JOIN` consulta que carga `Department` datos junto con el `Course` datos y que incluye un `WHERE` cláusula.

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>Trabajar con clases de Proxy

Cuando Entity Framework crea instancias de entidad (por ejemplo, cuando se ejecuta una consulta), a menudo crea como instancias de un tipo derivado generada dinámicamente que actúa como un proxy para la entidad. Este proxy reemplaza algunas propiedades de la entidad que se va a insertar enlaces para realizar automáticamente acciones cuando se tiene acceso a la propiedad virtuales. Por ejemplo, este mecanismo se utiliza para admitir la carga diferida de relaciones.

La mayoría de los casos no es necesario tener en cuenta este uso de los servidores proxy, pero hay excepciones:

- En algunos escenarios puede impedir que Entity Framework creen instancias de proxy. Por ejemplo, serializar instancias de proxy no podría ser más eficaz que serializar instancias de proxy.
- Al crear instancias de una clase de entidad usando el `new` , no obtiene una instancia del proxy. Esto significa que no obtendrá una funcionalidad, como el seguimiento de cambios automático y la carga diferida. Esto normalmente no importa; por lo general no se necesita la carga diferida, porque va a crear una nueva entidad que no se encuentra en la base de datos y, por lo general no es necesario si va a marcar explícitamente la entidad como de seguimiento de cambios `Added`. Sin embargo, si necesita la carga diferida y necesita seguimiento de cambios, puede crear nuevas instancias de entidad con servidores proxy mediante la `Create` método de la `DbSet` clase.
- Es posible que desee obtener un tipo de entidad real de un tipo de proxy. Puede usar el `GetObjectType` método de la `ObjectContext` clase para obtener el tipo de entidad real de una instancia del tipo de proxy.

Para obtener más información, consulte [trabajar con servidores proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) en el blog del equipo de Entity Framework.

## <a name="disabling-automatic-detection-of-changes"></a>Deshabilitar la detección automática de cambios

Entity Framework determina cómo ha cambiado una entidad (y, por tanto, las actualizaciones que hay que enviar a la base de datos) comparando los valores actuales de una entidad con los valores originales. Cuando se consulta o se adjunta la entidad, se almacenan los valores originales. Algunos de los métodos que provocan la detección de cambios automática son los siguientes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si realiza un seguimiento de un gran número de entidades y llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas si desactiva la detección de cambios automático mediante temporalmente el [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx) propiedad. Para obtener más información, consulte [detectar cambios automáticamente](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx).

## <a name="disabling-validation-when-saving-changes"></a>Deshabilitar la validación al guardar los cambios

Cuando se llama a la `SaveChanges` método, de forma predeterminada, Entity Framework valida los datos de todas las propiedades de todas las entidades modificadas antes de actualizar la base de datos. Si ha actualizado un gran número de entidades y ya ha validado los datos, este trabajo no es necesario y lo puede hacer el proceso de guardar los cambios tardan menos temporalmente si se desactiva la validación. Puede hacerlo mediante el [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx) propiedad. Para obtener más información, consulte [validación](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx).

## <a name="summary"></a>Resumen

Con esto finaliza esta serie de tutoriales sobre el uso de Entity Framework en una aplicación ASP.NET MVC. Pueden encontrar vínculos a otros recursos de Entity Framework en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obtener más información sobre cómo implementar la aplicación web después de que ha creado, consulte [mapa de contenido de implementación ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx) en MSDN Library.

Para obtener información sobre otros temas relacionados con MVC, como la autenticación y autorización, consulte el [recursos recomendados de MVC](../../getting-started/recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>Agradecimientos

- Tom Dykstra escribió la versión original de este tutorial y es programadora senior en el equipo de contenido de las herramientas y plataforma Web de Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) es coautor de este tutorial y ha sido la mayoría del trabajo actualizarlo para 5 de EF y MVC 4. Rick es programadora senior de Microsoft, centrándose en Azure y MVC.
- [Rowan Miller](http://www.romiller.com) y otros miembros del equipo de Entity Framework participaron con revisiones de código y ayudaron a depurar muchos problemas con las migraciones que surgieron mientras se estaba actualizando el tutorial de EF 5.

## <a name="vb"></a>VB

Cuando se generó originalmente el tutorial, hemos proporcionado versiones de C# y VB del proyecto haya finalizado la descarga. Con esta actualización, ofrecemos un proyecto descargable C# para cada capítulo para que resulte más fácil empezar a trabajar en cualquier parte de la serie, pero debido a las limitaciones de tiempo y otras prioridades no lo hicimos para VB. Si compila un proyecto VB con estos tutoriales y estaría dispuesto a compartirlo con otros usuarios, háganoslo saber.

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>Errores y soluciones alternativas

### <a name="cannot-createshadow-copy"></a>No se puede crear/shadow el copia

Mensaje de error:

*No se puede crear/shadow copy 'DotNetOpenAuth.OpenId' cuando ese archivo ya existe.*

Solución:

Espere unos segundos y actualice la página.

### <a name="update-database-not-recognized"></a>Actualizar base de datos que no se reconoce

Mensaje de error:

*El término 'Update-Database' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe la ortografía del nombre, o si incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta e inténtelo de nuevo.* (Desde el *`Update-Database`* comando en la PMC.)

Solución:

Salga de Visual Studio. Vuelva a abrir el proyecto e inténtelo de nuevo.

### <a name="validation-failed"></a>Error de validación

Mensaje de error:

*Error de validación de una o más entidades. Vea la propiedad 'EntityValidationErrors' para obtener más detalles.* (Desde el *`Update-Database`* comando en la PMC.)

Solución:

Una causa de este problema es errores de validación cuando el `Seed` ejecuciones del método. Consulte [Seeding y bases de datos de depuración de Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para sugerencias sobre cómo depurar el `Seed` método.

### <a name="http-50019-error"></a>HTTP 500.19 error

Mensaje de error:

*HTTP Error 500.19 - Error interno del servidor la página solicitada no es accesible porque los datos de configuración de la página no están válidos.*

Solución:

Una manera que puede obtener este error es de varias copias de la solución, cada uno de ellos con el mismo número de puerto. Normalmente puede resolver este problema al salir de todas las instancias de Visual Studio, a continuación, reiniciar el proyecto en la que está trabajando. Si esto no funciona, pruebe a cambiar el número de puerto. Haga clic con el botón derecho en el archivo de proyecto y, a continuación, haga clic en Propiedades. Seleccione el **Web** pestaña y, a continuación, cambie el número de puerto en el **dirección Url del proyecto** cuadro de texto.

### <a name="error-locating-sql-server-instance"></a>Error al buscar la instancia de SQL Server

Mensaje de error:

*Se produjo un error relacionado con la red o específico de la instancia al establecer una conexión a SQL Server. No se encontró el servidor o éste no estaba accesible. Compruebe que el nombre de instancia es correcto y que SQL Server está configurado para permitir conexiones remotas. (proveedor: Interfaces de red SQL, error: 26 - Error al buscar servidor/instancia especificado)*

Solución:

Compruebe la cadena de conexión. Si ha eliminado manualmente la base de datos, cambie el nombre de la base de datos en la cadena de construcción.

> [!div class="step-by-step"]
> [Anterior](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [Siguiente](building-the-ef5-mvc4-chapter-downloads.md)
