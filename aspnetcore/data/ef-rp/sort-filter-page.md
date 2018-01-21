---
title: "Páginas de Razor con EF Core - ordenar, filtrar y paginación - 3 de 8"
author: rick-anderson
description: "En este tutorial agregará ordenación, filtrado y paginación funcionalidad en la página utilizando ASP.NET Core y Entity Framework Core."
ms.author: riande
ms.date: 10/22/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 08f00e183dd8a8daa883d0b9ff15698b3a39f625
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="sorting-filtering-paging-and-grouping---ef-core-with-razor-pages-3-of-8"></a>Ordenación, filtrado, paginación y agrupar - Core EF con páginas de Razor (3 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT), y [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

En este tutorial, ordenar, filtrar, agrupar y paginación, se agrega la funcionalidad.

En la siguiente ilustración muestra una página completa. Los encabezados de columna son vínculos activos para ordenar la columna. Al hacer clic en una columna varias veces por alterna entre orden ascendente y orden descendente.

![Página de índice de estudiantes](sort-filter-page/_static/paging.png)

Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

## <a name="add-sorting-to-the-index-page"></a>Agregar ordenación, en la página de índice

Agregar cadenas a la *Students/Index.cshtml.cs* `PageModel` para que contenga los parámetros de ordenación:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]


Actualización de la *Students/Index.cshtml.cs* `OnGetAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

El código anterior recibe un `sortOrder` parámetro de la cadena de consulta en la dirección URL. La dirección URL (incluida la cadena de consulta) generada por el [auxiliar de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
)

El `sortOrder` parámetro es "Name" o "Fecha". El `sortOrder` parámetro opcionalmente puede ir seguido de "_desc" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

Cuando se solicita la página de índice de la **estudiantes** vincular, no hay ninguna cadena de consulta. Los alumnos se muestran en orden ascendente por apellido. Orden ascendente por apellido es el valor predeterminado (paso explícito caso) en el `switch` instrucción. Cuando el usuario hace clic en un vínculo de encabezado de columna, la correspondiente `sortOrder` valor se proporciona en el valor de cadena de consulta.

`NameSort`y `DateSort` se usan en la página de Razor para configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuada:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

Contiene el siguiente código C# [?: operador](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/conditional-operator):

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

La primera línea especifica que, cuando `sortOrder` es nulo o está vacío, `NameSort` se establece en "name_desc". Si `sortOrder` es **no** nulo o está vacío, `NameSort` se establece en una cadena vacía.

El `?: operator` es también conocido como el operador ternario.

Estas dos instrucciones habilitar la vista establecer la columna hipervínculos de encabezado como sigue:

| Criterio de ordenación actual | Hipervínculo apellido | Hipervínculo de fecha |
|:--------------------:|:-------------------:|:--------------:|
| Último nombre ascendente | descending        | ascending      |
| Último nombre descendente | ascending           | ascending      |
| Fecha ascendente       | ascending           | descending     |
| Fecha descendente      | ascending           | ascending      |

El método usa LINQ to Entities para especificar la columna para ordenar por. El código inicializa un `IQueryable<Student> ` antes de la instrucción switch y se modifica en la instrucción switch:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-)]

 Cuando un`IQueryable` se crea o modifica, no hay ninguna consulta se envía a la base de datos. La consulta no se ejecuta hasta que la `IQueryable` objeto se convierte en una colección. `IQueryable`se convierten en una colección mediante una llamada a un método como `ToListAsync`. Por lo tanto, la `IQueryable` resultados en una única consulta que no se ejecuta hasta la siguiente instrucción de código:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync`podría obtener detallado con un gran número de columnas.

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>Agregar los hipervínculos del encabezado de columna a la vista de índice de estudiante

Reemplace el código de *Students/Index.cshtml*, con lo siguiente aparece resaltado este código:

[!code-html[](intro/samples/cu/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

El código anterior:

* Agrega hipervínculos a la `LastName` y `EnrollmentDate` encabezados de columna.
* Usa la información de `NameSort` y `DateSort` para configurar hipervínculos con los valores actuales de criterio de ordenación.

Para comprobar que la ordenación funcione:

* Ejecute la aplicación y seleccione la **estudiantes** ficha.
* Haga clic en **apellido**.
* Haga clic en **fecha de inscripción**.

Para obtener una mejor comprensión del código:

* En *Student/Index.cshtml.cs*, establecer un punto de interrupción en `switch (sortOrder)`.
* Agregue una inspección para `NameSort` y `DateSort`.
* En *Student/Index.cshtml*, establecer un punto de interrupción en `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Paso a paso a través del depurador.

## <a name="add-a-search-box-to-the-students-index-page"></a>Agregue un cuadro de búsqueda a la página de índice de estudiantes

Para agregar el filtrado a la página de índice de los alumnos:

* Un cuadro de texto y un botón de envío se agrega a la página de Razor. El cuadro de texto proporciona una cadena de búsqueda en el nombre del primer o último.
* El archivo de código subyacente se actualiza para usar el valor del cuadro de texto.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar funcionalidad de filtrado para el método de índice

Actualización de la *Students/Index.cshtml.cs* `OnGetAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

El código anterior:

* Agrega el `searchString` parámetro para el `OnGetAsync` método. El valor de cadena de búsqueda se recibe de un cuadro de texto que se agrega en la sección siguiente.
* Agrega a la instrucción LINQ un `Where` cláusula. El `Where` cláusula selecciona sólo los estudiantes cuyo apellido o apellidos contienen la cadena de búsqueda. La instrucción LINQ se ejecuta sólo si hay un valor que se buscará.

Nota: Las llamadas de código anterior la `Where` método en un `IQueryable` objeto y el filtro se procesan en el servidor. En algunos casos, puede llamar aplicación tha el `Where` método como un método de extensión en una colección en memoria. Por ejemplo, suponga que `_context.Students` cambia de núcleo de EF `DbSet` a un método de repositorio que devuelve un `IEnumerable` colección. El resultado podría ser normalmente el mismo, pero en algunos casos puede ser diferente.

Por ejemplo, la implementación de .NET Framework de `Contains` realiza una comparación entre mayúsculas y minúsculas de forma predeterminada. En SQL Server, `Contains` esta característica viene determinado por la configuración de intercalación de la instancia de SQL Server. SQL Server de forma predeterminada en mayúsculas de minúsculas. `ToUpper`se podría llamar para que la prueba explícitamente entre mayúsculas y minúsculas:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

El código anterior garantiza que los resultados están entre mayúsculas y minúsculas si cambia el código para usar `IEnumerable`. Cuando `Contains` se llama en un `IEnumerable` colección, el núcleo de .NET se utiliza la implementación. Cuando `Contains` se llama en un `IQueryable` de objeto, se utiliza la implementación de la base de datos. Devolver un `IEnumerable` desde un repositorio puede tener una penality significativa del rendimiento:

1. Todas las filas se devuelven desde el servidor de base de datos.
1. El filtro se aplica a todas las filas devueltas en la aplicación.

Hay una reducción del rendimiento para llamar a `ToUpper`. La `ToUpper` código agrega una función en la cláusula WHERE de la instrucción SELECT de TSQL. La función agregada impide que el optimizador use un índice. Dado que está instalado SQL como entre mayúsculas y minúsculas, es mejor evitar la `ToUpper` llamar cuando no es necesaria.

### <a name="add-a-search-box-to-the-student-index-view"></a>Agregue un cuadro de búsqueda a la vista de índice de estudiante

En *Views/Student/Index.cshtml*, agregue el código resaltado siguiente para crear un **búsqueda** botón y chrome ordenado.

[!code-html[](intro/samples/cu/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

El código anterior usa el `<form>` [etiqueta auxiliar](xref:mvc/views/tag-helpers/intro) para agregar el cuadro de texto de búsqueda y el botón. De forma predeterminada, la `<form>` auxiliar de etiquetas envía datos de formulario con una entrada de blog. Utiliza el método POST, los parámetros se pasan en el cuerpo del mensaje HTTP y no en la dirección URL. Cuando se utiliza HTTP GET, los datos del formulario se pasan en la dirección URL como las cadenas de consulta. Pasar los datos con cadenas de consulta, permite a los usuarios marcar la dirección URL. El [directrices de W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) recomienda que GET debe utilizarse cuando la acción no da como resultado una actualización.

Pruebe la aplicación:

* Seleccione el **estudiantes** pestaña y escriba una cadena de búsqueda.
* Seleccione **búsqueda**.

Observe que la dirección URL contiene la cadena de búsqueda.

```html
http://localhost:5000/Students?SearchString=an
```

Si la página tiene un marcador, el marcador contiene la dirección URL a la página y el `SearchString` la cadena de consulta. El `method="get"` en la `form` etiqueta es la causa de la cadena de consulta que se genere.

Actualmente, cuando se selecciona un vínculo de ordenación del encabezado de columna, el filtro de valor de la **búsqueda** cuadro se pierde. El valor de filtro perdido se fija en la sección siguiente.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Agregar funcionalidad de paginación a la página de índice de estudiantes

En esta sección, un `PaginatedList` se crea una clase para admitir la paginación. El `PaginatedList` clase utiliza `Skip` y `Take` instrucciones para filtrar los datos en el servidor en lugar de recuperar todas las filas de la tabla. La ilustración siguiente muestra los botones de paginación.

![Página con vínculos de paginación de índice de estudiantes](sort-filter-page/_static/paging.png)

En la carpeta del proyecto, crear `PaginatedList.cs` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/PaginatedList.cs)]

El `CreateAsync` método en el código anterior toma el tamaño de página y número de página y se aplica a la correspondiente `Skip` y `Take` instrucciones para el `IQueryable`. Cuando `ToListAsync` se llama en el `IQueryable`, devuelve una lista que contiene solo la página solicitada. Las propiedades `HasPreviousPage` y `HasNextPage` se utilizan para habilitar o deshabilitar **anterior** y **siguiente** botones de paginación.

El `CreateAsync` método se utiliza para crear la `PaginatedList<T>`. No se puede crear un constructor de la `PaginatedList<T>` del objeto, los constructores no pueden ejecutar código asincrónico.

## <a name="add-paging-functionality-to-the-index-method"></a>Agregar funcionalidad de paginación para el método de índice

En *Students/Index.cshtml.cs*, actualice el tipo de `Student` de `IList<Student>` a `PaginatedList<Student>`:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Actualización de la *Students/Index.cshtml.cs* `OnGetAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-)]

El código anterior agrega el índice de la página actual `sortOrder`y el `currentFilter` para la firma del método.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Todos los parámetros son null cuando:

* Se llama a la página desde la **estudiantes** vínculo.
* El usuario no ha seleccionado una paginación u ordenación vínculo.

Cuando se hace clic en un vínculo de paginación, la variable de índice de la página contiene el número de página para mostrar.

`CurrentSort`proporciona la página de Razor con el criterio de ordenación actual. El criterio de ordenación actual debe incluirse en los vínculos de paginación para mantener el criterio de ordenación durante la paginación.

`CurrentFilter`proporciona la página de Razor con la cadena de filtro actual. El `CurrentFilter` valor:

* Deben incluirse en los vínculos de paginación para mantener la configuración del filtro durante la paginación.
* Deben restaurarse en el cuadro de texto cuando se vuelve a mostrar la página.

Si se cambia la cadena de búsqueda durante la paginación, la página se restablece en 1. Tiene que se restablece en 1, porque el nuevo filtro puede dar lugar a datos diferentes para mostrar la página. Cuando se escribe un valor de búsqueda y **enviar** está seleccionado:

* Se cambia la cadena de búsqueda.
* El `searchString` parámetro no es null.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

El `PaginatedList.CreateAsync` método convierte la consulta de estudiante en una sola página de alumnos de un tipo de colección que admite la paginación. Esa página única de los alumnos se pasa a la página de Razor.

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Los dos signos de interrogación en `PaginatedList.CreateAsync` representan el [operador de uso combinado de null](https://docs.microsoft.com/ dotnet/csharp/language-reference/operators/null-conditional-operator). El operador de uso combinado de null define un valor predeterminado para un tipo que acepta valores NULL. La expresión `(pageIndex ?? 1)` significa que devuelva el valor de `pageIndex` si tiene un valor. Si `pageIndex` no tiene un valor, se devuelven 1.

## <a name="add-paging-links-to-the-student-razor-page"></a>Agregar vínculos de paginación al alumno con la página de Razor

Actualizar el marcado en *Students/Index.cshtml*. Se resaltan los cambios:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-)]

Los vínculos de encabezado de columna utilizan la cadena de consulta para pasar la cadena de búsqueda actual en la `OnGetAsync` método para que el usuario puede ordenar en los resultados del filtro:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=28-31)]

Se muestran los botones de paginación mediante aplicaciones auxiliares de etiquetas:

[!code-html[](intro/samples/cu/Pages/Students/Index.cshtml?range=72-)]

Ejecutar la aplicación y navegue hasta la página de los alumnos.

* Para asegurarse de que funciona la paginación, haga clic en los vínculos de paginación en distintos criterios de ordenación.
* Para comprobar que la paginación funciona correctamente con el filtrado y ordenación, escriba una cadena de búsqueda e inténtelo de paginación.

![Página con vínculos de paginación de índice de estudiantes](sort-filter-page/_static/paging.png)

Para obtener una mejor comprensión del código:

* En *Student/Index.cshtml.cs*, establecer un punto de interrupción en `switch (sortOrder)`.
* Agregue una inspección para `NameSort`, `DateSort`, `CurrentSort`, y `Model.Student.PageIndex`.
* En *Student/Index.cshtml*, establecer un punto de interrupción en `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Paso a paso a través del depurador.

## <a name="update-the-about-page-to-show-student-statistics"></a>Actualizar la página acerca de para mostrar las estadísticas de estudiante

En este paso, *Pages/About.cshtml* se actualiza para mostrar los alumnos cuántos han inscrito para cada fecha de inscripción. La actualización usa la agrupación e incluye los pasos siguientes:

* Cree una clase de modelo de vista para los datos utilizados por la **sobre** página.
* Modificar el archivo de código subyacente y la página acerca de Razor.

### <a name="create-the-view-model"></a>Crear el modelo de vista

Crear un *SchoolViewModels* carpeta en el *modelos* carpeta.

En el *SchoolViewModels* carpeta, agregue un *EnrollmentDateGroup.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-code-behind-page"></a>La página de código subyacente de acerca de la actualización

Actualización de la *Pages/About.cshtml.cs* archivo con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/About.cshtml.cs)]

La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula el número de entidades en cada grupo y almacena los resultados en una colección de `EnrollmentDateGroup` ver objetos del modelo.

Nota: LINQ `group` comandos básicos de EF no se admiten actualmente. En el código anterior, se devuelven todos los registros de estudiantes de SQL Server. El `group` comando se aplica en la aplicación de las páginas de Razor, no en el servidor SQL Server. EF Core 2.1 admitirá esta LINQ `group` operador y la agrupación se produce en el servidor SQL Server. Vea [relacionales: admiten traducir GroupBy() para SQL](https://github.com/aspnet/EntityFrameworkCore/issues/2341). [Núcleo EF 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/roadmap) se publicará con .NET Core 2.1. Para obtener más información, consulte el [guía básica de .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md).

### <a name="modify-the-about-razor-page"></a>Modificar la página de Razor

Reemplace el código de la *Views/Home/About.cshtml* archivo con el código siguiente:

[!code-html[](intro/samples/cu/Pages/About.cshtml)]

Ejecutar la aplicación y navegue hasta la página About. El número de alumnos para cada fecha de inscripción se muestra en una tabla.

Si experimenta problemas no puede resolver, descargue el [aplicación final de esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Acerca de la página](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Depuración del código fuente de ASP.NET Core 2.x](https://github.com/aspnet/Docs/issues/4155)

En el tutorial siguiente, la aplicación utiliza las migraciones para actualizar el modelo de datos.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/crud)
[Siguiente](xref:data/ef-rp/migrations)
