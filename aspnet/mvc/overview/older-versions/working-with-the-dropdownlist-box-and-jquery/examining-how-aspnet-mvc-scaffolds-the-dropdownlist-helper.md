---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinar cómo ASP.NET MVC scaffolds la aplicación auxiliar DropDownList | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 09d2d7a0df5e8ffa14160b7d3c16b1e9da905fa1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874088"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a>Examinar cómo ASP.NET MVC scaffolds la aplicación auxiliar DropDownList
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, seleccione **Agregar controlador**. Nombre del controlador **StoreManagerController**. Establecer las opciones de la **Agregar controlador** diálogo tal como se muestra en la imagen siguiente.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

Editar la *StoreManager\Index.cshtml* ver y quitar `AlbumArtUrl`. Quitar `AlbumArtUrl` hará que la presentación sea más legible. A continuación se muestra el código completado.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

Abra la *Controllers\StoreManagerController.cs* de archivos y busque el `Index` método. Agregar el `OrderBy` cláusula para los álbumes que se va a ordenar por precio. El código completo se muestra a continuación.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

Ordenar por precio le resultará más fácil de probar los cambios a la base de datos. Cuando se prueba por la edición y crear métodos, que puede usar un precio mínimo para que los datos guardados aparecerán en primer lugar.

Abra la *StoreManager\Edit.cshtml* archivo. Agregue la siguiente línea justo después de la etiqueta de leyenda.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

El código siguiente muestra el contexto de este cambio:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

El `AlbumId` es necesario para realizar cambios en un registro de álbum.

Presione CTRL+F5 para ejecutar la aplicación. Seleccione esta opción para la **administración** vincular, a continuación, seleccione la **crear nuevo** vínculo para crear un nuevo álbum. Compruebe que la información del álbum se guardó. Editar un álbum y comprobar los cambios realizados se conservan.

### <a name="the-album-schema"></a>El esquema de álbum

El `StoreManager` controlador creado mediante el mecanismo de scaffolding MVC permite el acceso CRUD (crear, leer, Update, Delete) a los álbumes en la base de datos de almacén de música. A continuación se muestra el esquema para obtener información de álbum:

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

El `Albums` tabla no almacena el género del álbum y la descripción, almacena una clave externa a la `Genres` tabla. El `Genres` tabla contiene el nombre de género y la descripción. Del mismo modo, el `Albums` tabla no contiene el nombre de intérpretes del álbum, pero una clave externa a la `Artists` tabla. El `Artists` tabla contiene el nombre del intérprete. Si examina los datos de la `Albums` tabla, puede ver cada fila contiene una clave externa a la `Genres` tabla y una clave externa a la `Artists` tabla. La imagen siguiente se muestran algunos datos de la tabla desde el `Albums` tabla.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a>La etiqueta de selección HTML

El código HTML `<select>` elemento (creado por el código HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar) se usa para mostrar una lista completa de valores (por ejemplo, la lista de géneros). Para los formularios de edición, cuando se conoce el valor actual, la lista de selección puede mostrar el valor actual. Hemos visto este previamente cuando se establezca el valor seleccionado en **Comedia**. La lista de selección es ideal para mostrar la categoría o datos de clave externa. El `<select>` (elemento) para la clave externa de género muestra la lista de nombres de género posibles, pero cuando se guarda el formulario se actualiza la propiedad de género con el género valor de clave externa, no el nombre mostrado género. En la imagen siguiente, el género seleccionado es **Disco** y es el intérprete **Donna Summer**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a>Examen de MVC de ASP.NET scaffolding código

Abra la *Controllers\StoreManagerController.cs* de archivos y busque el `HTTP GET Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

El `Create` método agrega dos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objetos a la `ViewBag`, uno para que contenga la información de género y otro para que contenga la información de intérprete. El [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) sobrecarga del constructor usado anteriormente toma tres argumentos:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. *elementos*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista. En el ejemplo anterior, la lista de géneros devuelta por `db.Genres`.
2. *dataValueField*: el nombre de la propiedad en el **IEnumerable** lista que contiene el valor de clave. En el ejemplo anterior, `GenreId` y `ArtistId`.
3. *dataTextField*: el nombre de la propiedad en el **IEnumerable** lista que contiene la información que se va a mostrar. En el intérprete y la tabla de género, la `name` campo se usa.

Abra la *Views\StoreManager\Create.cshtml* de archivo y examine la `Html.DropDownList` marcado de aplicación auxiliar para el campo de género.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

La primera línea muestra que la vista de creación toma un `Album` modelo. En el `Create` método mostrado anteriormente, se ha pasado ningún modelo, por lo que se obtiene de la vista de un **null** `Album` modelo. En este momento vamos a crear un nuevo álbum para que no tenga `Album` datos para él.

El [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) sobrecarga mostrado anteriormente toma el nombre del campo que desea enlazar al modelo. También usa este nombre para buscar un **ViewBag** objeto que contiene un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objeto. Utilizar esta sobrecarga, es necesario al nombre de la **ViewBag SelectList** objeto `GenreId`. El segundo parámetro (`String.Empty`) es el texto que se muestra cuando se selecciona ningún elemento. Esto es exactamente lo que queremos al crear un nuevo álbum. Si quita el segundo parámetro y usa el código siguiente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

La lista de selección se predeterminado para el primer elemento o Rock en nuestro ejemplo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

Examinar el `HTTP POST Create` método.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

Esta sobrecarga de la `Create` método toma un `album` objeto creado por el sistema de enlace de modelo de MVC de ASP.NET de los valores de formulario expuestos. Cuando se envía un nuevo álbum, si el estado del modelo es válido y no hay ningún error de base de datos, el nuevo álbum se agrega a la base de datos. La siguiente imagen muestra la creación de un nuevo álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

Puede usar el [herramienta fiddler](http://www.fiddler2.com/fiddler2/) para examinar los valores de formulario expuesto ese enlace de modelo de MVC de ASP.NET usa para crear el objeto de álbum.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).

### <a name="refactoring-the-viewbag-selectlist-creation"></a>La creación de ViewBag SelectList de refactorización

Tanto el `Edit` métodos y la `HTTP POST Create` método tener código idéntico para configurar el **SelectList** en el **ViewBag**. Siguiendo la filosofía de [SECA](http://en.wikipedia.org/wiki/Don't_repeat_yourself), se va a refactorizar este código. Nos aseguraremos de hacer uso de este refactoriza código más adelante.

Cree un nuevo método para agregar un género y el intérprete **SelectList** a la **ViewBag**.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

Reemplace las dos líneas establecer el `ViewBag` en cada uno de los `Create` y `Edit` métodos con una llamada a la `SetGenreArtistViewBag` (método). A continuación se muestra el código completado.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

Crear un nuevo álbum y editar un álbum para comprobar que los cambios funcionan.

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a>Se pasa explícitamente el SelectList a los controles DropDownList

Las vistas de creación y edición de crean mediante el uso de la técnica scaffolding de ASP.NET MVC siguiente **DropDownList** sobrecarga:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

El `DropDownList` a continuación se muestra el marcado para la vista de creación.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

Porque el `ViewBag` propiedad para la `SelectList` se denomina `GenreId`, el **DropDownList** auxiliar usará el `GenreId` **SelectList** en el **ViewBag** . En la siguiente **DropDownList** sobrecarga, el `SelectList` se pasa explícitamente.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

Abra la *Views\StoreManager\Edit.cshtml* y cambie el **DropDownList** llamada pasar de manera explícita en el **SelectList**, utilizando la sobrecarga anterior. Haga esto con la categoría género. El código completo se muestra a continuación:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

Ejecute la aplicación y haga clic en el **administración** vincular, a continuación, navegue hasta un álbum Jazz y seleccione el **editar** vínculo.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

En lugar de mostrar Jazz como el género seleccionado actualmente, se muestra Rock. Cuando el argumento de cadena (la propiedad para enlazar) y la **SelectList** objeto tienen el mismo nombre, no se utiliza el valor seleccionado. Cuando no hay ha proporcionado ningún valor seleccionado, exploradores de forma predeterminada en el primer elemento de la **SelectList**(que es **Rock** en el ejemplo anterior). Se trata de una limitación conocida de la **DropDownList** auxiliar.

Abra la *Controllers\StoreManagerController.cs* y cambie el **SelectList** a los nombres de objeto `Genres` y `Artists`. El código completo se muestra a continuación:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

Los nombres géneros e intérpretes son mejores en las categorías, ya que contienen algo más que el identificador de cada categoría. La refactorización que hicimos anteriormente dado buenos resultados. En lugar de cambiar la **ViewBag** en cuatro métodos, los cambios se han aislado para el `SetGenreArtistViewBag` método.

Cambiar el **DropDownList** llamar al crear y editar vistas para usar la nueva **SelectList** nombres. La nueva marca para la vista de edición se muestra a continuación:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

La vista de creación requiere una cadena vacía para impedir que se muestre el primer elemento de la SelectList.

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

Crear un nuevo álbum y editar un álbum para comprobar que los cambios funcionan. Probar el código de edición seleccionando un álbum con un género distinto Rock.

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a>Uso de un modelo de vista con la aplicación auxiliar DropDownList

Cree una nueva clase en la carpeta ViewModels denominada `AlbumSelectListViewModel`. Reemplace el código de la `AlbumSelectListViewModel` clase por lo siguiente:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

El `AlbumSelectListViewModel` constructor toma un álbum, una lista de intérpretes, géneros y crea un objeto que contiene el álbum y un `SelectList` para géneros e intérpretes.

Compile el proyecto por lo que el `AlbumSelectListViewModel` está disponible cuando se crea una vista en el paso siguiente.

Agregar un `EditVM` método para el `StoreManagerController`. A continuación se muestra el código completado.

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

Haga clic en `AlbumSelectListViewModel`, seleccione **resolver**, a continuación, **con MvcMusicStore.ViewModels;**.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

Como alternativa, puede agregar la siguiente instrucción using:

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

Haga clic en `EditVM` y seleccione **agregar vista**. Utilice las opciones que se muestra a continuación.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

Seleccione **agregar**, a continuación, reemplace el contenido de la *Views\StoreManager\EditVM.cshtml* archivo con lo siguiente:

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

El `EditVM` marcado es muy similar a la versión original `Edit` marcado con las siguientes excepciones.

- Modelar las propiedades en el `Edit` vista tienen el formato `model.property`(por ejemplo, `model.Title` ). Modelar las propiedades en el `EditVm` vista tienen el formato `model.Album.property`(por ejemplo, `model.Album.Title`). Esto es así porque el `EditVM` vista se pasa un contenedor para una `Album`, no un `Album` como en el `Edit` vista.
- El **DropDownList** segundo parámetro procede del modelo de vista, no el **ViewBag**.
- El **BeginForm** auxiliares en el `EditVM` vea explícitamente las entradas a la `Edit` método de acción. Publicando hacia la `Edit` acción, no es necesario escribir un `HTTP POST EditVM` acción y puede volver a usar el `HTTP POST` `Edit` acción.

Ejecute la aplicación y editar un álbum. Cambiar la dirección URL debe usar `EditVM`. Cambiar un campo y pulse la **guardar** botón para comprobar el código funciona.

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a>¿Qué método se debe usar?

Los tres métodos que se muestran son acceptible. Muchos desarrolladores prefieren explictily pase el `SelectList` a la `DropDownList` mediante el `ViewBag`. Este enfoque tiene la ventaja añadida de lo que le ofrece la flexibilidad de usar un nombre más adecuado para la colección. Una advertencia es que no se asigne un nombre a la `ViewBag SelectList` el mismo nombre que la propiedad del modelo de objetos.

Algunos desarrolladores prefieren el enfoque ViewModel. Otras tenga en cuenta el marcado más detallado y HTML del enfoque ViewModel generan por un inconveniente.

En esta sección, hemos aprendimos tres métodos para usar la **DropDownList** con datos de categoría. En la siguiente sección, le mostraremos cómo agregar una nueva categoría.

> [!div class="step-by-step"]
> [Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Siguiente](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)
