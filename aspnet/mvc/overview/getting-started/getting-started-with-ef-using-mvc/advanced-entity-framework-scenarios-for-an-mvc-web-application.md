---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: "Advanced escenarios de Entity Framework 6 para una aplicación de MVC 5 Web (12 de 12) | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 85276377671b96e65406639c8584d9ebf8d77ff7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>Escenarios de avanzada de Entity Framework 6 para una aplicación de MVC 5 Web (12 de 12)
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descarga de PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio 2013. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En el tutorial anterior implementa la herencia de tabla por jerarquía. Este tutorial incluye presenta varios temas que son útiles para tener en cuenta cuando va más allá de los conceptos básicos sobre el desarrollo de aplicaciones web ASP.NET que usan Entity Framework Code First. Instrucciones paso a paso le guiará por el código y el uso de Visual Studio para los siguientes temas:

- [Realizar consultas SQL sin formato](#rawsql)
- [Realizar consultas de seguimiento no](#notracking)
- [Examinar SQL enviados a la base de datos](#sql)

El tutorial presentan varios temas con la introducción breve seguido de vínculos a recursos para obtener más información:

- [Repositorio y una unidad de patrones de trabajo](#repo)
- [Clases de proxy](#proxies)
- [Detección de cambios automático](#changedetection)
- [Validación automática](#validation)
- [Herramientas EF para Visual Studio](#tools)
- [Código fuente de Entity Framework](#source)

El tutorial también incluye las siguientes secciones:

- [Resumen](#summary)
- [Confirmaciones](#acknowledgments)
- [Una nota sobre VB](#vb)
- [Errores comunes así como las soluciones para ellos](#errors)

Para la mayoría de estos temas, trabajará con páginas que se han crean previamente. Para usar SQL sin formato para realizar actualizaciones masivas creará una nueva página que actualiza el número de créditos de todos los cursos en la base de datos:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>Realizar consultas SQL sin formato

La API First de Entity Framework código incluye métodos que permiten pasar comandos SQL directamente a la base de datos. Tiene las siguientes opciones:

- Use la [DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx) método para las consultas que devuelven tipos de entidad. Los objetos devueltos deben ser del tipo esperado por la `DbSet` objeto y se realiza automáticamente un seguimiento por el contexto de base de datos a menos que desactive seguimiento. (Consulte la sección siguiente la [AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx) método.)
- Use la [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx) método para las consultas que devuelven tipos que no son entidades. No realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si utiliza este método para recuperar tipos de entidad.
- Use la [Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx) para comandos de consulta no.

Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado ajustado a un método concreto de almacenamiento de datos. Hace esto mediante la generación de consultas SQL y comandos para usted, que también se evita tener que escribir usted mismo. Pero hay situaciones excepcionales cuando se necesita ejecutar consultas específicas de SQL que ha creado manualmente, y estos métodos permiten controlar dichas excepciones.

Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques de inyección de SQL. Una manera de hacerlo es utilizar consultas parametrizadas para asegurarse de que no se puede interpretar cadenas enviadas por una página web que los comandos SQL. En este tutorial usará las consultas con parámetros cuando se integra proporcionados por el usuario en una consulta.

### <a name="calling-a-query-that-returns-entities"></a>Llamar a una consulta que devuelve las entidades

El [DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx) clase proporciona un método que puede usar para ejecutar una consulta que devuelve una entidad de tipo `TEntity`. Para ver cómo funciona esto, cambiará el código en el `Details` método de la `Department` controlador.

En *DepartmentController.cs*, en la `Details` método, reemplace el `db.Departments.FindAsync` llamada al método con un `db.Departments.SqlQuery` llamada al método, como se muestra en el código resaltado siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

Para comprobar que el nuevo código funciona correctamente, seleccione la **departamentos** ficha y, a continuación, **detalles** para uno de los departamentos.

![Detalles del departamento](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>Llamar a una consulta que devuelve otros tipos de objetos

Anteriormente, se crea una cuadrícula de estadísticas de estudiante de la página acerca de que el número de alumnos se ha explicado para cada fecha de inscripción. El código que lo haga en *HomeController.cs* utiliza LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

Suponga que desea escribir el código que permita recuperar estos datos directamente en SQL, en lugar de usar LINQ. Para ello necesita ejecutar una consulta que devuelve un valor distinto de objetos entidad, lo que significa que debe usar el [Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx) método.

En *HomeController.cs*, reemplace la instrucción LINQ en el `About` método con una instrucción SQL, como se muestra en el código resaltado siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

Ejecute la página acerca de. Muestra los mismos datos que lo hacía anteriormente.

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>Llamar a una consulta Update

Imagine que desean que los administradores de la Universidad de Contoso poder realizar cambios masivos en la base de datos, como cambiar el número de créditos para cada curso. Si la Universidad tiene un gran número de cursos, sería ineficaz para recuperarlos todos como entidades y cambiarlos de forma individual. En esta sección implementaremos una página web que permite al usuario especificar un factor por el que se va a cambiar el número de créditos para todos los cursos y podrá realizar el cambio mediante la ejecución de una instancia de SQL `UPDATE` instrucción. La página web tendrá un aspecto similar de la ilustración siguiente:

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

En *CourseContoller.cs*, agregar `UpdateCourseCredits` métodos para `HttpGet` y `HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

Cuando el controlador procesa una `HttpGet` solicitud, se devuelve nada en el `ViewBag.RowsAffected` variable y la vista muestra un cuadro de texto vacío y un botón de envío, tal como se muestra en la ilustración anterior.

Cuando el **actualización** se hace clic en el botón, el `HttpPost` método se llama, y `multiplier` tiene el valor especificado en el cuadro de texto. El código, a continuación, ejecuta las instrucciones SQL que actualiza cursos y devuelve el número de filas afectadas a la vista en la `ViewBag.RowsAffected` variable. Cuando la vista obtiene un valor que variables, que muestra el número de filas actualizadas en lugar del cuadro de texto y envíe botón, tal como se muestra en la siguiente ilustración:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

En *CourseController.cs*, haga clic en uno de los `UpdateCourseCredits` métodos y, a continuación, haga clic en **agregar vista**.

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

En *Views\Course\UpdateCourseCredits.cshtml*, reemplace el código de plantilla con el código siguiente:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

Ejecute el `UpdateCourseCredits` método seleccionando la **cursos** ficha, a continuación, agregar "/ UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:50205/Course/UpdateCourseCredits`). Escriba un número en el cuadro de texto:

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

Haga clic en **Actualizar**. Ver el número de filas afectadas:

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

Haga clic en **volver a la lista** para ver la lista de cursos con el número de créditos revisado.

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

Para obtener más información acerca de las consultas SQL sin formato, vea [sin procesar consultas SQL](https://msdn.microsoft.com/data/jj592907) en MSDN.

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>Consultas de seguimiento no

Cuando un contexto de base de datos recupera las filas de tabla y crea objetos de entidad que tienen tipos de archivo, de forma predeterminada realiza un seguimiento de si las entidades en memoria están sincronizadas con el contenido de la base de datos. Los datos en memoria actúa como una memoria caché y se utilizan cuando se actualiza una entidad. Este almacenamiento en caché a menudo es necesario en una aplicación web porque el contexto de instancias son normalmente de corta duración (un nuevo objeto uno se crea y se elimina para cada solicitud) y el contexto que lee una entidad normalmente se elimina antes de esa entidad se vuelve a usar.

Puede deshabilitar el seguimiento de los objetos de entidad en la memoria utilizando la [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) método. En el que se puede hacer que los escenarios típicos son los siguientes:

- Una consulta recupera un gran volumen de datos que pueden mejorar considerablemente el rendimiento al desactivar el seguimiento.
- Que desea asociar una entidad para actualizarla, pero anteriormente se recupera la misma entidad para un propósito diferente. Dado que la entidad ya está sometida a seguimiento por el contexto de base de datos, no se puede adjuntar la entidad que desea cambiar. Una manera de controlar esta situación es utilizar el `AsNoTracking` opción con la consulta anterior.

Para obtener un ejemplo que muestra cómo utilizar el [AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx) método, consulte [la versión anterior de este tutorial](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Esta versión del tutorial no establezca la marca de modificado en una entidad de enlazador de modelo creado en el método de edición, por lo que no es necesario `AsNoTracking`.

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>Examinar SQL enviados a la base de datos

A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos. En un tutorial anterior ha visto cómo hacerlo en el código de interceptor. Ahora verá algunas maneras de hacerlo sin necesidad de escribir código de interceptor. Para probar esto, podrá ver una consulta simple y, a continuación, examine qué ocurre con los a medida que agregue opciones tal diligente cargar, filtrado y ordenación.

En *controladores/CourseController*, reemplace el `Index` método por el código siguiente para detener temporalmente la carga diligente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

Ahora se establezca un punto de interrupción en el `return` instrucción (F9 con el cursor en esa línea). Presione F5 para ejecutar el proyecto en modo de depuración y seleccione la página de índice de curso. Cuando el código alcanza el punto de interrupción, examine el `sql` variable. Vea la consulta que se envía a SQL Server. Es una sencilla `Select` instrucción.

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

Haga clic en la clase de aumento para ver la consulta en el **visualizador de texto**.

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

Ahora agregará una lista desplegable a la página de índice de cursos para que los usuarios pueden filtrar para un departamento concreto. Ordenará los cursos por título, y especificará carga diligente de la `Department` propiedad de navegación.

En *CourseController.cs*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

Restaurar el punto de interrupción en el `return` instrucción.

El método recibe el valor seleccionado de la lista desplegable en el `SelectedDepartment` parámetro. Si se selecciona nada, este parámetro será null.

Un `SelectList` colección que contiene todos los departamentos se pasa a la vista de la lista desplegable. Los parámetros se pasan a la `SelectList` constructor especifica el nombre del campo de valor, el nombre del campo de texto y el elemento seleccionado.

Para el `Get` método de la `Course` repositorio, el código especifica una expresión de filtro, un criterio de ordenación y diligente para cargar la `Department` propiedad de navegación. Devuelve la expresión de filtro siempre `true` si se selecciona nada en la lista desplegable (es decir, `SelectedDepartment` es null).

En *Views\Course\Index.cshtml*, inmediatamente antes de la apertura `table` etiqueta, agregue el código siguiente para crear la lista desplegable y un botón de envío:

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

Con el punto de interrupción establecido, ejecute la página de índice de curso. Continuar a través de las veces que el código alcanza un punto de interrupción para que la página se muestre en el explorador. Seleccione un departamento en la lista desplegable y haga clic en **filtro**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

Este tiempo será el primer punto de interrupción de la lista desplegable de la consulta de departamentos. Omitir que y ver el `query` variable la próxima vez que el código alcanza el punto de interrupción para ver qué la `Course` consultar ahora parece que. Verá algo parecido a lo siguiente:

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

Puede ver que la consulta es ahora un `JOIN` consulta que carga `Department` datos junto con el `Course` datos y que incluye un `WHERE` cláusula.

Quitar el `var sql = courses.ToString()` línea.

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>Repositorio y una unidad de patrones de trabajo

Muchos desarrolladores escriben código para implementar el repositorio y una unidad de patrones de trabajo como un contenedor alrededor de código que funcione con Entity Framework. Estos modelos están diseñados para crear una capa de abstracción entre la capa de acceso a datos y la capa de lógica empresarial de una aplicación. Implementación de estos modelos puede ayudar a aislar la aplicación de cambios en el almacén de datos y puede facilitar el desarrollo controlado por pruebas (TDD) o pruebas unitarias automatizadas. Sin embargo, escribir código adicional para implementar estos patrones no siempre es la mejor opción para las aplicaciones que usan EF, por varias razones:

- La propia clase de contexto EF aísla el código desde el código específico del almacén de datos.
- La clase de contexto EF puede actuar como una clase de unidad de trabajo para la base de datos de las actualizaciones que se hace con EF.
- Características introducidas en Entity Framework 6 sea más fácil implementar TDD sin escribir código de repositorio.

Para obtener más información sobre cómo implementar el repositorio y una unidad de patrones de trabajo, consulte [la versión de Entity Framework 5 de esta serie de tutoriales](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md). Para obtener información sobre maneras de implementar TDD en Entity Framework 6, consulte los siguientes recursos:

- [Cómo EF6 permite Mocking DbSets más fácilmente](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [Probar con un marco de simulación](https://msdn.microsoft.com/data/dn314429)
- [Las pruebas con sus propios dobles de pruebas](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Clases de proxy

Cuando Entity Framework crea instancias de la entidad (por ejemplo, cuando se ejecuta una consulta), a menudo crea como instancias de un tipo derivado generada dinámicamente que actúa como un proxy para la entidad. Por ejemplo, vea las siguientes dos imágenes de depurador. En la primera imagen, verá que el `student` variable es el esperado `Student` escriba inmediatamente después de crear una instancia de la entidad. En la segunda imagen EF si se utiliza para leer una entidad de estudiante de la base de datos, vea la clase de proxy.

![Antes de la clase de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Después de la clase de proxy](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

Esta clase de proxy reemplaza algunas propiedades virtuales de la entidad que se va a insertar enlaces para realizar automáticamente acciones cuando se tiene acceso a la propiedad. Este mecanismo se usa para una función es la carga diferida.

La mayoría de las veces no es necesario tener en cuenta este uso de los servidores proxy, pero hay excepciones:

- En algunos escenarios puede evitar que Entity Framework puedan crear instancias de proxy. Por ejemplo, cuando se está serializando entidades se suele preferir las clases POCO, no las clases de proxy. Es una manera de evitar problemas de serialización serializar objetos de transferencia de datos (dto) en lugar de objetos de entidad, como se muestra en el [usar Web API con Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md) tutorial. Otra manera es [deshabilitar la creación de proxy](https://msdn.microsoft.com/data/jj592886.aspx).
- Cuando cree instancias de una clase de entidad con la `new` , no obtiene una instancia del proxy. Esto significa que no aparece la funcionalidad, como el seguimiento de cambios automático y la carga diferida. Esto suele ser muy bien; por lo general no necesita la carga diferida, porque va a crear una nueva entidad que no se encuentra en la base de datos, y generalmente no es necesario si va a marcar explícitamente la entidad como el seguimiento de cambios `Added`. Sin embargo, si necesita la carga diferida y necesita seguimiento de cambios, puede crear nuevas instancias de entidad con servidores proxy mediante la [crear](https://msdn.microsoft.com/library/gg679504.aspx) método de la `DbSet` clase.
- Puede obtener un tipo de entidad real de un tipo de proxy. Puede usar el [GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx) método de la `ObjectContext` clase para obtener el tipo de entidad real de una instancia de tipo de proxy.

Para obtener más información, consulte [trabajar con servidores proxy](https://msdn.microsoft.com/data/JJ592886.aspx) en MSDN.

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>Detección de cambios automático

Entity Framework determina cómo ha cambiado una entidad (y, por tanto, las actualizaciones que necesitan para enviarse a la base de datos) mediante la comparación de los valores actuales de una entidad con los valores originales. Cuando se consulta o se adjunta la entidad, se almacenan los valores originales. Algunos de los métodos que provocan la detección de cambios automático son los siguientes:

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

Si está realizando el seguimiento de un gran número de entidades y se llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas desactivando temporalmente detección de cambios automático mediante el [AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx) propiedad. Para obtener más información, consulte [detectar cambios automáticamente](https://msdn.microsoft.com/data/jj556205) en MSDN.

<a id="validation"></a>
## <a name="automatic-validation"></a>Validación automática

Cuando se llama a la `SaveChanges` método, de forma predeterminada, Entity Framework valida los datos de todas las propiedades de todas las entidades modificadas antes de actualizar la base de datos. Si ha actualizado un gran número de entidades y ya ha validado los datos, este trabajo no es necesario y podría hacer que el proceso de guardar los cambios tardan menos tiempo temporalmente si se desactiva la validación. Puede hacer que el uso de la [ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx) propiedad. Para obtener más información, consulte [validación](https://msdn.microsoft.com/data/gg193959) en MSDN.

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Herramientas de alimentación de Entity Framework

[Herramientas de Entity Framework Power](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d) es un complemento de Visual Studio que se usó para crear los diagramas de modelo de datos se muestran en estos tutoriales. Las herramientas también pueden hacer otra función como generar clases de entidad basada en las tablas en una base de datos existente para que puedan utilizar la base de datos con Code First. Después de instalar las herramientas, aparecerán algunas opciones adicionales en los menús contextuales. Por ejemplo, cuando hace doble clic en la clase de contexto en **el Explorador de soluciones**, obtendrá una opción para generar un diagrama. Cuando se usa Code First no se puede cambiar el modelo de datos en el diagrama, pero puede moverse cosas para que resulten más fáciles de entender.

![EF en el menú contextual](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![Diagrama EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Código fuente de Entity Framework

Está disponible en el código fuente de Entity Framework 6 [GitHub](https://github.com/aspnet/EntityFramework6). Se pueden informar de errores y puede aportar sus propias mejoras en el código fuente EF.

Aunque el código fuente está abierto, Entity Framework se admite totalmente como un producto de Microsoft. El equipo de Microsoft Entity Framework mantiene el control sobre el que se aceptan las contribuciones y comprueba todos los cambios de código para garantizar la calidad de cada versión.

<a id="summary"></a>
## <a name="summary"></a>Resumen

Con esto finaliza la siguiente serie de tutoriales sobre el uso de Entity Framework en una aplicación ASP.NET MVC. Para obtener más información sobre cómo trabajar con datos mediante Entity Framework, vea la [página de documentación de EF en MSDN](https://msdn.microsoft.com/data/ee712907) y [ASP.NET Data Access: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

Para obtener más información acerca de cómo implementar la aplicación web después de que haya creado, vea [implementación Web de ASP.NET: recomienda recursos](../../../../whitepapers/aspnet-web-deployment-content-map.md) en MSDN Library.

Para obtener información acerca de otros temas relacionados con MVC, como la autenticación y autorización, consulte la [ASP.NET MVC - recomienda recursos](../recommended-resources-for-mvc.md).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Confirmaciones

- Tom Dykstra la versión original de este tutorial se escribió, coautor la actualización 5 de EF y la actualización 6 de EF se escribió. Tom es un sistema de escritura de programación senior en el equipo de contenido de herramientas y plataforma Web de Microsoft.
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) realizó gran parte del trabajo actualizando el tutorial de EF 5 y MVC 4 y la actualización 6 de EF es coautor. Rick es un escritor de programación senior de Microsoft que se centran en Azure y MVC.
- [Rowan Miller](http://www.romiller.com) y otros miembros del equipo de Entity Framework asistida con revisiones de código y ayudaron a muchos problemas de las migraciones que se produjo mientras se estábamos actualizando el tutorial de EF 5 y 6 de EF de depuración.

<a id="vb"></a>
## <a name="vb"></a>VB

Cuando se generó originalmente el tutorial de EF 4.1, se proporcionan versiones de C# y VB del proyecto haya finalizado la descarga. Debido a las limitaciones de tiempo y otra prioridad no hemos para esta versión. Si compila un proyecto VB con estos tutoriales y ¿estaría dispuesto a compartir con otras personas, háganoslo saber.

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>Errores comunes así como las soluciones para ellos

### <a name="cannot-createshadow-copy"></a>No se puede crear/ocultación el copia

Mensaje de error:

> No se puede crear/ocultación el copia '&lt;filename&gt;' cuando ese archivo ya existe.


Soluciones

Espere unos segundos y actualice la página.

### <a name="update-database-not-recognized"></a>Actualizar base de datos no reconocida

Mensaje de error (desde el `Update-Database` comando el PMC):

> El término 'Update-Database' no se reconoce como el nombre de un cmdlet, función, archivo de script o un programa ejecutable. Compruebe la ortografía del nombre, o si incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta e inténtelo de nuevo.


Soluciones

Salga de Visual Studio. Vuelva a Abrir proyecto y vuelva a intentarlo.

### <a name="validation-failed"></a>Error de validación

Mensaje de error (desde el `Update-Database` comando el PMC):

> Error de validación de una o más entidades. Vea la propiedad 'EntityValidationErrors' para obtener más detalles.


Soluciones

Una causa de este problema es que los errores de validación cuando el `Seed` ejecuciones de método. Vea [Seeding y bases de datos de depuración de Entity Framework (EF)](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) para obtener sugerencias sobre cómo depurar el `Seed` método.

### <a name="http-50019-error"></a>HTTP 500.19 error

Mensaje de error:

> Error HTTP 500.19 - Error interno del servidor  
> No se puede tener acceso a la página solicitada porque los datos de configuración relacionados de la página no están válidos.


Soluciones

Una manera que se puede obtener este error es de varias copias de la solución, cada uno de ellos con el mismo número de puerto. Normalmente puede resolver este problema, salir de todas las instancias de Visual Studio y luego reiniciar el proyecto que está trabajando. Si esto no funciona, pruebe a cambiar el número de puerto. Haga clic con el botón secundario en el archivo de proyecto y, a continuación, haga clic en Propiedades. Seleccione el **Web** ficha y, a continuación, cambie el número de puerto en el **dirección Url del proyecto** cuadro de texto.

### <a name="error-locating-sql-server-instance"></a>Instancia de SQL Server de localización de error

Mensaje de error:

> Se ha producido un error relacionado con la red o específico de la instancia al establecer una conexión a SQL Server. No se encontró el servidor o éste no estaba accesible. Compruebe que el nombre de la instancia es correcto y que SQL Server está configurado para admitir conexiones remotas. (proveedor: Interfaces de red SQL, error: 26 - Error Buscar servidor/instancia especificado)


Soluciones

Compruebe la cadena de conexión. Si se ha eliminado manualmente la base de datos, cambiar el nombre de la base de datos en la cadena de construcción.

>[!div class="step-by-step"]
[Anterior](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
