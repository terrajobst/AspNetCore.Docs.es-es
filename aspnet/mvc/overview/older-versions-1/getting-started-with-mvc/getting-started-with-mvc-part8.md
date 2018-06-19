---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
title: Agregar una columna al modelo | Documentos de Microsoft
author: shanselman
description: Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Crear una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 7ae696b9-348f-4993-8ebb-a838acbe0c28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part8
msc.type: authoredcontent
ms.openlocfilehash: 7a0b64ce00fc5ee6d49990f1d4d93a154c467bf5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "30879262"
---
<a name="adding-a-column-to-the-model"></a><span data-ttu-id="09f12-104">Agregar una columna al modelo</span><span class="sxs-lookup"><span data-stu-id="09f12-104">Adding a Column to the Model</span></span>
====================
<span data-ttu-id="09f12-105">by [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="09f12-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="09f12-106">Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09f12-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="09f12-107">Se creará una aplicación web simple que lee y escribe desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="09f12-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="09f12-108">Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.</span><span class="sxs-lookup"><span data-stu-id="09f12-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="09f12-109">En esta sección, vamos a recorrer cómo podemos realizar cambios en el esquema de la base de datos y controlar los cambios en nuestra aplicación.</span><span class="sxs-lookup"><span data-stu-id="09f12-109">In this section we are going to walk through how we can make changes to the schema of our database, and handle the changes within our application.</span></span>

<span data-ttu-id="09f12-110">Vamos a agregar a una columna "Clasificación" a la tabla de la película.</span><span class="sxs-lookup"><span data-stu-id="09f12-110">Let's add a "Rating" Colum to the Movie table.</span></span> <span data-ttu-id="09f12-111">Volver al IDE y haga clic en el Explorador de base de datos.</span><span class="sxs-lookup"><span data-stu-id="09f12-111">Go back to the IDE and click the Database Explorer.</span></span> <span data-ttu-id="09f12-112">Haga clic en la tabla de la película y seleccione Abrir definición de tabla.</span><span class="sxs-lookup"><span data-stu-id="09f12-112">Right click the Movie table and select Open Table Definition.</span></span>

<span data-ttu-id="09f12-113">Agregar una columna "Clasificación" tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="09f12-113">Add a "Rating" column as seen below.</span></span> <span data-ttu-id="09f12-114">Puesto que no tenemos ninguna clasificación ahora, la columna puede permitir valores NULL.</span><span class="sxs-lookup"><span data-stu-id="09f12-114">Since we don't have any Ratings now, the column can allow nulls.</span></span> <span data-ttu-id="09f12-115">Haga clic en Guardar.</span><span class="sxs-lookup"><span data-stu-id="09f12-115">Click Save.</span></span>

<span data-ttu-id="09f12-116">[![Edición de películas de tabla](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="09f12-116">[![Editing Movies Table](getting-started-with-mvc-part8/_static/image2.png)](getting-started-with-mvc-part8/_static/image1.png)</span></span>

<span data-ttu-id="09f12-117">A continuación, vuelva al explorador de soluciones y abra el archivo Movies.edmx (que se encuentra en la carpeta \Models).</span><span class="sxs-lookup"><span data-stu-id="09f12-117">Next, return to the Solution Explorer and open up the Movies.edmx file (which is in the \Models folder).</span></span> <span data-ttu-id="09f12-118">Haga clic con el botón secundario en la superficie de diseño (el área blanca) y seleccione el modelo de actualización de base de datos.</span><span class="sxs-lookup"><span data-stu-id="09f12-118">Right click on the design surface (the white area) and select Update Model from Database.</span></span>

<span data-ttu-id="09f12-119">[![Películas - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="09f12-119">[![Movies - Microsoft Visual Web Developer 2010 Express (11)](getting-started-with-mvc-part8/_static/image4.png)](getting-started-with-mvc-part8/_static/image3.png)</span></span>

<span data-ttu-id="09f12-120">Se iniciará el Asistente para actualización de"".</span><span class="sxs-lookup"><span data-stu-id="09f12-120">This will launch the "Update Wizard".</span></span> <span data-ttu-id="09f12-121">Haga clic en la pestaña de actualización dentro de él y haga clic en Finalizar.</span><span class="sxs-lookup"><span data-stu-id="09f12-121">Click the Refresh tab within it and click Finish.</span></span> <span data-ttu-id="09f12-122">Nuestra clase de modelo de película, a continuación, se actualizará con la nueva columna.</span><span class="sxs-lookup"><span data-stu-id="09f12-122">Our Movie model class will then be updated with the new column.</span></span>

![Asistente para actualización de (2)](getting-started-with-mvc-part8/_static/image5.png)

<span data-ttu-id="09f12-124">Después de hacer clic en Finalizar, puede ver que la nueva columna de clasificación se ha agregado a la entidad de la película en nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="09f12-124">After clicking Finish, you can see the new Rating Column has been added to the Movie Entity in our model.</span></span>

<span data-ttu-id="09f12-125">[![Entidad de película](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="09f12-125">[![Movie Entity](getting-started-with-mvc-part8/_static/image7.png)](getting-started-with-mvc-part8/_static/image6.png)</span></span>

<span data-ttu-id="09f12-126">Hemos agregado una columna en el modelo de base de datos, pero las vistas no saberlo.</span><span class="sxs-lookup"><span data-stu-id="09f12-126">We've added a column in the database model, but the Views don't know about it.</span></span>

## <a name="update-views-with-model-changes"></a><span data-ttu-id="09f12-127">Actualizar vistas con los cambios de modelo</span><span class="sxs-lookup"><span data-stu-id="09f12-127">Update Views with Model Changes</span></span>

<span data-ttu-id="09f12-128">Hay varias maneras de podríamos actualizar nuestras plantillas de vista para reflejar la nueva columna de clasificación.</span><span class="sxs-lookup"><span data-stu-id="09f12-128">There are a few ways we could update our view templates to reflect the new Rating column.</span></span> <span data-ttu-id="09f12-129">Puesto que estas vistas se creó mediante generarlos mediante el cuadro de diálogo Agregar vista, podríamos eliminarlos y volver a crearlos de nuevo.</span><span class="sxs-lookup"><span data-stu-id="09f12-129">Since we created these Views by generating them via the Add View dialog, we could delete them and recreate them again.</span></span> <span data-ttu-id="09f12-130">Sin embargo, normalmente personas ya se habrá realizado modificaciones en las plantillas de vista de la generación con scaffolding inicial y desearán agregar o eliminar campos manualmente, como hicimos en el campo ID para crear.</span><span class="sxs-lookup"><span data-stu-id="09f12-130">However, typically people will have already made modifications to their View templates from the initial scaffolded generation and will want to add or delete fields manually, just as we did with the ID field for Create.</span></span>

<span data-ttu-id="09f12-131">Abra la plantilla de \Views\Movies\Index.aspx y agregue un &lt;th&gt;clasificación&lt;/th&gt; en el encabezado de la tabla de la película.</span><span class="sxs-lookup"><span data-stu-id="09f12-131">Open up the \Views\Movies\Index.aspx template and add a &lt;th&gt;Rating&lt;/th&gt; to the head of the Movie table.</span></span> <span data-ttu-id="09f12-132">Había agregado extraiga después de género.</span><span class="sxs-lookup"><span data-stu-id="09f12-132">I added mine after Genre.</span></span> <span data-ttu-id="09f12-133">A continuación, en la misma posición de columna pero más abajo, agregue una línea para nuestra nueva clasificación de salida.</span><span class="sxs-lookup"><span data-stu-id="09f12-133">Then, in the same column position but lower down, add a line to output our new Rating.</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample1.aspx)]

<span data-ttu-id="09f12-134">Nuestra plantilla Index.aspx final tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="09f12-134">Our final Index.aspx template will look like this:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample2.aspx)]

<span data-ttu-id="09f12-135">A continuación, vamos a abrir la plantilla \Views\Movies\Create.aspx y agregar una etiqueta y un cuadro de texto para la nueva propiedad de clasificación:</span><span class="sxs-lookup"><span data-stu-id="09f12-135">Let's then open up the \Views\Movies\Create.aspx template and add a Label and Textbox for our new Rating property:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample3.aspx)]

<span data-ttu-id="09f12-136">La plantilla Create.aspx final se este aspecto y vamos a cambiar el título y la base de datos secundaria de nuestro navegador &lt;h2&gt; título en algo similar a "Crear una película" mientras se encuentra aquí.</span><span class="sxs-lookup"><span data-stu-id="09f12-136">Our final Create.aspx template will look like this, and let's change our browser's title and secondary &lt;h2&gt; title to something like "Create a Movie" while we're in here!</span></span>

[!code-aspx[Main](getting-started-with-mvc-part8/samples/sample4.aspx)]

<span data-ttu-id="09f12-137">Ejecutar la aplicación y ahora ya tiene un campo nuevo en la base de datos que se han agregado a la página Crear.</span><span class="sxs-lookup"><span data-stu-id="09f12-137">Run your app and now you've got a new field in the database that's been added to the Create page.</span></span> <span data-ttu-id="09f12-138">Agregar una película nueva - en este momento con una clasificación - y haga clic en crear.</span><span class="sxs-lookup"><span data-stu-id="09f12-138">Add a new Movie - this time with a Rating - and click Create.</span></span>

<span data-ttu-id="09f12-139">[![Crear una película - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="09f12-139">[![Create a Movie - Windows Internet Explorer](getting-started-with-mvc-part8/_static/image9.png)](getting-started-with-mvc-part8/_static/image8.png)</span></span>

<span data-ttu-id="09f12-140">Después de hacer clic en crear, se envían a la página de índice donde se nuevos película aparece con la nueva columna de clasificación en la base de datos</span><span class="sxs-lookup"><span data-stu-id="09f12-140">After you click Create, you're sent to the Index page where you new Movie is listed with the new Rating Column in the database</span></span>

<span data-ttu-id="09f12-141">[![Lista de películas - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="09f12-141">[![Movie List - Windows Internet Explorer (12)](getting-started-with-mvc-part8/_static/image11.png)](getting-started-with-mvc-part8/_static/image10.png)</span></span>

<span data-ttu-id="09f12-142">Este tutorial básico de tenemos que pueda empezar a hacer que los controladores, asociarlos con las vistas y pasar sobre los datos codificados de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="09f12-142">This basic tutorial got you started making Controllers, associating them with Views and passing around hard-coded data.</span></span> <span data-ttu-id="09f12-143">A continuación, se crea y diseñado una base de datos y poner algunos datos en.</span><span class="sxs-lookup"><span data-stu-id="09f12-143">Then we created and designed a Database and put some data it in.</span></span> <span data-ttu-id="09f12-144">Se recuperan los datos de la base de datos y se muestran en una tabla HTML.</span><span class="sxs-lookup"><span data-stu-id="09f12-144">We retrieved the data from the database and displayed it in an HTML table.</span></span> <span data-ttu-id="09f12-145">A continuación, agregamos un formulario de creación que permiten al usuario agregar datos a la base de datos por sí mismos desde dentro de la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="09f12-145">Then we added a Create form that let the user add data to the database themselves from within the Web Application.</span></span> <span data-ttu-id="09f12-146">Se agrega validación, a continuación, realice la validación usa JavaScript en el cliente.</span><span class="sxs-lookup"><span data-stu-id="09f12-146">We added validation, then made the validation use JavaScript on the client-side.</span></span> <span data-ttu-id="09f12-147">Por último, se cambia la base de datos para incluir una nueva columna de datos, a continuación, actualizan nuestras dos páginas para crear y mostrar los nuevos datos.</span><span class="sxs-lookup"><span data-stu-id="09f12-147">Finally, we changed the database to include a new column of data, then updated our two pages to create and display this new data.</span></span>

<span data-ttu-id="09f12-148">Ahora le animo a pasar a nuestro tutorial de nivel intermedio "[tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)", así como los vídeos y recursos en muchas [ https://asp.net/mvc ](https://asp.net/mvc) para más información sobre ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="09f12-148">I now encourage you to move on to our intermediate-level tutorial "[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)" as well as the many videos and resources at [https://asp.net/mvc](https://asp.net/mvc) to learn even more about ASP.NET MVC!</span></span>

<span data-ttu-id="09f12-149">Disfrútelo.</span><span class="sxs-lookup"><span data-stu-id="09f12-149">Enjoy!</span></span>

- <span data-ttu-id="09f12-150">Scott Hanselman - [ http://hanselman.com ](http://hanselman.com) y [ @shanselman ](http://twitter.com/shanselman) en Twitter.</span><span class="sxs-lookup"><span data-stu-id="09f12-150">Scott Hanselman - [http://hanselman.com](http://hanselman.com) and [@shanselman](http://twitter.com/shanselman) on Twitter.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="09f12-151">Anterior</span><span class="sxs-lookup"><span data-stu-id="09f12-151">Previous</span></span>](getting-started-with-mvc-part7.md)
