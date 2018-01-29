---
title: "Núcleo de ASP.NET MVC con EF Core - avanzada - 10 de 10"
author: tdykstra
description: "Este tutorial presentan varios temas que son útiles para tener en cuenta cuando va más allá de los conceptos básicos sobre el desarrollo de aplicaciones web ASP.NET que usan Entity Framework Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/advanced
ms.openlocfilehash: b1d1cb8710595acd72bd5a0786bf1b1fed8b1d79
ms.sourcegitcommit: 09b342b45e7372ba9ebf17f35eee331e5a08fb26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2018
---
# <a name="advanced-topics---ef-core-with-aspnet-core-mvc-tutorial-10-of-10"></a>Temas avanzados - Core EF con el tutorial de MVC de ASP.NET Core (10 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En el tutorial anterior, se implementa la herencia de tabla por jerarquía. Este tutorial presentan varios temas que son útiles para tener en cuenta cuando va más allá de los conceptos básicos sobre el desarrollo de aplicaciones web de ASP.NET Core que usan Entity Framework Core.

## <a name="raw-sql-queries"></a>Consultas SQL sin formato

Una de las ventajas del uso de Entity Framework es que evita enlazar el código demasiado ajustado a un método concreto de almacenamiento de datos. Hace esto mediante la generación de consultas SQL y comandos para usted, que también se evita tener que escribir usted mismo. Pero hay situaciones excepcionales cuando se necesita ejecutar consultas específicas de SQL que ha creado manualmente. En estos casos, la API First de Entity Framework código incluye métodos que permiten pasar comandos SQL directamente a la base de datos. Tiene las siguientes opciones en EF Core 1.0:

* Use la `DbSet.FromSql` método para las consultas que devuelven tipos de entidad. Los objetos devueltos deben ser del tipo esperado por la `DbSet` objeto y se le realiza automáticamente un seguimiento por el contexto de base de datos a menos que se [desactivar el seguimiento](crud.md#no-tracking-queries).

* Use la `Database.ExecuteSqlCommand` para comandos de consulta no.

Si tiene que ejecutar una consulta que devuelve tipos que no son entidades, puede usar ADO.NET con la conexión de base de datos proporcionada por EF. No realiza un seguimiento de los datos devueltos por el contexto de la base de datos, incluso si utiliza este método para recuperar tipos de entidad.

Como siempre es true cuando ejecuta comandos SQL en una aplicación web, debe tomar precauciones para proteger su sitio contra los ataques de inyección de SQL. Una manera de hacerlo es utilizar consultas parametrizadas para asegurarse de que no se puede interpretar cadenas enviadas por una página web que los comandos SQL. En este tutorial usará las consultas con parámetros cuando se integra proporcionados por el usuario en una consulta.

## <a name="call-a-query-that-returns-entities"></a>Llamar a una consulta que devuelve las entidades

El `DbSet<TEntity>` clase proporciona un método que puede usar para ejecutar una consulta que devuelve una entidad de tipo `TEntity`. Para ver cómo funciona esto, cambiará el código en el `Details` método del controlador de departamento.

En *DepartmentsController.cs*, en la `Details` método, reemplace el código que recupera un departamento con un `FromSql` llamada al método, como se muestra en el código resaltado siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/DepartmentsController.cs?name=snippet_RawSQL&highlight=8,9,10,13)]

Para comprobar que el nuevo código funciona correctamente, seleccione la **departamentos** ficha y, a continuación, **detalles** para uno de los departamentos.

![Detalles del departamento](advanced/_static/department-details.png)

## <a name="call-a-query-that-returns-other-types"></a>Llamar a una consulta que devuelve otros tipos

Anteriormente, se crea una cuadrícula de estadísticas de estudiante de la página acerca de que el número de alumnos se ha explicado para cada fecha de inscripción. Obtuvo los datos en el conjunto de entidades de los alumnos (`_context.Students`) y utiliza LINQ para proyectar los resultados en una lista de `EnrollmentDateGroup` ver objetos del modelo. Suponga que desea escribir la instrucción SQL propia en lugar de usar LINQ. Para hacer que se necesita ejecutar una consulta SQL que devuelve un valor distinto de objetos entidad. En EF Core 1.0, una manera de hacerlo es escribir código de ADO.NET y obtener la conexión de base de datos de EF.

En *HomeController.cs*, reemplace el `About` método por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseRawSQL&highlight=3-32)]

Agregar un elemento using instrucción:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings2)]

Ejecute la aplicación y vaya a la página acerca de. Muestra los mismos datos que lo hacía anteriormente.

![Acerca de la página](advanced/_static/about.png)

## <a name="call-an-update-query"></a>Llamar a una consulta update

Imagine que desean que los administradores de la Universidad de Contoso realizar cambios globales en la base de datos, como cambiar el número de créditos para cada curso. Si la Universidad tiene un gran número de cursos, sería ineficaz para recuperarlos todos como entidades y cambiarlos de forma individual. En esta sección implementaremos una página web que permite al usuario especificar un factor por el que se va a cambiar el número de créditos para todos los cursos y podrá realizar el cambio mediante la ejecución de una instrucción UPDATE de SQL. La página web tendrá un aspecto similar de la ilustración siguiente:

![Página de créditos del curso de actualización](advanced/_static/update-credits.png)

En *CoursesContoller.cs*, agregue métodos UpdateCourseCredits para HttpGet y HttpPost:

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdateGet)]

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_UpdatePost)]

Cuando el controlador procesa una solicitud HttpGet, se devuelve nada en `ViewData["RowsAffected"]`, y la vista muestra un cuadro de texto vacío y un botón de envío, tal como se muestra en la ilustración anterior.

Cuando el **actualización** se hace clic en el botón, se llama al método HttpPost y multiplicador tiene el valor especificado en el cuadro de texto. El código, a continuación, ejecuta las instrucciones SQL que actualiza cursos y devuelve el número de filas afectadas a la vista en `ViewData`. Cuando se obtiene la vista de un `RowsAffected` valor, muestra el número de filas actualizadas.

En **el Explorador de soluciones**, haga clic en el *vistas/cursos* carpeta y, a continuación, haga clic en **Agregar > nuevo elemento**.

En el **Agregar nuevo elemento** cuadro de diálogo, haga clic en **ASP.NET** en **instalado** en el panel izquierdo, haga clic en **página vista de MVC**y el nombre de la nueva vista  *UpdateCourseCredits.cshtml*.

En *Views/Courses/UpdateCourseCredits.cshtml*, reemplace el código de plantilla con el código siguiente:

[!code-html[Main](intro/samples/cu/Views/Courses/UpdateCourseCredits.cshtml)]

Ejecute el `UpdateCourseCredits` método seleccionando la **cursos** ficha, a continuación, agregar "/ UpdateCourseCredits" al final de la dirección URL en la barra de direcciones del explorador (por ejemplo: `http://localhost:5813/Courses/UpdateCourseCredits`). Escriba un número en el cuadro de texto:

![Página de créditos del curso de actualización](advanced/_static/update-credits.png)

Haga clic en **Actualizar**. Ver el número de filas afectadas:

![Página de actualización curso créditos filas afectadas](advanced/_static/update-credits-rows-affected.png)

Haga clic en **volver a la lista** para ver la lista de cursos con el número de créditos revisado.

Tenga en cuenta que el código de producción garantiza que siempre actualiza el resultado de datos válido. El código simplificado que se muestra a continuación puede multiplicar al número de créditos suficiente para dar lugar a un número superior a 5. (El `Credits` propiedad tiene un `[Range(0, 5)]` atributo.) La consulta update podría funcionar, pero los datos no válidos pudieron producir resultados inesperados en otras partes del sistema que asumen que el número de créditos es 5 o menos.

Para obtener más información acerca de las consultas SQL sin formato, vea [sin procesar consultas SQL](https://docs.microsoft.com/ef/core/querying/raw-sql).

## <a name="examine-sql-sent-to-the-database"></a>Examinar SQL enviado a la base de datos

A veces resulta útil poder ver las consultas SQL reales que se envían a la base de datos. EF Core usa automáticamente la funcionalidad de registro integrados para ASP.NET Core para escribir los registros que contienen el código SQL para las consultas y actualizaciones. En esta sección podrá ver algunos ejemplos de inicio de sesión SQL.

Abra *StudentsController.cs* y en el `Details` método establece un punto de interrupción en el `if (student == null)` instrucción.

Ejecutar la aplicación en modo de depuración y vaya a la página de detalles acerca de un estudiante.

Vaya a la **salida** de salida de ventana que muestra la depuración, y verá que la consulta:

```
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (56ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT TOP(2) [s].[ID], [s].[Discriminator], [s].[FirstName], [s].[LastName], [s].[EnrollmentDate]
FROM [Person] AS [s]
WHERE ([s].[Discriminator] = N'Student') AND ([s].[ID] = @__id_0)
ORDER BY [s].[ID]
Microsoft.EntityFrameworkCore.Database.Command:Information: Executed DbCommand (122ms) [Parameters=[@__id_0='?'], CommandType='Text', CommandTimeout='30']
SELECT [s.Enrollments].[EnrollmentID], [s.Enrollments].[CourseID], [s.Enrollments].[Grade], [s.Enrollments].[StudentID], [e.Course].[CourseID], [e.Course].[Credits], [e.Course].[DepartmentID], [e.Course].[Title]
FROM [Enrollment] AS [s.Enrollments]
INNER JOIN [Course] AS [e.Course] ON [s.Enrollments].[CourseID] = [e.Course].[CourseID]
INNER JOIN (
    SELECT TOP(1) [s0].[ID]
    FROM [Person] AS [s0]
    WHERE ([s0].[Discriminator] = N'Student') AND ([s0].[ID] = @__id_0)
    ORDER BY [s0].[ID]
) AS [t] ON [s.Enrollments].[StudentID] = [t].[ID]
ORDER BY [t].[ID]
```

Verá algo aquí que actuales podrían sorprenderle: la instrucción SQL selecciona filas hasta 2 (`TOP(2)`) de la tabla Person. El `SingleOrDefaultAsync` método no se resuelve en 1 fila en el servidor. El motivo es el siguiente:

* Si la consulta devuelve varias filas, el método devuelve null.
* Para determinar si la consulta devuelve varias filas, EF tiene que comprobar si devuelve al menos 2.

Tenga en cuenta que no tienes que usar el modo de depuración y dejar a un punto de interrupción para obtener los resultados del registro en el **salida** ventana. Es una manera cómoda de detener el registro en el punto que desea buscar en la salida. Si no hacerlo, el registro continúa y tiene que desplazarse hacia atrás para encontrar los elementos que le interesa.

## <a name="repository-and-unit-of-work-patterns"></a>Repositorio y una unidad de patrones de trabajo

Muchos desarrolladores escriben código para implementar el repositorio y una unidad de patrones de trabajo como un contenedor alrededor de código que funcione con Entity Framework. Estos modelos están diseñados para crear una capa de abstracción entre la capa de acceso a datos y la capa de lógica empresarial de una aplicación. Implementación de estos modelos puede ayudar a aislar la aplicación de cambios en el almacén de datos y puede facilitar el desarrollo controlado por pruebas (TDD) o pruebas unitarias automatizadas. Sin embargo, escribir código adicional para implementar estos patrones no siempre es la mejor opción para las aplicaciones que usan EF, por varias razones:

* La propia clase de contexto EF aísla el código desde el código específico del almacén de datos.

* La clase de contexto EF puede actuar como una clase de unidad de trabajo para la base de datos de las actualizaciones que se hace con EF.

* EF incluye características para implementar TDD sin escribir código de repositorio.

Para obtener información sobre cómo implementar el repositorio y una unidad de patrones de trabajo, consulte [la versión de Entity Framework 5 de esta serie de tutoriales](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application).

Núcleo de Entity Framework implementa un proveedor de base de datos en memoria que puede utilizarse para realizar pruebas. Para obtener más información, consulte [pruebas con InMemory](https://docs.microsoft.com/ef/core/miscellaneous/testing/in-memory).

## <a name="automatic-change-detection"></a>Detección de cambios automático

Entity Framework determina cómo ha cambiado una entidad (y, por tanto, las actualizaciones que necesitan para enviarse a la base de datos) mediante la comparación de los valores actuales de una entidad con los valores originales. Cuando se consulta o se adjunta la entidad, se almacenan los valores originales. Algunos de los métodos que provocan la detección de cambios automático son los siguientes:

* DbContext.SaveChanges

* DbContext.Entry

* ChangeTracker.Entries

Si está realizando el seguimiento de un gran número de entidades y se llama a uno de estos métodos muchas veces en un bucle, podría obtener mejoras de rendimiento significativas desactivando temporalmente detección de cambios automático mediante el `ChangeTracker.AutoDetectChangesEnabled` propiedad. Por ejemplo:

```csharp
_context.ChangeTracker.AutoDetectChangesEnabled = false;
```

## <a name="entity-framework-core-source-code-and-development-plans"></a>Entity Framework Core origen código y planes de desarrollo

El origen de Entity Framework Core está en [https://github.com/aspnet/EntityFrameworkCore](https://github.com/aspnet/EntityFrameworkCore). El repositorio principal de EF contiene las compilaciones nocturnas, seguimiento de problemas, las especificaciones de la característica, notas, de las reuniones de diseño y [el plan de desarrollo futuro](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap). Puede archivos o buscar errores y contribuir.

Aunque el código fuente está abierto, Entity Framework Core es totalmente compatible como un producto de Microsoft. El equipo de Microsoft Entity Framework mantiene el control sobre el que se aceptan las contribuciones y comprueba todos los cambios de código para garantizar la calidad de cada versión.

## <a name="reverse-engineer-from-existing-database"></a>Ingeniería inversa de la base de datos existente

Para aplicar ingeniería inversa a un modelo de datos, incluidas las clases de entidad de una base de datos existente, use la [scaffold dbcontext](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell#scaffold-dbcontext) comando. Consulte la [tutorial de introducción](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db).

<a id="dynamic-linq"></a>
## <a name="use-dynamic-linq-to-simplify-sort-selection-code"></a>Utilizar LINQ dinámicos para simplificar código de selección de ordenación

El [tercer tutorial de esta serie](sort-filter-page.md) muestra cómo escribir código LINQ codificar de forma rígida los nombres de columna en un `switch` instrucción. Con dos columnas para elegir, esto funciona bien, pero si tiene muchas columnas el código podría considerarse detallado. Para solucionar este problema, puede usar el `EF.Property` método para especificar el nombre de la propiedad como una cadena. Para probar este enfoque, reemplace la `Index` método en la `StudentsController` con el código siguiente.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DynamicLinq)]

## <a name="next-steps"></a>Pasos siguientes

Con esto finaliza la siguiente serie de tutoriales sobre cómo utilizar el núcleo de Entity Framework en una aplicación ASP.NET MVC.

Para obtener más información sobre el núcleo de EF, consulte la [documentación de Entity Framework Core](https://docs.microsoft.com/ef/core). También está disponible un libro: [Entity Framework Core en acción](https://www.manning.com/books/entity-framework-core-in-action).

Para obtener información sobre cómo implementar una aplicación web, consulte [Host e implementar](xref:host-and-deploy/index).

Para obtener información acerca de otros temas relacionados con ASP.NET MVC de núcleo, como la autenticación y autorización, consulte la [documentación principal de ASP.NET](https://docs.microsoft.com/aspnet/core/).

## <a name="acknowledgments"></a>Confirmaciones

Tom Dykstra y Rick Anderson (twitter @RickAndMSFT) en este tutorial se escribió. Rowan Miller, Diego Vega y otros miembros del equipo de Entity Framework había asistida con revisiones de código y ayudó a depurar problemas que se produjeron mientras se estábamos escribiendo código para los tutoriales.

## <a name="common-errors"></a>Errores comunes  

### <a name="contosouniversitydll-used-by-another-process"></a>ContosoUniversity.dll utilizado por otro proceso

Mensaje de error:

> No se puede abrir ' … bin\Debug\netcoreapp1.0\ContosoUniversity.dll' para escritura: ' el proceso no puede tener acceso al archivo '... \bin\Debug\netcoreapp1.0\ContosoUniversity.dll' porque está siendo utilizado por otro proceso.

Solución:

Detener el sitio en IIS Express. Ir a la bandeja del sistema Windows, buscar IIS Express y haga clic en su icono, seleccione el sitio de la Universidad de Contoso y, a continuación, haga clic en **Detener sitio**.

### <a name="migration-scaffolded-with-no-code-in-up-and-down-methods"></a>Migración scaffolding sin código en métodos de arriba y abajo

Causa posible:

Los comandos de CLI de EF no cerrar automáticamente y guardar archivos de código. Si no ha guardado los cambios cuando se ejecuta el `migrations add` comando EF no encontrará los cambios.

Solución:

Ejecute el `migrations remove` de comandos, guardar los cambios de código y vuelva a ejecutar el `migrations add` comando.

### <a name="errors-while-running-database-update"></a>Errores durante la ejecución de actualización de base de datos

Es posible obtener otros errores al realizar cambios de esquema en una base de datos con los datos existentes. Si se producen errores de migración que no se puede resolver, puede cambiar el nombre de la base de datos en la cadena de conexión o eliminar la base de datos. Con una base de datos, no hay ningún dato para migrar y el comando de actualización de base de datos es mucho más probable que se completan sin errores.

El enfoque más sencillo consiste en cambiar el nombre de la base de datos *appSettings.JSON que se*. La próxima vez que ejecute `database update`, se creará una nueva base de datos.

Para eliminar una base de datos en SSOX, haga clic en la base de datos, haga clic en **eliminar**y, a continuación, en la **Eliminar base de datos** cuadro de diálogo, seleccione **cerrar conexiones existentes** y haga clic en  **Aceptar**.

Para eliminar una base de datos mediante el uso de la CLI, ejecute el `database drop` comando de CLI:

```console
dotnet ef database drop
```

### <a name="error-locating-sql-server-instance"></a>Instancia de SQL Server de localización de error

Mensaje de error:

> Se ha producido un error relacionado con la red o específico de la instancia al establecer una conexión a SQL Server. No se encontró el servidor o éste no estaba accesible. Compruebe que el nombre de la instancia es correcto y que SQL Server está configurado para admitir conexiones remotas. (proveedor: Interfaces de red SQL, error: 26 - Error Buscar servidor/instancia especificado)

Solución:

Compruebe la cadena de conexión. Si se ha eliminado manualmente el archivo de base de datos, cambiar el nombre de la base de datos en la cadena de construcción para volver a empezar con una base de datos.

>[!div class="step-by-step"]
[Anterior](inheritance.md)
