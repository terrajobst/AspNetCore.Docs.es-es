---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Ordenación, filtrado y paginación con Entity Framework en una aplicación de MVC de ASP.NET (3 de 10) | Documentos de Microsoft
author: tdykstra
description: La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 mediante Code First de Entity Framework 5 y Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 09327b760d9be38d7e004cbcef08cad4eab3a26c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>Ordenación, filtrado y paginación con Entity Framework en una aplicación de MVC de ASP.NET (3 de 10)
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 4 con Code First de Entity Framework 5 y Visual Studio 2012. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Puede iniciar la serie de tutoriales desde el principio o [descargar un proyecto de inicio para este capítulo](building-the-ef5-mvc4-chapter-downloads.md) y empiece aquí.
> 
> > [!NOTE] 
> > 
> > Si experimenta un problema no se puede resolver, [descargar el capítulo completado](building-the-ef5-mvc4-chapter-downloads.md) e intente reproducir el problema. Por lo general puede encontrar la solución al problema comparando el código para el código completo. Para que algunos errores comunes y cómo resolverlos, vea [errores y soluciones alternativas.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


En el tutorial anterior implementa un conjunto de páginas web para las operaciones CRUD básicas para `Student` entidades. En este tutorial agregará la ordenación, el filtrado y la funcionalidad de paginación para la **estudiantes** página de índice. También crearemos una página que realice agrupaciones sencillas.

En la siguiente ilustración se muestra el aspecto que tendrá la página cuando haya terminado. Los encabezados de columna son vínculos en los que el usuario puede hacer clic para ordenar las columnas correspondientes. Si se hace clic de forma consecutiva en el encabezado de una columna, el criterio de ordenación alterna entre ascendente y descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Agregar vínculos de ordenación de columnas en la página de índice de Students

Para agregar ordenación, en la página de índice de estudiantes, cambiará la `Index` método de la `Student` controlador y agregar código a la `Student` indizar la vista.

### <a name="add-sorting-functionality-to-the-index-method"></a>Agregar funcionalidad para el método de índice de ordenación

En *Controllers\StudentController.cs*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Este código recibe un parámetro `sortOrder` de la cadena de consulta en la dirección URL. El valor de cadena de consulta se proporciona mediante ASP.NET MVC como un parámetro al método de acción. El parámetro es una cadena que puede ser "Name" o "Date", seguido (opcionalmente) por un guión bajo y la cadena "desc" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

La primera vez que se solicita la página de índice, no hay ninguna cadena de consulta. Los alumnos se muestran en orden ascendente por `LastName`, que es el valor predeterminado establecido por el caso paso explícito en el `switch` instrucción. Cuando el usuario hace clic en un hipervínculo de encabezado de columna, se proporciona el valor `sortOrder` correspondiente en la cadena de consulta.

Los dos `ViewBag` las variables se usan para que la vista puede configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuada:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Estas son las instrucciones ternarias. La primera de ellas especifica que si el `sortOrder` parámetro es null o está vacío, `ViewBag.NameSortParm` debe establecerse en "nombre\_desc"; en caso contrario, se debe establecer en una cadena vacía. Estas dos instrucciones habilitan la vista para establecer los hipervínculos de encabezado de columna de la forma siguiente:

| Criterio de ordenación actual | Hipervínculo de apellido | Hipervínculo de fecha |
| --- | --- | --- |
| Apellido: ascendente | descending | ascending |
| Apellido: descendente | ascending | ascending |
| Fecha: ascendente | ascending | descending |
| Fecha: descendente | ascending | ascending |

El método usa [LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx) para especificar la columna para ordenar por. El código crea un [IQueryable](https://msdn.microsoft.com/library/bb351562.aspx) variable antes de la `switch` modifica la instrucción, en la `switch` instrucción y llamadas a la `ToList` método después el `switch` instrucción. Al crear y modificar variables `IQueryable`, no se envía ninguna consulta a la base de datos. La consulta no se ejecuta hasta que convierta el `IQueryable` objeto en una colección mediante una llamada a un método como `ToList`. Por lo tanto, este código produce una única consulta que no se ejecuta hasta que el `return View` instrucción.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Agregar hipervínculos a la vista de índice de estudiante de encabezado de columna

En *Views\Student\Index.cshtml*, reemplace la `<tr>` y `<th>` elementos de la fila de encabezado con el código resaltado:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Este código usa la información de la `ViewBag` valores de cadena de propiedades para configurar hipervínculos con la consulta adecuada.

Ejecute la página y haga clic en el **Last Name** y **fecha de inscripción** funciona encabezados de columna para comprobar que la ordenación.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Tras hacer clic en el **Last Name** encabezado, los alumnos se muestran de forma descendente último nombre.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Agregue un cuadro de búsqueda a la página de índice de estudiantes

Para agregar filtrado a la página de índice de Students, agregue un cuadro de texto y un botón de envío a la vista y haga los cambios correspondientes en el método `Index`. El cuadro de texto le permite escribir la cadena que quiera buscar en los campos de nombre y apellido.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar funcionalidad de filtrado para el método de índice

En *Controllers\StudentController.cs*, reemplace el `Index` método por el código siguiente (los cambios aparecen resaltados):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Ha agregado un parámetro `searchString` al método `Index`. También ha agregado a la instrucción LINQ un `where` clausethat selecciona sólo los estudiantes cuyo apellido o apellidos contienen la cadena de búsqueda. El valor de cadena de búsqueda se recibe de un cuadro de texto que agregará a la vista de índice. La instrucción que agrega el [donde](https://msdn.microsoft.com/library/bb535040.aspx) cláusula se ejecuta sólo si hay un valor que se buscará.

> [!NOTE]
> En muchos casos puede llamar al mismo método en un conjunto de entidades de Entity Framework o como un método de extensión en una colección en memoria. Los resultados son normalmente los mismos pero en algunos casos pueden ser diferentes. Por ejemplo, la implementación de .NET Framework de la `Contains` método devuelve todas las filas cuando se pasa una cadena vacía a él, pero el proveedor de Entity Framework para SQL Server Compact 4.0 devuelve cero filas para las cadenas vacías. Por lo tanto, el código del ejemplo (colocar el `Where` instrucción dentro de un `if` instrucción) se asegura de que obtendrá los mismos resultados para todas las versiones de SQL Server. Además, la implementación de .NET Framework de la `Contains` método realiza una comparación entre mayúsculas y minúsculas de forma predeterminada, pero los proveedores de Entity Framework SQL Server realizan comparaciones entre mayúsculas y minúsculas de forma predeterminada. Por lo tanto, al llamar a la `ToUpper` método para realizar la prueba explícitamente entre mayúsculas y minúsculas se asegura de que los resultados no cambian cuando cambia el código de una versión posterior para usar un repositorio, lo que devolverá una `IEnumerable` colección en lugar de un `IQueryable` objeto. (Al hacer una llamada al método `Contains` en una colección `IEnumerable`, obtendrá la implementación de .NET Framework; al hacer una llamada a un objeto `IQueryable`, obtendrá la implementación del proveedor de base de datos).


### <a name="add-a-search-box-to-the-student-index-view"></a>Agregar un cuadro de búsqueda a la vista de índice de Student

En *Views\Student\Index.cshtml*, agregue el código resaltado inmediatamente antes de la apertura `table` etiqueta con el fin de crear un título, un cuadro de texto y un **búsqueda** botón.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Ejecute la página, escriba una cadena de búsqueda y haga clic en **búsqueda** para comprobar que el filtrado está funcionando.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tenga en cuenta que la dirección URL no contiene "una" cadena de búsqueda, lo que significa que si Marque esta página, no obtendrá la lista filtrada cuando se utiliza el marcador. Cambiará la **búsqueda** botón que se usa cadenas de consulta para los criterios de filtro, más adelante en el tutorial.

## <a name="add-paging-to-the-students-index-page"></a>Agregar paginación a la página de índice de estudiantes

Para agregar paginación a la página de índice de estudiantes, podrá empezar mediante la instalación de la **PagedList.Mvc** paquete NuGet. A continuación, podrá realizar cambios adicionales en el `Index` (método) y agregar vínculos de paginación a la `Index` vista. **PagedList.Mvc** es uno de los muchos paginación buena y ordenar paquetes para ASP.NET MVC y su uso aquí está pensado únicamente como ejemplo, no como una recomendación para él a través de otras opciones. La ilustración siguiente muestra los vínculos de paginación.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instale el paquete PagedList.MVC NuGet

NuGet **PagedList.Mvc** paquete instala automáticamente el **PagedList** paquete como una dependencia. El **PagedList** paquete instala un `PagedList` para los métodos de tipo y la extensión de recopilación `IQueryable` y `IEnumerable` colecciones. Los métodos de extensión crean una sola página de datos en un `PagedList` colección fuera de su `IQueryable` o `IEnumerable`y el `PagedList` colección proporciona varias propiedades y métodos que facilitan la paginación. El **PagedList.Mvc** paquete instala una aplicación auxiliar de paginación que muestra los botones de paginación.

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, **administrar paquetes de NuGet para la solución**.

En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en el **Online** ficha de la izquierda y, a continuación, escriba "paginado" en el cuadro de búsqueda. Cuando aparezca la **PagedList.Mvc** del paquete, haga clic en **instalar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

En el **seleccionar proyectos** cuadro, haga clic en **Aceptar**.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>Agregar funcionalidad de paginación para el método de índice

En *Controllers\StudentController.cs*, agregue un `using` instrucción para el `PagedList` espacio de nombres:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Reemplace el método `Index` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Este código agrega un `page` parámetro, un parámetro de criterio de ordenación actual y un parámetro de filtro actual para la firma del método, como se muestra aquí:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La primera vez que se muestra la página, o si el usuario no ha hecho clic en un vínculo de ordenación o paginación, todos los parámetros son nulos. Si se hace clic en un vínculo de paginación, el `page` variable contendrá el número de página para mostrar.

`A ViewBag` propiedad proporciona la vista con el criterio de ordenación actual, porque esto debe estar incluida en los vínculos de paginación para mantener el criterio de ordenación de los mismos durante la paginación:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Otra propiedad, `ViewBag.CurrentFilter`, proporciona la vista con la cadena de filtro actual. Este valor debe incluirse en los vínculos de paginación para mantener la configuración de filtrado durante la paginación y debe restaurarse en el cuadro de texto cuando se vuelve a mostrar la página. Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede hacer que se muestren diferentes datos. Cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar, se cambia la cadena de búsqueda. En ese caso, el `searchString` parámetro no es null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Al final del método, el `ToPagedList` método de extensión en los alumnos `IQueryable` objeto convierte la consulta de estudiante en una sola página de alumnos de un tipo de colección que admite la paginación. Esa única página de estudiantes, a continuación, se pasa a la vista:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El método `ToPagedList` toma un número de página. Los dos signos de interrogación representan el [operador de uso combinado de null](https://msdn.microsoft.com/library/ms173224.aspx). El operador de uso combinado de NULL define un valor predeterminado para un tipo que acepta valores NULL; la expresión `(page ?? 1)` devuelve el valor de `page` si tiene algún valor o devuelve 1 si `page` es NULL.

### <a name="add-paging-links-to-the-student-index-view"></a>Agregar vínculos de paginación a la vista de índice de estudiante

En *Views\Student\Index.cshtml*, reemplace el código existente por el código siguiente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

La instrucción `@model` de la parte superior de la página especifica que ahora la vista obtiene un objeto `PagedList` en lugar de un objeto `List`.

El `using` instrucción para `PagedList.Mvc` proporciona acceso a la aplicación auxiliar MVC para los botones de paginación.

El código usa una sobrecarga de [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que le permite especificar [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

El valor predeterminado [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) envía datos del formulario con una entrada, lo que significa que se pasan en el cuerpo del mensaje HTTP y no en la dirección URL como las cadenas de consulta de parámetros. Al especificar HTTP GET, los datos de formulario se pasan en la dirección URL como cadenas de consulta, lo que permite que los usuarios marquen la dirección URL. El [directrices de W3C para el uso de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) especificar que debe utilizar GET cuando la acción no da como resultado una actualización.

El cuadro de texto se inicializa con la cadena de búsqueda actual para que al hacer clic en una nueva página que pueda ver la cadena de búsqueda actual.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Los vínculos del encabezado de la columna usan la cadena de consulta para pasar la cadena de búsqueda actual al controlador, de modo que el usuario pueda ordenar los resultados del filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Se muestra el número actual de página y el total de páginas.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Si no hay ninguna página para mostrar, se muestra "Página 0 0". (En ese caso el número de página es mayor que el recuento de páginas porque `Model.PageNumber` es 1, y `Model.PageCount` es 0.)

Se muestran los botones de paginación mediante el `PagedListPager` auxiliar:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

El `PagedListPager` auxiliar proporciona una serie de opciones que se pueden personalizar, incluidas las direcciones URL y aplicación de estilos. Para obtener más información, consulte [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) en el sitio de GitHub.

Ejecute la página.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que la paginación funciona correctamente. A continuación, escriba una cadena de búsqueda e intente llevar a cabo la paginación de nuevo, para comprobar que la paginación también funciona correctamente con filtrado y ordenación.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Crear un sobre la página que muestra estadísticas de estudiante

Para la Universidad de Contoso del sitio Web acerca de la página, mostrará los alumnos cuántos han inscrito para cada fecha de inscripción. Esto requiere realizar agrupaciones y cálculos sencillos en los grupos. Para conseguirlo, haga lo siguiente:

- Cree una clase de modelo de vista para los datos que necesita pasar a la vista.
- Modificar el `About` método en el `Home` controlador.
- Modificar el `About` vista.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Crear un *ViewModels* carpeta. En esa carpeta, agregue un archivo de clase *EnrollmentDateGroup.cs* y reemplace el código existente por el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modificación del controlador Home

En *HomeController.cs*, agregue las siguientes `using` las instrucciones en la parte superior del archivo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Agregue una variable de clase para el contexto de base de datos inmediatamente después de la llave de apertura para la clase:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Reemplace el método `About` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula la cantidad de entidades que se incluyen en cada grupo y almacena los resultados en una colección de objetos de modelo de la vista `EnrollmentDateGroup`.

Agregar un `Dispose` método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificación de la vista About

Reemplace el código de la *Views\Home\About.cshtml* archivo con el código siguiente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Ejecutar la aplicación y haga clic en el **sobre** vínculo. En una tabla se muestra el número de alumnos para cada fecha de inscripción.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>Opcional: Implementar la aplicación en Windows Azure

Hasta ahora la aplicación ha se ejecuta de manera localmente en IIS Express en el equipo de desarrollo. Para que esté disponible para otras personas a través de Internet, se debe implementar en un proveedor de hospedaje web. En esta sección opcional del tutorial podrá implementarlo en un sitio Web de Windows Azure.

### <a name="using-code-first-migrations-to-deploy-the-database"></a>Uso de migraciones de Code First para implementar la base de datos

Para implementar la base de datos deberá usar migraciones de Code First. Cuando se crea el perfil de publicación que utiliza para configurar las opciones de implementación desde Visual Studio, seleccionará una casilla que tiene la etiqueta **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**. Esta configuración hace que el proceso de implementación configurar automáticamente la aplicación *Web.config* de archivos en el servidor de destino para que utilice Code First la `MigrateDatabaseToLatestVersion` clase inicializador.

Visual Studio no hace nada con la base de datos durante el proceso de implementación. Cuando la aplicación implementada tiene acceso a la base de datos por primera vez después de la implementación, Code First creará automáticamente la base de datos o actualiza el esquema de base de datos a la versión más reciente. Si la aplicación implementa una migraciones `Seed` método, se ejecuta el método después de la base de datos se crea o se actualiza el esquema.

Las migraciones `Seed` método inserta datos de prueba. Si se han a implementar en un entorno de producción, deberá cambiar el `Seed` método por lo que TI sólo inserta los datos que se van a insertar en la base de datos de producción. Por ejemplo, en el modelo de datos actual debe tener cursos reales, pero los alumnos ficticios en la base de datos de desarrollo. Puede escribir una `Seed` para cargar en desarrollo y, a continuación, convierta en comentario los alumnos ficticios antes de implementar en producción. O bien puede escribir un `Seed` método para cargar solo los cursos y escribir manualmente los alumnos ficticios en la base de datos de prueba mediante el uso de la interfaz de usuario de la aplicación.

### <a name="get-a-windows-azure-account"></a>Obtener una cuenta de Windows Azure

Necesitará una cuenta de Windows Azure. Si aún no tiene uno, puede crear una cuenta de prueba gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Windows Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>Crear un sitio web y una base de datos SQL en Windows Azure

El sitio Web de Windows Azure se ejecutará en un entorno de hospedaje compartido, lo que significa que se ejecuta en máquinas virtuales (VM) que se comparten con otros clientes de Windows Azure. Un entorno de hospedaje compartido es una manera de bajo coste para empezar a trabajar en la nube. Más adelante, si aumenta el tráfico web, puede escalar la aplicación para satisfacer la necesidad de mediante la ejecución en máquinas virtuales dedicadas. Si necesita una arquitectura más compleja, puede migrar a un servicio de nube de Windows Azure. Servicios en la nube que se ejecutan en máquinas virtuales dedicadas que se pueden configurar según sus necesidades.

Base de datos de SQL de Windows Azure es un servicio de base de datos relacional en la nube que se basa en tecnologías de SQL Server. Herramientas y aplicaciones que funcionan con SQL Server también funcionan con la base de datos SQL.

1. En el [Portal de administración de Windows Azure](https://manage.windowsazure.com/), haga clic en **sitios Web** en la pestaña de la izquierda y, a continuación, haga clic en **nuevo**.

    ![Botón nuevo en el Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. Haga clic en **creación personalizada**.

    ![Crear con un vínculo de base de datos en el Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   El **nuevo sitio Web: creación personalizada** abre el asistente.
3. En el **nuevo sitio Web** paso del asistente, escriba una cadena en el **URL** cuadro que se usará como la dirección URL única para la aplicación. La dirección URL completa constará de las que se especifican aquí más el sufijo que verá junto al cuadro de texto. La ilustración muestra "ConU", pero esa dirección URL probablemente se realiza por lo que tendrá que elegir una diferente.

    ![Crear con un vínculo de base de datos en el Portal de administración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. En el **región** lista desplegable lista, elija una región más cercanos. Esta configuración especifica el sitio web se ejecutará en qué centro de datos.
5. En el **base de datos** desplegable Elija **crear una base de datos SQL de 20 MB gratuita**.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. En el **nombre de cadena de conexión de base de datos**, escriba *SchoolContext*.

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. Haga clic en la flecha que señala a la derecha en la parte inferior del cuadro. El asistente avanza a la **configuración de base de datos** paso.
8. En el **nombre** cuadro, escriba *ContosoUniversityDB*.
9. En el **Server** cuadro, seleccione **servidor de base de datos SQL**. O bien, si ha creado un servidor, puede seleccionar que el servidor de la lista desplegable.
10. Especifique un administrador **nombre de inicio de sesión** y **contraseña**. Si seleccionó **servidor de base de datos SQL** no se escribe un nombre existente y la contraseña aquí, que se están escribiendo un nuevo nombre y una contraseña que se está definiendo ahora para usarla más adelante cuando tiene acceso a la base de datos. Si ha seleccionado un servidor que creó anteriormente, que especifique las credenciales para ese servidor. Para este tutorial, no seleccione la ***avanzadas*** casilla de verificación. El ***avanzadas*** opciones permiten establecer la base de datos [intercalación](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx).
11. Elija el mismo **región** que eligió para el sitio web.
12. Haga clic en la marca de verificación en la parte inferior derecha de la casilla para indicar que haya terminado.   
  
    ![Paso de configuración de base de datos del nuevo sitio de Web - crear con el Asistente para la base de datos](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    La siguiente imagen muestra la utilización de un servidor SQL Server existente y el inicio de sesión.   
  
    ![Paso de configuración de base de datos del nuevo sitio de Web - crear con el Asistente para la base de datos](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    El Portal de administración que se devuelve a la página de sitios Web y el **estado** columna muestra que se está creando el sitio. Después de un tiempo (normalmente menos de un minuto), el **estado** columna muestra que el sitio se creó correctamente. En la barra de navegación de la izquierda, el número de sitios que tenga en su cuenta aparece junto a la **sitios Web** aparece junto al icono y el número de bases de datos de la **bases de datos SQL** icono.

## <a name="deploy-the-application-to-windows-azure"></a>Implementar la aplicación en Windows Azure

1. En Visual Studio, haga clic en el proyecto en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.  
  
    ![Publicar en el menú contextual del proyecto](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. En el **perfil** pestaña de la **Publicar Web** asistente, haga clic en **importación**.  
  
    ![Importar configuración de publicación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. Si no ha agregado previamente su suscripción de Windows Azure en Visual Studio, realice los pasos siguientes. En estos pasos agregue su suscripción para que la lista desplegable lista en **importar desde un sitio web de Windows Azure** incluirá el sitio web.

    a. En el **importar un perfil de publicación** cuadro de diálogo, haga clic en **importar desde un sitio web de Windows Azure**y, a continuación, haga clic en **suscripción agregar Windows Azure**.

    ![Agregar suscripción a Windows Azure](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. En el **Importar suscripciones de Windows Azure** cuadro de diálogo, haga clic en **archivo de suscripción de descarga**.

    ![Descargar archivo de suscripción](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c. En la ventana del explorador, guarde el *.publishsettings* archivo.

    ![Descargue el archivo .publishsettings](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > Seguridad: el *publishsettings* archivo contiene las credenciales (sin codificar) que se usan para administrar sus servicios y suscripciones de Windows Azure. Es la práctica recomendada de seguridad para este archivo se va a almacenar temporalmente fuera de los directorios de origen (por ejemplo, en la *bibliotecas\documentos* carpeta) y, a continuación, eliminarlo una vez completada la importación. Un usuario malintencionado que obtenga acceso a la `.publishsettings` archivo puede editar, crear y eliminar sus servicios de Windows Azure.

    d. En el **Importar suscripciones de Windows Azure** cuadro de diálogo, haga clic en **examinar** y navegue hasta la *.publishsettings* archivo.

    ![sub de descarga](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. Haga clic en **importación**.

    ![importación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. En el **importar un perfil de publicación** cuadro de diálogo, seleccione **importar desde un sitio web de Windows Azure**, seleccione el sitio web de la lista desplegable y, a continuación, haga clic en **Aceptar**.  
  
    ![Importar perfil de publicación](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. En el **conexión** , haga clic en **validar conexión** para asegurarse de que la configuración es correcta.  
  
    ![Validar la conexión](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. Cuando se ha validado la conexión, se muestra junto a una marca de verificación verde el **validar conexión** botón. Haga clic en **Siguiente**.  
  
    ![Conexión validada correctamente](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. Abra la **cadena de conexión remota** lista desplegable situada bajo **SchoolContext** y seleccione la cadena de conexión para la base de datos que creó.
8. Seleccione **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**.
9. Desactive la opción **utilizar esta cadena de conexión en tiempo de ejecución** para el **UserContext (DefaultConnection)**, ya que esta aplicación no está utilizando la base de datos de pertenencia.   
  
    ![Pestaña Configuración](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. Haga clic en **Siguiente**.
11. En el **vista previa** , haga clic en **iniciar Preview**.  
  
    ![Botón de inicio en la ficha Vista previa](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    La pestaña muestra una lista de los archivos que se copiarán en el servidor. Mostrar la vista previa no es necesaria para publicar la aplicación, pero es una función útil tener en cuenta. En este caso, no es necesario hacer nada con la lista de archivos que se muestra. La próxima vez que implemente esta aplicación, sólo los archivos que han cambiado aparecerá en esta lista.  
  
    ![Salida del archivo de inicio](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. Haga clic en **Publicar**.  
    Visual Studio inicia el proceso de copiar los archivos en el servidor de Windows Azure.
13. El **salida** ventana muestra qué acciones de implementación se realizaron y notifica la finalización correcta de la implementación.  
  
    ![Ventana de salida correcta implementación de informes](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. Tras una implementación correcta, el explorador predeterminado se abre automáticamente a la dirección URL del sitio web implementado.  
    Ahora se está ejecutando la aplicación que creó en la nube. Haga clic en la pestaña de alumnos.  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

En este momento su *SchoolContext* base de datos se ha creado en la base de datos de SQL de Windows Azure porque seleccionó **ejecutar migraciones de Code First (se ejecuta en el inicio de aplicación)**. El *Web.config* archivo en el sitio web implementado se ha modificado para que la [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador ejecutaría la primera vez que el código lee o escribe datos en la base de datos (que se produjo cuando selecciona el **estudiantes** pestaña):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

El proceso de implementación también crea una nueva cadena de conexión *(SchoolContext\_DatabasePublish*) para migraciones de Code First que se usará para actualizar el esquema de base de datos y la propagación de la base de datos.

![Cadena de conexión de Database_Publish](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

El *DefaultConnection* cadena de conexión es la base de datos de pertenencia (que no estamos utilizando en este tutorial). El *SchoolContext* cadena de conexión es la base de datos de ContosoUniversity.

Puede encontrar la versión implementada del archivo Web.config en su propio equipo en *ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*. Puede tener acceso a implementado *Web.config* propio archivo mediante FTP. Para obtener instrucciones, consulte [implementación de Web de ASP.NET con Visual Studio: implementar una actualización del código](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Siga las instrucciones que comienzan con "para usar una herramienta FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña."

> [!NOTE]
> La aplicación web no implementa la seguridad, por lo que cualquier persona que se encuentra la dirección URL puede cambiar los datos. Para obtener instrucciones sobre cómo proteger el sitio web, consulte [implementar una aplicación de MVC de ASP.NET segura con pertenencia, OAuth y base de datos SQL a un sitio Web de Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Puede impedir que otras personas usando el sitio mediante el Portal de administración de Windows Azure o **Explorador de servidores** en Visual Studio para detener el sitio.


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>Inicializadores de primer código

En la sección de implementación se ha visto la [MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx) inicializador usándola. Código primero también proporciona otros inicializadores que pueden utilizarse, incluidas [CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) (valor predeterminado), [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) y [ DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx). El `DropCreateAlways` inicializador puede ser útil para configurar las condiciones para las pruebas unitarias. También puede escribir a sus propio inicializadores, y se puede llamar a un inicializador explícitamente si no desea esperar hasta que la aplicación lee o escribe en la base de datos. Para obtener una explicación completa de inicializadores, consulte el capítulo 6 de la libreta de [Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman y Rowan Miller.

## <a name="summary"></a>Resumen

En este tutorial ha visto cómo crear un modelo de datos e implementar básica CRUD, ordenación, filtrado, paginación y la característica de agrupación. En el siguiente tutorial comenzará examinando temas más avanzados expandiendo el modelo de datos.

Vínculos a otros recursos de Entity Framework pueden encontrarse en el [mapa de contenido de acceso de datos de ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Siguiente](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
