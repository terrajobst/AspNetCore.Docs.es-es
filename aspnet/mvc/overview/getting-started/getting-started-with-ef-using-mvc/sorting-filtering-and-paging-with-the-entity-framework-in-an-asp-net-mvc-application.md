---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: Ordenación, filtrado y paginación con Entity Framework en una aplicación ASP.NET MVC | Microsoft Docs
author: tdykstra
description: La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a72d6cbce0e5f20360e1e9bfc20f4a75bfeb3343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835740"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Ordenación, filtrado y paginación con Entity Framework en una aplicación ASP.NET MVC
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descargar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de Contoso University muestra cómo crear aplicaciones de ASP.NET MVC 5 con Entity Framework 6 Code First y Visual Studio 2013. Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


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

Como alternativa a escribir las instrucciones LINQ diferentes para cada criterio de ordenación, puede crear dinámicamente una instrucción LINQ. Para obtener información acerca de LINQ dinámica, consulte [LINQ dinámico](https://go.microsoft.com/fwlink/?LinkID=323957).

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

Ha agregado un parámetro `searchString` al método `Index`. El valor de la cadena de búsqueda se recibe desde un cuadro de texto que agregará a la vista de índice. También ha agregado a la instrucción LINQ una `where` cláusula que solo selecciona los alumnos cuyo nombre o apellido contienen la cadena de búsqueda. La instrucción que agrega el [donde](https://msdn.microsoft.com/library/bb535040.aspx) cláusula se ejecuta solo si hay un valor que se busca.

> [!NOTE]
> En muchos casos puede llamar al mismo método en un conjunto de entidades de Entity Framework o como un método de extensión en una colección en memoria. Los resultados son normalmente los mismos, pero en algunos casos pueden ser diferentes.
> 
> Por ejemplo, la implementación de .NET Framework de la `Contains` método devuelve todas las filas al pasar una cadena vacía a él, pero el proveedor de Entity Framework para SQL Server Compact 4.0 devuelve cero filas las cadenas están vacías. Por lo tanto, el código del ejemplo (poner el `Where` instrucción dentro de un `if` instrucción) se asegura de que obtendrá los mismos resultados para todas las versiones de SQL Server. Además, la implementación de .NET Framework la `Contains` método realiza una comparación entre mayúsculas y minúsculas de forma predeterminada, pero los proveedores de Entity Framework, SQL Server realizan comparaciones entre mayúsculas y minúsculas de forma predeterminada. Por lo tanto, una llamada a la `ToUpper` método para realizar la prueba explícitamente entre mayúsculas y minúsculas garantiza que los resultados no cambian al cambiar el código posterior para usar un repositorio, que devolverá una `IEnumerable` colección en lugar de un `IQueryable` objeto. (Al hacer una llamada al método `Contains` en una colección `IEnumerable`, obtendrá la implementación de .NET Framework; al hacer una llamada a un objeto `IQueryable`, obtendrá la implementación del proveedor de base de datos).
> 
> Control de valores NULL también puede ser diferente para los proveedores de base de datos diferente o al usar un `IQueryable` objeto en comparación con cuando se usa un `IEnumerable` colección. Por ejemplo, en algunos escenarios un `Where` como condición `table.Column != 0` no pueden devolver las columnas que tienen `null` como valor. Para obtener más información, consulte [tratamiento incorrecto de las variables de null en la cláusula 'where'](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Agregar un cuadro de búsqueda a la vista de índice de Student

En *Views\Student\Index.cshtml*, agregue el código resaltado inmediatamente antes de la apertura `table` etiqueta con el fin de crear un título, un cuadro de texto y un **búsqueda** botón.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Ejecute la página, escriba una cadena de búsqueda y haga clic en **búsqueda** para comprobar que el filtrado funciona correctamente.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tenga en cuenta que la dirección URL no contiene "una" cadena de búsqueda, lo que significa que si marca esta página, no obtendrá la lista filtrada al usar el marcador. Esto se aplica también a los vínculos de ordenación de la columna, tal como ordenará la lista completa. Va a cambiar el **búsqueda** botón para utilizar cadenas de consulta para los criterios de filtro más adelante en el tutorial.

## <a name="add-paging-to-the-students-index-page"></a>Agregar paginación a la página de índice de Students

Para agregar paginación a la página de índice de Students, empezará instalando el **PagedList.Mvc** paquete NuGet. A continuación, podrá realizar cambios adicionales en el `Index` método y agregue vínculos de paginación para el `Index` vista. **PagedList.Mvc** es uno de muchos paginación buena y ordenar los paquetes para ASP.NET MVC y aquí para su uso está pensado únicamente como ejemplo, no como una recomendación para ella a través de otras opciones. La siguiente ilustración muestra los vínculos de paginación.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instale el paquete PagedList.MVC NuGet

El paquete NuGet **PagedList.Mvc** paquete se instala automáticamente el **PagedList** paquete como una dependencia. El **PagedList** paquete instala una `PagedList` para los métodos de tipo y la extensión de recopilación `IQueryable` y `IEnumerable` colecciones. Los métodos de extensión crean una sola página de datos en un `PagedList` colección fuera de su `IQueryable` o `IEnumerable`y el `PagedList` colección proporciona varias propiedades y métodos que facilitan la paginación. El **PagedList.Mvc** paquete instala una aplicación auxiliar de paginación que muestra los botones de paginación.

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.

En el **Package Manager Console** ventana, asegúrese de que el **origen del paquete** es **nuget.org** y **proyecto predeterminado** es **ContosoUniversity**y, a continuación, escriba el siguiente comando:

`Install-Package PagedList.Mvc`

![Instalar PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Compile el proyecto. 

### <a name="add-paging-functionality-to-the-index-method"></a>Agregar funcionalidad de paginación al método Index

En *Controllers\StudentController.cs*, agregue un `using` instrucción para el `PagedList` espacio de nombres:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Reemplace el método `Index` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Este código agrega un `page` parámetro, un parámetro de criterio de ordenación actual y un parámetro de filtro actual para la firma del método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La primera vez que se muestra la página, o si el usuario no ha hecho clic en un vínculo de ordenación o paginación, todos los parámetros son nulos. Si se hace clic en un vínculo de paginación, la `page` variable contendrá el número de página para mostrar.

Un `ViewBag` propiedad proporciona la vista con el criterio de ordenación actual, ya que esto debe incluirse en los vínculos de paginación para mantener el criterio de ordenación durante la paginación:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Otra propiedad, `ViewBag.CurrentFilter`, proporciona la vista con la cadena de filtro actual. Este valor debe incluirse en los vínculos de paginación para mantener la configuración de filtrado durante la paginación y debe restaurarse en el cuadro de texto cuando se vuelve a mostrar la página. Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede hacer que se muestren diferentes datos. Cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar, se cambia la cadena de búsqueda. En ese caso, el `searchString` parámetro no es null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Al final del método, el `ToPagedList` método de extensión en los estudiantes `IQueryable` objeto convierte la consulta del alumno en una sola página de alumnos de un tipo de colección que admita la paginación. Esa única página de alumnos, a continuación, se pasa a la vista:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El método `ToPagedList` toma un número de página. Los dos signos de interrogación representan el [operador de uso combinado de null](https://msdn.microsoft.com/library/ms173224.aspx). El operador de uso combinado de NULL define un valor predeterminado para un tipo que acepta valores NULL; la expresión `(page ?? 1)` devuelve el valor de `page` si tiene algún valor o devuelve 1 si `page` es NULL.

### <a name="add-paging-links-to-the-student-index-view"></a>Agregar vínculos de paginación a la vista de índice de Student

En *Views\Student\Index.cshtml*, reemplace el código existente por el código siguiente. Los cambios aparecen resaltados.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

La instrucción `@model` de la parte superior de la página especifica que ahora la vista obtiene un objeto `PagedList` en lugar de un objeto `List`.

El `using` instrucción para `PagedList.Mvc` proporciona acceso a la aplicación auxiliar MVC para los botones de paginación.

El código usa una sobrecarga de [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que le permite especificar [FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

El valor predeterminado [BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) envía datos de formulario con POST, lo que significa que los parámetros se pasan en el cuerpo del mensaje HTTP y no en la dirección URL como cadenas de consulta. Al especificar HTTP GET, los datos de formulario se pasan en la dirección URL como cadenas de consulta, lo que permite que los usuarios marquen la dirección URL. El [directrices de W3C para el uso de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recomienda que se debe utilizar GET cuando la acción no produzca ninguna actualización.

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que la paginación funciona correctamente. A continuación, escriba una cadena de búsqueda e intente llevar a cabo la paginación de nuevo, para comprobar que la paginación también funciona correctamente con filtrado y ordenación.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Crear un sobre la página que muestra las estadísticas de alumno

Para Contoso University del sitio Web acerca de la página, se muestran cuántos alumnos se han inscrito por cada fecha de inscripción. Esto requiere realizar agrupaciones y cálculos sencillos en los grupos. Para conseguirlo, haga lo siguiente:

- Cree una clase de modelo de vista para los datos que necesita pasar a la vista.
- Modificar el `About` método en el `Home` controlador.
- Modificar el `About` vista.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Crear un *ViewModels* carpeta en la carpeta del proyecto. En esa carpeta, agregue un archivo de clase *EnrollmentDateGroup.cs* y reemplace el código de plantilla con el código siguiente:

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo crear un modelo de datos e implementar CRUD básicas, ordenación, filtrado, paginación y funcionalidad de agrupación. En el siguiente tutorial comenzará examinando temas más avanzados expandiendo el modelo de datos.

Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Pueden encontrar vínculos a otros recursos de Entity Framework en [acceso a datos de ASP.NET - recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [Siguiente](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
