---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Ordenación, filtrado y paginación con Entity Framework en una aplicación ASP.NET MVC | Documentos de Microsoft"
author: tdykstra
description: "La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 8d11bf47f8c43040ef30d7132f0bb756748dbacd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Ordenación, filtrado y paginación con Entity Framework en una aplicación ASP.NET MVC
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) o [descarga de PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones de ASP.NET MVC 5 con Code First de Entity Framework 6 y Visual Studio 2013. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


En el tutorial anterior implementa un conjunto de páginas web para las operaciones CRUD básicas para `Student` entidades. En este tutorial agregará la ordenación, el filtrado y la funcionalidad de paginación para la **estudiantes** página de índice. También podrá crear una página que realiza la agrupación sencilla.

En la siguiente ilustración muestra el aspecto de la página cuando haya terminado. Los encabezados de columna son vínculos que el usuario puede hacer clic para ordenar por esa columna. Al hacer clic en una columna por repetidamente alterna entre orden ascendente y orden descendente.

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Agregar vínculos de ordenación de la columna a la página de índice de estudiantes

Para agregar ordenación, en la página de índice de estudiantes, cambiará la `Index` método de la `Student` controlador y agregar código a la `Student` indizar la vista.

### <a name="add-sorting-functionality-to-the-index-method"></a>Agregar funcionalidad para el método de índice de ordenación

En *Controllers\StudentController.cs*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

Este código recibe un `sortOrder` parámetro de la cadena de consulta en la dirección URL. El valor de cadena de consulta se proporciona mediante ASP.NET MVC como un parámetro al método de acción. El parámetro será una cadena que es "Name" o "Fecha", seguido opcionalmente por un guión bajo y la cadena "desc" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

La primera vez que se solicita la página de índice, no hay ninguna cadena de consulta. Los alumnos se muestran en orden ascendente por `LastName`, que es el valor predeterminado establecido por el caso paso explícito en el `switch` instrucción. Cuando el usuario hace clic en un hipervínculo de encabezado de columna, la correspondiente `sortOrder` valor se proporciona en la cadena de consulta.

Los dos `ViewBag` las variables se usan para que la vista puede configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuada:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Estas son las instrucciones ternarias. La primera de ellas especifica que si el `sortOrder` parámetro es null o está vacío, `ViewBag.NameSortParm` debe establecerse en "nombre\_desc"; en caso contrario, se debe establecer en una cadena vacía. Estas dos instrucciones habilitar la vista establecer la columna hipervínculos de encabezado como sigue:

| Criterio de ordenación actual | Hipervínculo apellido | Hipervínculo de fecha |
| --- | --- | --- |
| Último nombre ascendente | descending | ascending |
| Último nombre descendente | ascending | ascending |
| Fecha ascendente | ascending | descending |
| Fecha descendente | ascending | ascending |

El método usa [LINQ to Entities](https://msdn.microsoft.com/en-us/library/bb386964.aspx) para especificar la columna para ordenar por. El código crea un [IQueryable](https://msdn.microsoft.com/en-us/library/bb351562.aspx) variable antes de la `switch` modifica la instrucción, en la `switch` instrucción y llamadas a la `ToList` método después el `switch` instrucción. Al crear y modificar `IQueryable` variables, no hay ninguna consulta se envía a la base de datos. La consulta no se ejecuta hasta que convierta el `IQueryable` objeto en una colección mediante una llamada a un método como `ToList`. Por lo tanto, este código produce una única consulta que no se ejecuta hasta que el `return View` instrucción.

Como alternativa a escribir las instrucciones LINQ diferentes para cada criterio de ordenación, puede crear dinámicamente una instrucción LINQ. Para obtener información acerca de LINQ dinámica, consulte [LINQ dinámico](https://go.microsoft.com/fwlink/?LinkID=323957).

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Agregar hipervínculos a la vista de índice de estudiante de encabezado de columna

En *Views\Student\Index.cshtml*, reemplace la `<tr>` y `<th>` elementos de la fila de encabezado con el código resaltado:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

Este código usa la información de la `ViewBag` valores de cadena de propiedades para configurar hipervínculos con la consulta adecuada.

Ejecute la página y haga clic en el **Last Name** y **fecha de inscripción** funciona encabezados de columna para comprobar que la ordenación.

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Tras hacer clic en el **Last Name** encabezado, los alumnos se muestran de forma descendente último nombre.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Agregue un cuadro de búsqueda a la página de índice de estudiantes

Para agregar el filtrado a la página de índice de estudiantes, podrá agregar un cuadro de texto y un botón de envío a la vista y aplicar los cambios correspondientes en el `Index` método. El cuadro de texto le permitirá escribir una cadena que se busca en el nombre y los últimos campos de nombre.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar funcionalidad de filtrado para el método de índice

En *Controllers\StudentController.cs*, reemplace el `Index` método por el código siguiente (los cambios aparecen resaltados):

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

Ha agregado un `searchString` parámetro para el `Index` método. El valor de cadena de búsqueda se recibe de un cuadro de texto que agregará a la vista de índice. También ha agregado a la instrucción LINQ un `where` cláusula que selecciona sólo los estudiantes cuyo apellido o apellidos contienen la cadena de búsqueda. La instrucción que agrega el [donde](https://msdn.microsoft.com/en-us/library/bb535040.aspx) cláusula se ejecuta sólo si hay un valor que se buscará.

> [!NOTE]
> En muchos casos puede llamar al mismo método en un conjunto de entidades de Entity Framework o como un método de extensión en una colección en memoria. Los resultados son normalmente los mismos pero en algunos casos pueden ser diferentes.
> 
> Por ejemplo, la implementación de .NET Framework de la `Contains` método devuelve todas las filas cuando se pasa una cadena vacía a él, pero el proveedor de Entity Framework para SQL Server Compact 4.0 devuelve cero filas para las cadenas vacías. Por lo tanto, el código del ejemplo (colocar el `Where` instrucción dentro de un `if` instrucción) se asegura de que obtendrá los mismos resultados para todas las versiones de SQL Server. Además, la implementación de .NET Framework de la `Contains` método realiza una comparación entre mayúsculas y minúsculas de forma predeterminada, pero los proveedores de Entity Framework SQL Server realizan comparaciones entre mayúsculas y minúsculas de forma predeterminada. Por lo tanto, al llamar a la `ToUpper` método para realizar la prueba explícitamente entre mayúsculas y minúsculas se asegura de que los resultados no cambian cuando cambia el código de una versión posterior para usar un repositorio, lo que devolverá una `IEnumerable` colección en lugar de un `IQueryable` objeto. (Cuando se llama a la `Contains` método en un `IEnumerable` colección, obtendrá la implementación en .NET Framework; cuando se llama en un `IQueryable` de objeto, obtendrá la implementación del proveedor de base de datos.)
> 
> Control de valores NULL también puede ser diferente para los proveedores de base de datos diferente o si usa un `IQueryable` objeto en comparación con cuando se usa un `IEnumerable` colección. Por ejemplo, en algunos escenarios un `Where` condición como `table.Column != 0` no se puede devolver columnas que tengan `null` como el valor. Para obtener más información, consulte [tratamiento incorrecto de las variables de null en la cláusula 'where'](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl).


### <a name="add-a-search-box-to-the-student-index-view"></a>Agregue un cuadro de búsqueda a la vista de índice de estudiante

En *Views\Student\Index.cshtml*, agregue el código resaltado inmediatamente antes de la apertura `table` etiqueta con el fin de crear un título, un cuadro de texto y un **búsqueda** botón.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

Ejecute la página, escriba una cadena de búsqueda y haga clic en **búsqueda** para comprobar que el filtrado está funcionando.

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Tenga en cuenta que la dirección URL no contiene "una" cadena de búsqueda, lo que significa que si Marque esta página, no obtendrá la lista filtrada cuando se utiliza el marcador. Esto se aplica también a los vínculos de ordenación de la columna tal y como ordenará la lista completa. Cambiará la **búsqueda** botón que se usa cadenas de consulta para los criterios de filtro, más adelante en el tutorial.

## <a name="add-paging-to-the-students-index-page"></a>Agregar paginación a la página de índice de estudiantes

Para agregar paginación a la página de índice de estudiantes, podrá empezar mediante la instalación de la **PagedList.Mvc** paquete NuGet. A continuación, podrá realizar cambios adicionales en el `Index` (método) y agregar vínculos de paginación a la `Index` vista. **PagedList.Mvc** es uno de los muchos paginación buena y ordenar paquetes para ASP.NET MVC y su uso aquí está pensado únicamente como ejemplo, no como una recomendación para él a través de otras opciones. La ilustración siguiente muestra los vínculos de paginación.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>Instale el paquete PagedList.MVC NuGet

NuGet **PagedList.Mvc** paquete instala automáticamente el **PagedList** paquete como una dependencia. El **PagedList** paquete instala un `PagedList` para los métodos de tipo y la extensión de recopilación `IQueryable` y `IEnumerable` colecciones. Los métodos de extensión crean una sola página de datos en un `PagedList` colección fuera de su `IQueryable` o `IEnumerable`y el `PagedList` colección proporciona varias propiedades y métodos que facilitan la paginación. El **PagedList.Mvc** paquete instala una aplicación auxiliar de paginación que muestra los botones de paginación.

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca** y, a continuación, **Package Manager Console**.

En el **Package Manager Console** ventana, asegúrese de que el **origen del paquete** es **nuget.org** y **proyecto predeterminado** es **ContosoUniversity**y, a continuación, escriba el comando siguiente:

`Install-Package PagedList.Mvc`

![Instalar PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Compile el proyecto. 

### <a name="add-paging-functionality-to-the-index-method"></a>Agregar funcionalidad de paginación para el método de índice

En *Controllers\StudentController.cs*, agregue un `using` instrucción para el `PagedList` espacio de nombres:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Reemplace el método `Index` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

Este código agrega un `page` parámetro, un parámetro de criterio de ordenación actual y un parámetro de filtro actual para la firma del método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

La primera vez que se muestra la página, o si el usuario no ha hecho clic en una paginación u ordenación vínculo, todos los parámetros será nulos. Si se hace clic en un vínculo de paginación, el `page` variable contendrá el número de página para mostrar.

Un `ViewBag` propiedad proporciona la vista con el criterio de ordenación actual, porque esto debe estar incluida en los vínculos de paginación para mantener el criterio de ordenación de los mismos durante la paginación:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Otra propiedad, `ViewBag.CurrentFilter`, proporciona la vista con la cadena de filtro actual. Este valor debe estar incluido en los vínculos de paginación para mantener la configuración del filtro durante la paginación y deben restaurarse en el cuadro de texto cuando se vuelve a mostrar la página. Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede dar lugar a datos diferente para mostrar. Cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar, se cambia la cadena de búsqueda. En ese caso, el `searchString` parámetro no es null.

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Al final del método, el `ToPagedList` método de extensión en los alumnos `IQueryable` objeto convierte la consulta de estudiante en una sola página de alumnos de un tipo de colección que admite la paginación. Esa única página de estudiantes, a continuación, se pasa a la vista:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

El `ToPagedList` método toma un número de página. Los dos signos de interrogación representan el [operador de uso combinado de null](https://msdn.microsoft.com/en-us/library/ms173224.aspx). El operador de uso combinado de null define un valor predeterminado para un tipo que acepta valores null; la expresión `(page ?? 1)` significa que devuelva el valor de `page` si tiene un valor, o devuelve 1 si `page` es null.

### <a name="add-paging-links-to-the-student-index-view"></a>Agregar vínculos de paginación a la vista de índice de estudiante

En *Views\Student\Index.cshtml*, reemplace el código existente por el código siguiente. Los cambios aparecen resaltados.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

El `@model` instrucción en la parte superior de la página especifica que la vista obtiene ahora un `PagedList` en lugar del objeto un `List` objeto.

El `using` instrucción para `PagedList.Mvc` proporciona acceso a la aplicación auxiliar MVC para los botones de paginación.

El código usa una sobrecarga de [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) que le permite especificar [FormMethod.Get](https://msdn.microsoft.com/en-us/library/system.web.mvc.formmethod(v=vs.100).aspx/css).

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

El valor predeterminado [BeginForm](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) envía datos del formulario con una entrada, lo que significa que se pasan en el cuerpo del mensaje HTTP y no en la dirección URL como las cadenas de consulta de parámetros. Cuando se especifica HTTP GET, los datos del formulario se pasan en la dirección URL como cadenas de consulta, lo que permite a los usuarios marcar la dirección URL. El [directrices de W3C para el uso de HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html) recomienda que se debe utilizar GET cuando la acción no da como resultado una actualización.

El cuadro de texto se inicializa con la cadena de búsqueda actual para que al hacer clic en una nueva página que pueda ver la cadena de búsqueda actual.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

Los vínculos de encabezado de columna use la cadena de consulta para pasar la cadena de búsqueda actual al controlador de modo que el usuario puede ordenar en los resultados del filtro:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

Se muestra el número actual de página y el total de páginas.

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

Si no hay ninguna página para mostrar, se muestra "Página 0 0". (En ese caso el número de página es mayor que el recuento de páginas porque `Model.PageNumber` es 1, y `Model.PageCount` es 0.)

Se muestran los botones de paginación mediante el `PagedListPager` auxiliar:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

El `PagedListPager` auxiliar proporciona una serie de opciones que se pueden personalizar, incluidas las direcciones URL y aplicación de estilos. Para obtener más información, consulte [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) en el sitio de GitHub.

Ejecute la página.

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que funciona de paginación. A continuación, escriba una cadena de búsqueda e inténtelo de paginación nuevo para comprobar que la paginación también la funciona correctamente con el filtrado y ordenación.

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>Crear un sobre la página que muestra estadísticas de estudiante

Para la Universidad de Contoso del sitio Web acerca de la página, mostrará los alumnos cuántos han inscrito para cada fecha de inscripción. Esto requiere cálculos de agrupación y simples en los grupos. Para ello, deberá llevar a cabo lo siguiente:

- Cree una clase de modelo de vista para los datos que necesita para pasar a la vista.
- Modificar el `About` método en el `Home` controlador.
- Modificar el `About` vista.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Crear un *ViewModels* en la carpeta de proyecto. En esa carpeta, agregue un archivo de clase *EnrollmentDateGroup.cs* y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>Modifique el controlador Home

En *HomeController.cs*, agregue las siguientes `using` las instrucciones en la parte superior del archivo:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Agregue una variable de clase para el contexto de base de datos inmediatamente después de la llave de apertura para la clase:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

Reemplace el método `About` con el código siguiente:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula el número de entidades en cada grupo y almacena los resultados en una colección de `EnrollmentDateGroup` ver objetos del modelo.

Agregar un `Dispose` método:

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>Modificar la vista

Reemplace el código de la *Views\Home\About.cshtml* archivo con el código siguiente:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

Ejecutar la aplicación y haga clic en el **sobre** vínculo. El número de alumnos para cada fecha de inscripción se muestra en una tabla.

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>Resumen

En este tutorial ha visto cómo crear un modelo de datos e implementar básica CRUD, ordenación, filtrado, paginación y la característica de agrupación. En el siguiente tutorial comenzará examinando temas más avanzados expandiendo el modelo de datos.

Vota sobre cómo le gustó este tutorial y lo que podemos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Vínculos a otros recursos de Entity Framework se pueden encontrar en [ASP.NET Data Access: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Anterior](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
[Siguiente](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
