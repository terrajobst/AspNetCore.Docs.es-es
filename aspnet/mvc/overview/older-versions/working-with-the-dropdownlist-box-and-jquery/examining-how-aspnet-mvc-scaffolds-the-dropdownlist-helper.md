---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
title: "Examinar cómo ASP.NET MVC scaffolds la aplicación auxiliar DropDownList | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 8921d7f2-21f0-427a-8b27-2df7251174b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper
msc.type: authoredcontent
ms.openlocfilehash: 737773ab424b3ec3b6139b8c238a60ca23de2e69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="examining--how--aspnet-mvc-scaffolds-the-dropdownlist-helper"></a><span data-ttu-id="c0fa3-102">Examinar cómo ASP.NET MVC scaffolds la aplicación auxiliar DropDownList</span><span class="sxs-lookup"><span data-stu-id="c0fa3-102">Examining  how  ASP.NET MVC scaffolds the DropDownList Helper</span></span>
====================
<span data-ttu-id="c0fa3-103">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c0fa3-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="c0fa3-104">En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, seleccione **Agregar controlador**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-104">In **Solution Explorer**, right-click the *Controllers* folder and then select **Add Controller**.</span></span> <span data-ttu-id="c0fa3-105">Nombre del controlador **StoreManagerController**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-105">Name the controller **StoreManagerController**.</span></span> <span data-ttu-id="c0fa3-106">Establecer las opciones de la **Agregar controlador** diálogo tal como se muestra en la imagen siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-106">Set the options for the **Add Controller** dialog as shown in the image below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image1.png)

<span data-ttu-id="c0fa3-107">Editar la *StoreManager\Index.cshtml* ver y quitar `AlbumArtUrl`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-107">Edit the *StoreManager\Index.cshtml* view and remove `AlbumArtUrl`.</span></span> <span data-ttu-id="c0fa3-108">Quitar `AlbumArtUrl` hará que la presentación sea más legible.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-108">Removing `AlbumArtUrl` will make the presentation more readable.</span></span> <span data-ttu-id="c0fa3-109">El código completo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-109">The completed code is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample1.cshtml)]

<span data-ttu-id="c0fa3-110">Abra la *Controllers\StoreManagerController.cs* de archivos y busque el `Index` método.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-110">Open the *Controllers\StoreManagerController.cs* file and find the `Index` method.</span></span> <span data-ttu-id="c0fa3-111">Agregar el `OrderBy` cláusula para los álbumes que se va a ordenar por precio.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-111">Add the `OrderBy` clause so the albums will be sorted by price.</span></span> <span data-ttu-id="c0fa3-112">El código completo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-112">The complete code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample2.cs)]

<span data-ttu-id="c0fa3-113">Ordenar por precio le resultará más fácil de probar los cambios a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-113">Sorting by price will make it easier to test changes to the database.</span></span> <span data-ttu-id="c0fa3-114">Cuando se prueba por la edición y crear métodos, que puede usar un precio mínimo para que los datos guardados aparecerán en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-114">When you are testing the edit and create methods, you can use a low price so the saved data will appear first.</span></span>

<span data-ttu-id="c0fa3-115">Abra la *StoreManager\Edit.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-115">Open the *StoreManager\Edit.cshtml* file.</span></span> <span data-ttu-id="c0fa3-116">Agregue la siguiente línea justo después de la etiqueta de leyenda.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-116">Add the following line just after the legend tag.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample3.cshtml)]

<span data-ttu-id="c0fa3-117">El código siguiente muestra el contexto de este cambio:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-117">The following code shows the context of this change:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample4.cshtml)]

<span data-ttu-id="c0fa3-118">El `AlbumId` es necesario para realizar cambios en un registro de álbum.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-118">The `AlbumId` is required to make changes to an album record.</span></span>

<span data-ttu-id="c0fa3-119">Presione CTRL+F5 para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-119">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="c0fa3-120">Seleccione esta opción para la **administración** vincular, a continuación, seleccione la **crear nuevo** vínculo para crear un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-120">Select to the **Admin** link, then select the **Create New** link to create a new album.</span></span> <span data-ttu-id="c0fa3-121">Compruebe que la información del álbum se guardó.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-121">Verify the album information was saved.</span></span> <span data-ttu-id="c0fa3-122">Editar un álbum y comprobar los cambios realizados se conservan.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-122">Edit an album and verify the changes you made are persisted.</span></span>

### <a name="the-album-schema"></a><span data-ttu-id="c0fa3-123">El esquema de álbum</span><span class="sxs-lookup"><span data-stu-id="c0fa3-123">The Album Schema</span></span>

<span data-ttu-id="c0fa3-124">El `StoreManager` controlador creado mediante el mecanismo de scaffolding MVC permite el acceso CRUD (crear, leer, Update, Delete) a los álbumes en la base de datos de almacén de música.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-124">The `StoreManager` controller created by the MVC scaffolding mechanism allows CRUD (Create, Read, Update, Delete) access to the albums in the music store database.</span></span> <span data-ttu-id="c0fa3-125">A continuación se muestra el esquema para obtener información de álbum:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-125">The schema for album information is shown below:</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image2.png)

<span data-ttu-id="c0fa3-126">El `Albums` tabla no almacena el género del álbum y la descripción, almacena una clave externa a la `Genres` tabla.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-126">The `Albums` table does not store the album genre and description, it stores a foreign key to the `Genres` table.</span></span> <span data-ttu-id="c0fa3-127">El `Genres` tabla contiene el nombre de género y la descripción.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-127">The `Genres` table contains the genre name and description.</span></span> <span data-ttu-id="c0fa3-128">Del mismo modo, el `Albums` tabla no contiene el nombre de intérpretes del álbum, pero una clave externa a la `Artists` tabla.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-128">Likewise, the `Albums` table doesn't contain the album artists name, but a foreign key to the `Artists` table.</span></span> <span data-ttu-id="c0fa3-129">El `Artists` tabla contiene el nombre del intérprete.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-129">The `Artists` table contains the artist's name.</span></span> <span data-ttu-id="c0fa3-130">Si examina los datos de la `Albums` tabla, puede ver cada fila contiene una clave externa a la `Genres` tabla y una clave externa a la `Artists` tabla.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-130">If you examine the data in the `Albums` table, you can see each row contains a foreign key to the `Genres` table and a foreign key to the `Artists` table.</span></span> <span data-ttu-id="c0fa3-131">La imagen siguiente se muestran algunos datos de la tabla desde el `Albums` tabla.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-131">The image below show some table data from the `Albums` table.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image3.png)

### <a name="the-html-select-tag"></a><span data-ttu-id="c0fa3-132">La etiqueta de selección HTML</span><span class="sxs-lookup"><span data-stu-id="c0fa3-132">The HTML Select Tag</span></span>

<span data-ttu-id="c0fa3-133">El código HTML `<select>` elemento (creado por el código HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) auxiliar) se usa para mostrar una lista completa de valores (por ejemplo, la lista de géneros).</span><span class="sxs-lookup"><span data-stu-id="c0fa3-133">The HTML `<select>` element (created by the HTML [DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) helper) is used to display a complete list of values (such as the list of genres).</span></span> <span data-ttu-id="c0fa3-134">Para los formularios de edición, cuando se conoce el valor actual, la lista de selección puede mostrar el valor actual.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-134">For edit forms, when the current value is known, the select list can display the current value.</span></span> <span data-ttu-id="c0fa3-135">Hemos visto este previamente cuando se establezca el valor seleccionado en **Comedia**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-135">We saw this previously when we set the selected value to **Comedy**.</span></span> <span data-ttu-id="c0fa3-136">La lista de selección es ideal para mostrar la categoría o datos de clave externa.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-136">The select list is ideal for displaying category or foreign key data.</span></span> <span data-ttu-id="c0fa3-137">El `<select>` (elemento) para la clave externa de género muestra la lista de nombres de género posibles, pero cuando se guarda el formulario se actualiza la propiedad de género con el género valor de clave externa, no el nombre mostrado género.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-137">The `<select>` element for the Genre foreign key displays the list of possible genre names, but when you save the form the Genre property is updated with the Genre foreign key value, not the displayed genre name.</span></span> <span data-ttu-id="c0fa3-138">En la imagen siguiente, el género seleccionado es **Disco** y es el intérprete **Donna Summer**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-138">In the image below, the genre selected is **Disco** and the artist is **Donna Summer**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image4.png)

### <a name="examining-the-aspnet-mvc-scaffolded-code"></a><span data-ttu-id="c0fa3-139">Examen de MVC de ASP.NET scaffolding código</span><span class="sxs-lookup"><span data-stu-id="c0fa3-139">Examining the ASP.NET MVC Scaffolded Code</span></span>

<span data-ttu-id="c0fa3-140">Abra la *Controllers\StoreManagerController.cs* de archivos y busque el `HTTP GET Create` método.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-140">Open the *Controllers\StoreManagerController.cs* file and find the `HTTP GET Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample5.cs)]

<span data-ttu-id="c0fa3-141">El `Create` método agrega dos [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objetos a la `ViewBag`, uno para que contenga la información de género y otro para que contenga la información de intérprete.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-141">The `Create` method adds two [SelectList](https://msdn.microsoft.com/library/system.web.mvc.selectlist.aspx) objects to the `ViewBag`, one to contain the genre information, and one to contain the artist information.</span></span> <span data-ttu-id="c0fa3-142">El [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) sobrecarga del constructor usado anteriormente toma tres argumentos:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-142">The [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) constructor overload used above takes three arguments:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample6.cs)]

1. <span data-ttu-id="c0fa3-143">*elementos*: un [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) que contiene los elementos de la lista.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-143">*items*: An [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) containing the items in the list.</span></span> <span data-ttu-id="c0fa3-144">En el ejemplo anterior, la lista de géneros devuelta por `db.Genres`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-144">In the example above, the list of genres returned by `db.Genres`.</span></span>
2. <span data-ttu-id="c0fa3-145">*dataValueField*: el nombre de la propiedad en el **IEnumerable** lista que contiene el valor de clave.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-145">*dataValueField*: The name of the property in the **IEnumerable** list that contains the key value.</span></span> <span data-ttu-id="c0fa3-146">En el ejemplo anterior, `GenreId` y `ArtistId`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-146">In the example above, `GenreId` and `ArtistId`.</span></span>
3. <span data-ttu-id="c0fa3-147">*dataTextField*: el nombre de la propiedad en el **IEnumerable** lista que contiene la información que se va a mostrar.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-147">*dataTextField*: The name of the property in the **IEnumerable** list that contains the information to display.</span></span> <span data-ttu-id="c0fa3-148">En el intérprete y la tabla de género, la `name` campo se usa.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-148">In both the artists and genre table, the `name` field is used.</span></span>

<span data-ttu-id="c0fa3-149">Abra la *Views\StoreManager\Create.cshtml* de archivo y examine la `Html.DropDownList` marcado de aplicación auxiliar para el campo de género.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-149">Open the *Views\StoreManager\Create.cshtml* file and examine the `Html.DropDownList` helper markup for the genre field.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample7.cshtml)]

<span data-ttu-id="c0fa3-150">La primera línea muestra que la vista de creación toma un `Album` modelo.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-150">The first line shows that the create view takes an `Album` model.</span></span> <span data-ttu-id="c0fa3-151">En el `Create` método mostrado anteriormente, se ha pasado ningún modelo, por lo que se obtiene de la vista de un **null** `Album` modelo.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-151">In the `Create` method shown above, no model was passed, so the view gets a **null** `Album` model.</span></span> <span data-ttu-id="c0fa3-152">En este momento vamos a crear un nuevo álbum para que no tenga `Album` datos para él.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-152">At this point we are creating a new album so we don't have any `Album` data for it.</span></span>

<span data-ttu-id="c0fa3-153">El [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) sobrecarga mostrado anteriormente toma el nombre del campo que desea enlazar al modelo.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-153">The [Html.DropDownList](https://msdn.microsoft.com/library/dd492948.aspx) overload shown above takes the name of the field to bind to the model.</span></span> <span data-ttu-id="c0fa3-154">También usa este nombre para buscar un **ViewBag** objeto que contiene un [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) objeto.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-154">It also uses this name to look for a **ViewBag** object containing a [SelectList](https://msdn.microsoft.com/library/dd505286.aspx) object.</span></span> <span data-ttu-id="c0fa3-155">Utilizar esta sobrecarga, es necesario al nombre de la **ViewBag SelectList** objeto `GenreId`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-155">Using this overload, you are required to name the **ViewBag SelectList** object `GenreId`.</span></span> <span data-ttu-id="c0fa3-156">El segundo parámetro (`String.Empty`) es el texto que se muestra cuando se selecciona ningún elemento.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-156">The second parameter (`String.Empty`) is the text to display when no item is selected.</span></span> <span data-ttu-id="c0fa3-157">Esto es exactamente lo que queremos al crear un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-157">This is exactly what we want when creating a new album.</span></span> <span data-ttu-id="c0fa3-158">Si quita el segundo parámetro y usa el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-158">If you removed the second parameter and used the following code:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample8.cshtml)]

<span data-ttu-id="c0fa3-159">La lista de selección se predeterminado para el primer elemento o Rock en nuestro ejemplo.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-159">The select list would default to the first element, or Rock in our sample.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image5.png)

<span data-ttu-id="c0fa3-160">Examinar el `HTTP POST Create` método.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-160">Examining the `HTTP POST Create` method.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample9.cs)]

<span data-ttu-id="c0fa3-161">Esta sobrecarga de la `Create` método toma un `album` objeto creado por el sistema de enlace de modelo de MVC de ASP.NET de los valores de formulario expuestos.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-161">This overload of the `Create` method takes an `album` object, created by the ASP.NET MVC model binding system from the form values posted.</span></span> <span data-ttu-id="c0fa3-162">Cuando se envía un nuevo álbum, si el estado del modelo es válido y no hay ningún error de base de datos, el nuevo álbum se agrega a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-162">When you submit a new album, if model state is valid and there are no database errors, the new album is added the database.</span></span> <span data-ttu-id="c0fa3-163">La siguiente imagen muestra la creación de un nuevo álbum.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-163">The following image shows the creation of a new album.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image6.png)

<span data-ttu-id="c0fa3-164">Puede usar el [herramienta fiddler](http://www.fiddler2.com/fiddler2/) para examinar los valores de formulario expuesto ese enlace de modelo de MVC de ASP.NET usa para crear el objeto de álbum.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-164">You can use the [fiddler tool](http://www.fiddler2.com/fiddler2/) to examine the posted form values that ASP.NET MVC model binding uses to create the album object.</span></span>

<span data-ttu-id="c0fa3-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span><span class="sxs-lookup"><span data-stu-id="c0fa3-165">![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image7.png).</span></span>

### <a name="refactoring-the-viewbag-selectlist-creation"></a><span data-ttu-id="c0fa3-166">La creación de ViewBag SelectList de refactorización</span><span class="sxs-lookup"><span data-stu-id="c0fa3-166">Refactoring the ViewBag SelectList Creation</span></span>

<span data-ttu-id="c0fa3-167">Tanto el `Edit` métodos y la `HTTP POST Create` método tener código idéntico para configurar el **SelectList** en el **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-167">Both the `Edit` methods and the `HTTP POST Create` method have identical code to set up the **SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="c0fa3-168">Siguiendo la filosofía de [SECA](http://en.wikipedia.org/wiki/Don't_repeat_yourself), se va a refactorizar este código.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-168">In the spirit of [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), we will refactor this code.</span></span> <span data-ttu-id="c0fa3-169">Nos aseguraremos de hacer uso de este refactoriza código más adelante.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-169">We'll make use of this refactored code later.</span></span>

<span data-ttu-id="c0fa3-170">Cree un nuevo método para agregar un género y el intérprete **SelectList** a la **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-170">Create a new method to add a genre and artist **SelectList** to the **ViewBag**.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample10.cs)]

<span data-ttu-id="c0fa3-171">Reemplace las dos líneas establecer el `ViewBag` en cada uno de los `Create` y `Edit` métodos con una llamada a la `SetGenreArtistViewBag` (método).</span><span class="sxs-lookup"><span data-stu-id="c0fa3-171">Replace the two lines setting the `ViewBag` in each of the `Create` and `Edit` methods with a call to the `SetGenreArtistViewBag` method.</span></span> <span data-ttu-id="c0fa3-172">El código completo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-172">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample11.cs)]

<span data-ttu-id="c0fa3-173">Crear un nuevo álbum y editar un álbum para comprobar que los cambios funcionan.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-173">Create a new album and edit an album to verify the changes work.</span></span>

### <a name="explicitly-passing-the-selectlist-to-the-dropdownlist"></a><span data-ttu-id="c0fa3-174">Se pasa explícitamente el SelectList a los controles DropDownList</span><span class="sxs-lookup"><span data-stu-id="c0fa3-174">Explicitly Passing the SelectList to the DropDownList</span></span>

<span data-ttu-id="c0fa3-175">Las vistas de creación y edición de crean mediante el uso de la técnica scaffolding de ASP.NET MVC siguiente **DropDownList** sobrecarga:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-175">The create and edit views created by the ASP.NET MVC scaffolding use the following **DropDownList** overload:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample12.cs)]

<span data-ttu-id="c0fa3-176">El `DropDownList` a continuación se muestra el marcado para la vista de creación.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-176">The `DropDownList` markup for the create view is shown below.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample13.cshtml)]

<span data-ttu-id="c0fa3-177">Porque el `ViewBag` propiedad para la `SelectList` se denomina `GenreId`, el **DropDownList** auxiliar usará el `GenreId` **SelectList** en el **ViewBag** .</span><span class="sxs-lookup"><span data-stu-id="c0fa3-177">Because the `ViewBag` property for the `SelectList` is named `GenreId`, the **DropDownList** helper will use the `GenreId`**SelectList** in the **ViewBag**.</span></span> <span data-ttu-id="c0fa3-178">En la siguiente **DropDownList** sobrecarga, el `SelectList` se pasa explícitamente.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-178">In the following **DropDownList** overload, the `SelectList` is explicitly passed in.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample14.cs)]

<span data-ttu-id="c0fa3-179">Abra la *Views\StoreManager\Edit.cshtml* y cambie el **DropDownList** llamada pasar de manera explícita en el **SelectList**, utilizando la sobrecarga anterior.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-179">Open the *Views\StoreManager\Edit.cshtml* file, and change the **DropDownList** call to explicitly pass in the **SelectList**, using the overload above.</span></span> <span data-ttu-id="c0fa3-180">Haga esto con la categoría género.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-180">Do this for the Genre category.</span></span> <span data-ttu-id="c0fa3-181">El código completo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-181">The completed code is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample15.cshtml)]

<span data-ttu-id="c0fa3-182">Ejecute la aplicación y haga clic en el **administración** vincular, a continuación, navegue hasta un álbum Jazz y seleccione el **editar** vínculo.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-182">Run the application and click the **Admin** link, then navigate to a Jazz album and select the **Edit** link.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image8.png)

<span data-ttu-id="c0fa3-183">En lugar de mostrar Jazz como el género seleccionado actualmente, se muestra Rock.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-183">Instead of showing Jazz as the currently selected genre, Rock is displayed.</span></span> <span data-ttu-id="c0fa3-184">Cuando el argumento de cadena (la propiedad para enlazar) y la **SelectList** objeto tienen el mismo nombre, no se utiliza el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-184">When the string argument (the property to bind) and the **SelectList** object have the same name, the selected value is not used.</span></span> <span data-ttu-id="c0fa3-185">Cuando no hay ha proporcionado ningún valor seleccionado, exploradores de forma predeterminada en el primer elemento de la **SelectList**(que es **Rock** en el ejemplo anterior).</span><span class="sxs-lookup"><span data-stu-id="c0fa3-185">When there is no selected value provided, browsers default to the first element in the **SelectList**(which is **Rock** in the example above).</span></span> <span data-ttu-id="c0fa3-186">Se trata de una limitación conocida de la **DropDownList** auxiliar.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-186">This is a known limitation of the **DropDownList** helper.</span></span>

<span data-ttu-id="c0fa3-187">Abra la *Controllers\StoreManagerController.cs* y cambie el **SelectList** a los nombres de objeto `Genres` y `Artists`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-187">Open the *Controllers\StoreManagerController.cs* file and change the **SelectList** object names to `Genres` and `Artists`.</span></span> <span data-ttu-id="c0fa3-188">El código completo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-188">The completed code is shown below:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample16.cs)]

<span data-ttu-id="c0fa3-189">Los nombres géneros e intérpretes son mejores en las categorías, ya que contienen algo más que el identificador de cada categoría.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-189">The names Genres and Artists are better names for the categories, as they contain more than just the ID of each category.</span></span> <span data-ttu-id="c0fa3-190">La refactorización que hicimos anteriormente dado buenos resultados.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-190">The refactoring we did earlier paid off.</span></span> <span data-ttu-id="c0fa3-191">En lugar de cambiar la **ViewBag** en cuatro métodos, los cambios se han aislado para el `SetGenreArtistViewBag` método.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-191">Instead of changing the **ViewBag** in four methods, our changes were isolated to the `SetGenreArtistViewBag` method.</span></span>

<span data-ttu-id="c0fa3-192">Cambiar el **DropDownList** llamar al crear y editar vistas para usar la nueva **SelectList** nombres.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-192">Change the **DropDownList** call in the create and edit views to use the new **SelectList** names.</span></span> <span data-ttu-id="c0fa3-193">La nueva marca para la vista de edición se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-193">The new markup for the edit view is shown below:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample17.cshtml)]

<span data-ttu-id="c0fa3-194">La vista de creación requiere una cadena vacía para impedir que se muestre el primer elemento de la SelectList.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-194">The Create view requires an empty string to prevent the first item in the SelectList from being displayed.</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample18.cshtml)]

<span data-ttu-id="c0fa3-195">Crear un nuevo álbum y editar un álbum para comprobar que los cambios funcionan.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-195">Create a new album and edit an album to verify the changes work.</span></span> <span data-ttu-id="c0fa3-196">Probar el código de edición seleccionando un álbum con un género distinto Rock.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-196">Test the edit code by selecting an album with a genre other than Rock.</span></span>

### <a name="using-a-view-model-with-the-dropdownlist-helper"></a><span data-ttu-id="c0fa3-197">Uso de un modelo de vista con la aplicación auxiliar DropDownList</span><span class="sxs-lookup"><span data-stu-id="c0fa3-197">Using a View Model with the DropDownList Helper</span></span>

<span data-ttu-id="c0fa3-198">Cree una nueva clase en la carpeta ViewModels denominada `AlbumSelectListViewModel`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-198">Create a new class in the ViewModels folder named `AlbumSelectListViewModel`.</span></span> <span data-ttu-id="c0fa3-199">Reemplace el código de la `AlbumSelectListViewModel` clase por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-199">Replace the code in the `AlbumSelectListViewModel` class with the following:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample19.cs)]

<span data-ttu-id="c0fa3-200">El `AlbumSelectListViewModel` constructor toma un álbum, una lista de intérpretes, géneros y crea un objeto que contiene el álbum y un `SelectList` para géneros e intérpretes.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-200">The `AlbumSelectListViewModel` constructor takes an album, a list of artists and genres and creates an object containing the album and a `SelectList` for genres and artists.</span></span>

<span data-ttu-id="c0fa3-201">Compile el proyecto por lo que el `AlbumSelectListViewModel` está disponible cuando se crea una vista en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-201">Build the project so the `AlbumSelectListViewModel` is available when we create a view in the next step.</span></span>

<span data-ttu-id="c0fa3-202">Agregar un `EditVM` método para el `StoreManagerController`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-202">Add an `EditVM` method to the `StoreManagerController`.</span></span> <span data-ttu-id="c0fa3-203">El código completo se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-203">The completed code is shown below.</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample20.cs)]

<span data-ttu-id="c0fa3-204">Haga clic en `AlbumSelectListViewModel`, seleccione **resolver**, a continuación, **con MvcMusicStore.ViewModels;**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-204">Right click `AlbumSelectListViewModel`, select **Resolve**, then **using MvcMusicStore.ViewModels;**.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image9.png)

<span data-ttu-id="c0fa3-205">Como alternativa, puede agregar la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-205">Alternatively, you can add the following using statement:</span></span>

[!code-csharp[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample21.cs)]

<span data-ttu-id="c0fa3-206">Haga clic en `EditVM` y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-206">Right click `EditVM` and select **Add View**.</span></span> <span data-ttu-id="c0fa3-207">Utilice las opciones que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-207">Use the options shown below.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image10.png)

<span data-ttu-id="c0fa3-208">Seleccione **agregar**, a continuación, reemplace el contenido de la *Views\StoreManager\EditVM.cshtml* archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c0fa3-208">Select **Add**, then replace the contents of the *Views\StoreManager\EditVM.cshtml* file with the following:</span></span>

[!code-cshtml[Main](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/samples/sample22.cshtml)]

<span data-ttu-id="c0fa3-209">El `EditVM` marcado es muy similar a la versión original `Edit` marcado con las siguientes excepciones.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-209">The `EditVM` markup is very similar to the original `Edit` markup with the following exceptions.</span></span>

- <span data-ttu-id="c0fa3-210">Modelar las propiedades en el `Edit` vista tienen el formato `model.property`(por ejemplo, `model.Title` ).</span><span class="sxs-lookup"><span data-stu-id="c0fa3-210">Model properties in the `Edit` view are of the form `model.property`(for example, `model.Title` ).</span></span> <span data-ttu-id="c0fa3-211">Modelar las propiedades en el `EditVm` vista tienen el formato `model.Album.property`(por ejemplo, `model.Album.Title`).</span><span class="sxs-lookup"><span data-stu-id="c0fa3-211">Model properties in the `EditVm` view are of the form `model.Album.property`(for example, `model.Album.Title`).</span></span> <span data-ttu-id="c0fa3-212">Esto es así porque el `EditVM` vista se pasa un contenedor para una `Album`, no un `Album` como en el `Edit` vista.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-212">That's because the `EditVM` view is passed a container for an `Album`, not an `Album` as in the `Edit` view.</span></span>
- <span data-ttu-id="c0fa3-213">El **DropDownList** segundo parámetro procede del modelo de vista, no el **ViewBag**.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-213">The **DropDownList** second parameter comes from the view model, not the **ViewBag**.</span></span>
- <span data-ttu-id="c0fa3-214">El **BeginForm** auxiliares en el `EditVM` vea explícitamente las entradas a la `Edit` método de acción.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-214">The **BeginForm** helper in the `EditVM` view explicitly posts back to the `Edit` action method.</span></span> <span data-ttu-id="c0fa3-215">Publicando hacia la `Edit` acción, no es necesario escribir un `HTTP POST EditVM` acción y puede volver a usar el `HTTP POST` `Edit` acción.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-215">By posting back to the `Edit` action, we don't have to write an `HTTP POST EditVM` action and can reuse the `HTTP POST` `Edit` action.</span></span>

<span data-ttu-id="c0fa3-216">Ejecute la aplicación y editar un álbum.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-216">Run the application and edit an album.</span></span> <span data-ttu-id="c0fa3-217">Cambiar la dirección URL debe usar `EditVM`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-217">Change the URL to use `EditVM`.</span></span> <span data-ttu-id="c0fa3-218">Cambiar un campo y pulse la **guardar** botón para comprobar el código funciona.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-218">Change a field and hit the **Save** button to verify the code is working.</span></span>

![](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper/_static/image11.png)

### <a name="which-approach-should-you-use"></a><span data-ttu-id="c0fa3-219">¿Qué método se debe usar?</span><span class="sxs-lookup"><span data-stu-id="c0fa3-219">Which Approach Should You Use?</span></span>

<span data-ttu-id="c0fa3-220">Los tres métodos que se muestran son acceptible.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-220">All three approaches shown are acceptible.</span></span> <span data-ttu-id="c0fa3-221">Muchos desarrolladores prefieren explictily pase el `SelectList` a la `DropDownList` mediante el `ViewBag`.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-221">Many developers prefer to explictily pass the `SelectList` to the `DropDownList` using the `ViewBag`.</span></span> <span data-ttu-id="c0fa3-222">Este enfoque tiene la ventaja añadida de lo que le ofrece la flexibilidad de usar un nombre más adecuado para la colección.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-222">This approach has the added advantage of giving you the flexibility of using a more appropriate name for the collection.</span></span> <span data-ttu-id="c0fa3-223">Una advertencia es que no se asigne un nombre a la `ViewBag SelectList` el mismo nombre que la propiedad del modelo de objetos.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-223">The one caveat is you cannot name the `ViewBag SelectList` object the same name as the model property.</span></span>

<span data-ttu-id="c0fa3-224">Algunos desarrolladores prefieren el enfoque ViewModel.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-224">Some developers prefer the ViewModel approach.</span></span> <span data-ttu-id="c0fa3-225">Otras personas que considere más detallado marcado y código HTML generado de ViewModel enfocan un inconveniente.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-225">Others consider the the more verbose markup and generated HTML of the ViewModel approach a disadvantage.</span></span>

<span data-ttu-id="c0fa3-226">En esta sección, hemos aprendimos tres métodos para usar la **DropDownList** con datos de categoría.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-226">In this section we have learned three approaches to using the **DropDownList** with category data.</span></span> <span data-ttu-id="c0fa3-227">En la siguiente sección, le mostraremos cómo agregar una nueva categoría.</span><span class="sxs-lookup"><span data-stu-id="c0fa3-227">In the next section, we'll show how to add a new category.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c0fa3-228">[Anterior](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Siguiente](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="c0fa3-228">[Previous](using-the-dropdownlist-helper-with-aspnet-mvc.md)
[Next](adding-a-new-category-to-the-dropdownlist-using-jquery-ui.md)</span></span>
