---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: Examinar cómo ASP.NET MVC aplica la técnica scaffolding la aplicación auxiliar DropDownList | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: ab45a14c4eda9e7552af4831851396af3c13dce1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838175"
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="ff127-102">Examinar cómo ASP.NET MVC aplica la técnica scaffolding la aplicación auxiliar DropDownList</span><span class="sxs-lookup"><span data-stu-id="ff127-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="ff127-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ff127-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="ff127-104">En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, seleccione **Agregar controlador**.</span><span class="sxs-lookup"><span data-stu-id="ff127-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="ff127-105">Asigne al controlador el **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="ff127-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="ff127-106">Establecer las opciones de la **Agregar controlador** diálogo tal como se muestra en la imagen siguiente.</span><span class="sxs-lookup"><span data-stu-id="ff127-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="ff127-107">Editar el *StoreManager\Index.cshtml* ver y quitar `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="ff127-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="ff127-108">Quitar `AlbumArtUrl` hará que la presentación sea más legible.</span><span class="sxs-lookup"><span data-stu-id="ff127-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="ff127-109">A continuación se muestra el código completado.</span><span class="sxs-lookup"><span data-stu-id="ff127-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="ff127-110">Abra el *Controllers\StoreManagerController.cs* de archivo y busque el `Index` método.</span><span class="sxs-lookup"><span data-stu-id="ff127-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="ff127-111">Agregar el `OrderBy` cláusula por lo que los álbumes se ordenarán por el precio.</span><span class="sxs-lookup"><span data-stu-id="ff127-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="ff127-112">El código completo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ff127-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="ff127-113">Ordenar por precio le resultará más fácil probar los cambios realizados en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ff127-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="ff127-114">Cuando se está probando la edición y crear métodos, puede usar un precio bajo por lo que los datos guardados aparecerán primeros.</span><span class="sxs-lookup"><span data-stu-id="ff127-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="ff127-115">Abra el *StoreManager\Edit.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="ff127-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="ff127-116">Agregue la siguiente línea justo después de la etiqueta de leyenda.</span><span class="sxs-lookup"><span data-stu-id="ff127-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="ff127-117">El código siguiente muestra el contexto de este cambio:</span><span class="sxs-lookup"><span data-stu-id="ff127-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="ff127-118">El `AlbumId` es necesario para realizar cambios en un registro de álbum.</span><span class="sxs-lookup"><span data-stu-id="ff127-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="ff127-119">Presione CTRL+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ff127-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="ff127-120">Seleccione esta opción para la **Admin** vincular y, después, seleccione el **crear nuevo** vínculo para crear un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="ff127-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="ff127-121">Compruebe que se ha guardado la información del álbum.</span><span class="sxs-lookup"><span data-stu-id="ff127-121">Verify the album information was saved.</span></span> <span data-ttu-id="ff127-122">Editar un álbum y comprobar los cambios realizados se conservan.</span><span class="sxs-lookup"><span data-stu-id="ff127-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="ff127-123">El esquema de álbum</span><span class="sxs-lookup"><span data-stu-id="ff127-123">The Album Schema</span></span>

<span data-ttu-id="ff127-124">El `StoreManager` controlador creado por el mecanismo de scaffolding de MVC permite el acceso CRUD (creación, lectura, actualización, eliminación) a los álbumes en la base de datos de tienda de música.</span><span class="sxs-lookup"><span data-stu-id="ff127-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="ff127-125">A continuación se muestra el esquema de información del álbum:</span><span class="sxs-lookup"><span data-stu-id="ff127-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="ff127-126">El `Albums` tabla no almacena el género del álbum y la descripción, almacena una clave externa para el `Genres` tabla.</span><span class="sxs-lookup"><span data-stu-id="ff127-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="ff127-127">El `Genres` tabla contiene el nombre de género y la descripción.</span><span class="sxs-lookup"><span data-stu-id="ff127-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="ff127-128">Del mismo modo, el `Albums` tabla no contiene el nombre de intérpretes del álbum, pero con una clave externa para el `Artists` tabla.</span><span class="sxs-lookup"><span data-stu-id="ff127-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ff127-129">El `Artists` tabla contiene el nombre del artista.</span><span class="sxs-lookup"><span data-stu-id="ff127-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="ff127-130">Si examina los datos en el `Albums` tabla, puede ver cada fila contiene una clave externa a la `Genres` tabla y una clave externa para el `Artists` tabla.</span><span class="sxs-lookup"><span data-stu-id="ff127-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="ff127-131">La imagen siguiente se muestran algunos datos de la tabla desde el `Albums` tabla.</span><span class="sxs-lookup"><span data-stu-id="ff127-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="ff127-132">La etiqueta HTML Select</span><span class="sxs-lookup"><span data-stu-id="ff127-132">The HTML Select Tag</span></span>

<span data-ttu-id="ff127-133">El código HTML `<select>` elemento (creado por el código HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar) se usa para mostrar una lista completa de valores (por ejemplo, la lista de géneros).</span><span class="sxs-lookup"><span data-stu-id="ff127-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="ff127-134">Para editar formularios, cuando se conoce el valor actual, la lista de selección puede mostrar el valor actual.</span><span class="sxs-lookup"><span data-stu-id="ff127-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="ff127-135">Se ha visto este anteriormente cuando se establece el valor seleccionado **Comedia**.</span><span class="sxs-lookup"><span data-stu-id="ff127-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="ff127-136">La lista de selección es ideal para mostrar la categoría o datos de clave externa.</span><span class="sxs-lookup"><span data-stu-id="ff127-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="ff127-137">El `<select>` (elemento) para la clave externa de género muestra la lista de nombres de género posibles, pero al guardar el formulario de la propiedad de género se actualiza con el género valor de clave externa, no el nombre de género mostrada.</span><span class="sxs-lookup"><span data-stu-id="ff127-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="ff127-138">En la imagen siguiente, el género seleccionado es **Disco** y es el intérprete **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="ff127-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="ff127-139">Examinar el MVC de ASP.NET de código con scaffold</span><span class="sxs-lookup"><span data-stu-id="ff127-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="ff127-140">Abra el *Controllers\StoreManagerController.cs* de archivo y busque el `HTTP GET Create` método.</span><span class="sxs-lookup"><span data-stu-id="ff127-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="ff127-141">El `Create` método agrega dos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objetos a la `ViewBag`, uno para contener la información de género y otro para contener la información del intérprete.</span><span class="sxs-lookup"><span data-stu-id="ff127-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="ff127-142">El [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) usado anteriormente sobrecarga del constructor toma tres argumentos:</span><span class="sxs-lookup"><span data-stu-id="ff127-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="ff127-143">*elementos*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista.</span><span class="sxs-lookup"><span data-stu-id="ff127-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="ff127-144">En el ejemplo anterior devuelve la lista de géneros `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="ff127-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="ff127-145">*dataValueField*: el nombre de la propiedad en el **IEnumerable** lista que contiene el valor de clave.</span><span class="sxs-lookup"><span data-stu-id="ff127-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="ff127-146">En el ejemplo anterior, `GenreId` y `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="ff127-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="ff127-147">*dataTextField*: el nombre de la propiedad en el **IEnumerable** lista que contiene la información para mostrar.</span><span class="sxs-lookup"><span data-stu-id="ff127-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="ff127-148">En los artistas y la tabla de género, el `name` se usa el campo.</span><span class="sxs-lookup"><span data-stu-id="ff127-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="ff127-149">Abra el *Views\StoreManager\Create.cshtml* de archivo y examine el `Html.DropDownList` marcado de aplicación auxiliar para el campo del género.</span><span class="sxs-lookup"><span data-stu-id="ff127-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="ff127-150">La primera línea muestra que la vista de creación tarda un `Album` modelo.</span><span class="sxs-lookup"><span data-stu-id="ff127-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="ff127-151">En el `Create` método mostrado anteriormente, se ha pasado ningún modelo, por lo que la vista obtiene un **null** `Album` modelo.</span><span class="sxs-lookup"><span data-stu-id="ff127-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="ff127-152">En este momento estamos creando un nuevo álbum no tengamos cualquiera `Album` datos para él.</span><span class="sxs-lookup"><span data-stu-id="ff127-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="ff127-153">El [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) sobrecarga anterior toma el nombre del campo que se va a enlazar el modelo.</span><span class="sxs-lookup"><span data-stu-id="ff127-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="ff127-154">También usa este nombre para buscar un **ViewBag** objeto que contiene un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objeto.</span><span class="sxs-lookup"><span data-stu-id="ff127-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="ff127-155">Utilizar esta sobrecarga, se requiere que el nombre de la **ViewBag SelectList** objeto `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="ff127-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="ff127-156">El segundo parámetro (`String.Empty`) es el texto que se muestra cuando se selecciona ningún elemento.</span><span class="sxs-lookup"><span data-stu-id="ff127-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="ff127-157">Esto es exactamente lo que queremos crear un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="ff127-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="ff127-158">Si quita el segundo parámetro y usa el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="ff127-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="ff127-159">La lista de selección haría de forma predeterminada el primer elemento o Rock en nuestro ejemplo.</span><span class="sxs-lookup"><span data-stu-id="ff127-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="ff127-160">Examinar el `HTTP POST Create` método.</span><span class="sxs-lookup"><span data-stu-id="ff127-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="ff127-161">Esta sobrecarga de la `Create` método toma un `album` objeto, creado por el sistema de enlace de modelo de ASP.NET MVC de los valores de formulario publicados.</span><span class="sxs-lookup"><span data-stu-id="ff127-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="ff127-162">Cuando se envía un álbum nuevo, si el estado del modelo es válido y no hay ningún error de base de datos, el álbum nuevo se agrega a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="ff127-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="ff127-163">La siguiente imagen muestra la creación de un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="ff127-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="ff127-164">Puede usar el [herramienta fiddler](http://www.fiddler2.com/fiddler2/) para examinar los valores de formulario enviados ese enlace de modelos de ASP.NET MVC usa para crear el objeto de álbum.</span><span class="sxs-lookup"><span data-stu-id="ff127-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="ff127-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="ff127-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="ff127-166">Refactorización de la creación de ViewBag SelectList</span><span class="sxs-lookup"><span data-stu-id="ff127-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="ff127-167">Tanto el `Edit` métodos y la `HTTP POST Create` método tener código idéntico para configurar el **SelectList** en el **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ff127-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ff127-168">Siguiendo el espíritu de [seco](http://en.wikipedia.org/wiki/Don't_repeat_yourself), se va a refactorizar este código.</span><span class="sxs-lookup"><span data-stu-id="ff127-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="ff127-169">Nos aseguraremos de hacer uso de este refactoriza código más adelante.</span><span class="sxs-lookup"><span data-stu-id="ff127-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="ff127-170">Cree un nuevo método para agregar un género y el intérprete **SelectList** a la **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ff127-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="ff127-171">Reemplace las dos líneas de configuración el `ViewBag` en cada uno de los `Create` y `Edit` métodos con una llamada a la `SetGenreArtistViewBag` método.</span><span class="sxs-lookup"><span data-stu-id="ff127-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="ff127-172">A continuación se muestra el código completado.</span><span class="sxs-lookup"><span data-stu-id="ff127-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="ff127-173">Crear un álbum nuevo y editar un álbum para comprobar que los cambios funcionan.</span><span class="sxs-lookup"><span data-stu-id="ff127-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="ff127-174">Se pasa explícitamente el SelectList al control DropDownList</span><span class="sxs-lookup"><span data-stu-id="ff127-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="ff127-175">Las vistas create y edit crean mediante el uso de scaffolding de ASP.NET MVC, la siguiente **DropDownList** sobrecarga:</span><span class="sxs-lookup"><span data-stu-id="ff127-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="ff127-176">El `DropDownList` marcado para la vista de creación se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ff127-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="ff127-177">Porque el `ViewBag` propiedad para el `SelectList` se denomina `GenreId`, el **DropDownList** auxiliar usará el `GenreId` **SelectList** en el **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="ff127-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="ff127-178">En la siguiente **DropDownList** sobrecarga, el `SelectList` se pasa explícitamente.</span><span class="sxs-lookup"><span data-stu-id="ff127-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="ff127-179">Abra el *Views\StoreManager\Edit.cshtml* de archivo y cambie el **DropDownList** llamada pasar explícitamente la **SelectList**, mediante la sobrecarga del anterior.</span><span class="sxs-lookup"><span data-stu-id="ff127-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="ff127-180">Hacer esto para la categoría género.</span><span class="sxs-lookup"><span data-stu-id="ff127-180">Do this for the Genre category.</span></span> <span data-ttu-id="ff127-181">El código completo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="ff127-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="ff127-182">Ejecute la aplicación y haga clic en el **Admin** vincular, navegue a un álbum de Jazz y seleccione el **editar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="ff127-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="ff127-183">En lugar de mostrar Jazz como el género seleccionado actualmente, se muestra Rock.</span><span class="sxs-lookup"><span data-stu-id="ff127-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="ff127-184">Cuando el argumento de cadena (la propiedad para enlazar) y el **SelectList** objeto tienen el mismo nombre, no se utiliza el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="ff127-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="ff127-185">Cuando no hay ha proporcionado ningún valor seleccionado, los exploradores de forma predeterminada el primer elemento en el **SelectList**(que es **Rock** en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="ff127-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="ff127-186">Se trata de una limitación conocida de la **DropDownList** auxiliar.</span><span class="sxs-lookup"><span data-stu-id="ff127-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="ff127-187">Abra el *Controllers\StoreManagerController.cs* y cambie el **SelectList** a los nombres de objeto `Genres` y `Artists`.</span><span class="sxs-lookup"><span data-stu-id="ff127-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="ff127-188">El código completo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="ff127-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="ff127-189">Los nombres de géneros y artistas son nombres mejor para las categorías, ya que contienen algo más que el identificador de cada categoría.</span><span class="sxs-lookup"><span data-stu-id="ff127-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="ff127-190">La refactorización que se realizaron anteriormente pagado.</span><span class="sxs-lookup"><span data-stu-id="ff127-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="ff127-191">En lugar de cambiar el **ViewBag** en cuatro métodos, los cambios eran aislados en el `SetGenreArtistViewBag` método.</span><span class="sxs-lookup"><span data-stu-id="ff127-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="ff127-192">Cambiar el **DropDownList** llamar en crear y editar vistas para usar la nueva **SelectList** nombres.</span><span class="sxs-lookup"><span data-stu-id="ff127-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="ff127-193">El nuevo marcado para la vista de edición se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="ff127-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="ff127-194">La vista de creación requiere una cadena vacía para impedir que el primer elemento de la SelectList que se muestre.</span><span class="sxs-lookup"><span data-stu-id="ff127-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="ff127-195">Crear un álbum nuevo y editar un álbum para comprobar que los cambios funcionan.</span><span class="sxs-lookup"><span data-stu-id="ff127-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="ff127-196">Probar el código de edición seleccionando un álbum con un género Rock distinto de.</span><span class="sxs-lookup"><span data-stu-id="ff127-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="ff127-197">Uso de un modelo de vista con la aplicación auxiliar DropDownList</span><span class="sxs-lookup"><span data-stu-id="ff127-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="ff127-198">Cree una nueva clase en la carpeta ViewModels denominada `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="ff127-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="ff127-199">Reemplace el código de la `AlbumSelectListViewModel` clase con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ff127-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="ff127-200">El `AlbumSelectListViewModel` constructor toma un álbum, una lista de los artistas y géneros y crea un objeto que contiene el álbum y un `SelectList` para géneros y artistas.</span><span class="sxs-lookup"><span data-stu-id="ff127-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="ff127-201">Compile el proyecto para que el `AlbumSelectListViewModel` está disponible cuando se crea una vista en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="ff127-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="ff127-202">Agregar un `EditVM` método a la `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="ff127-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="ff127-203">A continuación se muestra el código completado.</span><span class="sxs-lookup"><span data-stu-id="ff127-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="ff127-204">Haga clic en `AlbumSelectListViewModel`, seleccione **resolver**, a continuación, **mediante MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="ff127-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="ff127-205">Como alternativa, puede agregar la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="ff127-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="ff127-206">Haga clic en `EditVM` y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="ff127-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="ff127-207">Use las opciones que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ff127-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="ff127-208">Seleccione **agregar**, a continuación, reemplace el contenido de la *Views\StoreManager\EditVM.cshtml* archivo con la siguiente:</span><span class="sxs-lookup"><span data-stu-id="ff127-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="ff127-209">El `EditVM` marcado es muy similar a la versión original `Edit` marcado con las siguientes excepciones.</span><span class="sxs-lookup"><span data-stu-id="ff127-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="ff127-210">Modelar las propiedades en el `Edit` vista tienen el formato `model.property`(por ejemplo, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="ff127-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="ff127-211">Modelar las propiedades en el `EditVm` vista tienen el formato `model.Album.property`(por ejemplo, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="ff127-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="ff127-212">Eso es porque la `EditVM` vista se pasa un contenedor para un `Album`, no un `Album` como en el `Edit` vista.</span><span class="sxs-lookup"><span data-stu-id="ff127-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="ff127-213">El **DropDownList** segundo parámetro procede el modelo de vista, no el **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="ff127-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="ff127-214">El **BeginForm** auxiliar en el `EditVM` vea explícitamente las entradas a la `Edit` método de acción.</span><span class="sxs-lookup"><span data-stu-id="ff127-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="ff127-215">Mediante la publicación de nuevo a la `Edit` acción, no tiene que escribir un `HTTP POST EditVM` acción y puede reutilizar el `HTTP POST` `Edit` acción.</span><span class="sxs-lookup"><span data-stu-id="ff127-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="ff127-216">Ejecute la aplicación y modificar un álbum.</span><span class="sxs-lookup"><span data-stu-id="ff127-216">Run the application and edit an album.</span></span> <span data-ttu-id="ff127-217">Cambie la dirección URL para usar `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="ff127-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="ff127-218">Cambiar un campo y presione la **guardar** botón para comprobar el código que funciona.</span><span class="sxs-lookup"><span data-stu-id="ff127-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="ff127-219">¿Qué enfoque se debe usar?</span><span class="sxs-lookup"><span data-stu-id="ff127-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="ff127-220">Los tres métodos que se muestran son aconseja.</span><span class="sxs-lookup"><span data-stu-id="ff127-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="ff127-221">Muchos desarrolladores prefieren explictily pase la `SelectList` a la `DropDownList` mediante el `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="ff127-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="ff127-222">Este enfoque tiene la ventaja añadida de lo que le ofrece la flexibilidad de usar un nombre más adecuado para la colección.</span><span class="sxs-lookup"><span data-stu-id="ff127-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="ff127-223">Tenerse en cuenta es que no se asigne el `ViewBag SelectList` el mismo nombre que la propiedad del modelo de objetos.</span><span class="sxs-lookup"><span data-stu-id="ff127-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="ff127-224">Algunos desarrolladores prefieren el enfoque de ViewModel.</span><span class="sxs-lookup"><span data-stu-id="ff127-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="ff127-225">Otros Observe el marcado más detallado y generan una desventaja HTML del enfoque de ViewModel.</span><span class="sxs-lookup"><span data-stu-id="ff127-225">Others consider the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="ff127-226">En esta sección, hemos aprendido tres métodos para usar el **DropDownList** con datos de categoría.</span><span class="sxs-lookup"><span data-stu-id="ff127-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="ff127-227">En la siguiente sección, mostraremos cómo agregar una nueva categoría.</span><span class="sxs-lookup"><span data-stu-id="ff127-227">In the next section, we'll show how to add a new category.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ff127-228">[Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
> [Siguiente](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="ff127-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
