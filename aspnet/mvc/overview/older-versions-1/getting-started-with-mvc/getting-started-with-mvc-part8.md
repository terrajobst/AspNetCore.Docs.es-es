---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Agregar una columna al modelo | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 241f9108394561fecb15db5b6efa2661fdb128d6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361765"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="27c05-104">Agregar una columna al modelo</span><span class="sxs-lookup"><span data-stu-id="27c05-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="27c05-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="27c05-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="27c05-106">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27c05-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="27c05-107">Creará una aplicación web simple que lee y escribe desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="27c05-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="27c05-108">Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.</span><span class="sxs-lookup"><span data-stu-id="27c05-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="27c05-109">En esta sección vamos a recorrer cómo podemos realizar cambios en el esquema de nuestra base de datos y controlar los cambios dentro de nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="27c05-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="27c05-110">Vamos a agregar a una columna "Clasificación" a la tabla de la película.</span><span class="sxs-lookup"><span data-stu-id="27c05-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="27c05-111">Vuelva al IDE y haga clic en el Explorador de base de datos.</span><span class="sxs-lookup"><span data-stu-id="27c05-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="27c05-112">Haga clic en la tabla de la película y seleccione Abrir definición de tabla.</span><span class="sxs-lookup"><span data-stu-id="27c05-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="27c05-113">Agregar una columna de "Rating" tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="27c05-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="27c05-114">Puesto que no hay ninguna clasificación ahora, la columna puede permitir valores NULL.</span><span class="sxs-lookup"><span data-stu-id="27c05-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="27c05-115">Haga clic en Guardar.</span><span class="sxs-lookup"><span data-stu-id="27c05-115">Click Save.</span></span>

<span data-ttu-id="27c05-116">[![Edición de la tabla de películas](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="27c05-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="27c05-117">A continuación, vuelva al explorador de soluciones y abra el archivo Movies.edmx (que se encuentra en la carpeta \Models).</span><span class="sxs-lookup"><span data-stu-id="27c05-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="27c05-118">Haga clic con el botón derecho en la superficie de diseño (el área blanca) y seleccione el modelo de actualización de base de datos.</span><span class="sxs-lookup"><span data-stu-id="27c05-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="27c05-119">[![Películas - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="27c05-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="27c05-120">Esto iniciará al Asistente de actualización de"".</span><span class="sxs-lookup"><span data-stu-id="27c05-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="27c05-121">Haga clic en la pestaña de actualización dentro de él y haga clic en Finalizar.</span><span class="sxs-lookup"><span data-stu-id="27c05-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="27c05-122">Nuestra clase de modelo de película, a continuación, se actualizará con la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="27c05-122">Our Movie model class will then be updated with the new column.</span></span>

![Asistente para actualizar (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="27c05-124">Después de hacer clic en Finalizar, puede ver que la nueva columna de clasificación se ha agregado a la entidad de la película en nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="27c05-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="27c05-125">[![Entidad de película](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="27c05-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="27c05-126">Hemos agregado una columna en el modelo de base de datos, pero no conocen las vistas.</span><span class="sxs-lookup"><span data-stu-id="27c05-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="27c05-127">Actualizar las vistas con los cambios del modelo</span><span class="sxs-lookup"><span data-stu-id="27c05-127">Update Views with Model Changes</span></span>

<span data-ttu-id="27c05-128">Hay varias maneras de podríamos actualizamos nuestras plantillas de vista para reflejar la nueva columna de clasificación.</span><span class="sxs-lookup"><span data-stu-id="27c05-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="27c05-129">Puesto que hemos creado estas vistas mediante la generación de mediante el cuadro de diálogo Agregar vista, podríamos eliminarlas y volver a crearlas de nuevo.</span><span class="sxs-lookup"><span data-stu-id="27c05-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="27c05-130">Sin embargo, normalmente las personas ya se habrá realizado modificaciones en sus plantillas de vista de la generación inicial con scaffolding y conveniente agregar o eliminar campos manualmente, tal como hicimos con el campo Id. para crear.</span><span class="sxs-lookup"><span data-stu-id="27c05-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="27c05-131">Abra la plantilla \Views\Movies\Index.aspx y agregue un &lt;th&gt;clasificación&lt;/th&gt; en el encabezado de la tabla Movie.</span><span class="sxs-lookup"><span data-stu-id="27c05-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="27c05-132">Agregué los míos después de género.</span><span class="sxs-lookup"><span data-stu-id="27c05-132">I added mine after Genre.</span></span> <span data-ttu-id="27c05-133">A continuación, en la misma posición de columna pero más abajo, agregue una línea para nuestra nueva clasificación de salida.</span><span class="sxs-lookup"><span data-stu-id="27c05-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="27c05-134">Nuestra plantilla Index.aspx final tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="27c05-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="27c05-135">A continuación, vamos a abrir la plantilla \Views\Movies\Create.aspx y agregar una etiqueta y un cuadro de texto para la nueva propiedad de clasificación:</span><span class="sxs-lookup"><span data-stu-id="27c05-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="27c05-136">Este aspecto el nuestra plantilla Create.aspx final y vamos a cambiar el título y la base de datos secundaria de nuestro explorador &lt;h2&gt; título a algo parecido a "Crear una película" mientras estamos aquí!</span><span class="sxs-lookup"><span data-stu-id="27c05-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="27c05-137">Ejecute la aplicación y ahora tenemos un nuevo campo en la base de datos que se han agregado a la página Create.</span><span class="sxs-lookup"><span data-stu-id="27c05-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="27c05-138">Agregar una nueva película - esta vez con una clasificación - y haga clic en crear.</span><span class="sxs-lookup"><span data-stu-id="27c05-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="27c05-139">[![Crear una película - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="27c05-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="27c05-140">Después de hacer clic en crear, se le envía a la página de índice donde se nueva película aparece con la nueva columna de clasificación en la base de datos</span><span class="sxs-lookup"><span data-stu-id="27c05-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="27c05-141">[![Lista de películas - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="27c05-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="27c05-142">Este tutorial básico de lo que a realizar los controladores de su asociación con las vistas y pasan datos codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="27c05-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="27c05-143">A continuación, creamos y diseñado una base de datos y colocar algunos datos en.</span><span class="sxs-lookup"><span data-stu-id="27c05-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="27c05-144">Se recuperan los datos de la base de datos y se muestra en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="27c05-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="27c05-145">A continuación, agregamos un formulario de creación que permiten al usuario que se agreguen datos a la base de datos desde dentro de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="27c05-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="27c05-146">Se agregó la validación y luego realiza la validación de usar JavaScript en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="27c05-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="27c05-147">Por último, se puede cambiar la base de datos para incluir una nueva columna de datos y luego actualizan nuestras dos páginas para crear y mostrar estos nuevos datos.</span><span class="sxs-lookup"><span data-stu-id="27c05-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="27c05-148">Ahora le animo a pasar a nuestro tutorial de nivel intermedio "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", así como los vídeos y recursos en muchas [ https://asp.net/mvc ](https://asp.net/mvc) para obtener más información sobre ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="27c05-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="27c05-149">Disfrútelo.</span><span class="sxs-lookup"><span data-stu-id="27c05-149">Enjoy!</span></span>

- <span data-ttu-id="27c05-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) y [ @shanselman ](http://twitter.com/shanselman) en Twitter.</span><span class="sxs-lookup"><span data-stu-id="27c05-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="27c05-151">Anterior</span><span class="sxs-lookup"><span data-stu-id="27c05-151">Previous</span></span>](getting-started-with-mvc-part7.md)
