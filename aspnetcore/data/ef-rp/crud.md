---
title: "Páginas de Razor con EF Core - CRUD - 2 de 8"
author: rick-anderson
description: "Muestra cómo crear, leer, actualizar y eliminar con EF principales"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: c26ba75f6a401d50a6b46bd7ee40500c5736f20f
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a>Crear, leer, actualizar y eliminar - Core EF con páginas de Razor (2 de 8)

Por [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), y [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

En este tutorial, el con scaffolding CRUD (crear, leer, actualizar y eliminar) es revisar y personalizar el código.

Nota: Para minimizar la complejidad y mantener estos tutoriales se centra en EF Core, código EF básico se usa en los archivos de código subyacente de las páginas de Razor. Algunos programadores utilizan un patrón de capa o en el repositorio de servicios en para crear una capa de abstracción entre la interfaz de usuario (las páginas de Razor) y la capa de acceso a datos.

En este tutorial, crear, editar, eliminar y detalles de las páginas de Razor en el *estudiante* carpeta se modifican.

El código con scaffolding usa el modelo siguiente para crear, editar y eliminar páginas:

* Obtener y mostrar los datos solicitados con el método GET de HTTP `OnGetAsync`.
* Guardar los cambios en los datos con el método HTTP POST `OnPostAsync`.

Las páginas de índice y detalles de obtener y mostrar los datos solicitados con el método GET de HTTP`OnGetAsync`

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a>Reemplace SingleOrDefaultAsync por FirstOrDefaultAsync

El código generado utiliza [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) para capturar la entidad solicitada. [FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) es más eficaz en una entidad de filas:

* A menos que el código necesita para comprobar que no hay más de una entidad devuelta por la consulta. 
* `SingleOrDefaultAsync`captura más datos y realiza trabajo innecesario.
* `SingleOrDefaultAsync`produce una excepción si hay más de una entidad que se ajuste a la parte del filtro.
*  `FirstOrDefaultAsync`no produce una excepción si no hay más de una entidad que se ajuste a la parte del filtro.

Reemplace globalmente `SingleOrDefaultAsync` con `FirstOrDefaultAsync`. `SingleOrDefaultAsync`se utiliza en 5 lugares:

* `OnGetAsync`en la página de detalles.
* `OnGetAsync`y `OnPostAsync` en las páginas de editar y eliminar.

<a name="FindAsync"></a>
### <a name="findasync"></a>FindAsync

En gran parte del código con scaffolding, [aplica findasync a](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) puede usarse en lugar de `FirstOrDefaultAsync` o `SingleOrDefaultAsync`. 

`FindAsync`:

* Busca una entidad con la clave principal (PK). Si una entidad con la clave principal es sometida a seguimiento por el contexto, se devuelve sin una solicitud a la base de datos.
* Es sencilla y concisa.
* Está optimizado para buscar una sola entidad.
* Puede tener ventajas de rendimiento en algunas situaciones, pero rara vez entran en juego para escenarios web normal.
* Utiliza implícitamente [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) en lugar de [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).
Pero si van a incluir otras entidades, buscar ya no es apropiada. Esto significa que puede que necesite abandonar Find y mover a una consulta como su progresa de aplicación.

## <a name="customize-the-details-page"></a>Personalizar la página de detalles

Vaya a `Pages/Students` página. El **editar**, **detalles**, y **eliminar** vínculos son generados por el [auxiliar de etiquetas de delimitador](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) en el *páginas/estudiantes / Index.cshtml* archivo.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

Seleccione un vínculo de detalles. La dirección URL tiene el formato `http://localhost:5000/Students/Details?id=2`. Se pasa el Id. de estudiante utilizando una cadena de consulta (`?id=2`).

Actualizar la edición, los detalles y eliminar las páginas de Razor para usar el `"{id:int}"` plantilla de ruta. Cambie la directiva de página de cada una de estas páginas de `@page` a `@page "{id:int}"`.

Una solicitud a la página con la plantilla de ruta "{Id.: int}" encargada de **no** incluyen una ruta valor devuelve HTTP 404 (no encontrado) errores de enteros. Por ejemplo, `http://localhost:5000/Students/Details` devuelve un error 404. Para que el identificador sea opcional, anexe `?` a la restricción de ruta:

 ```cshtml
@page "{id:int?}"
```

Ejecute la aplicación, haga clic en un vínculo de detalles y compruebe la dirección URL está pasando el identificador como datos de ruta (`http://localhost:5000/Students/Details/2`).

No cambiar globalmente `@page` a `@page "{id:int}"`, esta acción saltos así los vínculos a la página principal y crear páginas.

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a>Agregar datos relacionados

El código con scaffolding de la página de índice de los estudiantes que no incluye el `Enrollments` propiedad. En esta sección, el contenido de la `Enrollments` recopilación se muestra en la página de detalles.

El `OnGetAsync` método *Pages/Students/Details.cshtml.cs* utiliza la `FirstOrDefaultAsync` método para recuperar un único `Student` entidad. Agregue el código resaltado siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

El `Include` y `ThenInclude` métodos hacen que el contexto cargar la `Student.Enrollments` propiedad de navegación y dentro de cada inscripción el `Enrollment.Course` propiedad de navegación. Estos métodos son examinied detalladamente en el tutorial relacionadas con la lectura de datos.

El `AsNoTracking` método mejora el rendimiento en escenarios cuando las entidades devueltas no se actualizan en el contexto actual. `AsNoTracking`se explica más adelante en este tutorial.

### <a name="display-related-enrollments-on-the-details-page"></a>Mostrar las inscripciones relacionadas en la página de detalles

Abra *Pages/Students/Details.cshtml*. Agregue el siguiente código resaltado para mostrar una lista de las inscripciones:

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

Si no es correcta la sangría del código después de que se pega el código, presione CTRL-K-D para corregirlo.

El código anterior recorre las entidades en el `Enrollments` propiedad de navegación. Para cada inscripción muestra el título del curso y el grado de. Se recupera el título del curso de la entidad de curso que se almacena en la `Course` propiedad de navegación de la entidad de inscripciones.

Ejecutar la aplicación, seleccione la **estudiantes** ficha y haga clic en el **detalles** vínculo acerca de un estudiante. Se muestra la lista de cursos y sus notas para el alumno seleccionado.

## <a name="update-the-create-page"></a>Actualizar la página Crear

Actualización de la `OnPostAsync` método *Pages/Students/Create.cshtml.cs* con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a>TryUpdateModelAsync

Examine la [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) código:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

En el código anterior, `TryUpdateModelAsync<Student>` intenta actualizar el `emptyStudent` objeto usando los valores de formulario expuesto desde el [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) propiedad en el [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0). `TryUpdateModelAsync`solo actualiza las propiedades enumeradas (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).

En el ejemplo anterior:

* El segundo argumento (` "student", // Prefix`) es el prefijo que se usa para buscar valores. No distingue mayúsculas de minúsculas.
* Los valores de formulario expuesto se convierten a los tipos en el `Student` modelados con [enlace de modelo](xref:mvc/models/model-binding#how-model-binding-works).

<a id="overpost"></a>
### <a name="overposting"></a>Publicación excesiva

Usar `TryUpdateModel` actualizar los campos con valores registrados es una práctica recomendada de seguridad porque evita overposting. Por ejemplo, suponga que la entidad Student incluye un `Secret` propiedad que esta página web no debe actualizar o agregar:

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

Incluso si la aplicación no tiene un `Secret` campo en la creación o actualización de página de Razor, un pirata informático podría establecer el `Secret` valor por publicación excesiva. Un pirata informático podría usar una herramienta como Fiddler o escribir código de JavaScript, para registrar un `Secret` formar el valor. El código original no limita los campos que el enlazador de modelos se utiliza cuando crea una instancia de estudiante.

Valor el pirata especificado para el `Secret` se actualiza el campo de formulario en la base de datos. La siguiente imagen muestra la adición de la herramienta de Fiddler el `Secret` campo (con el valor "OverPost") a los valores de formulario expuesto.

![Campo de secreto adición de Fiddler](../ef-mvc/crud/_static/fiddler.png)

El valor "OverPost" se ha agregado correctamente a la `Secret` propiedades de la fila insertada. El Diseñador de aplicaciones que se habían previsto el `Secret` propiedad se establece con la página que se creó.

<a name="vm"></a>
### <a name="view-model"></a>Modelo de vista

Normalmente, un modelo de vista contiene un subconjunto de las propiedades incluidas en el modelo utilizado por la aplicación. El modelo de aplicación se suele denominar el modelo de dominio. El modelo de dominio normalmente contiene todas las propiedades requeridas por la entidad correspondiente en la base de datos. El modelo de vista contiene solo las propiedades necesarias para la capa de interfaz de usuario (por ejemplo, en la página Crear). Además del modelo de vista, algunas aplicaciones usan un modelo de enlace o el modelo de entrada para pasar datos entre la clase de código subyacente de las páginas de Razor y el explorador. Tenga en cuenta la siguiente `Student` modelo de vista:

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

Ver modelos proporcionan una manera alternativa para evitar overposting. El modelo de vista contiene solo las propiedades para ver (pantalla) o actualizar.

El siguiente código utiliza el `StudentVM` modelo de vista para crear un nuevo alumno:

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

El [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) método establece los valores de este objeto al leer valores de otra [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) objeto. `SetValues`usa la coincidencia de nombres de propiedad. El tipo de modelo de vista no necesita estar relacionado con el tipo de modelo, basta con que tienen propiedades que coinciden con.

Usar `StudentVM` requiere [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) actualizarse para utilizar `StudentVM` en lugar de `Student`.

En las páginas de Razor, el `PageModel` clase derivada es el modelo de vista. 

## <a name="update-the-edit-page"></a>Actualizar la página de edición

Actualice el archivo de código subyacente de la página de edición:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

Los cambios de código son similares a la página que se creó con unas pocas excepciones:

* `OnPostAsync`tiene una función opcional `id` parámetro.
* Se capturan los estudiantes actual desde la base de datos, en lugar de crear un estudiante vacía.
* `FirstOrDefaultAsync`se ha reemplazado por [aplica findasync a](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0). `FindAsync`es una buena elección cuando se selecciona una entidad de la clave principal. Vea [aplica findasync a](#FindAsync) para obtener más información.

### <a name="test-the-edit-and-create-pages"></a>La edición de prueba y crear páginas

Crear y editar algunas entidades de estudiante.

## <a name="entity-states"></a>Estados de entidad

El contexto de base de datos realiza un seguimiento de si las entidades en memoria están sincronizadas con sus filas correspondientes en la base de datos. La información de sincronización del contexto de base de datos determina qué ocurre cuando `SaveChanges` se llama. Por ejemplo, cuando una nueva entidad se pasa a la `Add` método, que el estado de la entidad se establece en `Added`. Cuando `SaveChanges` se llama, la base de datos contexto emite un comando INSERT de SQL.

Una entidad puede estar en uno de los siguientes estados:

* `Added`: La entidad no existe todavía en la base de datos. El `SaveChanges` método emite una instrucción INSERT.

* `Unchanged`: No hay cambios tienen que guardarse con esta entidad. Una entidad tiene este estado cuando se leen desde la base de datos.

* `Modified`: Se han modificado algunos o todos los valores de propiedad de la entidad. El `SaveChanges` método emite una instrucción UPDATE.

* `Deleted`: La entidad se ha marcado para su eliminación. El `SaveChanges` método emite una instrucción DELETE.

* `Detached`: La entidad no sometida a seguimiento por el contexto de base de datos.

En una aplicación de escritorio, normalmente se establecen automáticamente los cambios de estado. Se lee una entidad, se realizan cambios y el estado de la entidad que se cambie automáticamente al `Modified`. Al llamar a `SaveChanges` genera una instrucción UPDATE de SQL que actualiza sólo las propiedades modificadas.

En una aplicación web, el `DbContext` que lee una entidad y muestra los datos se eliminan una vez que se representa una página. Cuando una página de `OnPostAsync` se llama al método, se realiza una solicitud web nueva y con una nueva instancia de la `DbContext`. Volver a leer la entidad en ese contexto nuevo simula el procesamiento de escritorio.

## <a name="update-the-delete-page"></a>Actualizar la página de borrado

En esta sección, se agrega código al implementar un error personalizado aparece un mensaje cuando la llamada a `SaveChanges` se produce un error. Agregue una cadena para que contenga los posibles mensajes de error:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

Reemplace el método `OnGetAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

El código anterior contiene el parámetro opcional `saveChangesError`. `saveChangesError`indica si se llamó al método después de un error al eliminar el objeto de student. La operación de eliminación puede producir un error debido a problemas de red transitorios. Errores transitorios de red están más probable que en la nube. `saveChangesError`es false cuando la página de borrado `OnGetAsync` se llama desde la interfaz de usuario. Cuando `OnGetAsync` llama a `OnPostAsync` (debido a un error la operación de eliminación), el `saveChangesError` del parámetro es true.

### <a name="the-delete-pages-onpostasync-method"></a>El método de eliminación páginas OnPostAsync

Reemplace el `OnPostAsync` con el código siguiente:

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

El código anterior recupera la entidad seleccionada, a continuación, llama el `Remove` método para establecer el estado de la entidad como `Deleted`. Cuando `SaveChanges` se denomina un DELETE de SQL se genere un comando. Si `Remove` se produce un error:

* Se detectó la excepción de base de datos.
* Las páginas de Delete `OnGetAsync` método se llama con `saveChangesError=true`.

### <a name="update-the-delete-razor-page"></a>Actualizar la página de Razor de borrado

Agregue el siguiente mensaje de error resaltado a la página de Razor eliminar.

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

Pruebe a eliminar.

## <a name="common-errors"></a>Errores comunes

/ Página principal del alumno u otros vínculos no funcionan:

Compruebe la página de Razor contiene el valor correcto `@page` directiva. Por ejemplo, la página de Razor estudiante/principal debe **no** contiene una plantilla de ruta:

```cshtml
@page "{id:int}"
```

Cada página de Razor debe incluir el `@page` directiva.

>[!div class="step-by-step"]
[Anterior](xref:data/ef-rp/intro)
[Siguiente](xref:data/ef-rp/sort-filter-page)
