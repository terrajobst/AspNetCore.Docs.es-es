---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinar cómo ASP.NET MVC aplica la técnica scaffolding la aplicación auxiliar DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: ece3645f2b37550058a5c93bdd9badbc088ae11c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382970"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examinar cómo ASP.NET MVC aplica la técnica scaffolding la aplicación auxiliar DropDownList
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, seleccione **Agregar controlador**. Asigne al controlador el **StoreManagerController**. Establecer las opciones de la **Agregar controlador** diálogo tal como se muestra en la imagen siguiente.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Editar el *StoreManager\Index.cshtml* ver y quitar `AlbumArtUrl`. Quitar `AlbumArtUrl` hará que la presentación sea más legible. A continuación se muestra el código completado.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Abra el *Controllers\StoreManagerController.cs* de archivo y busque el `Index` método. Agregar el `OrderBy` cláusula por lo que los álbumes se ordenarán por el precio. El código completo se muestra a continuación.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Ordenar por precio le resultará más fácil probar los cambios realizados en la base de datos. Cuando se está probando la edición y crear métodos, puede usar un precio bajo por lo que los datos guardados aparecerán primeros.

Abra el *StoreManager\Edit.cshtml* archivo. Agregue la siguiente línea justo después de la etiqueta de leyenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

El código siguiente muestra el contexto de este cambio:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

El `AlbumId` es necesario para realizar cambios en un registro de álbum.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione esta opción para la **Admin** vincular y, después, seleccione el **crear nuevo** vínculo para crear un nuevo álbum. Compruebe que se ha guardado la información del álbum. Editar un álbum y comprobar los cambios realizados se conservan.

### <a name="the-album-schema"></a>El esquema de álbum

El `StoreManager` controlador creado por el mecanismo de scaffolding de MVC permite el acceso CRUD (creación, lectura, actualización, eliminación) a los álbumes en la base de datos de tienda de música. A continuación se muestra el esquema de información del álbum:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

El `Albums` tabla no almacena el género del álbum y la descripción, almacena una clave externa para el `Genres` tabla. El `Genres` tabla contiene el nombre de género y la descripción. Del mismo modo, el `Albums` tabla no contiene el nombre de intérpretes del álbum, pero con una clave externa para el `Artists` tabla. El `Artists` tabla contiene el nombre del artista. Si examina los datos en el `Albums` tabla, puede ver cada fila contiene una clave externa a la `Genres` tabla y una clave externa para el `Artists` tabla. La imagen siguiente se muestran algunos datos de la tabla desde el `Albums` tabla.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>La etiqueta HTML Select

El código HTML `<select>` elemento (creado por el código HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar) se usa para mostrar una lista completa de valores (por ejemplo, la lista de géneros). Para editar formularios, cuando se conoce el valor actual, la lista de selección puede mostrar el valor actual. Se ha visto este anteriormente cuando se establece el valor seleccionado **Comedia**. La lista de selección es ideal para mostrar la categoría o datos de clave externa. El `<select>` (elemento) para la clave externa de género muestra la lista de nombres de género posibles, pero al guardar el formulario de la propiedad de género se actualiza con el género valor de clave externa, no el nombre de género mostrada. En la imagen siguiente, el género seleccionado es **Disco** y es el intérprete **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Examinar el MVC de ASP.NET de código con scaffold

Abra el *Controllers\StoreManagerController.cs* de archivo y busque el `HTTP GET Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

El `Create` método agrega dos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objetos a la `ViewBag`, uno para contener la información de género y otro para contener la información del intérprete. El [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) usado anteriormente sobrecarga del constructor toma tres argumentos:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *elementos*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista. En el ejemplo anterior devuelve la lista de géneros `db.Genres`.
2. *dataValueField*: el nombre de la propiedad en el **IEnumerable** lista que contiene el valor de clave. En el ejemplo anterior, `GenreId` y `ArtistId`.
3. *dataTextField*: el nombre de la propiedad en el **IEnumerable** lista que contiene la información para mostrar. En los artistas y la tabla de género, el `name` se usa el campo.

Abra el *Views\StoreManager\Create.cshtml* de archivo y examine el `Html.DropDownList` marcado de aplicación auxiliar para el campo del género.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La primera línea muestra que la vista de creación tarda un `Album` modelo. En el `Create` método mostrado anteriormente, se ha pasado ningún modelo, por lo que la vista obtiene un **null** `Album` modelo. En este momento estamos creando un nuevo álbum no tengamos cualquiera `Album` datos para él.

El [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) sobrecarga anterior toma el nombre del campo que se va a enlazar el modelo. También usa este nombre para buscar un **ViewBag** objeto que contiene un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objeto. Utilizar esta sobrecarga, se requiere que el nombre de la **ViewBag SelectList** objeto `GenreId`. El segundo parámetro (`String.Empty`) es el texto que se muestra cuando se selecciona ningún elemento. Esto es exactamente lo que queremos crear un nuevo álbum. Si quita el segundo parámetro y usa el código siguiente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

La lista de selección haría de forma predeterminada el primer elemento o Rock en nuestro ejemplo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examinar el `HTTP POST Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Esta sobrecarga de la `Create` método toma un `album` objeto, creado por el sistema de enlace de modelo de ASP.NET MVC de los valores de formulario publicados. Cuando se envía un álbum nuevo, si el estado del modelo es válido y no hay ningún error de base de datos, el álbum nuevo se agrega a la base de datos. La siguiente imagen muestra la creación de un nuevo álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Puede usar el [herramienta fiddler](http://www.fiddler2.com/fiddler2/) para examinar los valores de formulario enviados ese enlace de modelos de ASP.NET MVC usa para crear el objeto de álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>Refactorización de la creación de ViewBag SelectList

Tanto el `Edit` métodos y la `HTTP POST Create` método tener código idéntico para configurar el **SelectList** en el **ViewBag**. Siguiendo el espíritu de [seco](http://en.wikipedia.org/wiki/Don't_repeat_yourself), se va a refactorizar este código. Nos aseguraremos de hacer uso de este refactoriza código más adelante.

Cree un nuevo método para agregar un género y el intérprete **SelectList** a la **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Reemplace las dos líneas de configuración el `ViewBag` en cada uno de los `Create` y `Edit` métodos con una llamada a la `SetGenreArtistViewBag` método. A continuación se muestra el código completado.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Crear un álbum nuevo y editar un álbum para comprobar que los cambios funcionan.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Se pasa explícitamente el SelectList al control DropDownList

Las vistas create y edit crean mediante el uso de scaffolding de ASP.NET MVC, la siguiente **DropDownList** sobrecarga:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

El `DropDownList` marcado para la vista de creación se muestra a continuación.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Porque el `ViewBag` propiedad para el `SelectList` se denomina `GenreId`, el **DropDownList** auxiliar usará el `GenreId` **SelectList** en el **ViewBag** . En la siguiente **DropDownList** sobrecarga, el `SelectList` se pasa explícitamente.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Abra el *Views\StoreManager\Edit.cshtml* de archivo y cambie el **DropDownList** llamada pasar explícitamente la **SelectList**, mediante la sobrecarga del anterior. Hacer esto para la categoría género. El código completo se muestra a continuación:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Ejecute la aplicación y haga clic en el **Admin** vincular, navegue a un álbum de Jazz y seleccione el **editar** vínculo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

En lugar de mostrar Jazz como el género seleccionado actualmente, se muestra Rock. Cuando el argumento de cadena (la propiedad para enlazar) y el **SelectList** objeto tienen el mismo nombre, no se utiliza el valor seleccionado. Cuando no hay ha proporcionado ningún valor seleccionado, los exploradores de forma predeterminada el primer elemento en el **SelectList**(que es **Rock** en el ejemplo anterior). Se trata de una limitación conocida de la **DropDownList** auxiliar.

Abra el *Controllers\StoreManagerController.cs* y cambie el **SelectList** a los nombres de objeto `Genres` y `Artists`. El código completo se muestra a continuación:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Los nombres de géneros y artistas son nombres mejor para las categorías, ya que contienen algo más que el identificador de cada categoría. La refactorización que se realizaron anteriormente pagado. En lugar de cambiar el **ViewBag** en cuatro métodos, los cambios eran aislados en el `SetGenreArtistViewBag` método.

Cambiar el **DropDownList** llamar en crear y editar vistas para usar la nueva **SelectList** nombres. El nuevo marcado para la vista de edición se muestra a continuación:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

La vista de creación requiere una cadena vacía para impedir que el primer elemento de la SelectList que se muestre.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Crear un álbum nuevo y editar un álbum para comprobar que los cambios funcionan. Probar el código de edición seleccionando un álbum con un género Rock distinto de.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Uso de un modelo de vista con la aplicación auxiliar DropDownList

Cree una nueva clase en la carpeta ViewModels denominada `AlbumSelectListViewModel`. Reemplace el código de la `AlbumSelectListViewModel` clase con lo siguiente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

El `AlbumSelectListViewModel` constructor toma un álbum, una lista de los artistas y géneros y crea un objeto que contiene el álbum y un `SelectList` para géneros y artistas.

Compile el proyecto para que el `AlbumSelectListViewModel` está disponible cuando se crea una vista en el paso siguiente.

Agregar un `EditVM` método a la `StoreManagerController`. A continuación se muestra el código completado.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Haga clic en `AlbumSelectListViewModel`, seleccione **resolver**, a continuación, **mediante MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Como alternativa, puede agregar la siguiente instrucción using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Haga clic en `EditVM` y seleccione **agregar vista**. Use las opciones que se muestra a continuación.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Seleccione **agregar**, a continuación, reemplace el contenido de la *Views\StoreManager\EditVM.cshtml* archivo con la siguiente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

El `EditVM` marcado es muy similar a la versión original `Edit` marcado con las siguientes excepciones.

- Modelar las propiedades en el `Edit` vista tienen el formato `model.property`(por ejemplo, `model.Title` ). Modelar las propiedades en el `EditVm` vista tienen el formato `model.Album.property`(por ejemplo, `model.Album.Title`). Eso es porque la `EditVM` vista se pasa un contenedor para un `Album`, no un `Album` como en el `Edit` vista.
- El **DropDownList** segundo parámetro procede el modelo de vista, no el **ViewBag**.
- El **BeginForm** auxiliar en el `EditVM` vea explícitamente las entradas a la `Edit` método de acción. Mediante la publicación de nuevo a la `Edit` acción, no tiene que escribir un `HTTP POST EditVM` acción y puede reutilizar el `HTTP POST` `Edit` acción.

Ejecute la aplicación y modificar un álbum. Cambie la dirección URL para usar `EditVM`. Cambiar un campo y presione la **guardar** botón para comprobar el código que funciona.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>¿Qué enfoque se debe usar?

Los tres métodos que se muestran son aconseja. Muchos desarrolladores prefieren explictily pase la `SelectList` a la `DropDownList` mediante el `ViewBag`. Este enfoque tiene la ventaja añadida de lo que le ofrece la flexibilidad de usar un nombre más adecuado para la colección. Tenerse en cuenta es que no se asigne el `ViewBag SelectList` el mismo nombre que la propiedad del modelo de objetos.

Algunos desarrolladores prefieren el enfoque de ViewModel. Otros Observe el marcado más detallado y generan una desventaja HTML del enfoque de ViewModel.

En esta sección, hemos aprendido tres métodos para usar el **DropDownList** con datos de categoría. En la siguiente sección, mostraremos cómo agregar una nueva categoría.

> [!div class="step-by-step"]
> [Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Siguiente](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
