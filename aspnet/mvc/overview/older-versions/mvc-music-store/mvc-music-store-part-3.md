---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: 'Parte 3: Vistas y ViewModels | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 3 aborda las vistas y ViewModels.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 828ff18abcc5932f82be71a45ebde589eeb051fa
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829623"
---
<a name="part-3-views-and-viewmodels"></a><span data-ttu-id="8228f-104">Parte 3: Vistas y ViewModels</span><span class="sxs-lookup"><span data-stu-id="8228f-104">Part 3: Views and ViewModels</span></span>
====================
<span data-ttu-id="8228f-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8228f-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8228f-106">El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="8228f-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="8228f-107">El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="8228f-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="8228f-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8228f-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="8228f-109">Parte 3 aborda las vistas y ViewModels.</span><span class="sxs-lookup"><span data-stu-id="8228f-109">Part 3 covers Views and ViewModels.</span></span>


<span data-ttu-id="8228f-110">Hasta ahora nos hemos simplemente ha devolver cadenas acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="8228f-110">So far we've just been returning strings from controller actions.</span></span> <span data-ttu-id="8228f-111">Que es una buena forma de hacerse una idea de cómo funcionan los controladores, pero es no cómo se desea crear una aplicación web real.</span><span class="sxs-lookup"><span data-stu-id="8228f-111">That's a nice way to get an idea of how controllers work, but it's not how you'd want to build a real web application.</span></span> <span data-ttu-id="8228f-112">Vamos a quiera una manera mejor de generar HTML a exploradores que visitan nuestro sitio: uno donde podemos usar los archivos de plantilla para personalizar más fácilmente el contenido HTML volver a enviar.</span><span class="sxs-lookup"><span data-stu-id="8228f-112">We are going to want a better way to generate HTML back to browsers visiting our site – one where we can use template files to more easily customize the HTML content send back.</span></span> <span data-ttu-id="8228f-113">Eso es exactamente lo que hacer las vistas.</span><span class="sxs-lookup"><span data-stu-id="8228f-113">That's exactly what Views do.</span></span>

## <a name="adding-a-view-template"></a><span data-ttu-id="8228f-114">Adición de una plantilla de vista</span><span class="sxs-lookup"><span data-stu-id="8228f-114">Adding a View template</span></span>

<span data-ttu-id="8228f-115">Para usar una plantilla de vista, se deberá cambiar el método de índice de HomeController para devolver un ActionResult y que devuelva View(), al igual que a continuación:</span><span class="sxs-lookup"><span data-stu-id="8228f-115">To use a view-template, we'll change the HomeController Index method to return an ActionResult, and have it return View(), like below:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

<span data-ttu-id="8228f-116">El cambio anterior indica que, en lugar de devolver una cadena, queremos usar una "vista" para generar un resultado de vuelta en su lugar.</span><span class="sxs-lookup"><span data-stu-id="8228f-116">The above change indicates that instead of returned a string, we instead want to use a "View" to generate a result back.</span></span>

<span data-ttu-id="8228f-117">Ahora vamos a agregar una plantilla de vista adecuada para nuestro proyecto.</span><span class="sxs-lookup"><span data-stu-id="8228f-117">We'll now add an appropriate View template to our project.</span></span> <span data-ttu-id="8228f-118">Para ello se deberá colocar el cursor de texto dentro del método de acción del índice, a continuación, haga clic en y seleccione "Agregar vista".</span><span class="sxs-lookup"><span data-stu-id="8228f-118">To do this we'll position the text cursor within the Index action method, then right-click and select "Add View".</span></span> <span data-ttu-id="8228f-119">Se abrirá el cuadro de diálogo Agregar vista:</span><span class="sxs-lookup"><span data-stu-id="8228f-119">This will bring up the Add View dialog:</span></span>

<span data-ttu-id="8228f-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8228f-120">![](mvc-music-store-part-3/_static/image1.jpg) ![](mvc-music-store-part-3/_static/image1.png)</span></span>

<span data-ttu-id="8228f-121">El cuadro de diálogo "Agregar vista" nos permite rápida y fácilmente generar los archivos de plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-121">The "Add View" dialog allows us to quickly and easily generate View template files.</span></span> <span data-ttu-id="8228f-122">Cuadro de diálogo rellena previamente el nombre de la plantilla de vista para crear para que coincida con el método de acción que va a usar de manera predeterminada en "Agregar vista".</span><span class="sxs-lookup"><span data-stu-id="8228f-122">By default the "Add View" dialog pre-populates the name of the View template to create so that it matches the action method that will use it.</span></span> <span data-ttu-id="8228f-123">Dado que hemos usado el menú contextual "Agregar vista" dentro del método de acción de Index() de nuestro HomeController, el cuadro de diálogo "Agregar vista" anterior tiene "Index" como el nombre de vista que se rellenan de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="8228f-123">Because we used the "Add View" context menu within the Index() action method of our HomeController, the "Add View" dialog above has "Index" as the view name pre-populated by default.</span></span> <span data-ttu-id="8228f-124">No necesitamos cambiar cualquiera de las opciones de este cuadro de diálogo, haga clic en el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="8228f-124">We don't need to change any of the options on this dialog, so click the Add button.</span></span>

<span data-ttu-id="8228f-125">Cuando hacemos clic en el botón Agregar, Visual Web Developer creará un Index.cshtml nueva plantilla de vista para nosotros en el directorio \Views\Home, creación de la carpeta si aún no existe.</span><span class="sxs-lookup"><span data-stu-id="8228f-125">When we click the Add button, Visual Web Developer will create a new Index.cshtml view template for us in the \Views\Home directory, creating the folder if doesn't already exist.</span></span>

![](mvc-music-store-part-3/_static/image2.png)

<span data-ttu-id="8228f-126">El nombre y una ubicación del archivo "Index.cshtml" es importante y sigue las convenciones de nomenclatura predeterminada ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8228f-126">The name and folder location of the "Index.cshtml" file is important, and follows the default ASP.NET MVC naming conventions.</span></span> <span data-ttu-id="8228f-127">El nombre del directorio, \Views\Home, coincide con el controlador - que se denomina HomeController.</span><span class="sxs-lookup"><span data-stu-id="8228f-127">The directory name, \Views\Home, matches the controller - which is named HomeController.</span></span> <span data-ttu-id="8228f-128">El nombre de plantilla de vista Index, coincide con el método de acción de controlador que se va a mostrar la vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-128">The view template name, Index, matches the controller action method which will be displaying the view.</span></span>

<span data-ttu-id="8228f-129">ASP.NET MVC nos permite evitar tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando usamos esta convención de nomenclatura para devolver una vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-129">ASP.NET MVC allows us to avoid having to explicitly specify the name or location of a view template when we use this naming convention to return a view.</span></span> <span data-ttu-id="8228f-130">De forma predeterminada procesará la plantilla de vista \Views\Home\Index.cshtml cuando escribimos código similar al siguiente a continuación dentro de nuestro HomeController:</span><span class="sxs-lookup"><span data-stu-id="8228f-130">It will by default render the \Views\Home\Index.cshtml view template when we write code like below within our HomeController:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

<span data-ttu-id="8228f-131">Visual Web Developer crea y abre la plantilla de vista "Index.cshtml" después de que se hace clic en el botón "Agregar" en el cuadro de diálogo "Agregar vista".</span><span class="sxs-lookup"><span data-stu-id="8228f-131">Visual Web Developer created and opened the "Index.cshtml" view template after we clicked the "Add" button within the "Add View" dialog.</span></span> <span data-ttu-id="8228f-132">A continuación, se muestra el contenido de Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8228f-132">The contents of Index.cshtml are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

<span data-ttu-id="8228f-133">Esta vista es mediante la sintaxis de Razor, que es más concisa que el motor de vistas de formularios Web Forms usado en formularios Web Forms de ASP.NET y las versiones anteriores de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="8228f-133">This view is using the Razor syntax, which is more concise than the Web Forms view engine used in ASP.NET Web Forms and previous versions of ASP.NET MVC.</span></span> <span data-ttu-id="8228f-134">El motor de vistas de formularios Web Forms todavía está disponible en ASP.NET MVC 3, pero muchos desarrolladores consideran que el motor de vistas Razor se ajusta a desarrollo de ASP.NET MVC realmente bien.</span><span class="sxs-lookup"><span data-stu-id="8228f-134">The Web Forms view engine is still available in ASP.NET MVC 3, but many developers find that the Razor view engine fits ASP.NET MVC development really well.</span></span>

<span data-ttu-id="8228f-135">Las tres primeras líneas establecen el título de página con ViewBag.Title.</span><span class="sxs-lookup"><span data-stu-id="8228f-135">The first three lines set the page title using ViewBag.Title.</span></span> <span data-ttu-id="8228f-136">Se examinará cómo funciona esto con más detalle en breve, pero primero vamos a actualizar el texto del título de texto y ver la página.</span><span class="sxs-lookup"><span data-stu-id="8228f-136">We'll look at how this works in more detail soon, but first let's update the text heading text and view the page.</span></span> <span data-ttu-id="8228f-137">Actualización de la &lt;h2&gt; etiqueta a la que quiere decir "Esta es la página principal", tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="8228f-137">Update the &lt;h2&gt; tag to say "This is the Home Page" as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

<span data-ttu-id="8228f-138">Ejecuta la aplicación de muestra que nuestro nuevo texto esté visible en la página principal.</span><span class="sxs-lookup"><span data-stu-id="8228f-138">Running the application shows that our new text is visible on the home page.</span></span>

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a><span data-ttu-id="8228f-139">Uso de un diseño para los elementos comunes del sitio</span><span class="sxs-lookup"><span data-stu-id="8228f-139">Using a Layout for common site elements</span></span>

<span data-ttu-id="8228f-140">La mayoría de los sitios Web tienen contenido que se comparte entre varias páginas: exploración, pies de página, las imágenes de logotipo, referencias de hoja de estilos, etcetera. El motor de vistas Razor facilita esta tarea administrar el uso de una página denominada \_Layout.cshtml que se ha creado automáticamente para que podamos dentro de la carpeta/Views/Shared.</span><span class="sxs-lookup"><span data-stu-id="8228f-140">Most websites have content which is shared between many pages: navigation, footers, logo images, stylesheet references, etc. The Razor view engine makes this easy to manage using a page called \_Layout.cshtml which has automatically been created for us inside the /Views/Shared folder.</span></span>

![](mvc-music-store-part-3/_static/image4.png)

<span data-ttu-id="8228f-141">Haga doble clic en esta carpeta para ver el contenido, que se indican a continuación.</span><span class="sxs-lookup"><span data-stu-id="8228f-141">Double-click on this folder to view the contents, which are shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

<span data-ttu-id="8228f-142">Se mostrará el contenido de nuestras vistas individuales de la @RenderBodycomando () y cualquier contenido común que se va a aparecer fuera de la que se pueden agregar a la \_Layout.cshtml marcado.</span><span class="sxs-lookup"><span data-stu-id="8228f-142">The content from our individual views will be displayed by the @RenderBody() command, and any common content that we want to appear outside of that can be added to the \_Layout.cshtml markup.</span></span> <span data-ttu-id="8228f-143">Desearemos nuestro Store música de MVC para tener un encabezado común con vínculos a nuestra área de página principal y Store de todas las páginas del sitio, por lo que vamos a agregar a la plantilla directamente encima de esa @RenderBodyinstrucción ().</span><span class="sxs-lookup"><span data-stu-id="8228f-143">We'll want our MVC Music Store to have a common header with links to our Home page and Store area on all pages in the site, so we'll add that to the template directly above that @RenderBody() statement.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a><span data-ttu-id="8228f-144">Actualización de la hoja de estilos</span><span class="sxs-lookup"><span data-stu-id="8228f-144">Updating the StyleSheet</span></span>

<span data-ttu-id="8228f-145">La plantilla proyecto vacío incluye un archivo CSS muy simplificado que solo incluye los estilos que se utiliza para mostrar mensajes de validación.</span><span class="sxs-lookup"><span data-stu-id="8228f-145">The empty project template includes a very streamlined CSS file which just includes styles used to display validation messages.</span></span> <span data-ttu-id="8228f-146">El diseñador ha proporcionado algunos adicionales CSS e imágenes para definir la apariencia de nuestro sitio, así que agregaremos los ahora.</span><span class="sxs-lookup"><span data-stu-id="8228f-146">Our designer has provided some additional CSS and images to define the look and feel for our site, so we'll add those in now.</span></span>

<span data-ttu-id="8228f-147">El archivo CSS e imágenes actualizados se incluyen en el directorio de contenido de Assets.zip MvcMusicStore que está disponible en [MVC-música-Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="8228f-147">The updated CSS file and Images are included in the Content directory of MvcMusicStore-Assets.zip which is available at [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span> <span data-ttu-id="8228f-148">Seleccionaremos ambos en el Explorador de Windows y colocarlos en la carpeta de contenido de nuestra solución en Visual Web Developer, tal como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="8228f-148">We'll select both of them in Windows Explorer and drop them into our Solution's Content folder in Visual Web Developer, as shown below:</span></span>

![](mvc-music-store-part-3/_static/image5.png)

<span data-ttu-id="8228f-149">Le pedirá que confirme si desea sobrescribir el archivo Site.css existente.</span><span class="sxs-lookup"><span data-stu-id="8228f-149">You'll be asked to confirm if you want to overwrite the existing Site.css file.</span></span> <span data-ttu-id="8228f-150">Haga clic en Sí.</span><span class="sxs-lookup"><span data-stu-id="8228f-150">Click Yes.</span></span>

![](mvc-music-store-part-3/_static/image6.png)

<span data-ttu-id="8228f-151">La carpeta de contenido de la aplicación aparecerá ahora como sigue:</span><span class="sxs-lookup"><span data-stu-id="8228f-151">The Content folder of your application will now appear as follows:</span></span>

![](mvc-music-store-part-3/_static/image7.png)

<span data-ttu-id="8228f-152">Ahora vamos a ejecutar la aplicación y ver el aspecto de los cambios en la página principal.</span><span class="sxs-lookup"><span data-stu-id="8228f-152">Now let's run the application and see how our changes look on the Home page.</span></span>

![](mvc-music-store-part-3/_static/image8.png)

- <span data-ttu-id="8228f-153">Vamos a revisar lo que ha cambiado: método de acción de HomeController el índice se encuentra y muestra la plantilla \Views\Home\Index.cshtmlView, aunque nuestro código llamado "return View()", porque la plantilla de vista seguido la convención de nomenclatura estándar.</span><span class="sxs-lookup"><span data-stu-id="8228f-153">Let's review what's changed: The HomeController's Index action method found and displayed the \Views\Home\Index.cshtmlView template, even though our code called "return View()", because our View template followed the standard naming convention.</span></span>
- <span data-ttu-id="8228f-154">Un simple mensaje de bienvenida que se define dentro de la plantilla de vista \Views\Home\Index.cshtml muestra la página principal.</span><span class="sxs-lookup"><span data-stu-id="8228f-154">The Home Page is displaying a simple welcome message that is defined within the \Views\Home\Index.cshtml view template.</span></span>
- <span data-ttu-id="8228f-155">Está usando la página principal de nuestra \_Layout.cshtml plantilla, por lo que se encuentra el mensaje de bienvenida del diseño del sitio estándar HTML.</span><span class="sxs-lookup"><span data-stu-id="8228f-155">The Home Page is using our \_Layout.cshtml template, and so the welcome message is contained within the standard site HTML layout.</span></span>

## <a name="using-a-model-to-pass-information-to-our-view"></a><span data-ttu-id="8228f-156">Usar un modelo para pasar información a nuestro punto de vista</span><span class="sxs-lookup"><span data-stu-id="8228f-156">Using a Model to pass information to our View</span></span>

<span data-ttu-id="8228f-157">Una plantilla de vista que muestra HTML codificados no va a hacer que un sitio web muy interesantes.</span><span class="sxs-lookup"><span data-stu-id="8228f-157">A View template that just displays hardcoded HTML isn't going to make a very interesting web site.</span></span> <span data-ttu-id="8228f-158">Para crear un sitio web dinámico, en su lugar, desearemos pasar información de las acciones del controlador a nuestras plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-158">To create a dynamic web site, we'll instead want to pass information from our controller actions to our view templates.</span></span>

<span data-ttu-id="8228f-159">En el patrón Model-View-Controller, el término A que modelo hace referencia objetos que representan los datos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8228f-159">In the Model-View-Controller pattern, the term Model refers to objects which represent the data in the application.</span></span> <span data-ttu-id="8228f-160">A menudo, los objetos del modelo se corresponden con las tablas de la base de datos, pero no tienen que.</span><span class="sxs-lookup"><span data-stu-id="8228f-160">Often, model objects correspond to tables in your database, but they don't have to.</span></span>

<span data-ttu-id="8228f-161">Los métodos de acción de controlador que devuelven un ActionResult pueden pasar un objeto de modelo a la vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-161">Controller action methods which return an ActionResult can pass a model object to the view.</span></span> <span data-ttu-id="8228f-162">Esto permite que un controlador para el paquete correctamente toda la información necesaria para generar una respuesta y, a continuación, pase la esta información para una plantilla de vista a usar para generar la respuesta adecuada de HTML.</span><span class="sxs-lookup"><span data-stu-id="8228f-162">This allows a Controller to cleanly package up all the information needed to generate a response, and then pass this information off to a View template to use to generate the appropriate HTML response.</span></span> <span data-ttu-id="8228f-163">Esto es más fácil comprender por verlo en acción, así que vamos a empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="8228f-163">This is easiest to understand by seeing it in action, so let's get started.</span></span>

<span data-ttu-id="8228f-164">En primer lugar, vamos a crear algunas clases de modelo para representar los géneros y álbumes dentro de nuestra tienda.</span><span class="sxs-lookup"><span data-stu-id="8228f-164">First we'll create some Model classes to represent Genres and Albums within our store.</span></span> <span data-ttu-id="8228f-165">Comencemos por crear una clase de género.</span><span class="sxs-lookup"><span data-stu-id="8228f-165">Let's start by creating a Genre class.</span></span> <span data-ttu-id="8228f-166">Haga clic en la carpeta "Models" dentro del proyecto, elija la opción "Agregar clase" y "Genre.cs" el nombre del archivo.</span><span class="sxs-lookup"><span data-stu-id="8228f-166">Right-click the "Models" folder within your project, choose the "Add Class" option, and name the file "Genre.cs".</span></span>

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

<span data-ttu-id="8228f-167">A continuación, agregue una propiedad de nombre de cadena pública a la clase que se creó:</span><span class="sxs-lookup"><span data-stu-id="8228f-167">Then add a public string Name property to the class that was created:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

<span data-ttu-id="8228f-168">*Nota: en caso lo está preguntando, el {get; configurar;} notación está haciendo uso de la característica propiedades autoimplementadas de C#. Esto nos proporciona las ventajas de una propiedad sin necesidad de nosotros declarar un campo de respaldo.*</span><span class="sxs-lookup"><span data-stu-id="8228f-168">*Note: In case you're wondering, the { get; set; } notation is making use of C#'s auto-implemented properties feature. This gives us the benefits of a property without requiring us to declare a backing field.*</span></span>

<span data-ttu-id="8228f-169">A continuación, siga los mismos pasos para crear una clase de álbum (denominada Album.cs) que tiene un título y una propiedad de género:</span><span class="sxs-lookup"><span data-stu-id="8228f-169">Next, follow the same steps to create an Album class (named Album.cs) that has a Title and a Genre property:</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

<span data-ttu-id="8228f-170">Ahora podemos modificar el StoreController para usar las vistas que muestran información dinámica de nuestro modelo.</span><span class="sxs-lookup"><span data-stu-id="8228f-170">Now we can modify the StoreController to use Views which display dynamic information from our Model.</span></span> <span data-ttu-id="8228f-171">Si - para fines de demostración - ahora hemos llamado a nuestro álbumes según el identificador de solicitud, podríamos mostramos esa información como se muestra en la vista siguiente.</span><span class="sxs-lookup"><span data-stu-id="8228f-171">If - for demonstration purposes right now - we named our Albums based on the request ID, we could display that information as in the view below.</span></span>

![](mvc-music-store-part-3/_static/image10.png)

<span data-ttu-id="8228f-172">Comenzaremos por cambiar la acción de Store detalles para que muestre la información de un álbum.</span><span class="sxs-lookup"><span data-stu-id="8228f-172">We'll start by changing the Store Details action so it shows the information for a single album.</span></span> <span data-ttu-id="8228f-173">Agregue una instrucción "using" en la parte superior de la **StoreControllers** clase para que incluya el espacio de nombres MvcMusicStore.Models, por lo que no necesitamos escribir MvcMusicStore.Models.Album cada vez que deseamos utilizar la clase del álbum.</span><span class="sxs-lookup"><span data-stu-id="8228f-173">Add a "using" statement to the top of the **StoreControllers** class to include the MvcMusicStore.Models namespace, so we don't need to type MvcMusicStore.Models.Album every time we want to use the album class.</span></span> <span data-ttu-id="8228f-174">La sección "using" de esa clase debería aparecer ahora como sigue.</span><span class="sxs-lookup"><span data-stu-id="8228f-174">The "usings" section of that class should now appear as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

<span data-ttu-id="8228f-175">A continuación, actualizaremos la acción de controlador de detalles para que devuelva un ActionResult en lugar de una cadena, como hicimos con el método de índice de HomeController.</span><span class="sxs-lookup"><span data-stu-id="8228f-175">Next, we'll update the Details controller action so that it returns an ActionResult rather than a string, as we did with the HomeController's Index method.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

<span data-ttu-id="8228f-176">Ahora podemos modificar la lógica para devolver un objeto de álbum a la vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-176">Now we can modify the logic to return an Album object to the view.</span></span> <span data-ttu-id="8228f-177">Más adelante en este tutorial se recuperará los datos de una base de datos, pero por ahora vamos a utilizar "ficticio datos" para empezar a trabajar.</span><span class="sxs-lookup"><span data-stu-id="8228f-177">Later in this tutorial we will be retrieving the data from a database – but for right now we will use "dummy data" to get started.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

<span data-ttu-id="8228f-178">*Nota: Si no está familiarizado con C#, se puede suponer que usar var significa que nuestra variable álbum es en tiempo de ejecución. Esto no es correcto: el compilador de C# usa la inferencia de tipos en función de lo que estamos asignar a la variable para determinar el que álbum es de tipo álbum y compilar la variable local álbum como tipo de álbum, por lo que obtenemos la comprobación en tiempo de compilación y el editor de código de Visual Studio soporte técnico.*</span><span class="sxs-lookup"><span data-stu-id="8228f-178">*Note: If you're unfamiliar with C#, you may assume that using var means that our album variable is late-bound. That's not correct – the C# compiler is using type-inference based on what we're assigning to the variable to determine that album is of type Album and compiling the local album variable as an Album type, so we get compile-time checking and Visual Studio code-editor support.*</span></span>

<span data-ttu-id="8228f-179">Ahora creemos una plantilla de vista que usa nuestro álbum para generar una respuesta HTML.</span><span class="sxs-lookup"><span data-stu-id="8228f-179">Let's now create a View template that uses our Album to generate an HTML response.</span></span> <span data-ttu-id="8228f-180">Antes de hacerlo es necesario compilar el proyecto para que el cuadro de diálogo Agregar vista sepa sobre nuestra clase álbum recién creado.</span><span class="sxs-lookup"><span data-stu-id="8228f-180">Before we do that we need to build the project so that the Add View dialog knows about our newly created Album class.</span></span> <span data-ttu-id="8228f-181">Puede compilar el proyecto seleccionando el Debug⇨Build MvcMusicStore elemento de menú (para obtener información adicional, puede utilizar el método abreviado Ctrl-Mayús-B para compilar el proyecto).</span><span class="sxs-lookup"><span data-stu-id="8228f-181">You can build the project by selecting the Debug⇨Build MvcMusicStore menu item (for extra credit, you can use the Ctrl-Shift-B shortcut to build the project).</span></span>

![](mvc-music-store-part-3/_static/image11.png)

<span data-ttu-id="8228f-182">Ahora que hemos configurado nuestra clases auxiliares, estamos listos para crear la plantilla de vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-182">Now that we've set up our supporting classes, we're ready to build our View template.</span></span> <span data-ttu-id="8228f-183">Haga doble clic dentro del método de detalles y seleccione "Agregar vista..." en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="8228f-183">Right-click within the Details method and select "Add View…" from the context menu.</span></span>

![](mvc-music-store-part-3/_static/image12.png)

<span data-ttu-id="8228f-184">Vamos a crear una nueva plantilla de vista como hicimos antes con HomeController.</span><span class="sxs-lookup"><span data-stu-id="8228f-184">We are going to create a new View template like we did before with the HomeController.</span></span> <span data-ttu-id="8228f-185">Dado que vamos a crear desde el StoreController de forma predeterminada se generará en un archivo \Views\Store\Index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="8228f-185">Because we are creating it from the StoreController it will by default be generated in a \Views\Store\Index.cshtml file.</span></span>

<span data-ttu-id="8228f-186">A diferencia de antes, vamos a activar la casilla de la vista "Crear fuertemente tipada".</span><span class="sxs-lookup"><span data-stu-id="8228f-186">Unlike before, we are going to check the "Create a strongly-typed" view checkbox.</span></span> <span data-ttu-id="8228f-187">A continuación, vamos a seleccionar de nuestra clase "Álbum" dentro de la lista-downlist "Ver datos-class".</span><span class="sxs-lookup"><span data-stu-id="8228f-187">We are then going to select our "Album" class within the "View data-class" drop-downlist.</span></span> <span data-ttu-id="8228f-188">Esto hará que el cuadro de diálogo "Agregar vista" crear una plantilla de vista que se espera que un álbum de objeto se pasa al que se va a usar.</span><span class="sxs-lookup"><span data-stu-id="8228f-188">This will cause the "Add View" dialog to create a View template that expects that an Album object will be passed to it to use.</span></span>

![](mvc-music-store-part-3/_static/image13.png)

<span data-ttu-id="8228f-189">Cuando hacemos clic en el botón "Agregar" se creará la plantilla de vista \Views\Store\Details.cshtml, que contiene el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="8228f-189">When we click the "Add" button our \Views\Store\Details.cshtml View template will be created, containing the following code.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

<span data-ttu-id="8228f-190">Tenga en cuenta la primera línea, que indica que esta vista está fuertemente tipado en nuestra clase álbum.</span><span class="sxs-lookup"><span data-stu-id="8228f-190">Notice the first line, which indicates that this view is strongly-typed to our Album class.</span></span> <span data-ttu-id="8228f-191">El motor de vistas Razor entiende que se ha pasado un objeto de álbum, para que podamos acceder fácilmente a las propiedades del modelo y aún tiene la ventaja de IntelliSense en el editor de Visual Web Developer.</span><span class="sxs-lookup"><span data-stu-id="8228f-191">The Razor view engine understands that it has been passed an Album object, so we can easily access model properties and even have the benefit of IntelliSense in the Visual Web Developer editor.</span></span>

<span data-ttu-id="8228f-192">Actualización de la &lt;h2&gt; etiquetar para que se muestre la propiedad de título del álbum mediante la modificación de esa línea debe aparecer como sigue.</span><span class="sxs-lookup"><span data-stu-id="8228f-192">Update the &lt;h2&gt; tag so it displays the Album's Title property by modifying that line to appear as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

<span data-ttu-id="8228f-193">Tenga en cuenta que IntelliSense se activará cuando se escribe el período tras el @Model palabra clave, que muestra las propiedades y métodos que admite la clase del álbum.</span><span class="sxs-lookup"><span data-stu-id="8228f-193">Notice that IntelliSense is triggered when you enter the period after the @Model keyword, showing the properties and methods that the Album class supports.</span></span>

<span data-ttu-id="8228f-194">Ahora vamos a volver a ejecutar nuestro proyecto y visite la dirección URL de Store/detalles/5.</span><span class="sxs-lookup"><span data-stu-id="8228f-194">Let's now re-run our project and visit the /Store/Details/5 URL.</span></span> <span data-ttu-id="8228f-195">Veremos los detalles de un álbum, como a continuación.</span><span class="sxs-lookup"><span data-stu-id="8228f-195">We'll see details of an Album like below.</span></span>

![](mvc-music-store-part-3/_static/image14.png)

<span data-ttu-id="8228f-196">Ahora nos aseguraremos de hacer una actualización similar para el método de acción examinar Store.</span><span class="sxs-lookup"><span data-stu-id="8228f-196">Now we'll make a similar update for the Store Browse action method.</span></span> <span data-ttu-id="8228f-197">Actualice el método para que devuelva un ActionResult y modificar la lógica del método, por lo que crea un nuevo objeto de género y lo devuelve a la vista.</span><span class="sxs-lookup"><span data-stu-id="8228f-197">Update the method so it returns an ActionResult, and modify the method logic so it creates a new Genre object and returns it to the View.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

<span data-ttu-id="8228f-198">Haga clic en el método de exploración y seleccione "Agregar vista..." en el menú contextual, a continuación, agregará una vista que está fuertemente tipada agregue fuertemente tipados a la clase de género.</span><span class="sxs-lookup"><span data-stu-id="8228f-198">Right-click in the Browse method and select "Add View…" from the context menu, then add a View that is strongly-typed add a strongly typed to the Genre class.</span></span>

![](mvc-music-store-part-3/_static/image15.png)

<span data-ttu-id="8228f-199">Actualización de la &lt;h2&gt; elemento en la vista de código (en /Views/Store/Browse.cshtml) para mostrar la información de género.</span><span class="sxs-lookup"><span data-stu-id="8228f-199">Update the &lt;h2&gt; element in the view code (in /Views/Store/Browse.cshtml) to display the Genre information.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

<span data-ttu-id="8228f-200">¿Ahora vamos a volver a ejecutar nuestro proyecto y vaya a/Store/examinar? Género = la dirección URL del Disco.</span><span class="sxs-lookup"><span data-stu-id="8228f-200">Now let's re-run our project and browse to the /Store/Browse?Genre=Disco URL.</span></span> <span data-ttu-id="8228f-201">Veremos la página Examinar muestra como a continuación.</span><span class="sxs-lookup"><span data-stu-id="8228f-201">We'll see the Browse page displayed like below.</span></span>

![](mvc-music-store-part-3/_static/image16.png)

<span data-ttu-id="8228f-202">Por último, vamos a hacer una actualización de un poco más compleja la **Store índice** método de acción y vista para mostrar una lista de todos los géneros en nuestra tienda.</span><span class="sxs-lookup"><span data-stu-id="8228f-202">Finally, let's make a slightly more complex update to the **Store Index** action method and view to display a list of all the Genres in our store.</span></span> <span data-ttu-id="8228f-203">Haremos esto mediante el uso de una lista de géneros como el objeto del modelo, en lugar de simplemente un género único.</span><span class="sxs-lookup"><span data-stu-id="8228f-203">We'll do that by using a List of Genres as our model object, rather than just a single Genre.</span></span>

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

<span data-ttu-id="8228f-204">Haga clic en el método de acción del índice de Store y seleccione Agregar vista como antes, seleccione el género como la clase del modelo y presione el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="8228f-204">Right-click in the Store Index action method and select Add View as before, select Genre as the Model class, and press the Add button.</span></span>

![](mvc-music-store-part-3/_static/image17.png)

<span data-ttu-id="8228f-205">Primero, cambiaremos el @model declaración para indicar que la vista esperen género varios objetos en lugar de solo uno.</span><span class="sxs-lookup"><span data-stu-id="8228f-205">First we'll change the @model declaration to indicate that the view will be expecting several Genre objects rather than just one.</span></span> <span data-ttu-id="8228f-206">Cambio de la primera línea del /Store/Index.cshtml para que quede como sigue:</span><span class="sxs-lookup"><span data-stu-id="8228f-206">Change the first line of /Store/Index.cshtml to read as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

<span data-ttu-id="8228f-207">Esto indica que el motor de vistas de Razor que va a trabajar con un objeto de modelo que puede contener varios objetos de género.</span><span class="sxs-lookup"><span data-stu-id="8228f-207">This tells the Razor view engine that it will be working with a model object that can hold several Genre objects.</span></span> <span data-ttu-id="8228f-208">Estamos usando un objeto IEnumerable&lt;género&gt; en lugar de una lista&lt;género&gt; , ya que es más genérico, lo que nos permite cambiar más adelante nuestro tipo de modelo para cualquier tipo de objeto que admite la interfaz IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="8228f-208">We're using an IEnumerable&lt;Genre&gt; rather than a List&lt;Genre&gt; since it's more generic, allowing us to change our model type later to any object type that supports the IEnumerable interface.</span></span>

<span data-ttu-id="8228f-209">A continuación, podrá recorrer los objetos de género en el modelo tal como se muestra en el siguiente código de la vista completa.</span><span class="sxs-lookup"><span data-stu-id="8228f-209">Next, we'll loop through the Genre objects in the model as shown in the completed view code below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

<span data-ttu-id="8228f-210">Observe que tenemos compatibilidad completa con IntelliSense tal como se escriba este código, por lo que cuando se escribe "@Model."</span><span class="sxs-lookup"><span data-stu-id="8228f-210">Notice that we have full IntelliSense support as we enter this code, so that when we type "@Model."</span></span> <span data-ttu-id="8228f-211">vemos que todos los métodos y las propiedades que admite un IEnumerable de tipo género.</span><span class="sxs-lookup"><span data-stu-id="8228f-211">we see all methods and properties supported by an IEnumerable of type Genre.</span></span>

![](mvc-music-store-part-3/_static/image18.png)

<span data-ttu-id="8228f-212">Dentro de nuestro bucle "foreach", Visual Web Developer sepa que cada elemento es de tipo género, por lo que vemos IntelliSense para cada tipo género.</span><span class="sxs-lookup"><span data-stu-id="8228f-212">Within our "foreach" loop, Visual Web Developer knows that each item is of type Genre, so we see IntelliSense for each the Genre type.</span></span>

![](mvc-music-store-part-3/_static/image19.png)

<span data-ttu-id="8228f-213">A continuación, la característica de scaffolding examina el objeto de género y determina que cada uno tendrá una propiedad de nombre, por lo que recorre en bucle y escribe. También genera vínculos de edición, detalles y eliminación a cada elemento individual.</span><span class="sxs-lookup"><span data-stu-id="8228f-213">Next, the scaffolding feature examined the Genre object and determined that each will have a Name property, so it loops through and writes them out. It also generates Edit, Details, and Delete links to each individual item.</span></span> <span data-ttu-id="8228f-214">Deberá aprovechamos más adelante en el administrador del almacén, pero por ahora nos gustaría tener una lista simple en su lugar.</span><span class="sxs-lookup"><span data-stu-id="8228f-214">We'll take advantage of that later in our store manager, but for now we'd like to have a simple list instead.</span></span>

<span data-ttu-id="8228f-215">Cuando se ejecute la aplicación y vaya a/Store, vemos que se muestra el recuento y la lista de géneros.</span><span class="sxs-lookup"><span data-stu-id="8228f-215">When we run the application and browse to /Store, we see that both the count and list of Genres is displayed.</span></span>

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a><span data-ttu-id="8228f-216">Agregar vínculos entre páginas</span><span class="sxs-lookup"><span data-stu-id="8228f-216">Adding Links between pages</span></span>

<span data-ttu-id="8228f-217">La dirección URL/Store que enumera los géneros actualmente enumera los nombres de género simplemente como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="8228f-217">Our /Store URL that lists Genres currently lists the Genre names simply as plain text.</span></span> <span data-ttu-id="8228f-218">Vamos a cambiar esto por lo que instead of plain text en su lugar, tenemos el vínculo de los nombres de género a la dirección URL de exploración/Store apropiada, por lo que al hacer clic en un género musical, como "Disco" se le remitirá a/Store/examinar? genre = dirección URL del Disco.</span><span class="sxs-lookup"><span data-stu-id="8228f-218">Let's change this so that instead of plain text we instead have the Genre names link to the appropriate /Store/Browse URL, so that clicking on a music genre like "Disco" will navigate to the /Store/Browse?genre=Disco URL.</span></span> <span data-ttu-id="8228f-219">Podríamos actualizar nuestra plantilla de vista \Views\Store\Index.cshtml al resultado de estos vínculos mediante código como a continuación **(no escriba lo siguiente en: vamos a mejorarlo)**:</span><span class="sxs-lookup"><span data-stu-id="8228f-219">We could update our \Views\Store\Index.cshtml View template to output these links using code like below **(don't type this in - we're going to improve on it)**:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

<span data-ttu-id="8228f-220">Eso funciona, pero que podría dar lugar a problemas más adelante, ya que depende de una cadena estática.</span><span class="sxs-lookup"><span data-stu-id="8228f-220">That works, but it could lead to trouble later since it relies on a hardcoded string.</span></span> <span data-ttu-id="8228f-221">Por ejemplo, si quisiéramos cambiar el nombre del controlador, es necesario buscar a través de nuestro código busca vínculos que deben actualizarse.</span><span class="sxs-lookup"><span data-stu-id="8228f-221">For instance, if we wanted to rename the Controller, we'd need to search through our code looking for links that need to be updated.</span></span>

<span data-ttu-id="8228f-222">Es un enfoque alternativo, que podemos usar para aprovechar las ventajas de un método de aplicación auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="8228f-222">An alternative approach we can use is to take advantage of an HTML Helper method.</span></span> <span data-ttu-id="8228f-223">ASP.NET MVC incluye métodos de aplicación auxiliar HTML que están disponibles desde nuestro código de plantilla de vista para realizar una serie de tareas comunes de esta manera.</span><span class="sxs-lookup"><span data-stu-id="8228f-223">ASP.NET MVC includes HTML Helper methods which are available from our View template code to perform a variety of common tasks just like this.</span></span> <span data-ttu-id="8228f-224">El método auxiliar Html.ActionLink() es especialmente útil y facilita la tarea de generar HTML &lt;un&gt; vincula y se encarga de molestos detalles como asegurándose de que las rutas de acceso de dirección URL están correctamente codificado como URL.</span><span class="sxs-lookup"><span data-stu-id="8228f-224">The Html.ActionLink() helper method is a particularly useful one, and makes it easy to build HTML &lt;a&gt; links and takes care of annoying details like making sure URL paths are properly URL encoded.</span></span>

<span data-ttu-id="8228f-225">Html.ActionLink() tiene varias sobrecargas diferentes para permitir la especificación de toda la información que necesita para sus vínculos.</span><span class="sxs-lookup"><span data-stu-id="8228f-225">Html.ActionLink() has several different overloads to allow specifying as much information as you need for your links.</span></span> <span data-ttu-id="8228f-226">En el caso más simple, deberá proporcionar el texto del vínculo y el método de acción al que desplazarse cuando se hace clic en el hipervínculo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="8228f-226">In the simplest case, you'll supply just the link text and the Action method to go to when the hyperlink is clicked on the client.</span></span> <span data-ttu-id="8228f-227">Por ejemplo, podemos vinculamos a "/ Store /" Index() método en la página de detalles de Store con el texto del vínculo "Ir a la Store Index" mediante la siguiente llamada:</span><span class="sxs-lookup"><span data-stu-id="8228f-227">For example, we can link to "/Store/" Index() method on the Store Details page with the link text "Go to the Store Index" using the following call:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

<span data-ttu-id="8228f-228">*Nota: en este caso, no necesitamos especificar el nombre del controlador porque nos estamos simplemente vincula a otra acción dentro del mismo controlador que está representando la vista actual.*</span><span class="sxs-lookup"><span data-stu-id="8228f-228">*Note: In this case, we didn't need to specify the controller name because we're just linking to another action within the same controller that's rendering the current view.*</span></span>

<span data-ttu-id="8228f-229">Los vínculos a la página Examinar deberá pasar un parámetro, sin embargo, por lo que vamos a usar otra sobrecarga del método Html.ActionLink toma tres parámetros:</span><span class="sxs-lookup"><span data-stu-id="8228f-229">Our links to the Browse page will need to pass a parameter, though, so we'll use another overload of the Html.ActionLink method that takes three parameters:</span></span>

- 1. <span data-ttu-id="8228f-230">Texto del vínculo, que mostrará el nombre de género</span><span class="sxs-lookup"><span data-stu-id="8228f-230">Link text, which will display the Genre name</span></span>
- 2. <span data-ttu-id="8228f-231">Nombre de acción de controlador (Examinar)</span><span class="sxs-lookup"><span data-stu-id="8228f-231">Controller action name (Browse)</span></span>
- 3. <span data-ttu-id="8228f-232">Valores de parámetro de ruta, especificando el nombre (género) y el valor (nombre de género)</span><span class="sxs-lookup"><span data-stu-id="8228f-232">Route parameter values, specifying both the name (Genre) and the value (Genre name)</span></span>

<span data-ttu-id="8228f-233">Poner todo junto, que le presentamos cómo vamos a escribir esos vínculos a la vista de índice Store:</span><span class="sxs-lookup"><span data-stu-id="8228f-233">Putting that all together, here's how we'll write those links to the Store Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

<span data-ttu-id="8228f-234">Ahora, cuando se vuelva a ejecuta nuestro proyecto y tener acceso a la dirección URL de /Store/ veremos un listado con géneros.</span><span class="sxs-lookup"><span data-stu-id="8228f-234">Now when we run our project again and access the /Store/ URL we will see a list of genres.</span></span> <span data-ttu-id="8228f-235">Cada género es un hipervínculo: cuando hace clic en nos llevará a nuestro/Store/examinar? genre =*[género]* dirección URL.</span><span class="sxs-lookup"><span data-stu-id="8228f-235">Each genre is a hyperlink – when clicked it will take us to our /Store/Browse?genre=*[genre]* URL.</span></span>

![](mvc-music-store-part-3/_static/image3.jpg)

<span data-ttu-id="8228f-236">El código HTML de la lista de género tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="8228f-236">The HTML for the genre list looks like this:</span></span>

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> <span data-ttu-id="8228f-237">[Anterior](mvc-music-store-part-2.md)
> [Siguiente](mvc-music-store-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="8228f-237">[Previous](mvc-music-store-part-2.md)
[Next](mvc-music-store-part-4.md)</span></span>
