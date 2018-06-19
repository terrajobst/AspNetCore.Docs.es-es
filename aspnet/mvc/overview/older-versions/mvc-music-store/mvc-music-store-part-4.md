---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Acceso a datos y modelos | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 4 cubre el acceso a datos y modelos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879483"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="848f4-104">Parte 4: Acceso a datos y modelos</span><span class="sxs-lookup"><span data-stu-id="848f4-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="848f4-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="848f4-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="848f4-106">La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="848f4-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="848f4-107">La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="848f4-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="848f4-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="848f4-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="848f4-109">Parte 4 cubre el acceso a datos y modelos.</span><span class="sxs-lookup"><span data-stu-id="848f4-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="848f4-110">Hasta ahora, hemos solo ha pasamos "datos ficticios" de nuestros controladores a nuestras plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="848f4-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="848f4-111">Ahora estamos listos enlazar una base de datos real.</span><span class="sxs-lookup"><span data-stu-id="848f4-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="848f4-112">En este tutorial abordaremos cómo usar SQL Server Compact Edition (también denominados SQL CE) como el motor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="848f4-113">SQL CE es un archivo gratuita e incrustada, en función de base de datos que no requiere ninguna instalación o configuración, lo que es realmente adecuado para el desarrollo local.</span><span class="sxs-lookup"><span data-stu-id="848f4-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="848f4-114">Acceso de base de datos con Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="848f4-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="848f4-115">Vamos a usar la compatibilidad de Entity Framework (EF) que se incluye en los proyectos de ASP.NET MVC 3 para consultar y actualizar la base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="848f4-116">EF es un objeto flexible relacional de asignación de API que permite a los desarrolladores para consultar y actualizar datos almacenados en una base de datos de una manera orientada a objetos de datos de (objetos ORM).</span><span class="sxs-lookup"><span data-stu-id="848f4-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="848f4-117">Versión 4 de Entity Framework admite un paradigma de desarrollo que se llama a código en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="848f4-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="848f4-118">Código primero le permite crear el objeto de modelo mediante la escritura de clases simples (también conocido como POCO "plain old" objetos de CLR) y puede incluso crear la base de datos sobre la marcha de las clases.</span><span class="sxs-lookup"><span data-stu-id="848f4-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="848f4-119">Cambios en las clases de modelo</span><span class="sxs-lookup"><span data-stu-id="848f4-119">Changes to our Model Classes</span></span>

<span data-ttu-id="848f4-120">Se aprovecha la característica de creación de la base de datos en Entity Framework de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="848f4-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="848f4-121">Antes de hacerlo, sin embargo, vamos a hacer algunos cambios menores en nuestras clases de modelo para agregar en algunas cosas que se usará más adelante.</span><span class="sxs-lookup"><span data-stu-id="848f4-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="848f4-122">Agregar las clases del modelo de intérprete</span><span class="sxs-lookup"><span data-stu-id="848f4-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="848f4-123">Nuestro álbumes se asociarán con intérpretes, por lo que vamos a agregar una clase de modelo simple para describir a un intérprete.</span><span class="sxs-lookup"><span data-stu-id="848f4-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="848f4-124">Agregue una nueva clase en la carpeta de modelos denominada Artist.cs con el código que se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="848f4-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="848f4-125">Actualizar las clases de modelo</span><span class="sxs-lookup"><span data-stu-id="848f4-125">Updating our Model Classes</span></span>

<span data-ttu-id="848f4-126">Actualizar la clase de álbum tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="848f4-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="848f4-127">A continuación, realice las siguientes actualizaciones a la clase de género.</span><span class="sxs-lookup"><span data-stu-id="848f4-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="848f4-128">Agregar la aplicación\_carpeta de datos</span><span class="sxs-lookup"><span data-stu-id="848f4-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="848f4-129">Vamos a agregar una aplicación\_directorio de datos a nuestro proyecto que contenga los archivos de base de datos de SQL Server Express.</span><span class="sxs-lookup"><span data-stu-id="848f4-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="848f4-130">Aplicación\_datos están un directorio especial en ASP.NET que ya tiene los permisos de acceso de seguridad correctas para el acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="848f4-131">En el menú proyecto, seleccione Agregar carpeta ASP.NET y, a continuación, aplicación\_datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="848f4-132">Crear una cadena de conexión en el archivo web.config</span><span class="sxs-lookup"><span data-stu-id="848f4-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="848f4-133">Agregaremos unas cuantas líneas al archivo de configuración del sitio Web para que Entity Framework sepa cómo conectarse a nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="848f4-134">Haga doble clic en el archivo Web.config ubicado en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="848f4-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="848f4-135">Desplácese hasta el final de este archivo y agregar un &lt;connectionStrings&gt; sección directamente por encima de la última línea, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="848f4-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="848f4-136">Agregar una clase de contexto</span><span class="sxs-lookup"><span data-stu-id="848f4-136">Adding a Context Class</span></span>

<span data-ttu-id="848f4-137">Haga clic en la carpeta de modelos y agregue una nueva clase denominada MusicStoreEntities.cs.</span><span class="sxs-lookup"><span data-stu-id="848f4-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="848f4-138">Esta clase se representan el contexto de base de datos de Entity Framework y se controlen nuestro crear, leer, actualizar y eliminar operaciones para nosotros.</span><span class="sxs-lookup"><span data-stu-id="848f4-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="848f4-139">El código de esta clase se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="848f4-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="848f4-140">¡Eso es todo; no hay ninguna otra configuración, interfaces especiales etcetera. Si se extiende la clase base de DbContext, nuestra clase MusicStoreEntities es capaz de administrar nuestras operaciones de base de datos para que podamos.</span><span class="sxs-lookup"><span data-stu-id="848f4-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="848f4-141">Ahora que tenemos enlazan, vamos a agregar algunas propiedades más a nuestro clases de modelo para aprovechar las ventajas de la parte de la información adicional en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="848f4-142">Agregar los datos de catálogo del almacén</span><span class="sxs-lookup"><span data-stu-id="848f4-142">Adding our store catalog data</span></span>

<span data-ttu-id="848f4-143">Se aprovechará una característica de Entity Framework que agrega datos de "inicialización" a una base de datos recién creada.</span><span class="sxs-lookup"><span data-stu-id="848f4-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="848f4-144">Esto rellenará previamente nuestro catálogo de la tienda con una lista de géneros, intérpretes y álbumes.</span><span class="sxs-lookup"><span data-stu-id="848f4-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="848f4-145">La descarga de MvcMusicStore Assets.zip - que incluye los archivos de diseño de sitio usados anteriormente en este tutorial - tiene un archivo de clase con estos datos de inicialización, ubicados en una carpeta con el nombre de código.</span><span class="sxs-lookup"><span data-stu-id="848f4-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="848f4-146">En el código / carpeta Models, busque el archivo SampleData.cs y colóquelo en la carpeta de modelos en el proyecto, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="848f4-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="848f4-147">Ahora tenemos que agregar una línea de código y explíquenos esa clase SampleData en Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="848f4-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="848f4-148">Haga doble clic en el archivo Global.asax de la raíz del proyecto para abrirlo y agregue la siguiente línea a la parte superior de la aplicación\_Start (método).</span><span class="sxs-lookup"><span data-stu-id="848f4-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="848f4-149">En este punto, hemos completado el trabajo necesario configurar Entity Framework para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="848f4-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="848f4-150">Consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="848f4-150">Querying the Database</span></span>

<span data-ttu-id="848f4-151">Ahora vamos a actualizar nuestra StoreController para que en lugar de usar "ficticio datos" en su lugar, llama a nuestra base de datos para consultar toda su información.</span><span class="sxs-lookup"><span data-stu-id="848f4-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="848f4-152">Comenzaremos declarando un campo en el **StoreController** que hospede una instancia de la clase MusicStoreEntities, denominada storeDB:</span><span class="sxs-lookup"><span data-stu-id="848f4-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="848f4-153">Actualizar el índice de almacén para consultar la base de datos</span><span class="sxs-lookup"><span data-stu-id="848f4-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="848f4-154">La clase MusicStoreEntities se mantiene por Entity Framework y expone una propiedad de colección para cada tabla en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="848f4-155">Vamos a actualizar acción del índice de nuestro StoreController para recuperar todos los juegos en nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="848f4-156">Anteriormente se hizo esto al codificar los datos de cadena.</span><span class="sxs-lookup"><span data-stu-id="848f4-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="848f4-157">Ahora podemos en su lugar, utilizar el contexto de Entity Framework Generes colección:</span><span class="sxs-lookup"><span data-stu-id="848f4-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="848f4-158">Ningún cambio tiene que haber a la plantilla de vista, ya que todavía nos estamos devolver el mismo StoreIndexViewModel se devuelve antes - que sólo se devuelve datos en directo de nuestra base de datos ahora.</span><span class="sxs-lookup"><span data-stu-id="848f4-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="848f4-159">Cuando se vuelva a ejecuta el proyecto y visita la dirección URL "/ Store", se verá ahora una lista de todos los juegos en nuestra base de datos:</span><span class="sxs-lookup"><span data-stu-id="848f4-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="848f4-160">Actualización de almacén de examinar y detalles para utilizar datos en directo</span><span class="sxs-lookup"><span data-stu-id="848f4-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="848f4-161">Con/almacén/examinar? género =*[algunas de género]* método de acción, estamos buscando un género por su nombre.</span><span class="sxs-lookup"><span data-stu-id="848f4-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="848f4-162">Esperamos que solo un resultado, ya que no deberíamos nunca dos entradas para el mismo nombre de género, por lo que podemos usar la. Extensión de Single() en LINQ para consultar el objeto de género adecuado similar al siguiente (no escriba Esto todavía):</span><span class="sxs-lookup"><span data-stu-id="848f4-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="848f4-163">El único método toma una expresión Lambda como un parámetro, que especifica que se desea un único objeto de género tal que su nombre coincide con el valor que hemos definido.</span><span class="sxs-lookup"><span data-stu-id="848f4-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="848f4-164">En el caso anterior, vamos a cargar un único objeto de género con un valor de nombre que coincida con Disco.</span><span class="sxs-lookup"><span data-stu-id="848f4-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="848f4-165">Se podrá aprovechar las ventajas de una característica de Entity Framework que nos permite indicar otras entidades relacionadas que queremos cargar también cuando se recupera el objeto de género.</span><span class="sxs-lookup"><span data-stu-id="848f4-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="848f4-166">Esta característica se denomina forma del resultado de consulta y nos permite reducir el número de veces que se necesita para tener acceso a la base de datos para recuperar todos los datos que necesitamos.</span><span class="sxs-lookup"><span data-stu-id="848f4-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="848f4-167">Es deseable dar una captura previa de los álbumes de género recuperamos, por lo que actualizaremos nuestra consulta para incluir en Genres.Include("Albums") para indicar que deseamos así álbumes relacionados.</span><span class="sxs-lookup"><span data-stu-id="848f4-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="848f4-168">Esto es más eficaz, ya que se recuperarán datos de nuestro género y el álbum en una solicitud de base de datos único.</span><span class="sxs-lookup"><span data-stu-id="848f4-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="848f4-169">Con las explicaciones fuera de la vista, este es nuestro acción de controlador actualizada examinar aspecto:</span><span class="sxs-lookup"><span data-stu-id="848f4-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="848f4-170">Ahora podemos actualizar el almacén de examinar la vista para mostrar los álbumes que están disponibles en cada género.</span><span class="sxs-lookup"><span data-stu-id="848f4-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="848f4-171">Abra la plantilla de vista (se encuentra en /Views/Store/Browse.cshtml) y agregue una lista con viñetas de álbumes, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="848f4-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="848f4-172">Ejecutar la aplicación y vaya a/almacén/examinar? género = muestra Jazz que nuestros resultados ahora son que se extrae de la base de datos, mostrar todos los álbumes en nuestro género seleccionado.</span><span class="sxs-lookup"><span data-stu-id="848f4-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="848f4-173">Nos aseguraremos de hacer los mismos cambie a nuestro/Store/detalles / [id] dirección URL y reemplace nuestros datos ficticios por una consulta de base de datos que carga un álbum cuyo identificador coincide con el valor del parámetro.</span><span class="sxs-lookup"><span data-stu-id="848f4-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="848f4-174">Ejecutar la aplicación y vaya a /Store/Details/1 muestran que nuestros resultados ahora provienen de la base de datos.</span><span class="sxs-lookup"><span data-stu-id="848f4-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="848f4-175">Ahora que nuestra página de detalles del almacén está configurado para mostrar un álbum por el identificador del álbum, vamos a actualizar el **examinar** vista para vincular a la vista de detalles.</span><span class="sxs-lookup"><span data-stu-id="848f4-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="848f4-176">Usaremos Html.ActionLink, exactamente igual que hicimos para crear un vínculo desde el índice de almacén para examinar el almacén al final de la sección anterior.</span><span class="sxs-lookup"><span data-stu-id="848f4-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="848f4-177">El código fuente completo para la vista de exploración aparece debajo.</span><span class="sxs-lookup"><span data-stu-id="848f4-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="848f4-178">Ahora podemos examinar desde nuestra página de almacén a una página de género, que enumera los álbumes disponibles, y haciendo clic en un álbum podemos ver detalles del álbum.</span><span class="sxs-lookup"><span data-stu-id="848f4-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="848f4-179">[Anterior](mvc-music-store-part-3.md)
> [Siguiente](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="848f4-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
