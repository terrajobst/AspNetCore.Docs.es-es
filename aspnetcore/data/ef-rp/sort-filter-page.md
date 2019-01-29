---
title: 'Páginas de Razor con EF Core en ASP.NET Core: Ordenación, filtrado y paginación (3 de 8)'
author: rick-anderson
description: En este tutorial agregará la funcionalidad de ordenación, filtrado y paginación a la página usando ASP.NET Core y Entity Framework Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/sort-filter-page
ms.openlocfilehash: 350243fb94b4798293a5a61b580c3b3b4d8c6d4a
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444303"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---sort-filter-paging---3-of-8"></a>Páginas de Razor con EF Core en ASP.NET Core: Ordenación, filtrado y paginación (3 de 8)

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

Por [Tom Dykstra](https://github.com/tdykstra), [Rick Anderson](https://twitter.com/RickAndMSFT) y [Jon P Smith](https://twitter.com/thereformedprog)

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

En este tutorial se agregan las funcionalidades de ordenación, filtrado, agrupación y paginación.

En la siguiente ilustración se muestra una página completa. Los encabezados de columna son vínculos interactivos para ordenar la columna. Si se hace clic de forma consecutiva en el encabezado de una columna, el criterio de ordenación cambia entre ascendente y descendente.

![Página de índice de Students](sort-filter-page/_static/paging.png)

Si experimenta problemas que no puede resolver, descargue la [aplicación completada](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).

## <a name="add-sorting-to-the-index-page"></a>Agregar ordenación a la página de índice

Agregue cadenas al `PageModel` de *Students/Index.cshtml.cs* para que contenga los parámetros de ordenación:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet1&highlight=10-13)]

Actualice *Students/Index.cshtml.cs* `OnGetAsync` con el código siguiente:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly)]

El código anterior recibe un parámetro `sortOrder` de la cadena de consulta en la dirección URL. El [asistente de etiquetas delimitadoras](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper
) genera la dirección URL (incluida la cadena de consulta).

El parámetro `sortOrder` es "Name" o "Date". Opcionalmente, el parámetro `sortOrder` puede ir seguido de "_desc" para especificar el orden descendente. El criterio de ordenación predeterminado es el ascendente.

Cuando se solicita la página de índice del vínculo **Students** no hay ninguna cadena de consulta. Los alumnos se muestran en orden ascendente por apellido. El orden ascendente por apellido es el valor predeterminado (caso de paso explícito) en la instrucción `switch`. Cuando el usuario hace clic en un vínculo de encabezado de columna, se proporciona el valor `sortOrder` correspondiente en el valor de la cadena de consulta.

La página de Razor usa `NameSort` y `DateSort` para configurar los hipervínculos del encabezado de columna con los valores de cadena de consulta adecuados:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=3-4)]

El código siguiente contiene el [operador ?:](/dotnet/csharp/language-reference/operators/conditional-operator) condicional de C#:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_Ternary)]

La primera línea especifica que, cuando `sortOrder` es NULL o está vacío, `NameSort` se establece en "name_desc". Si `sortOrder` **no** es NULL ni está vacío, `NameSort` se establece en una cadena vacía.

El `?: operator` también se conoce como el operador ternario.

Estas dos instrucciones habilitan la página para establecer los hipervínculos de encabezado de columna de la siguiente forma:

| Criterio de ordenación actual | Hipervínculo de apellido | Hipervínculo de fecha |
|:--------------------:|:-------------------:|:--------------:|
| Apellido: ascendente | descending        | ascending      |
| Apellido: descendente | ascending           | ascending      |
| Fecha: ascendente       | ascending           | descending     |
| Fecha: descendente      | ascending           | ascending      |

El método usa LINQ to Entities para especificar la columna por la que se va a ordenar. El código inicializa un `IQueryable<Student>` antes de la instrucción switch y lo modifica en la instrucción switch:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnly&highlight=6-999)]

 Cuando se crea o se modifica un `IQueryable`, no se envía ninguna consulta a la base de datos. La consulta no se ejecuta hasta que el objeto `IQueryable` se convierte en una colección. `IQueryable` se convierte en una colección mediante una llamada a un método como `ToListAsync`. Por lo tanto, el código `IQueryable` produce una única consulta que no se ejecuta hasta la siguiente instrucción:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortOnlyRtn)]

`OnGetAsync` se podría detallar con un gran número de columnas ordenables.

### <a name="add-column-heading-hyperlinks-to-the-student-index-page"></a>Agregar hipervínculos de encabezado de columna a la página de índice de Student

Reemplace el código de *Students/Index.cshtml* con el siguiente código resaltado:

[!code-html[](intro/samples/cu21/Pages/Students/Index2.cshtml?highlight=17-19,25-27)]

El código anterior:

* Agrega hipervínculos a los encabezados de columna `LastName` y `EnrollmentDate`.
* Usa la información de `NameSort` y `DateSort` para configurar hipervínculos con los valores de criterio de ordenación actuales.

Para comprobar que la ordenación funciona:

* Ejecute la aplicación y haga clic en la pestaña **Students**.
* Haga clic en **Last Name**.
* Haga clic en **Enrollment Date**.

Para comprender mejor el código:

* En *Student/Index.cshtml.cs*, establezca un punto de interrupción en `switch (sortOrder)`.
* Agregue una inspección para `NameSort` y `DateSort`.
* En *Student/Index.cshtml*, establezca un punto de interrupción en `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Ejecute paso a paso el depurador.

## <a name="add-a-search-box-to-the-students-index-page"></a>Agregar un cuadro de búsqueda a la página de índice de Students

Para agregar un filtro a la página de índice de Students:

* Se agrega un cuadro de texto y un botón de envío a la página de Razor. El cuadro de texto proporciona una cadena de búsqueda de nombre o apellido.
* El modelo de página se actualiza para usar el valor del cuadro de texto.

### <a name="add-filtering-functionality-to-the-index-method"></a>Agregar la funcionalidad de filtrado al método Index

Actualice *Students/Index.cshtml.cs* `OnGetAsync` con el código siguiente:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

El código anterior:

* Agrega el parámetro `searchString` al método `OnGetAsync`. El valor de la cadena de búsqueda se recibe desde un cuadro de texto que se agrega en la siguiente sección.
* Se agregó una cláusula `Where` a la instrucción LINQ. La cláusula `Where` selecciona solo los alumnos cuyo nombre o apellido contienen la cadena de búsqueda. La instrucción LINQ se ejecuta solo si hay un valor para buscar.

Nota: El código anterior llama al método `Where` en un objeto `IQueryable` y el filtro se procesa en el servidor. En algunos escenarios, la aplicación puede hacer una llamada al método `Where` como un método de extensión en una colección en memoria. Por ejemplo, suponga que `_context.Students` cambia de `DbSet` de EF Core a un método de repositorio que devuelve una colección `IEnumerable`. Lo más habitual es que el resultado fuera el mismo, pero en algunos casos puede ser diferente.

Por ejemplo, la implementación de .NET Framework de `Contains` realiza una comparación que distingue mayúsculas de minúsculas de forma predeterminada. En SQL Server, la distinción entre mayúsculas y minúsculas de `Contains` viene determinada por la configuración de intercalación de la instancia de SQL Server. SQL Server no diferencia entre mayúsculas y minúsculas de forma predeterminada. Se podría llamar a `ToUpper` para hacer explícitamente que la prueba no distinga entre mayúsculas y minúsculas:

`Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())`

El código anterior garantiza que los resultados no distingan entre mayúsculas y minúsculas si cambia el código para que use `IEnumerable`. Cuando se llama a `Contains` en una colección `IEnumerable`, se usa la implementación de .NET Core. Cuando se llama a `Contains` en un objeto `IQueryable`, se usa la implementación de la base de datos. Devolver un `IEnumerable` desde un repositorio puede acarrear una disminución significativa del rendimiento:

1. Todas las filas se devuelven desde el servidor de base de datos.
1. El filtro se aplica a todas las filas devueltas en la aplicación.

Hay una disminución del rendimiento por llamar a `ToUpper`. El código `ToUpper` agrega una función en la cláusula WHERE de la instrucción SELECT de TSQL. La función agregada impide que el optimizador use un índice. Dado que SQL está instalado para no distinguir entre mayúsculas y minúsculas, es mejor evitar llamar a `ToUpper` cuando no sea necesario.

### <a name="add-a-search-box-to-the-student-index-page"></a>Agregar un cuadro de búsqueda a la página de índice de Student

En *Pages/Student/Index.cshtml*, agregue el siguiente código resaltado para crear un botón **Search** y cromo ordenado.

[!code-html[](intro/samples/cu21/Pages/Students/Index3.cshtml?highlight=14-23&range=1-25)]

El código anterior usa el [asistente de etiquetas](xref:mvc/views/tag-helpers/intro)`<form>` para agregar el cuadro de texto de búsqueda y el botón. De forma predeterminada, el asistente de etiquetas `<form>` envía datos de formulario con POST. Con POST, los parámetros se pasan en el cuerpo del mensaje HTTP y no en la dirección URL. Cuando se usa el método HTTP GET, los datos del formulario se pasan en la dirección URL como cadenas de consulta. Pasar los datos con cadenas de consulta permite a los usuarios marcar la dirección URL. Las [directrices de W3C](https://www.w3.org/2001/tag/doc/whenToUseGet.html) recomiendan el uso de GET cuando la acción no produzca ninguna actualización.

Pruebe la aplicación:

* Seleccione la pestaña **Students** y escriba una cadena de búsqueda.
* Seleccione **Search**.

Fíjese en que la dirección URL contiene la cadena de búsqueda.

```html
http://localhost:5000/Students?SearchString=an
```

Si se colocó un marcador en la página, el marcador contiene la dirección URL a la página y la cadena de consulta de `SearchString`. El `method="get"` en la etiqueta `form` es lo que ha provocado que se generara la cadena de consulta.

Actualmente, cuando se selecciona un vínculo de ordenación del encabezado de columna, el filtro de valor del cuadro **Search** se pierde. El valor de filtro perdido se fija en la sección siguiente.

## <a name="add-paging-functionality-to-the-students-index-page"></a>Agregar la funcionalidad de paginación a la página de índice de Students

En esta sección, se crea una clase `PaginatedList` para admitir la paginación. La clase `PaginatedList` usa las instrucciones `Skip` y `Take` para filtrar los datos en el servidor en lugar de recuperar todas las filas de la tabla. La ilustración siguiente muestra los botones de paginación.

![Página de índice de Students con vínculos de paginación](sort-filter-page/_static/paging.png)

En la carpeta del proyecto, cree `PaginatedList.cs` con el código siguiente:

[!code-csharp[](intro/samples/cu21/PaginatedList.cs)]

El método `CreateAsync` en el código anterior toma el tamaño y el número de la página, y aplica las instrucciones `Skip` y `Take` correspondientes a `IQueryable`. Cuando `ToListAsync` se llama en `IQueryable`, devuelve una lista que solo contiene la página solicitada. Las propiedades `HasPreviousPage` y `HasNextPage` se usan para habilitar o deshabilitar los botones de página **Previous** y **Next**.

El método `CreateAsync` se usa para crear la `PaginatedList<T>`. No se puede crear un constructor del objeto `PaginatedList<T>`, los constructores no pueden ejecutar código asincrónico.

## <a name="add-paging-functionality-to-the-index-method"></a>Agregar la funcionalidad de paginación al método Index

En *Students/Index.cshtml.cs*, actualice el tipo de `Student` de `IList<Student>` a `PaginatedList<Student>`:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPageType)]

Actualice *Students/Index.cshtml.cs* `OnGetAsync` con el código siguiente:

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage&highlight=1-4,7-14,41-999)]

El código anterior agrega el índice de la página, el `sortOrder` actual y el `currentFilter` a la firma del método.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage2)]

Todos los parámetros son NULL cuando:

* Se llama a la página desde el vínculo **Students**.
* El usuario no ha seleccionado un vínculo de ordenación o paginación.

Cuando se hace clic en un vínculo de paginación, la variable de índice de página contiene el número de página que se tiene que mostrar.

`CurrentSort` proporciona la página de Razor con el criterio de ordenación actual. Se debe incluir el criterio de ordenación actual en los vínculos de paginación para mantener el criterio de ordenación durante la paginación.

`CurrentFilter` proporciona la página de Razor con la cadena del filtro actual. El valor `CurrentFilter`:

* Debe incluirse en los vínculos de paginación para mantener la configuración del filtro durante la paginación.
* Debe restaurarse en el cuadro de texto cuando se vuelva a mostrar la página.

Si se cambia la cadena de búsqueda durante la paginación, la página se restablece a 1. La página debe restablecerse a 1 porque el nuevo filtro puede hacer que se muestren diferentes datos. Cuando se escribe un valor de búsqueda y se selecciona **Submit**:

* La cadena de búsqueda cambia.
* El parámetro `searchString` no es NULL.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage3)]

El método `PaginatedList.CreateAsync` convierte la consulta del alumno en una sola página de alumnos de un tipo de colección que admita la paginación. Esa única página de alumnos se pasa a la página de Razor.

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_SortFilterPage4)]

Los dos signos de interrogación en `PaginatedList.CreateAsync` representan el [operador de uso combinado de NULL](/dotnet/csharp/language-reference/operators/null-conditional-operator). El operador de uso combinado de NULL define un valor predeterminado para un tipo que acepta valores NULL. La expresión `(pageIndex ?? 1)` significa devolver el valor de `pageIndex` si tiene un valor. Devuelve 1 si `pageIndex` no tiene ningún valor.

## <a name="add-paging-links-to-the-student-razor-page"></a>Agregar vínculos de paginación a la página de Razor de alumno

Actualice el marcado en *Students/Index.cshtml*. Se resaltan los cambios:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?highlight=28-31,37-40,68-999)]

Los vínculos del encabezado de la columna usan la cadena de consulta para pasar la cadena de búsqueda actual al método `OnGetAsync`, de modo que el usuario pueda ordenar los resultados del filtro:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=28-31)]

Los botones de paginación se muestran mediante asistentes de etiquetas:

[!code-html[](intro/samples/cu21/Pages/Students/Index.cshtml?range=72-)]

Ejecute la aplicación y vaya a la página Students.

* Para comprobar que la paginación funciona correctamente, haga clic en los vínculos de paginación en distintos criterios de ordenación.
* Para comprobar que la paginación también funciona correctamente con filtrado y ordenación, escriba una cadena de búsqueda e intente llevar a cabo la paginación de nuevo.

![Página de índice de Students con vínculos de paginación](sort-filter-page/_static/paging.png)

Para comprender mejor el código:

* En *Student/Index.cshtml.cs*, establezca un punto de interrupción en `switch (sortOrder)`.
* Agregue una inspección para `NameSort`, `DateSort`, `CurrentSort` y `Model.Student.PageIndex`.
* En *Student/Index.cshtml*, establezca un punto de interrupción en `@Html.DisplayNameFor(model => model.Student[0].LastName)`.

Ejecute paso a paso el depurador.

## <a name="update-the-about-page-to-show-student-statistics"></a>Actualizar la página About para mostrar las estadísticas de los alumnos

En este paso, se actualiza *Pages/About.cshtml* para mostrar cuántos alumnos se han inscrito por cada fecha de inscripción. La actualización usa la agrupación e incluye los siguientes pasos:

* Cree un modelo de vista para los datos usados por la página **About**.
* Actualice la página About para usar el modelo de vista.

### <a name="create-the-view-model"></a>Creación del modelo de vista

Cree una carpeta *SchoolViewModels* en la carpeta *Models*.

En la carpeta *SchoolViewModels*, agregue *EnrollmentDateGroup.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu21/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="update-the-about-page-model"></a>Actualizar el modelo de la página About

Las plantillas web de ASP.NET Core 2.2 no incluyen la página About. Si usa ASP.NET Core 2.2, cree la página About de Razor Pages.

Actualice el archivo *Pages/About.cshtml.cs* con el código siguiente:

[!code-csharp[](intro/samples/cu21/Pages/About.cshtml.cs)]

La instrucción LINQ agrupa las entidades de alumnos por fecha de inscripción, calcula la cantidad de entidades que se incluyen en cada grupo y almacena los resultados en una colección de objetos de modelo de la vista `EnrollmentDateGroup`.

### <a name="modify-the-about-razor-page"></a>Modificar la página de Razor About

Reemplace el código del archivo *Pages/About.cshtml* por el código siguiente:

[!code-html[](intro/samples/cu21/Pages/About.cshtml)]

Ejecute la aplicación y vaya a la página About. En una tabla se muestra el número de alumnos para cada fecha de inscripción.

Si experimenta problemas que no puede resolver, descargue la [aplicación completada para esta fase](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part3-sorting).

![Página About](sort-filter-page/_static/about.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Depuración del código fuente de ASP.NET Core 2.x](https://github.com/aspnet/Docs/issues/4155)

En el tutorial siguiente, la aplicación usa las migraciones para actualizar el modelo de datos.

::: moniker-end

> [!div class="step-by-step"]
> [Anterior](xref:data/ef-rp/crud)
> [Siguiente](xref:data/ef-rp/migrations)
