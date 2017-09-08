---
title: "Núcleo de ASP.NET MVC con EF Core - ordenar, filtrar y paginación: 3 de 10"
author: tdykstra
description: "En este tutorial agregará ordenación, filtrado y paginación funcionalidad en la página utilizando ASP.NET Core y Entity Framework Core."
keywords: "ASP.NET Core, Entity Framework Core, ordenar, filtrar, paginación, agrupación"
ms.author: tdykstra
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: e6c1ff3c-5673-43bf-9c2d-077f6ada1f29
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: bc2896d0eeda7e84cef06ee3f235e637bfe04318
ms.sourcegitcommit: 5355c96a1768e5a1d5698a98c190e7addcc4ded5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2017
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-aspnet-core-mvc-tutorial-3-of-10"></a>Ordenación, filtrado, paginación y agrupar - Core EF con el tutorial de MVC de ASP.NET Core (3 de 10)

Por [Tom Dykstra](https://github.com/tdykstra) y [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación web de ejemplo de la Universidad de Contoso muestra cómo crear aplicaciones web de MVC de ASP.NET Core con Entity Framework Core y Visual Studio. Para obtener información acerca de la serie de tutoriales, vea [el primer tutorial de la serie](intro.md).

En el tutorial anterior, se implementa un conjunto de páginas web para las operaciones CRUD básicas para las entidades de estudiante. En este tutorial agregará la ordenación, el filtrado y la funcionalidad de paginación a la página de índice de los alumnos. También podrá crear una página que realiza la agrupación sencilla.

En la siguiente ilustración muestra el aspecto de la página cuando haya terminado. Los encabezados de columna son vínculos que el usuario puede hacer clic para ordenar por esa columna. Al hacer clic en una columna por repetidamente alterna entre orden ascendente y orden descendente.

![Página de índice de estudiantes](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>Agregar vínculos de ordenación de la columna a la página de índice de estudiantes

Para agregar ordenación, en la página de índice de estudiantes, cambiará la `Index` método del controlador de los alumnos y agregue código a la vista de índice de estudiante.

### <a name="add-sorting-functionality-to-the-index-method"></a>Agregar funcionalidad de ordenación para el método de índice

En *StudentsController.cs*, reemplace el `Index` método por el código siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

Este código recibe un `sortOrder` parámetro de la cadena de consulta en la dirección URL. El valor de cadena de consulta se proporciona por núcleo de ASP.NET MVC como un parámetro al método de acción. El parámetro será una cadena que es "Name" o "Fecha", seguido opcionalmente por un guión bajo y la cadena "desc" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

La primera vez que se solicita la página de índice, no hay ninguna cadena de consulta. Los alumnos se muestran en orden ascendente por apellido, que es el valor predeterminado establecido por el caso paso explícito en el `switch` instrucción. Cuando el usuario hace clic en un hipervínculo de encabezado de columna, la correspondiente `sortOrder` valor se proporciona en la cadena de consulta.

Los dos `ViewData` elementos (NameSortParm y DateSortParm) se usan por la vista para configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuada.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

Estas son las instrucciones ternarias. La primera de ellas especifica que si el `sortOrder` del parámetro es null o está vacío, NameSortParm debe establecerse en "name_desc"; en caso contrario, se debe establecer en una cadena vacía. Estas dos instrucciones habilitar la vista establecer la columna hipervínculos de encabezado como sigue:

|  Criterio de ordenación actual  | Hipervínculo apellido | Hipervínculo de fecha |
|:--------------------:|:-------------------:|:--------------:|
| Último nombre ascendente  | descending          | ascending      |
| Último nombre descendente | ascending           | ascending      |
| Fecha ascendente       | ascending           | descending     |
| Fecha descendente      | ascending           | ascending      |

El método usa LINQ to Entities para especificar la columna para ordenar por. El código crea un `IQueryable` variable antes de la instrucción switch, se modifica en la instrucción switch y llama el `ToListAsync` método después de la `switch` instrucción. Al crear y modificar `IQueryable` variables, no hay ninguna consulta se envía a la base de datos. La consulta no se ejecuta hasta que convierta el `IQueryable` objeto en una colección mediante una llamada a un método como `ToListAsync`. Por lo tanto, este código produce una única consulta que no se ejecuta hasta que el `return View` instrucción.

Este código podría obtener detallado con un gran número de columnas. [El último tutorial de esta serie](advanced.md#dynamic-linq) muestra cómo escribir código que le permite pasar el nombre de la `OrderBy` columna en una variable de cadena.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Agregar los hipervínculos del encabezado de columna a la vista de índice de estudiante

Reemplace el código de *Views/Students/Index.cshtml*, con el código siguiente para agregar los hipervínculos del encabezado de columna. Se resaltan las líneas modificadas.

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

Este código usa la información de `ViewData` valores de cadena de propiedades para configurar hipervínculos con la consulta adecuada.

Ejecute la página y haga clic en el **Last Name** y **fecha de inscripción** funciona encabezados de columna para comprobar que la ordenación.

![Página de índice de estudiantes en orden de nombres](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>Agregue un cuadro de búsqueda a la página de índice de estudiantes

Para agregar el filtrado a la página de índice de estudiantes, podrá agregar un cuadro de texto y un botón de envío a la vista y aplicar los cambios correspondientes en el `Index` método. El cuadro de texto le permitirá escribir una cadena que se busca en el nombre y los últimos campos de nombre.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar funcionalidad de filtrado para el método de índice

En *StudentsController.cs*, reemplace el `Index` método por el código siguiente (se resaltan los cambios).

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

Ha agregado un `searchString` parámetro para el `Index` método. El valor de cadena de búsqueda se recibe de un cuadro de texto que agregará a la vista de índice. También ha agregado a la instrucción LINQ where cláusula que selecciona sólo los estudiantes cuyo apellido o apellidos contienen la cadena de búsqueda. La instrucción que agrega where cláusula se ejecuta sólo si hay un valor que se buscará.

> [!NOTE]
> Aquí se está llamando a la `Where` método en un `IQueryable` objeto y el filtro se procesarán en el servidor. En algunos escenarios que podría llamando el `Where` método como un método de extensión en una colección en memoria. (Por ejemplo, imagine que cambiar la referencia a `_context.Students` para que en lugar de un EF `DbSet` hace referencia a un método de repositorio que devuelve un `IEnumerable` colección.) El resultado podría ser normalmente el mismo, pero en algunos casos puede ser diferente.
>
>Por ejemplo, la implementación de .NET Framework de la `Contains` método realiza una comparación entre mayúsculas y minúsculas de forma predeterminada, pero en SQL Server esto viene determinado por la configuración de intercalación de la instancia de SQL Server. Ese valor predeterminado es en mayúsculas de minúsculas. Podría llamar a la `ToUpper` método para realizar la prueba explícitamente entre mayúsculas y minúsculas: *donde (s = > s.LastName.ToUpper(). Contains(searchString.ToUpper())*. Que garantiza que resultados permanecen invariables, si cambia el código más adelante para usar un repositorio que devuelve un `IEnumerable` colección en lugar de un `IQueryable` objeto. (Cuando se llama a la `Contains` método en un `IEnumerable` colección, obtendrá la implementación en .NET Framework; cuando se llama en un `IQueryable` de objeto, obtendrá la implementación del proveedor de base de datos.) Sin embargo, hay una reducción del rendimiento para esta solución. La `ToUpper` código tendría que poner una función en la cláusula WHERE de la instrucción SELECT de TSQL. Que evitaría que el optimizador utiliza un índice. Dado que SQL principalmente se instala como entre mayúsculas y minúsculas, es mejor evitar la `ToUpper` código hasta que se migra a un almacén de datos distingue mayúsculas de minúsculas.

### <a name="add-a-search-box-to-the-student-index-view"></a>Agregue un cuadro de búsqueda a la vista de índice de estudiante

En *Views/Student/Index.cshtml*, agregue el código resaltado inmediatamente antes de que la apertura etiqueta de tabla para crear un título, un cuadro de texto y un **búsqueda** botón.

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

Este código usa el `<form>` [etiqueta auxiliar](https://docs.asp.net/en/latest/mvc/views/tag-helpers/intro.html) para agregar el cuadro de texto de búsqueda y el botón. De forma predeterminada, la `<form>` auxiliar de etiquetas envía datos de formulario con una entrada, lo que significa que se pasan en el cuerpo del mensaje HTTP y no en la dirección URL como las cadenas de consulta de parámetros. Cuando se especifica HTTP GET, los datos del formulario se pasan en la dirección URL como cadenas de consulta, lo que permite a los usuarios marcar la dirección URL. El W3C. se recomienda instrucciones que debe usar obtener cuando la acción no da como resultado una actualización.

Ejecute la página, escriba una cadena de búsqueda y haga clic en Buscar para comprobar que el filtrado está funcionando.

![Página de índice de los alumnos con filtrado](sort-filter-page/_static/filtering.png)

Observe que la dirección URL contiene la cadena de búsqueda.

```html
http://localhost:5813/Students?SearchString=an
```

Si Marque esta página, obtendrá la lista filtrada cuando se utiliza el marcador. Agregar `method="get"` a la `form` etiqueta es la causa de la cadena de consulta que se genere.

En esta fase, si hace clic en un vínculo de ordenación del encabezado de columna se pierde el valor de filtro que especificó en el **búsqueda** cuadro. Se corregirá en la siguiente sección.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Agregar funcionalidad de paginación a la página de índice de estudiantes

Para agregar paginación a la página de índice de estudiantes, podrá crear un `PaginatedList` clase que utiliza `Skip` y `Take` instrucciones para filtrar los datos en el servidor en lugar de recuperar siempre todas las filas de la tabla. A continuación, podrá realizar cambios adicionales en el `Index` (método) y agregar botones de paginación para el `Index` vista. La ilustración siguiente muestra los botones de paginación.

![Página con vínculos de paginación de índice de estudiantes](sort-filter-page/_static/paging.png)

En la carpeta del proyecto, crear `PaginatedList.cs`y, a continuación, reemplace el código de plantilla con el código siguiente.

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

El `CreateAsync` método en este código toma el tamaño de página y número de página y se aplica a la correspondiente `Skip` y `Take` instrucciones para el `IQueryable`. Cuando `ToListAsync` se llama en el `IQueryable`, devolverá una lista que contiene solo la página solicitada. Las propiedades `HasPreviousPage` y `HasNextPage` puede usarse para habilitar o deshabilitar **anterior** y **siguiente** botones de paginación.

A `CreateAsync` método se utiliza en lugar de un constructor para crear el `PaginatedList<T>` objeto porque los constructores no pueden ejecutar código asincrónico.

## <a name="add-paging-functionality-to-the-index-method"></a>Agregar funcionalidad de paginación para el método de índice

En *StudentsController.cs*, reemplace el `Index` método por el código siguiente.

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

Este código agrega un parámetro de número de página, un parámetro de criterio de ordenación actual y un parámetro de filtro actual para la firma del método.

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

La primera vez que se muestra la página, o si el usuario no ha hecho clic en una paginación u ordenación vínculo, todos los parámetros será nulos.  Si se hace clic en un vínculo de paginación, la variable de página contendrá el número de página para mostrar.

El `ViewData` elemento denominado CurrentSort proporciona la vista con el criterio de ordenación actual, porque esto debe estar incluida en los vínculos de paginación para mantener el criterio de ordenación de los mismos durante la paginación.

El `ViewData` elemento denominado FiltroActual ofrece la vista con la cadena de filtro actual. Este valor debe estar incluido en los vínculos de paginación para mantener la configuración del filtro durante la paginación y deben restaurarse en el cuadro de texto cuando se vuelve a mostrar la página.

Si se cambia la cadena de búsqueda durante la paginación, la página debe restablecerse a 1, porque el nuevo filtro puede dar lugar a datos diferente para mostrar. Cuando se escribe un valor en el cuadro de texto y se presiona el botón Enviar, se cambia la cadena de búsqueda. En ese caso, el `searchString` parámetro no es null.

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

Al final de la `Index` método, el `PaginatedList.CreateAsync` método convierte la consulta de estudiante en una sola página de alumnos de un tipo de colección que admite la paginación. Esa única página de estudiantes, a continuación, se pasa a la vista.

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

El `PaginatedList.CreateAsync` método toma un número de página. Los dos signos de interrogación representar el operador de uso combinado de null. El operador de uso combinado de null define un valor predeterminado para un tipo que acepta valores null; la expresión `(page ?? 1)` significa que devuelva el valor de `page` si tiene un valor, o devuelve 1 si `page` es null.

## <a name="add-paging-links-to-the-student-index-view"></a>Agregar vínculos de paginación a la vista de índice de estudiante

En *Views/Students/Index.cshtml*, reemplace el código existente por el código siguiente. Los cambios aparecen resaltados.

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

El `@model` instrucción en la parte superior de la página especifica que la vista obtiene ahora un `PaginatedList<T>` en lugar del objeto un `List<T>` objeto.

Los vínculos de encabezado de columna use la cadena de consulta para pasar la cadena de búsqueda actual al controlador de modo que el usuario puede ordenar en los resultados del filtro:

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

Se muestran los botones de paginación mediante aplicaciones auxiliares de etiquetas:

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

Ejecute la página.

![Página con vínculos de paginación de índice de estudiantes](sort-filter-page/_static/paging.png)

Haga clic en los vínculos de paginación en distintos criterios de ordenación para comprobar que funciona de paginación. A continuación, escriba una cadena de búsqueda e inténtelo de paginación nuevo para comprobar que la paginación también la funciona correctamente con el filtrado y ordenación.

## <a name="create-an-about-page-that-shows-student-statistics"></a>Crear una página acerca de que muestra estadísticas de estudiante

Para el sitio Web de Contoso Universidad **sobre** página, podrá mostrar alumnos cuántos han inscrito para cada fecha de inscripción. Esto requiere cálculos de agrupación y simples en los grupos. Para ello, deberá llevar a cabo lo siguiente:

* Cree una clase de modelo de vista para los datos que necesita para pasar a la vista.

* Modifique el método acerca del controlador Home.

* Modificar la vista acerca de.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Crear un *SchoolViewModels* carpeta en el *modelos* carpeta.

En la nueva carpeta, agregue un archivo de clase *EnrollmentDateGroup.cs* y reemplace el código de plantilla con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>Modifique el controlador Home

En *HomeController.cs*, agregue las siguientes instrucciones using en la parte superior del archivo:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

Agregue una variable de clase para el contexto de base de datos inmediatamente después de la llave de apertura para la clase y obtener una instancia del contexto de ASP.NET Core DI:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

Reemplace el método `About` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula el número de entidades en cada grupo y almacena los resultados en una colección de `EnrollmentDateGroup` ver objetos del modelo.
> [!NOTE] 
> En la 1.0 versión de Entity Framework Core, el conjunto de resultados completo se devuelve al cliente y agrupación se realiza en el cliente. En algunos casos esto puede crear problemas de rendimiento. Asegúrese de probar el rendimiento con los volúmenes de datos de producción y, si es necesario usar SQL sin formato para realizar la agrupación en el servidor. Para obtener información sobre cómo usar SQL sin formato, vea [del último tutorial de esta serie](advanced.md).

### <a name="modify-the-about-view"></a>Modificar la vista

Reemplace el código de la *Views/Home/About.cshtml* archivo con el código siguiente:

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

Ejecutar la aplicación y haga clic en el **sobre** vínculo. El número de alumnos para cada fecha de inscripción se muestra en una tabla.

![Acerca de la página](sort-filter-page/_static/about.png)

## <a name="summary"></a>Resumen

En este tutorial, ha visto cómo realizar la ordenación, filtrado, paginación y agrupación. En el siguiente tutorial, aprenderá cómo controlar los cambios del modelo de datos mediante el uso de las migraciones.

>[!div class="step-by-step"]
[Anterior](crud.md)
[Siguiente](migrations.md)  
