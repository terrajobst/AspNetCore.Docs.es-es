---
title: "Carga de archivos en una página de Razor en ASP.NET Core"
author: guardrex
description: "Aprenda a cargar archivos en una página de Razor."
manager: wpickett
ms.author: riande
ms.date: 09/12/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 24eaa0dd9293cc932c51d280300308e835a0840e
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>Carga de archivos en una página de Razor en ASP.NET Core

Por [Luke Latham](https://github.com/guardrex)

En esta sección se muestra cómo cargar archivos con una página de Razor.

La [aplicación de ejemplo Razor Pages Movie](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) de este tutorial usa el enlace de modelo simple para cargar archivos, lo que funciona bien para cargar archivos pequeños. Para más información sobre la transmisión de archivos de gran tamaño, vea [Uploading large files with streaming (Carga de archivos grandes mediante transmisión)](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

En los pasos siguientes se agrega una característica de carga de archivo de programación de película a la aplicación de ejemplo. Una programación de película está representada por una clase `Schedule`. La clase incluye dos versiones de la programación. Una versión se proporciona a los clientes, `PublicSchedule`. La otra se usa para los empleados de la empresa, `PrivateSchedule`. Cada versión se carga como un archivo independiente. El tutorial muestra cómo realizar dos cargas de archivos desde una página con un solo elemento POST en el servidor.

## <a name="add-a-fileupload-class"></a>Adición de una clase FileUpload

A continuación, cree una página de Razor para controlar un par de cargas de archivos. Agregue una clase `FileUpload`, que está enlazada a la página para obtener los datos de programación. Haga clic con el botón derecho en la carpeta *Models*. Seleccione **Agregar** > **Clase**. Asigne a la clase el nombre **FileUpload** y agregue las siguientes propiedades:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

La clase tiene una propiedad para el título de la programación y otra para cada una de las dos versiones de la programación. Las tres propiedades son necesarias y el título debe tener entre 3 y 60 caracteres.

## <a name="add-a-helper-method-to-upload-files"></a>Agregar un método auxiliar para cargar archivos

Para evitar la duplicación de código para el procesamiento de archivos de programación cargados, primero agregue un método auxiliar estático. Cree una carpeta *Utilities* en la aplicación y agregue un archivo *FileHelpers.cs* con el siguiente contenido. El método auxiliar, `ProcessFormFile`, toma un elemento [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) y [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) y devuelve una cadena con el contenido y el tamaño del archivo. Se comprueban el tipo de contenido y la longitud. Si el archivo no pasa una comprobación de validación, se agrega un error a `ModelState`.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a>Adición de la clase Schedule

Haga clic con el botón derecho en la carpeta *Models*. Seleccione **Agregar** > **Clase**. Asigne a la clase el nombre **Schedule** y agregue las siguientes propiedades:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

La clase usa los atributos `Display` y `DisplayFormat`, que generan títulos descriptivos y formato cuando se representan los datos de programación.

## <a name="update-the-moviecontext"></a>Actualización de MovieContext

Especifique `DbSet` en `MovieContext` (*Models/MovieContext.cs*) para las programaciones:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Adición de la tabla Schedule a la base de datos

Abra la Consola del Administrador de paquetes (PMC): **Herramientas** > **Administrador de paquetes NuGet** > **Consola del Administrador de paquetes**.

![Menú de PMC](../first-mvc-app/adding-model/_static/pmc.png)

En la PMC, ejecute los siguientes comandos. Estos comandos agregan una tabla `Schedule` a la base de datos:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Adición de una página de Razor de carga de archivos

En la carpeta *Pages*, cree una carpeta *Schedules*. En la carpeta *Schedules*, cree una página denominada *Index.cshtml* para cargar una programación con el siguiente contenido:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Cada grupo del formulario incluye una **\<etiqueta>** que muestra el nombre de cada propiedad de clase. Los atributos `Display` del modelo `FileUpload` proporcionan los valores de presentación de las etiquetas. Por ejemplo, el nombre para mostrar de la propiedad `UploadPublicSchedule` se establece con `[Display(Name="Public Schedule")]` y, por tanto, muestra "Programación pública" en la etiqueta cuando se presenta el formulario.

Cada grupo del formulario incluye un **\<intervalo>** de validación. Si la entrada del usuario no cumple los atributos de propiedad establecidos en la clase `FileUpload` o si se produce un error en alguna de las comprobaciones de validación del archivo del método `ProcessFormFile`, no se valida el modelo. Cuando se produce un error en la validación del modelo, se presenta un útil mensaje de validación al usuario. Por ejemplo, la propiedad `Title` se anota con `[Required]` y `[StringLength(60, MinimumLength = 3)]`. Si el usuario no proporciona un título, recibe un mensaje que indica que se necesita un valor. Si el usuario especifica un valor de menos de tres caracteres o de más de sesenta caracteres, recibe un mensaje que indica que el valor tiene una longitud incorrecta. Si se proporciona un archivo sin contenido, aparece un mensaje que indica que el archivo está vacío.

## <a name="add-the-page-model"></a>Agregar el modelo de página

Agregue el modelo de página (*Index.cshtml.cs*) a la carpeta *Schedules*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

El modelo de página (`IndexModel` en *Index.cshtml.cs*) enlaza la clase `FileUpload`:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

El modelo además usa una lista de las programaciones (`IList<Schedule>`) para mostrar las programaciones almacenadas en la base de datos en la página:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Cuando se carga la página con `OnGetAsync`, `Schedules` se rellena a partir de la base de datos y se usa para generar una tabla HTML de programaciones cargadas:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

Cuando el formulario se publica en el servidor, se activa `ModelState`. Si no es válido, `Schedule` se vuelve a generar y la página se presenta con uno o más mensajes de validación que indican el motivo del error de validación de la página. Si es válido, las propiedades `FileUpload` se usan en *OnPostAsync* para completar la carga de archivos para las dos versiones de la programación y para crear un nuevo objeto `Schedule` para almacenar los datos. La programación luego se guarda en la base de datos:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Vinculación de una página de Razor de carga de archivos

Abra *_Layout.cshtml* y agregue un vínculo a la barra de navegación para llegar a la página de carga de archivos:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Adición de una página para confirmar la eliminación de la programación

Cuando el usuario hace clic para eliminar una programación, se recomienda ofrecerle una oportunidad de cancelar la operación. Agregue una página de confirmación de eliminación (*Delete.cshtml*) a la carpeta *Schedules*:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

El modelo de página (*Delete.cshtml.cs*) carga una sola programación identificada por `id` en los datos de ruta de la solicitud. Agregue el archivo *Delete.cshtml.cs* a la carpeta *Schedules*:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

El método `OnPostAsync` controla la eliminación de la programación mediante su `id`:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

Después de eliminar correctamente la programación, `RedirectToPage` vuelve a enviar al usuario a la página de programaciones *Index.cshtml*.

## <a name="the-working-schedules-razor-page"></a>Página de Razor Schedules activa

Cuando se carga la página, las etiquetas y las entradas del título de la programación, la programación pública y la privada se presentan con un botón de envío:

![Página de Razor Schedules tal como se muestra en la carga inicial sin errores de validación ni campos vacíos](uploading-files/_static/browser1.png)

La selección del botón **Cargar** sin rellenar ninguno de los campos infringe los atributos `[Required]` en el modelo. `ModelState` no es válido. Los mensajes de error de validación se muestran al usuario:

![Los mensajes de error de validación aparecen junto a cada control de entrada](uploading-files/_static/browser2.png)

Escriba dos letras en el campo **Título**. El mensaje de validación cambia para indicar que el título debe tener entre 3 y 60 caracteres:

![Mensaje de validación de título modificado](uploading-files/_static/browser3.png)

Cuando se cargan una o más programaciones, la sección **Programaciones cargadas** presenta las programaciones cargadas:

![Tabla de programaciones cargadas que muestra el título de cada programación, la fecha de carga en UTC, el tamaño de archivo de versión pública y privada](uploading-files/_static/browser4.png)

El usuario puede hacer clic en el vínculo **Eliminar** desde allí para llegar a la vista de confirmación de eliminación, donde tiene una oportunidad de confirmar o cancelar la operación de eliminación.

## <a name="troubleshooting"></a>Solución de problemas

Para más información de solución de problemas de carga de `IFormFile`, vea [Cargas de archivos en ASP.NET Core: Solución de problemas](xref:mvc/models/file-uploads#troubleshooting).

Gracias por seguir esta introducción a las páginas de Razor. Le agradeceremos todos los comentarios que quiera hacernos. [Introducción a MVC y EF Core](xref:data/ef-mvc/intro) es un excelente seguimiento de este tutorial.

## <a name="additional-resources"></a>Recursos adicionales

* [Cargas de archivos en ASP.NET Core](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[Anterior: Validación](xref:tutorials/razor-pages/validation)
