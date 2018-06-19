---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Agregar una nueva categoría a los controles de DropDownList mediante jQuery UI | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 16f7af1d679aace24fff86abb19740beebafe785
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871940"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="6fc0e-102">Agregar una nueva categoría a los controles de DropDownList mediante jQuery UI</span><span class="sxs-lookup"><span data-stu-id="6fc0e-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="6fc0e-103">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6fc0e-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="6fc0e-104">El código HTML `Select` etiqueta es ideal para presentar una lista de los datos de categoría fijo, pero a menudo necesita agregar una nueva categoría.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="6fc0e-105">¿Supongamos que queremos a agregar el género "Opera" a las categorías en nuestra base de datos?</span><span class="sxs-lookup"><span data-stu-id="6fc0e-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="6fc0e-106">En esta sección, usaremos jQuery UI para agregar un cuadro de diálogo, podemos utilizar para agregar una nueva categoría.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="6fc0e-107">La imagen siguiente muestra cómo se presentará la interfaz de usuario en el explorador.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="6fc0e-108">Cuando un usuario selecciona el **agregar nueva género** vínculo, un cuadro de diálogo pide al usuario un nuevo nombre de género (y opcionalmente una descripción).</span><span class="sxs-lookup"><span data-stu-id="6fc0e-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="6fc0e-109">La imagen siguiente muestra el **agregar género** cuadro de diálogo emergente.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="6fc0e-110">Cuando se escriba un nuevo nombre de género y la **guardar** botón se inserta, ocurre lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6fc0e-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="6fc0e-111">Una llamada AJAX envía los datos para el método Create de la controladora de género, que guarda el género nueva en la base de datos y devuelve la nueva información de género (nombre de género e Id.) como JSON.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="6fc0e-112">JavaScript agrega los nuevos datos de género a la lista de selección.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="6fc0e-113">JavaScript convierte el género nuevo el elemento seleccionado.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="6fc0e-114">En la imagen siguiente, **Opera** se agrega a la base de datos y se selecciona en el **género** lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="6fc0e-115">Abra la *Views\StoreManager\Create.cshtml* archivo y reemplace el marcado de género con lo siguiente en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="6fc0e-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="6fc0e-116">El `_ChooseGenre` vista parcial contendrá toda la lógica para enlazar el JavaScript y jQuery usado para implementar la nueva característica de género agregar.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="6fc0e-117">Una vez que hemos completado el código resultará fácil de hacer lo mismo con el intérprete de interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="6fc0e-118">En el Explorador de soluciones, haga clic con el *Views\StoreManager* carpeta y seleccione **agregar**, a continuación, **vista**.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="6fc0e-119">En el **nombre de la vista** de entrada, escriba `_ChooseGenre` , a continuación, seleccione **agregar**.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="6fc0e-120">Reemplace el marcado en el *Views\StoreManager\\_ChooseGenre.cshtml* archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6fc0e-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="6fc0e-121">La primera línea declara que pasamos una `Album` como nuestro modelo exactamente la misma instrucción que se encuentra en la vista de creación del modelo.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="6fc0e-122">Las siguientes líneas son el **etiqueta** marcado de la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="6fc0e-123">La siguiente línea es la **DropDownList** auxiliar llama, exactamente igual que en la vista de creación original.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="6fc0e-124">La línea siguiente agrega un vínculo con el nombre `Add New Genre`, y estilos como un botón.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="6fc0e-125">La línea que contiene `ValidationMessageFor` se copia directamente desde la vista de creación.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="6fc0e-126">Las siguientes líneas:</span><span class="sxs-lookup"><span data-stu-id="6fc0e-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="6fc0e-127">crea un elemento div oculto, con el Id. de `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="6fc0e-128">Usaremos jQuery para enlazar nuestro **agregar género** cuadro de diálogo con el Id. de `genreDialog` en este div.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="6fc0e-129">Las dos últimas etiquetas de script contienen vínculos a los archivos JavaScript que se usará para implementar la nueva característica de género agregar.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="6fc0e-130">El */Scripts/chooseGenre.js* archivo es siempre automáticamente en el proyecto, se examinará, más adelante en el tutorial.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="6fc0e-131">Ejecute la aplicación y haga clic en el **agregar nueva género** botón.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="6fc0e-132">En el **agregar género** diálogo cuadro, escriba **Opera** en el **nombre** cuadro de entrada.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="6fc0e-133">Haga clic en el botón **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-133">Click the **Save** button.</span></span> <span data-ttu-id="6fc0e-134">Una llamada AJAX crea la categoría Opera y, a continuación, rellena la lista desplegable con Opera y establece Opera como el género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="6fc0e-135">Escriba un intérprete, título y el precio, a continuación, seleccione la **crear** botón.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="6fc0e-136">Si escribe un precio menor que $8,99, el nuevo álbum aparecerá en la parte superior de la vista de índice.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="6fc0e-137">Comprobar que la nueva entrada de álbum se guardó en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="6fc0e-138">Intente crear un nuevo género solo por una letra.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="6fc0e-139">El siguiente código en el *Models\Genre.cs* archivo establece la longitud mínima y máxima del nombre de género.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="6fc0e-140">Validación del lado cliente informa que debe especificar una cadena de entre 2 y 20 caracteres.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="6fc0e-141">Examinar cómo un género nueva se agrega a la base de datos y la lista Select.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="6fc0e-142">Abra la *Scripts\chooseGenre.js* de archivos y examine el código.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="6fc0e-143">La segunda línea usa el identificador de `genreDialog` para crear un cuadro de diálogo a la etiqueta div en la *Views\StoreManager\\_ChooseGenre.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="6fc0e-144">La mayoría de los parámetros con nombre es autoexplicativos.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="6fc0e-145">El `autoOpen` parámetro se establece en false, al seleccionar la **crear género** botón abrirá el cuadro de diálogo explícitamente (se describe esta última en).</span><span class="sxs-lookup"><span data-stu-id="6fc0e-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="6fc0e-146">El cuadro de diálogo tiene dos botones, **guardar** y **cancelar**.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="6fc0e-147">El **cancelar** botón cierra el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="6fc0e-148">El código siguiente muestra el **guardar** botón función.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="6fc0e-149">El `var createGenreForm` está seleccionado en el `createGenreForm` identificador.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="6fc0e-150">El `createGenreForm` Id. se establece en el código siguiente se encuentra en la *Views\Genre\\_CreateGenre.cshtml* archivo.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="6fc0e-151">El [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) sobrecarga auxiliar utilizada en el *Views\Genre\\_CreateGenre.cshtml* HTML genera el archivo con un atributo de acción que contiene la dirección URL para enviar el formulario.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="6fc0e-152">Puede ver esto mostrando la página de álbum de crear en un explorador y seleccionar origen de mostrar en el explorador.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="6fc0e-153">El marcado siguiente muestra el código HTML generado que contiene la etiqueta de formulario.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="6fc0e-154">JQuery `$.post` línea realiza una llamada de AJAX para el atributo de acción (`/StoreManager/Create`) y pasa los datos de la **crear género** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="6fc0e-155">Los datos se componen del nombre para el nuevo género y una descripción opcional.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="6fc0e-156">Si la llamada de AJAX es correcta, el nuevo nombre de género y el valor se agregan en el marcado de selección y el género nueva está establecido en el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="6fc0e-157">Como este es el marcado generado dinámicamente, no puede ver la nueva opción de seleccionar en el código fuente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="6fc0e-158">Puede ver el nuevo código HTML con las herramientas de desarrollo F12 de Internet Explorer 9.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="6fc0e-159">Para ver la opción de seleccionar nuevo, en Internet Explorer 9, presionar la tecla F12 para iniciar las herramientas de desarrollo F12.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="6fc0e-160">Vaya a la página Crear y agregar un género nueva para que el género nuevo está seleccionado en la lista de selección de género.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="6fc0e-161">En las herramientas de desarrollo F12:</span><span class="sxs-lookup"><span data-stu-id="6fc0e-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="6fc0e-162">Seleccione la ficha HTML.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="6fc0e-163">Alcanzó el icono de actualización.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="6fc0e-164">En el cuadro de búsqueda, escriba GenreID.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="6fc0e-165">Utilizando el icono siguiente,</span><span class="sxs-lookup"><span data-stu-id="6fc0e-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="6fc0e-166">Desplácese hasta la siguiente etiqueta select:</span><span class="sxs-lookup"><span data-stu-id="6fc0e-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="6fc0e-167">Expanda el último valor de opción.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="6fc0e-168">El siguiente código en el *Scripts\chooseGenre.js* archivo muestra cómo el **agregar nueva género** botón obtiene conectado al evento de clic y cómo el **agregar nueva género** cuadro de diálogo creado.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="6fc0e-169">La primera línea crea una función de haga clic en el **agregar nueva género** botón.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="6fc0e-170">El siguiente marcado de la Views\StoreManager\\_ChooseGenre.cshtml archivo muestra cómo el **agregar nueva género** botón se crea:</span><span class="sxs-lookup"><span data-stu-id="6fc0e-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="6fc0e-171">El método load crea y abre el cuadro de diálogo Agregar género y llama jQuery `parse` método por lo que se produce la validación de cliente en los datos escritos en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="6fc0e-172">En esta sección ha aprendido cómo crear un cuadro de diálogo que puede usarse para agregar nuevos datos de categoría a una lista de selección.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="6fc0e-173">Puede seguir el mismo procedimiento para crear la interfaz de usuario para agregar a un intérprete nuevo a la lista de selección de intérprete.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="6fc0e-174">Este tutorial ha proporcionado información general sobre cómo trabajar con la aplicación auxiliar HTML de MVC de ASP.NET **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="6fc0e-175">Para obtener más información sobre cómo trabajar con el **DropDownList**, vea la sección de referencias de suma más adelante.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="6fc0e-176">Háganoslo saber si este tutorial ha sido útil.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="6fc0e-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="6fc0e-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="6fc0e-178">Referencias adicionales</span><span class="sxs-lookup"><span data-stu-id="6fc0e-178">Additional References</span></span>

- <span data-ttu-id="6fc0e-179">[Lista desplegable – en cascada MVC de ASP.NET muestra los Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) por [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="6fc0e-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="6fc0e-180">[Elegido](http://harvesthq.github.com/chosen/) JavaScript de un complemento que admiten la selección múltiple y filtrado.</span><span class="sxs-lookup"><span data-stu-id="6fc0e-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="6fc0e-181">Colaboradores</span><span class="sxs-lookup"><span data-stu-id="6fc0e-181">Contributors</span></span>

- [<span data-ttu-id="6fc0e-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="6fc0e-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="6fc0e-183">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="6fc0e-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="6fc0e-184">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="6fc0e-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="6fc0e-185">Revisores</span><span class="sxs-lookup"><span data-stu-id="6fc0e-185">Reviewers</span></span>

- <span data-ttu-id="6fc0e-186">Jean-Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="6fc0e-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="6fc0e-187">Brad Wilson</span><span class="sxs-lookup"><span data-stu-id="6fc0e-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="6fc0e-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="6fc0e-188">Mike Pope</span></span>
- <span data-ttu-id="6fc0e-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="6fc0e-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="6fc0e-190">Anterior</span><span class="sxs-lookup"><span data-stu-id="6fc0e-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
