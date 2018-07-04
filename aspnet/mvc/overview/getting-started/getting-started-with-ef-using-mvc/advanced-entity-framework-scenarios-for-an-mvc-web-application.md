---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: Advanced escenarios de Entity Framework 6 para una aplicación de MVC 5 Web (12 de 12) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 43d7dec02a104b2bb29f598c17d252b0476a53f7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390493"
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Escenarios de opciones avanzadas de Entity Framework 6 para una aplicación de MVC 5 Web (12 de 12)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descargar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio 2013. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En el tutorial anterior implementa la herencia de tabla por jerarquía. Incluye en este tutorial se presentan varios temas que son útiles para tener en cuenta cuando va más allá de los aspectos básicos del desarrollo de aplicaciones web ASP.NET que usan Entity Framework Code First. Instrucciones paso a paso le guiarán por el código y con Visual Studio para los siguientes temas:

- [Realización de consultas SQL sin formato](#rawsql)
- [Realización de consultas de no seguimiento](#notracking)
- [Examinar SQL enviados a la base de datos](#sql)

El tutorial presenta varios temas con la introducción breve seguido de vínculos a recursos para obtener más información:

- [Repositorio y unidad de patrones de trabajo](#repo)
- [Clases de proxy](#proxies)
- [Detección de cambios automática](#changedetection)
- [Validación automática](#validation)
- [Herramientas EF para Visual Studio](#tools)
- [Código fuente de Entity Framework](#source)

El tutorial también incluye las siguientes secciones:

- [Resumen](#summary)
- [Confirmaciones](#acknowledgments)
- [Una nota acerca de VB](#vb)
- [Errores comunes así como las soluciones para ellos](#errors)

Para la mayoría de estos temas, trabajará con las páginas que ya ha creado. Para usar SQL sin procesar para realizar actualizaciones masivas creará una nueva página que actualiza el número de créditos de todos los cursos de la base de datos:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Realizar consultas SQL sin formato

La API de Entity Framework Code First incluye métodos que permiten pasar comandos SQL directamente a la base de datos. Tiene las siguientes opciones:

- Use la [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) método para las consultas que devuelven tipos de entidad. Los objetos devueltos deben ser del tipo esperado por la `DbSet` objeto y se realiza el seguimiento automático del contexto de base de datos a menos que desactive el seguimiento. (Consulte la sección siguiente la [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) método.)
- Use la [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) método para las consultas que devuelven tipos que no son entidades. No se realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si usa este método para recuperar tipos de entidad.
- Use la [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) para comandos sin consulta.

Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado estrechamente a un método concreto de almacenamiento de datos. Lo consigue mediante la generación de consultas SQL y comandos, lo que también le evita tener que escribirlos usted mismo. Pero hay situaciones excepcionales cuando se necesita ejecutar consultas específicas de SQL que ha creado manualmente, y estos métodos permiten controlar dichas excepciones.

Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques por inyección de código SQL. Una manera de hacerlo es mediante consultas parametrizadas para asegurarse de que las cadenas enviadas por una página web no se pueden interpretar como comandos SQL. En este tutorial usará las consultas con parámetros al integrar la entrada de usuario en una consulta.

### <a name="calling-a-query-that-returns-entities"></a>Llamar a una consulta que devuelve las entidades

El [DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) clase proporciona un método que puede usar para ejecutar una consulta que devuelve una entidad del tipo `TEntity`. Para ver cómo funciona, cambiará el código en el `Details` método de la `Department` controlador.

En *DepartmentController.cs*, en el `Details` método, reemplace el `db.Departments.FindAsync` llamada al método con un `db.Departments.SqlQuery` llamada al método, como se muestra en el siguiente código resaltado:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Para comprobar que el nuevo código funciona correctamente, seleccione la pestaña **Departments** y, después, **Details** para uno de los departamentos.

![Detalles del departamento](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Llamar a una consulta que devuelve otros tipos de objetos

Anteriormente creó una cuadrícula de estadísticas de alumno de la página About que mostraba el número de alumnos para cada fecha de inscripción. El código que hace esto en *HomeController.cs* usa LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Suponga que desea escribir el código que recupera estos datos directamente en SQL, en lugar de mediante LINQ. Para ello, necesita ejecutar una consulta que devuelve un valor distinto de objetos entidad, lo que significa deba usar el [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) método.

En *HomeController.cs*, reemplace la instrucción LINQ en el `About` método con una instrucción SQL, como se muestra en el siguiente código resaltado:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Ejecute la página About. Muestra los mismos datos que anteriormente.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Llamar a una consulta Update

Supongamos que los administradores de Contoso University quieren realizar cambios masivos en la base de datos, como cambiar el número de créditos para cada curso. Si la universidad tiene un gran número de cursos, sería poco eficaz recuperarlos todos como entidades y cambiarlos de forma individual. En esta sección implementará una página web que permite al usuario especificar un factor por el que se va a cambiar el número de créditos para todos los cursos y podrá realizar el cambio mediante la ejecución de una instancia de SQL `UPDATE` instrucción. La página web tendrá el mismo aspecto que la ilustración siguiente:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

En *CourseContoller.cs*, agregar `UpdateCourseCredits` métodos para `HttpGet` y `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Cuando el controlador procesa una `HttpGet` solicitud, se devuelve nada en el `ViewBag.RowsAffected` variable y la vista muestra un cuadro de texto vacío y un botón de envío, tal como se muestra en la ilustración anterior.

Cuando el **actualización** se hace clic en el botón, el `HttpPost` método se llama a, y `multiplier` tiene el valor especificado en el cuadro de texto. El código, a continuación, ejecuta la instrucción SQL que actualiza los cursos y devuelve el número de filas afectadas a la vista en el `ViewBag.RowsAffected` variable. Cuando la vista obtiene un valor en el que variables, se muestra el número de filas actualizadas en lugar del cuadro de texto y botón, de envío, tal como se muestra en la ilustración siguiente:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

En *CourseController.cs*, haga clic en uno de los `UpdateCourseCredits` métodos y, a continuación, haga clic en **agregar vista**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

En *Views\Course\UpdateCourseCredits.cshtml*, reemplace el código de plantilla con el código siguiente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Ejecute el método `UpdateCourseCredits` seleccionando la pestaña **Courses**, después, agregue "/UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Escriba un número en el cuadro de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Haga clic en **Actualizar**. Verá el número de filas afectadas:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Haga clic en **Volver a la lista** para ver la lista de cursos con el número de créditos revisado.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obtener más información sobre consultas SQL sin formato, vea [consultas SQL sin procesar](https://msdn.microsoft.com/data/jj592907) en MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>consultas de no seguimiento

Cuando un contexto de base de datos recupera las filas de tabla y crea objetos de entidad que las representa, de forma predeterminada realiza el seguimiento de si las entidades en memoria están sincronizadas con el contenido de la base de datos. Los datos en memoria actúan como una caché y se usan cuando se actualiza una entidad. Este almacenamiento en caché suele ser necesario en una aplicación web porque las instancias de contexto normalmente son de corta duración (para cada solicitud se crea una y se elimina) y el contexto que lee una entidad normalmente se elimina antes de volver a usar esa entidad.

Puede deshabilitar el seguimiento de los objetos de entidad en la memoria utilizando la [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) método. Los siguientes son escenarios típicos en los que es posible que quiera hacer esto:

- Una consulta recupera un gran volumen de datos que pueden mejorar considerablemente el rendimiento al desactivar el seguimiento.
- Desea adjuntar una entidad para actualizarla, pero que recuperó anteriormente la misma entidad para un propósito diferente. Como el contexto de base de datos ya está realizando el seguimiento de la entidad, no se puede adjuntar la entidad que se quiere cambiar. Una forma de controlar esta situación es usar el `AsNoTracking` opción con la consulta anterior.

Para obtener un ejemplo que muestra cómo usar el [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) método, consulte [la versión anterior de este tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Esta versión del tutorial no establezca la marca de modificado en una entidad de enlazador de modelo creado en el método de edición, por lo que no necesita `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Examinar SQL enviados a la base de datos

A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos. En un tutorial anterior hemos visto cómo hacerlo en el código de interceptor. Ahora verá algunas maneras de hacerlo sin tener que escribir código de interceptor. Para probar esto, examinará una consulta simple y, a continuación, ver lo que ocurre al mismo a medida que agrega opciones tal diligente cargar, filtrado y ordenación.

En *controladores/CourseController*, reemplace el `Index` método con el código siguiente para detener temporalmente la carga diligente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Ahora establezca un punto de interrupción en el `return` instrucción (F9 con el cursor en esa línea). Presione F5 para ejecutar el proyecto en modo de depuración y seleccione la página de índice del curso. Cuando el código alcanza el punto de interrupción, examinar el `sql` variable. Vea la consulta que se envía a SQL Server. Es una sencilla `Select` instrucción.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Haga clic en la clase aumento para ver la consulta en el **visualizador de texto**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ahora agregará una lista desplegable a la página de índice de cursos para que los usuarios pueden filtrar para un departamento determinado. Los cursos se ordena por título, y especificará la carga diligente para la `Department` propiedad de navegación.

En *CourseController.cs*, reemplace el `Index` método con el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Restaurar el punto de interrupción en el `return` instrucción.

El método recibe el valor seleccionado de la lista desplegable en el `SelectedDepartment` parámetro. Si se selecciona nada, este parámetro será nulo.

Un `SelectList` colección que contiene todos los departamentos se pasa a la vista de la lista desplegable. Los parámetros pasados a la `SelectList` constructor especifica el nombre del campo de valor, el nombre del campo de texto y el elemento seleccionado.

Para el `Get` método de la `Course` repositorio, el código especifica una expresión de filtro, un criterio de ordenación y la carga diligente para la `Department` propiedad de navegación. Devuelve la expresión de filtro siempre `true` si se selecciona nada en la lista desplegable (es decir, `SelectedDepartment` es null).

En *Views\Course\Index.cshtml*, inmediatamente antes de la apertura `table` etiqueta, agregue el código siguiente para crear la lista desplegable y un botón de envío:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Con el punto de interrupción establecido, ejecute la página de índice del curso. Continuar a través de las veces que el código alcanza un punto de interrupción, por lo que se muestra la página en el explorador. Seleccionar un departamento de la lista desplegable y haga clic en **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Esta vez será el primer punto de interrupción de la lista desplegable de la consulta de los departamentos de TI. Omitir ese paso y ver el `query` variable la próxima vez que el código alcanza el punto de interrupción con el fin de ver qué la `Course` ahora el aspecto de la consulta. Verá algo parecido a lo siguiente:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Puede ver que la consulta es ahora un `JOIN` consulta que carga `Department` datos junto con el `Course` datos y que incluye un `WHERE` cláusula.

Quitar el `var sql = courses.ToString()` línea.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Patrones de repositorio y de unidad de trabajo

Muchos desarrolladores escriben código para implementar el repositorio y una unidad de patrones de trabajo como un contenedor en torno al código que funciona con Entity Framework. Estos patrones están previstos para crear una capa de abstracción entre la capa de acceso de datos y la capa de lógica de negocios de una aplicación. Implementar estos patrones puede ayudar a aislar la aplicación de cambios en el almacén de datos y puede facilitar la realización de pruebas unitarias automatizadas o el desarrollo controlado por pruebas (TDD). Sin embargo, escribir código adicional para implementar estos patrones no siempre es la mejor opción para las aplicaciones que usan EF, por varias razones:

- La propia clase de contexto de EF aísla el código del código específico del almacén de datos.
- La clase de contexto de EF puede actuar como una clase de unidad de trabajo para las actualizaciones de base de datos que hace con EF.
- Las características introducidas en Entity Framework 6 hacer más fácil de implementar TDD sin escribir código de repositorio.

Para obtener más información sobre cómo implementar el repositorio y unidad de patrones de trabajo, consulte [la versión de Entity Framework 5 de esta serie de tutoriales](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Para obtener información sobre cómo implementar TDD en Entity Framework 6, consulte los siguientes recursos:

- [Cómo EF6 permite DbSets de simulación con más facilidad](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Las pruebas con un marco de simulación](https://msdn.microsoft.com/data/dn314429)
- [Las pruebas con sus propio dobles de pruebas](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Clases de proxy

Cuando Entity Framework crea instancias de entidad (por ejemplo, cuando se ejecuta una consulta), a menudo crea como instancias de un tipo derivado generada dinámicamente que actúa como un proxy para la entidad. Por ejemplo, vea las siguientes dos imágenes de depurador. En la primera imagen, verá que el `student` variable es el esperado `Student` escriba inmediatamente después de crear una instancia de la entidad. En la segunda imagen, una vez que se ha usado EF para leer una entidad student de la base de datos, vea la clase de proxy.

![Antes de la clase de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Después de la clase de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Esta clase de proxy reemplaza algunas propiedades de la entidad que se va a insertar enlaces para realizar automáticamente acciones cuando se tiene acceso a la propiedad virtuales. Este mecanismo es usar para una función es la carga diferida.

La mayoría de los casos no es necesario tener en cuenta este uso de los servidores proxy, pero hay excepciones:

- En algunos escenarios puede impedir que Entity Framework creen instancias de proxy. Por ejemplo, cuando se está serializando entidades generalmente desea las clases POCO, no las clases de proxy. Es una forma de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad, como se muestra en el [usar Web API con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) tutorial. Otra manera es [deshabilitar la creación de proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Al crear instancias de una clase de entidad usando el `new` , no obtiene una instancia del proxy. Esto significa que no obtendrá una funcionalidad, como el seguimiento de cambios automático y la carga diferida. Esto normalmente no importa; por lo general no se necesita la carga diferida, porque va a crear una nueva entidad que no se encuentra en la base de datos y, por lo general no es necesario si va a marcar explícitamente la entidad como de seguimiento de cambios `Added`. Sin embargo, si necesita la carga diferida y necesita seguimiento de cambios, puede crear nuevas instancias de entidad con servidores proxy mediante la [crear](https://msdn.microsoft.com/library/gg679504.aspx) método de la `DbSet` clase.
- Es posible que desee obtener un tipo de entidad real de un tipo de proxy. Puede usar el [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) método de la `ObjectContext` clase para obtener el tipo de entidad real de una instancia del tipo de proxy.

Para obtener más información, consulte [trabajar con servidores proxy](https://msdn.microsoft.com/data/JJ592886.aspx) en MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Detección de cambios automática

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

Si realiza un seguimiento de un gran número de entidades y llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas si desactiva la detección de cambios automático mediante temporalmente el [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) propiedad. Para obtener más información, consulte [detectar cambios automáticamente](https://msdn.microsoft.com/data/jj556205) en MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Validación automática

Cuando se llama a la `SaveChanges` método, de forma predeterminada, Entity Framework valida los datos de todas las propiedades de todas las entidades modificadas antes de actualizar la base de datos. Si ha actualizado un gran número de entidades y ya ha validado los datos, este trabajo no es necesario y lo puede hacer el proceso de guardar los cambios tardan menos temporalmente si se desactiva la validación. Puede hacerlo mediante el [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) propiedad. Para obtener más información, consulte [validación](https://msdn.microsoft.com/data/gg193959) en MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) es un complemento de Visual Studio que se usó para crear los diagramas de modelo de datos se muestra en estos tutoriales. Las herramientas también pueden realizar otra función, como generar clases de entidad en función de las tablas de base de datos existente para que pueda usar la base de datos con Code First. Después de instalar las herramientas, aparecerán algunas opciones adicionales en los menús contextuales. Por ejemplo, cuando hace doble clic en la clase de contexto en **el Explorador de soluciones**, obtendrá una opción para generar un diagrama. Cuando se usa Code First no se puede cambiar el modelo de datos en el diagrama, pero puede mover las cosas que resulte más fácil de entender.

![EF en el menú contextual](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![Diagrama EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Código fuente de Entity Framework

El código fuente de Entity Framework 6 está disponible en [GitHub](https://github.com/aspnet/EntityFramework6). Puede registrar errores y puede aportar sus propias mejoras al código fuente EF.

Aunque el código fuente está abierto, Entity Framework es totalmente compatible como producto de Microsoft. El equipo de Microsoft Entity Framework mantiene el control sobre qué contribuciones se aceptan y comprueba todos los cambios de código para garantizar la calidad de cada versión.

<a id="summary"></a>
## <a name="summary"></a>Resumen

Con esto finaliza esta serie de tutoriales sobre el uso de Entity Framework en una aplicación ASP.NET MVC. Para obtener más información sobre cómo trabajar con datos mediante Entity Framework, vea el [página de documentación de EF en MSDN](https://msdn.microsoft.com/data/ee712907) y [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obtener más información sobre cómo implementar la aplicación web después de que ha creado, consulte [implementación Web de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-web-deployment-content-map.md) en MSDN Library.

Para obtener información sobre otros temas relacionados con MVC, como la autenticación y autorización, consulte el [MVC de ASP.NET - recursos recomendados](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Agradecimientos

- Tom Dykstra escribió la versión original de este tutorial, es coautor de la actualización a EF 5 y ha escrito la actualización de EF 6. Tom es programadora senior en el equipo de contenido de las herramientas y plataforma Web de Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) realizó gran parte del trabajo actualizando el tutorial de EF 5 y MVC 4 y coautor de la actualización de EF 6. Rick es programadora senior de Microsoft, centrándose en Azure y MVC.
- [Rowan Miller](http://www.romiller.com) y otros miembros del equipo de Entity Framework participaron con revisiones de código y ayudaron a depurar muchos problemas con las migraciones que surgieron mientras se estaba actualizando el tutorial de EF 5 y 6 de EF.

<a id="vb"></a>
## <a name="vb"></a>VB

Cuando se generó originalmente el tutorial de EF 4.1, hemos proporcionado versiones de C# y VB del proyecto haya finalizado la descarga. Debido a las limitaciones de tiempo y otras prioridades no hemos hecho esta versión. Si compila un proyecto VB con estos tutoriales y estaría dispuesto a compartirlo con otros usuarios, háganoslo saber.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Errores comunes así como las soluciones para ellos

### <a name="cannot-createshadow-copy"></a>No se puede crear/shadow el copia

Mensaje de error:

> No se puede crear/shadow el copia '&lt;filename&gt;' cuando ese archivo ya existe.


Soluciones

Espere unos segundos y actualice la página.

### <a name="update-database-not-recognized"></a>Actualizar base de datos que no se reconoce

Mensaje de error (desde el `Update-Database` comando en la PMC):

> El término 'Update-Database' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe la ortografía del nombre, o si incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta e inténtelo de nuevo.


Soluciones

Salga de Visual Studio. Vuelva a abrir el proyecto e inténtelo de nuevo.

### <a name="validation-failed"></a>Error de validación

Mensaje de error (desde el `Update-Database` comando en la PMC):

> Error de validación de una o más entidades. Vea la propiedad 'EntityValidationErrors' para obtener más detalles.


Soluciones

Una causa de este problema es errores de validación cuando el `Seed` ejecuciones del método. Consulte [Seeding y bases de datos de depuración de Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para sugerencias sobre cómo depurar el `Seed` método.

### <a name="http-50019-error"></a>HTTP 500.19 error

Mensaje de error:

> Error HTTP 500.19 - Error interno del servidor  
> No se puede tener acceso la página solicitada porque los datos de configuración de la página no están válidos.


Soluciones

Una manera que puede obtener este error es de varias copias de la solución, cada uno de ellos con el mismo número de puerto. Normalmente puede resolver este problema al salir de todas las instancias de Visual Studio y luego reiniciar el proyecto que está trabajando. Si esto no funciona, pruebe a cambiar el número de puerto. Haga clic con el botón derecho en el archivo de proyecto y, a continuación, haga clic en Propiedades. Seleccione el **Web** pestaña y, a continuación, cambie el número de puerto en el **dirección Url del proyecto** cuadro de texto.

### <a name="error-locating-sql-server-instance"></a>Error al buscar la instancia de SQL Server

Mensaje de error:

> Error relacionado con la red o específico de la instancia mientras se establecía una conexión con el servidor SQL Server. No se encontró el servidor o éste no estaba accesible. Compruebe que el nombre de la instancia es correcto y que SQL Server está configurado para admitir conexiones remotas. (proveedor: interfaces de red de SQL, error: 26 -	Error al buscar el servidor o instancia especificado)


Soluciones

Compruebe la cadena de conexión. Si ha eliminado manualmente la base de datos, cambie el nombre de la base de datos en la cadena de construcción.

> [!div class="step-by-step"]
> [Anterior](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
