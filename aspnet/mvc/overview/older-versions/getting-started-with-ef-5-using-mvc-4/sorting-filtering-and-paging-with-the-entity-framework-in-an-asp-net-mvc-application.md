---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Ordenación, filtrado y paginación con Entity Framework en una aplicación ASP.NET MVC (3 de 10) | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 72180686ea1ee6f6695b24fcedf3af90c1eddcfa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839073"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Ordenación, filtrado y paginación con Entity Framework en una aplicación ASP.NET MVC (3 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio de este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y comience aquí.
> 
> > [!NOTE] 
> > 
> > Si surge un problema que no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, consulte [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior, ha implementado un conjunto de páginas web para las operaciones CRUD básicas `Student` entidades. En este tutorial agregará ordenación, filtrado y la funcionalidad de paginación a la **estudiantes** página de índice. También crearemos una página que realice agrupaciones sencillas.

En la siguiente ilustración se muestra el aspecto que tendrá la página cuando haya terminado. Los encabezados de columna son vínculos en los que el usuario puede hacer clic para ordenar las columnas correspondientes. Si se hace clic de forma consecutiva en el encabezado de una columna, el criterio de ordenación alterna entre ascendente y descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Agregar vínculos de ordenación de columnas en la página de índice de Students

Para agregar ordenación a la página de índice de Student, cambiará el `Index` método de la `Student` controlador y agregue código para el `Student` índice de la vista.

### <a name="add-sorting-functionality-to-the-index-method"></a>Agregar ordenación funcionalidad al método Index

En *Controllers\StudentController.cs*, reemplace el `Index` método con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Este código recibe un parámetro `sortOrder` de la cadena de consulta en la dirección URL. ASP.NET MVC proporciona el valor de cadena de consulta como un parámetro al método de acción. El parámetro es una cadena que puede ser "Name" o "Date", seguido (opcionalmente) por un guión bajo y la cadena "desc" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

La primera vez que se solicita la página de índice, no hay ninguna cadena de consulta. Los alumnos se muestran en orden ascendente por `LastName`, que es el valor predeterminado establecido por el caso desestimado en la `switch` instrucción. Cuando el usuario hace clic en un hipervínculo de encabezado de columna, se proporciona el valor `sortOrder` correspondiente en la cadena de consulta.

Los dos `ViewBag` se usan variables para que la vista puede configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuada:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Estas son las instrucciones ternarias. La primera de ellas especifica que si el `sortOrder` parámetro es null o está vacío, `ViewBag.NameSortParm` debe establecerse en "nombre\_desc"; en caso contrario, se debe establecer en una cadena vacía. Estas dos instrucciones habilitan la vista para establecer los hipervínculos de encabezado de columna de la forma siguiente:

| Criterio de ordenación actual | Hipervínculo de apellido | Hipervínculo de fecha |
| --- | --- | --- |
| Apellido: ascendente | descending | ascending |
| Apellido: descendente | ascending | ascending |
| Fecha: ascendente | ascending | descending |
| Fecha: descendente | ascending | ascending |

El método usa [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) para especificar la columna para ordenar por. El código crea un [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variable antes de la `switch` instrucción, se modifica en el `switch` instrucción y llama a la `ToList` método después de la `switch` instrucción. Al crear y modificar variables `IQueryable`, no se envía ninguna consulta a la base de datos. No se ejecuta la consulta hasta que convierta la `IQueryable` objeto en una colección mediante una llamada a un método como `ToList`. Por lo tanto, este código da como resultado una única consulta que no se ejecuta hasta el `return View` instrucción.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Agregar hipervínculos a la vista de índice de Student de encabezado de columna

En *Views\Student\Index.cshtml*, reemplace el `<tr>` y `<th>` elementos para la fila de encabezado con el código resaltado:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Este código usa la información de la `ViewBag` valores de cadena de propiedades para configurar hipervínculos con la consulta adecuada.

Ejecute la página y haga clic en el **apellido** y **Enrollment Date** funciona encabezados de columna para comprobar que la ordenación.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Tras hacer clic en el **apellido** encabezado, los alumnos se muestran de forma descendente de nombre de la última.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Agregar un cuadro de búsqueda a la página de índice de Students

Para agregar filtrado a la página de índice de Students, agregue un cuadro de texto y un botón de envío a la vista y haga los cambios correspondientes en el método `Index`. El cuadro de texto le permite escribir la cadena que quiera buscar en los campos de nombre y apellido.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar funcionalidad de filtrado al método Index

En *Controllers\StudentController.cs*, reemplace el `Index` método con el código siguiente (se resaltan los cambios):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Ha agregado un parámetro `searchString` al método `Index`. También ha agregado a la instrucción LINQ una `where` clausethat solo selecciona los alumnos cuyo nombre o apellido contienen la cadena de búsqueda. El valor de cadena de búsqueda se recibe desde un cuadro de texto que agregará a la vista de índice. La instrucción que agrega el [donde](https://msdn.microsoft.com/library/bb535040.aspx) cláusula se ejecuta solo si hay un valor que se busca.

> [!NOTE]
> En muchos casos puede llamar al mismo método en un conjunto de entidades de Entity Framework o como un método de extensión en una colección en memoria. Los resultados son normalmente los mismos, pero en algunos casos pueden ser diferentes. Por ejemplo, la implementación de .NET Framework de la `Contains` método devuelve todas las filas al pasar una cadena vacía a él, pero el proveedor de Entity Framework para SQL Server Compact 4.0 devuelve cero filas las cadenas están vacías. Por lo tanto, el código del ejemplo (poner el `Where` instrucción dentro de un `if` instrucción) se asegura de que obtendrá los mismos resultados para todas las versiones de SQL Server. Además, la implementación de .NET Framework la `Contains` método realiza una comparación entre mayúsculas y minúsculas de forma predeterminada, pero los proveedores de Entity Framework, SQL Server realizan comparaciones entre mayúsculas y minúsculas de forma predeterminada. Por lo tanto, una llamada a la `ToUpper` método para realizar la prueba explícitamente entre mayúsculas y minúsculas garantiza que los resultados no cambian al cambiar el código posterior para usar un repositorio, que devolverá una `IEnumerable` colección en lugar de un `IQueryable` objeto. (Al hacer una llamada al método `Contains` en una colección `IEnumerable`, obtendrá la implementación de .NET Framework; al hacer una llamada a un objeto `IQueryable`, obtendrá la implementación del proveedor de base de datos).


### <a name="add-a-search-box-to-the-student-index-view"></a>Agregar un cuadro de búsqueda a la vista de índice de Student

En *Views\Student\Index.cshtml*, agregue el código resaltado inmediatamente antes de la apertura `table` etiqueta con el fin de crear un título, un cuadro de texto y un **búsqueda** botón.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Ejecute la página, escriba una cadena de búsqueda y haga clic en **búsqueda** para comprobar que el filtrado funciona correctamente.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tenga en cuenta que la dirección URL no contiene "una" cadena de búsqueda, lo que significa que si marca esta página, no obtendrá la lista filtrada al usar el marcador. Va a cambiar el **búsqueda** botón para utilizar cadenas de consulta para los criterios de filtro más adelante en el tutorial.

## <a name="add-paging-to-the-students-index-page"></a>Agregar paginación a la página de índice de Students

Para agregar paginación a la página de índice de Students, empezará instalando el **PagedList.Mvc** paquete NuGet. A continuación, podrá realizar cambios adicionales en el `Index` método y agregue vínculos de paginación para el `Index` vista. **PagedList.Mvc** es uno de muchos paginación buena y ordenar los paquetes para ASP.NET MVC y aquí para su uso está pensado únicamente como ejemplo, no como una recomendación para ella a través de otras opciones. La siguiente ilustración muestra los vínculos de paginación.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instale el paquete PagedList.MVC NuGet

El paquete NuGet **PagedList.Mvc** paquete se instala automáticamente el **PagedList** paquete como una dependencia. El **PagedList** paquete instala una `PagedList` para los métodos de tipo y la extensión de recopilación `IQueryable` y `IEnumerable` colecciones. Los métodos de extensión crean una sola página de datos en un `PagedList` colección fuera de su `IQueryable` o `IEnumerable`y el `PagedList` colección proporciona varias propiedades y métodos que facilitan la paginación. El **PagedList.Mvc** paquete instala una aplicación auxiliar de paginación que muestra los botones de paginación.

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, **administrar paquetes de NuGet para la solución**.

En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en el **Online** pestaña a la izquierda y, a continuación, escriba "paginado" en el cuadro de búsqueda. Cuando vea el **PagedList.Mvc** del paquete, haga clic en **instalar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

En el **seleccionar proyectos** cuadro, haga clic en **Aceptar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Agregar funcionalidad de paginación al método Index

En *Controllers\StudentController.cs*, agregue un `using` instrucción para el `PagedList` espacio de nombres:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Reemplace el método `Index` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Este código agrega un `page` parámetro, un parámetro de criterio de ordenación actual y un parámetro de filtro actual para la firma del método, como se muestra aquí:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La primera vez que se muestra la página, o si el usuario no ha hecho clic en un vínculo de ordenación o paginación, todos los parámetros son nulos. Si se hace clic en un vínculo de paginación, la `page` variable contendrá el número de página para mostrar.

`A ViewBag` propiedad proporciona la vista con el criterio de ordenación actual, ya que esto debe incluirse en los vínculos de paginación para mantener el criterio de ordenación durante la paginación:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Otra propiedad, `ViewBag.CurrentFilter`, proporciona la vista con la cadena de filtro actual. Este valor debe incluirse en los vínculos de paginación para mantener la configuración de filtrado durante la paginación y debe restaurarse en el cuadro de texto cuando se vuelve a mostrar la página. Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede hacer que se muestren diferentes datos. Cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar, se cambia la cadena de búsqueda. En ese caso, el `searchString` parámetro no es null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Al final del método, el `ToPagedList` método de extensión en los estudiantes `IQueryable` objeto convierte la consulta del alumno en una sola página de alumnos de un tipo de colección que admita la paginación. Esa única página de alumnos, a continuación, se pasa a la vista:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El método `ToPagedList` toma un número de página. Los dos signos de interrogación representan el [operador de uso combinado de null](https://msdn.microsoft.com/library/ms173224.aspx). El operador de uso combinado de NULL define un valor predeterminado para un tipo que acepta valores NULL; la expresión `(page ?? 1)` devuelve el valor de `page` si tiene algún valor o devuelve 1 si `page` es NULL.

### <a name="add-paging-links-to-the-student-index-view"></a>Agregar vínculos de paginación a la vista de índice de Student

En *Views\Student\Index.cshtml*, reemplace el código existente por el código siguiente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

La instrucción `@model` de la parte superior de la página especifica que ahora la vista obtiene un objeto `PagedList` en lugar de un objeto `List`.

El `using` instrucción para `PagedList.Mvc` proporciona acceso a la aplicación auxiliar MVC para los botones de paginación.

El código usa una sobrecarga de [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que le permite especificar [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

El valor predeterminado [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) envía datos de formulario con POST, lo que significa que los parámetros se pasan en el cuerpo del mensaje HTTP y no en la dirección URL como cadenas de consulta. Al especificar HTTP GET, los datos de formulario se pasan en la dirección URL como cadenas de consulta, lo que permite que los usuarios marquen la dirección URL. El [directrices de W3C para el uso de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) especificar que deben utilizar GET cuando la acción no produzca ninguna actualización.

El cuadro de texto se inicializa con la cadena de búsqueda actual, por lo que al hacer clic en una nueva página puede ver la cadena de búsqueda actual.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Los vínculos del encabezado de la columna usan la cadena de consulta para pasar la cadena de búsqueda actual al controlador, de modo que el usuario pueda ordenar los resultados del filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Se muestra el número actual de página y el total de páginas.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Si no hay ninguna página para mostrar, se muestra la "Página 0 0". (En ese caso el número de página es mayor que el número de páginas porque `Model.PageNumber` es 1, y `Model.PageCount` es 0.)

Se muestran los botones de paginación mediante el `PagedListPager` auxiliar:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

El `PagedListPager` auxiliar proporciona una serie de opciones que se pueden personalizar, incluidas direcciones URL y aplicación de estilos. Para obtener más información, consulte [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) en el sitio de GitHub.

Ejecute la página.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que la paginación funciona correctamente. A continuación, escriba una cadena de búsqueda e intente llevar a cabo la paginación de nuevo, para comprobar que la paginación también funciona correctamente con filtrado y ordenación.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Crear un sobre la página que muestra las estadísticas de alumno

Para Contoso University del sitio Web acerca de la página, se muestran cuántos alumnos se han inscrito por cada fecha de inscripción. Esto requiere realizar agrupaciones y cálculos sencillos en los grupos. Para conseguirlo, haga lo siguiente:

- Cree una clase de modelo de vista para los datos que necesita pasar a la vista.
- Modificar el `About` método en el `Home` controlador.
- Modificar el `About` vista.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Crear un *ViewModels* carpeta. En esa carpeta, agregue un archivo de clase *EnrollmentDateGroup.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificación del controlador Home

En *HomeController.cs*, agregue las siguientes `using` instrucciones en la parte superior del archivo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Agregue una variable de clase para el contexto de base de datos inmediatamente después de la llave de apertura para la clase:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Reemplace el método `About` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula la cantidad de entidades que se incluyen en cada grupo y almacena los resultados en una colección de objetos de modelo de la vista `EnrollmentDateGroup`.

Agregar un `Dispose` método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificación de la vista About

Reemplace el código en el *Views\Home\About.cshtml* archivo con el código siguiente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Ejecute la aplicación y haga clic en el **sobre** vínculo. En una tabla se muestra el número de alumnos para cada fecha de inscripción.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcional: Implementar la aplicación en Windows Azure

Hasta ahora la aplicación se ejecutaba localmente en IIS Express en el equipo de desarrollo. Para que esté disponible para otras personas a través de Internet, tendrá que implementarlo en un proveedor de hospedaje web. En esta sección opcional del tutorial podrá implementarlo en un sitio Web de Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Uso de migraciones de Code First para implementar la base de datos

Para implementar la base de datos usará migraciones de Code First. Cuando se crea el perfil de publicación que usar para configurar las opciones de implementación desde Visual Studio, seleccionará una casilla de verificación con la etiqueta **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)**. Esta configuración hace que el proceso de implementación configurar automáticamente la aplicación *Web.config* de archivos en el servidor de destino para que utilice Code First la `MigrateDatabaseToLatestVersion` clase de inicializador.

Visual Studio no hace nada con la base de datos durante el proceso de implementación. Cuando la aplicación implementada se accede a la base de datos por primera vez después de la implementación, Code First automáticamente la base de datos se crea o actualiza el esquema de base de datos a la versión más reciente. Si la aplicación implementa un migraciones `Seed` método, se ejecuta el método después de la base de datos se crea o actualiza el esquema.

Las migraciones `Seed` método inserta datos de prueba. Si se implementa en un entorno de producción, tendría que cambiar el `Seed` método por lo que TI sólo inserta los datos que se van a insertarse en la base de datos de producción. Por ejemplo, en el modelo de datos actual es posible que desee tener cursos real pero estudiantes ficticios en la base de datos de desarrollo. Puede escribir un `Seed` método para cargar ambos en el desarrollo y, a continuación, marque como comentario los alumnos ficticios antes de implementar en producción. También se puede escribir un `Seed` método para cargar solo los cursos y escribir manualmente los alumnos ficticios en la base de datos de prueba mediante el uso de la interfaz de usuario de la aplicación.

### <a name="get-a-windows-azure-account"></a>Obtener una cuenta de Windows Azure

Necesitará una cuenta de Windows Azure. Si aún no tiene uno, puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Crear un sitio web y una base de datos SQL en Windows Azure

El sitio Web de Windows Azure se ejecutará en un entorno de hospedaje compartido, lo que significa que se ejecuta en máquinas virtuales (VM) que se comparten con otros clientes de Windows Azure. Un entorno de hospedaje compartido es una manera de bajo costo para empezar a trabajar en la nube. Más adelante, si aumenta el tráfico web, la aplicación puede escalar para satisfacer las necesidades de forma que se ejecutan en máquinas virtuales dedicadas. Si necesita una arquitectura más compleja, puede migrar a un servicio de nube de Windows Azure. Ejecutan servicios en la nube en máquinas virtuales dedicadas que se pueden configurar según sus necesidades.

Windows Azure SQL Database es un servicio de base de datos relacional basado en la nube que se basa en tecnologías de SQL Server. Herramientas y aplicaciones que funcionan con SQL Server también funcionan con SQL Database.

1. En el [Portal de administración de Windows Azure](https://manage.windowsazure.com/), haga clic en **sitios Web** en la pestaña de la izquierda y, a continuación, haga clic en **New**.

    ![Botón nuevo en el Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Haga clic en **creación personalizada**.

    ![Creación de vínculo de la base de datos en el Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   El **nuevo sitio Web - Creación personalizada** abre el asistente.
3. En el **nuevo sitio Web** paso del asistente, escriba una cadena en el **URL** cuadro que se usará como la dirección URL única para la aplicación. La dirección URL completa consistirá en lo que escriba más el sufijo que aparece junto al cuadro de texto. En la ilustración se muestra "ConU", pero probablemente se haya usado esa dirección URL por lo que tendrá que elegir una diferente.

    ![Creación de vínculo de la base de datos en el Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. En el **región** lista desplegable, elija una región cercana a usted. Esta configuración especifica el sitio web se ejecutará en el centro de datos.
5. En el **base de datos** lista desplegable, elija **crear una base de datos SQL de 20 MB gratuita**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. En el **nombre de cadena de conexión de base de datos**, escriba *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Haga clic en la flecha que señala a la derecha en la parte inferior del cuadro. El asistente avanza a la **configuración de la base de datos** paso.
8. En el **nombre** , escriba *ContosoUniversityDB*.
9. En el **Server** cuadro, seleccione **el servidor de base de datos SQL nueva**. Como alternativa, si ha creado anteriormente un servidor, puede seleccionar ese servidor en la lista desplegable.
10. Especifique un administrador **nombre de inicio de sesión** y **contraseña**. Si seleccionó **el servidor de base de datos SQL nueva** no escribirá un nombre existente y la contraseña aquí, que inserte un nuevo nombre y una contraseña que definirá ahora para usarlo más adelante cuando acceda a la base de datos. Si ha seleccionado un servidor que creó anteriormente, deberá escribir las credenciales para ese servidor. Para este tutorial, no seleccione la ***avanzadas*** casilla de verificación. El ***avanzadas*** opciones le permiten establecer la base de datos [intercalación](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Elija la misma **región** que eligió para el sitio web.
12. Haga clic en la marca de verificación en la parte inferior derecha de la casilla para indicar que haya terminado.   
  
    ![Paso de configuración de la base de datos del nuevo sitio Web de-crear con el Asistente para la base de datos](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    La siguiente imagen muestra el uso de un servidor SQL Server existente y el inicio de sesión.   
  
    ![Paso de configuración de la base de datos del nuevo sitio Web de-crear con el Asistente para la base de datos](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    El Portal de administración que se devuelve a la página de sitios Web y el **estado** columna muestra que se está creando el sitio. Después de un tiempo (normalmente menos de un minuto), el **estado** columna muestra que el sitio se creó correctamente. En la barra de navegación a la izquierda el número de sitios que tiene en su cuenta aparece junto a la **sitios Web** aparece junto al icono y el número de bases de datos la **bases de datos SQL** icono.

## <a name="deploy-the-application-to-windows-azure"></a>Implementar la aplicación en Windows Azure

1. En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.  
  
    ![Publicar en el menú contextual del proyecto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. En el **perfil** pestaña de la **publicación Web** asistente, haga clic en **importación**.  
  
    ![Importar configuración de publicación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Si no ha agregado anteriormente la suscripción de Azure en Visual Studio, realice los pasos siguientes. En estos pasos agregue su suscripción para que la lista desplegable lista **importar desde un sitio web de Windows Azure** incluirá su sitio web.

    a. En el **importar perfil de publicación** cuadro de diálogo, haga clic en **importar desde un sitio web de Windows Azure**y, a continuación, haga clic en **suscripción agregar Windows Azure**.

    ![Agregar suscripción a Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. En el **Importar suscripciones de Windows Azure** cuadro de diálogo, haga clic en **Descargar archivo de suscripción**.

    ![Descargar archivo de suscripción](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. En la ventana del explorador, guarde el *.publishsettings* archivo.

    ![Descargue el archivo .publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Seguridad: el *publishsettings* archivo contiene sus credenciales (sin codificar) que se usan para administrar sus servicios y suscripciones de Windows Azure. La práctica recomendada de seguridad para este archivo consiste en almacenarlo temporalmente fuera de los directorios de origen (por ejemplo, en el *bibliotecas\documentos* carpeta) y, a continuación, eliminarlo una vez completada la importación. Un usuario malintencionado que obtuviera acceso a la `.publishsettings` archivo puede editar, crear y eliminar sus servicios de Windows Azure.

    d. En el **Importar suscripciones de Windows Azure** cuadro de diálogo, haga clic en **examinar** y navegue hasta la *.publishsettings* archivo.

    ![Descargar sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Haga clic en **importación**.

    ![importación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. En el **importar perfil de publicación** cuadro de diálogo, seleccione **importar desde un sitio web de Windows Azure**, seleccione el sitio web de la lista desplegable y, a continuación, haga clic en **Aceptar**.  
  
    ![Importar perfil de publicación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. En el **conexión** , haga clic **validar conexión** para asegurarse de que la configuración es correcta.  
  
    ![Validar la conexión](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Cuando se haya validado la conexión, se muestra junto a una marca de verificación verde el **validar conexión** botón. Haga clic en **Siguiente**.  
  
    ![Conexión validada correctamente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Abra el **cadena de conexión remota** lista desplegable bajo **SchoolContext** y seleccione la cadena de conexión para la base de datos que creó.
8. Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciar la aplicación)**.
9. Desactive la opción **usar esta cadena de conexión en tiempo de ejecución** para el **UserContext (DefaultConnection)**, ya que esta aplicación no usa la base de datos de pertenencia.   
  
    ![Pestaña Configuración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Haga clic en **Siguiente**.
11. En el **Preview** , haga clic **iniciar vista previa**.  
  
    ![Botón de inicio de la pestaña de vista previa](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    La pestaña muestra una lista de los archivos que se copiarán en el servidor. Mostrar la vista previa, no es necesario publicar la aplicación, pero es una función útil tener en cuenta. En este caso, no es necesario hacer nada con la lista de archivos que se muestra. La próxima vez que implemente esta aplicación, que será sólo los archivos que han cambiado en esta lista.  
  
    ![Salida de archivo de inicio](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Haga clic en **Publicar**.  
    Visual Studio comienza el proceso de copiar los archivos en el servidor de Windows Azure.
13. El **salida** ventana muestra qué acciones de implementación se realizaron e informa la correcta finalización de la implementación.  
  
    ![Ventana de salida de informes de implementación correcta](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Tras una implementación correcta, el explorador predeterminado se abre automáticamente en la dirección URL del sitio web implementado.  
    Ahora se ejecuta la aplicación que creó en la nube. Haga clic en la pestaña Students.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

En este momento su *SchoolContext* base de datos se ha creado en la base de datos de SQL de Windows Azure porque seleccionó **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**. El *Web.config* en el sitio web implementado, ha sido modificado para que la [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador ejecutaría la primera vez que el código lee o escribe datos en la base de datos (que se produjo cuando selecciona el **estudiantes** pestaña):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

El proceso de implementación también crea una nueva cadena de conexión *(SchoolContext\_DatabasePublish*) para migraciones de Code First que se usará para actualizar el esquema de base de datos y la propagación de la base de datos.

![Cadena de conexión Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

El *DefaultConnection* es de cadena de conexión para la base de datos de pertenencia (que no vamos a usar en este tutorial). El *SchoolContext* es de cadena de conexión para la base de datos ContosoUniversity.

Puede encontrar la versión implementada del archivo Web.config en su propio equipo en *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Puede acceder a implementado *Web.config* propio archivo mediante FTP. Para obtener instrucciones, consulte [implementación Web de ASP.NET con Visual Studio: implementar una actualización de código](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Siga las instrucciones que comienzan con "para usar una herramienta FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña."

> [!NOTE]
> La aplicación web no implementa la seguridad, por lo que cualquier persona que encuentre la dirección URL puede cambiar los datos. Para obtener instrucciones sobre cómo proteger el sitio web, consulte [implementar una aplicación ASP.NET MVC segura con suscripción, OAuth y SQL Database a un sitio Web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Puede impedir que otras personas usando el sitio mediante el Portal de administración de Windows Azure o **Explorador de servidores** en Visual Studio para detener el sitio.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Código primeros inicializadores

En la sección de implementación vio el [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador que se va a usar. Código primero también proporciona otros inicializadores que puede usar, incluidas [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (valor predeterminado), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) y [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). El `DropCreateAlways` inicializador puede ser útil para configurar las condiciones para las pruebas unitarias. También puede escribir a sus propios inicializadores, y se puede llamar a un inicializador explícitamente si no desea esperar hasta que la aplicación lee o escribe en la base de datos. Para obtener una explicación completa de inicializadores, consulte el capítulo 6 del libro [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) , Julie Lerman y Rowan Miller.

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo crear un modelo de datos e implementar CRUD básicas, ordenación, filtrado, paginación y funcionalidad de agrupación. En el siguiente tutorial comenzará examinando temas más avanzados expandiendo el modelo de datos.

Pueden encontrar vínculos a otros recursos de Entity Framework en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Siguiente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
