---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Acceso a los datos del modelo desde un controlador | Microsoft Docs
author: shanselman
description: Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Cree una aplicación web simple que lee y escribe desde una base de datos.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 556eed77cbd9e81c0a2a1334bb0a8ee56abafd34
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815013"
---
<a name="accessing-your-models-data-from-a-controller"></a><span data-ttu-id="58594-104">Acceso a los datos del modelo desde un controlador</span><span class="sxs-lookup"><span data-stu-id="58594-104">Accessing your Model's Data from a Controller</span></span>
====================
<span data-ttu-id="58594-105">por [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="58594-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="58594-106">Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="58594-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="58594-107">Creará una aplicación web simple que lee y escribe desde una base de datos.</span><span class="sxs-lookup"><span data-stu-id="58594-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="58594-108">Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.</span><span class="sxs-lookup"><span data-stu-id="58594-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="58594-109">En esta sección vamos a crear una nueva clase de nombre moviescontroller al y escribir algún código que recupera los datos de película y lo muestra al explorador mediante una plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="58594-109">In this section we are going to create a new MoviesController class, and write some code that retrieves our Movie data and displays it back to the browser using a View template.</span></span>

<span data-ttu-id="58594-110">Haga clic con el botón derecho en la carpeta Controllers y realice un nombre moviescontroller al nuevo.</span><span class="sxs-lookup"><span data-stu-id="58594-110">Right click on the Controllers folder and make a new MoviesController.</span></span>

<span data-ttu-id="58594-111">[![Agregar controlador](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="58594-111">[![Add Controller](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)</span></span>

<span data-ttu-id="58594-112">Esto creará un nuevo archivo "MoviesController.cs" bajo la carpeta \Controllers dentro de nuestro proyecto.</span><span class="sxs-lookup"><span data-stu-id="58594-112">This will create a new "MoviesController.cs" file underneath our \Controllers folder within our project.</span></span> <span data-ttu-id="58594-113">Vamos a actualizar el MovieController para recuperar la lista de películas de nuestra base de datos recién agregado.</span><span class="sxs-lookup"><span data-stu-id="58594-113">Let's update the MovieController to retrieve the list of movies from our newly populated database.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

<span data-ttu-id="58594-114">Se está realizando una consulta LINQ para que solo recuperamos películas publicadas después el verano de 1984.</span><span class="sxs-lookup"><span data-stu-id="58594-114">We are performing a LINQ query so that we only retrieve movies released after the summer of 1984.</span></span> <span data-ttu-id="58594-115">Necesitaremos una plantilla de vista para representar esta lista de películas de vuelta, así que haga clic en el método y seleccione Agregar vista para crearlo.</span><span class="sxs-lookup"><span data-stu-id="58594-115">We'll need a View template to render this list of movies back, so right-click in the method and select Add View to create it.</span></span>

<span data-ttu-id="58594-116">En el cuadro de diálogo Agregar vista indicaremos que pasamos una lista&lt;Movies.Models.Movie&gt; a nuestra plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="58594-116">Within the Add View dialog we'll indicate that we are passing a List&lt;Movies.Models.Movie&gt; to our View template.</span></span> <span data-ttu-id="58594-117">A diferencia de las horas anteriores se usa el cuadro de diálogo Agregar vista y decidió crear una plantilla "vacía", esta vez que indicaremos que queremos que Visual Studio para automáticamente "aplicar scaffolding a" una plantilla de vista para que podamos con algún contenido de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="58594-117">Unlike the previous times we used the Add View dialog and chose to create an "Empty" template, this time we'll indicate that we want Visual Studio to automatically "scaffold" a view template for us with some default content.</span></span> <span data-ttu-id="58594-118">Haremos esto seleccionando el elemento "List" en el menú"ver contenido de lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="58594-118">We'll do this by selecting the "List" item within the "View content dropdown menu.</span></span>

<span data-ttu-id="58594-119">Recuerde que cuando ha creado una nueva clase, deberá compilar la aplicación para que se muestre en el cuadro de diálogo Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="58594-119">Remember, when you have a created a new class you'll need to compile your application for it to show up in the Add View Dialog.</span></span>

![Agregar vista](getting-started-with-mvc-part5/_static/image3.png)

<span data-ttu-id="58594-121">Haga clic en Agregar y el sistema generará automáticamente el código para obtener una vista para que podamos que muestra nuestra lista de películas.</span><span class="sxs-lookup"><span data-stu-id="58594-121">Click add and the system will automatically generate the code for a View for us that displays our list of movies.</span></span> <span data-ttu-id="58594-122">Esto es un buen momento para cambiar la &lt;h2&gt; encabezado a algo parecido a "My Movie List", como hicimos anteriormente con la vista de Hello World.</span><span class="sxs-lookup"><span data-stu-id="58594-122">This is a good time to change the &lt;h2&gt; heading to something like "My Movie List" like we did earlier with the Hello World view.</span></span>

<span data-ttu-id="58594-123">[![Películas - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="58594-123">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)</span></span>

<span data-ttu-id="58594-124">Ejecute la aplicación y visite /Movies en la barra de direcciones.</span><span class="sxs-lookup"><span data-stu-id="58594-124">Run your application and visit /Movies in the address bar.</span></span> <span data-ttu-id="58594-125">Ahora hemos recupera datos de la base de datos mediante una consulta básica en el controlador y devuelve los datos a una vista que sabe acerca de las películas.</span><span class="sxs-lookup"><span data-stu-id="58594-125">Now we've retrieved data from the database using a basic query inside the Controller and returned the data to a View that knows about Movies.</span></span> <span data-ttu-id="58594-126">Esa vista, a continuación, pone a través de la lista de películas y crea una tabla de datos para nosotros.</span><span class="sxs-lookup"><span data-stu-id="58594-126">That View then spins through the list of Movies and creates a table of data for us.</span></span>

<span data-ttu-id="58594-127">[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="58594-127">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)</span></span>

<span data-ttu-id="58594-128">Se no implementar funcionalidad de edición, detalles y eliminación con esta aplicación, por lo que no necesitamos los vínculos predeterminados creados por la plantilla scaffold para nosotros.</span><span class="sxs-lookup"><span data-stu-id="58594-128">We won't be implementing Edit, Details and Delete functionality with this application - so we don't need the default links that the scaffold template created for us.</span></span> <span data-ttu-id="58594-129">Abra el archivo /Movies/Index.aspx y quitarlas.</span><span class="sxs-lookup"><span data-stu-id="58594-129">Open up the /Movies/Index.aspx file and remove them.</span></span>

<span data-ttu-id="58594-130">Este es el código fuente para el aspecto que debería nuestra plantilla de vista actualizada una vez que realizamos estos cambios:</span><span class="sxs-lookup"><span data-stu-id="58594-130">Here is the source code for what our updated View template should look like once we make these changes:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

<span data-ttu-id="58594-131">Crear vínculos que no necesitamos, por lo que deberá eliminarlos en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="58594-131">It's creating links that we won't need, so we'll delete them for this example.</span></span> <span data-ttu-id="58594-132">Mantendremos el vínculo Crear nuevo sin embargo, ya que es el siguiente.</span><span class="sxs-lookup"><span data-stu-id="58594-132">We will keep our Create New link though, as that's next!</span></span> <span data-ttu-id="58594-133">Este es el aspecto de nuestra aplicación con esa columna eliminada.</span><span class="sxs-lookup"><span data-stu-id="58594-133">Here's what our app looks like with that column removed.</span></span>

<span data-ttu-id="58594-134">[![Lista de películas - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="58594-134">[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)</span></span>

<span data-ttu-id="58594-135">Ahora tenemos una lista simple de nuestros datos de la película.</span><span class="sxs-lookup"><span data-stu-id="58594-135">We now have a simple listing of our movie data.</span></span> <span data-ttu-id="58594-136">Sin embargo, si hacemos clic en el vínculo "Crear nuevo", obtendremos un error que no se enlazó!</span><span class="sxs-lookup"><span data-stu-id="58594-136">However, if we click the "Create New" link, we'll get an error as it's not hooked up!</span></span> <span data-ttu-id="58594-137">Vamos a implementar un método de acción Create y que los usuarios puedan introducir nuevas películas en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="58594-137">Let's implement a Create Action method and enable a user to enter new movies in our database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="58594-138">[Anterior](getting-started-with-mvc-part4.md)
> [Siguiente](getting-started-with-mvc-part6.md)</span><span class="sxs-lookup"><span data-stu-id="58594-138">[Previous](getting-started-with-mvc-part4.md)
[Next](getting-started-with-mvc-part6.md)</span></span>
