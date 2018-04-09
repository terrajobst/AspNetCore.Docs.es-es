---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 2 trata los controladores.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-2-controllers"></a><span data-ttu-id="a02c7-104">Parte 2: controladores</span><span class="sxs-lookup"><span data-stu-id="a02c7-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="a02c7-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="a02c7-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="a02c7-106">La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="a02c7-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="a02c7-107">La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="a02c7-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="a02c7-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a02c7-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="a02c7-109">Parte 2 trata los controladores.</span><span class="sxs-lookup"><span data-stu-id="a02c7-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="a02c7-110">Con marcos web tradicionales, las direcciones URL entrantes suelen asignarse a los archivos de disco.</span><span class="sxs-lookup"><span data-stu-id="a02c7-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="a02c7-111">Por ejemplo: una solicitud para una dirección URL, como "/ Products.aspx" o "/ Products.php" podría ser procesados por un archivo "Products.aspx" o "Products.php".</span><span class="sxs-lookup"><span data-stu-id="a02c7-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="a02c7-112">Marcos de trabajo basado en Web de MVC asignan las direcciones URL al código del servidor de forma ligeramente diferente.</span><span class="sxs-lookup"><span data-stu-id="a02c7-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="a02c7-113">En lugar de asignar direcciones URL entrantes a los archivos, se asignan las direcciones URL en su lugar a métodos en clases.</span><span class="sxs-lookup"><span data-stu-id="a02c7-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="a02c7-114">Estas clases se denominan "Controladores" y son responsables de procesar las solicitudes HTTP entrantes, control proporcionados por el usuario, recuperar y guardar los datos y determinar la respuesta para enviar al cliente (Mostrar HTML, descargar un archivo, redirija a otra Dirección URL, etcetera).</span><span class="sxs-lookup"><span data-stu-id="a02c7-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="a02c7-115">Agregar un HomeController</span><span class="sxs-lookup"><span data-stu-id="a02c7-115">Adding a HomeController</span></span>

<span data-ttu-id="a02c7-116">Comenzaremos nuestra aplicación de tienda de música de MVC agregando una clase de controlador que controlará las direcciones URL a la página principal de nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="a02c7-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="a02c7-117">Comenzaremos siguen las convenciones de nomenclatura predeterminado de ASP.NET MVC y llámelo HomeController.</span><span class="sxs-lookup"><span data-stu-id="a02c7-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="a02c7-118">Haga clic en la carpeta "Controladores" en el Explorador de soluciones y seleccione "Agregar" y, a continuación, el "controlador..."</span><span class="sxs-lookup"><span data-stu-id="a02c7-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…"</span></span> <span data-ttu-id="a02c7-119">comando:</span><span class="sxs-lookup"><span data-stu-id="a02c7-119">command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="a02c7-120">Se abrirá el cuadro de diálogo "Agregar controlador".</span><span class="sxs-lookup"><span data-stu-id="a02c7-120">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="a02c7-121">Asigne al controlador "HomeController" y presione el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="a02c7-121">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="a02c7-122">Esto creará un nuevo archivo, HomeController.cs, con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="a02c7-122">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="a02c7-123">Para iniciar tan simple como sea posible, vamos a sustituir el método de índice con un método simple que solo devuelva una cadena.</span><span class="sxs-lookup"><span data-stu-id="a02c7-123">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="a02c7-124">Nos aseguraremos de hacer dos cambios:</span><span class="sxs-lookup"><span data-stu-id="a02c7-124">We'll make two changes:</span></span>

- <span data-ttu-id="a02c7-125">Cambiar el método para devolver una cadena en lugar de un ActionResult</span><span class="sxs-lookup"><span data-stu-id="a02c7-125">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="a02c7-126">Cambie la instrucción return para devolver "Hello de inicio"</span><span class="sxs-lookup"><span data-stu-id="a02c7-126">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="a02c7-127">El método debe tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="a02c7-127">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="a02c7-128">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="a02c7-128">Running the Application</span></span>

<span data-ttu-id="a02c7-129">Ahora vamos a ejecutar el sitio.</span><span class="sxs-lookup"><span data-stu-id="a02c7-129">Now let's run the site.</span></span> <span data-ttu-id="a02c7-130">Podemos iniciar el servidor web y probar el sitio mediante cualquiera de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="a02c7-130">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="a02c7-131">Elija el elemento de menú Iniciar depuración de depuración ⇨</span><span class="sxs-lookup"><span data-stu-id="a02c7-131">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="a02c7-132">Haga clic en el botón de flecha verde en la barra de herramientas ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="a02c7-132">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="a02c7-133">Utilice el método abreviado de teclado F5.</span><span class="sxs-lookup"><span data-stu-id="a02c7-133">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="a02c7-134">Mediante cualquiera de los pasos anteriores se compile el proyecto y, a continuación, hacer que el servidor de desarrollo de ASP.NET que está integrada en Visual Web Developer para iniciar.</span><span class="sxs-lookup"><span data-stu-id="a02c7-134">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="a02c7-135">Una notificación aparecerá en la esquina inferior de la pantalla para indicar que el servidor de desarrollo de ASP.NET se ha iniciado y se mostrará al número de puerto que se está ejecutando bajo.</span><span class="sxs-lookup"><span data-stu-id="a02c7-135">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="a02c7-136">Visual Web Developer, a continuación, se abrirá una ventana del explorador cuya dirección URL señala a nuestro servidor web automáticamente.</span><span class="sxs-lookup"><span data-stu-id="a02c7-136">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="a02c7-137">Esto nos permitirá probar rápidamente la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="a02c7-137">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="a02c7-138">Bien, que era bastante rápida: hemos creado un nuevo sitio Web, agrega una función de tres líneas, y tenemos texto en un explorador.</span><span class="sxs-lookup"><span data-stu-id="a02c7-138">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="a02c7-139">No rocket ciencia, pero es un tutorial de inicio.</span><span class="sxs-lookup"><span data-stu-id="a02c7-139">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="a02c7-140">*Nota: Visual Web Developer incluye el servidor de desarrollo de ASP.NET, que se ejecutará el sitio Web en un número aleatorio libre "puerto". En la captura de pantalla anterior, el sitio se ejecuta en `http://localhost:26641/`, por lo que está utilizando el puerto 26641. El número de puerto será diferente. Cuando hablamos /Store/Browse like de la dirección URL en este tutorial, que pasa después del número de puerto. Suponiendo que un número de puerto de 26641, vaya a/almacén/examinar significará examinan `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="a02c7-140">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="a02c7-141">Agregar un StoreController</span><span class="sxs-lookup"><span data-stu-id="a02c7-141">Adding a StoreController</span></span>

<span data-ttu-id="a02c7-142">Hemos agregado un HomeController simple que implementa la página principal de nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="a02c7-142">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="a02c7-143">Ahora agreguemos otro controlador que vamos a usar para implementar la funcionalidad de exploración de la tienda de música.</span><span class="sxs-lookup"><span data-stu-id="a02c7-143">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="a02c7-144">El controlador de almacén será compatible con tres escenarios:</span><span class="sxs-lookup"><span data-stu-id="a02c7-144">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="a02c7-145">Una página de lista de los géneros de música en nuestra tienda de música</span><span class="sxs-lookup"><span data-stu-id="a02c7-145">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="a02c7-146">Una página de búsqueda que enumera todos los álbumes de música de un género determinado</span><span class="sxs-lookup"><span data-stu-id="a02c7-146">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="a02c7-147">Una página de detalles que muestra información sobre un álbum de música específico</span><span class="sxs-lookup"><span data-stu-id="a02c7-147">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="a02c7-148">Comenzaremos agregando una nueva clase StoreController...</span><span class="sxs-lookup"><span data-stu-id="a02c7-148">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="a02c7-149">Si no lo ha hecho ya, detener la ejecución de la aplicación por cerrar el explorador o seleccionar el elemento de menú Detener depuración ⇨ de depuración.</span><span class="sxs-lookup"><span data-stu-id="a02c7-149">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="a02c7-150">Ahora, agregue una nueva StoreController.</span><span class="sxs-lookup"><span data-stu-id="a02c7-150">Now add a new StoreController.</span></span> <span data-ttu-id="a02c7-151">Al igual que hicimos con HomeController, deberá hacer esto, haga doble clic en la carpeta "Controladores" en el Explorador de soluciones y elija Agregar -&gt;elemento de menú de controlador</span><span class="sxs-lookup"><span data-stu-id="a02c7-151">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="a02c7-152">Nuestro StoreController nuevo ya tiene un método de "Index".</span><span class="sxs-lookup"><span data-stu-id="a02c7-152">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="a02c7-153">Vamos a usar este método "Índice" implementar nuestra página de lista que enumera todos los juegos en nuestra tienda de música.</span><span class="sxs-lookup"><span data-stu-id="a02c7-153">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="a02c7-154">Agregaremos también dos métodos adicionales para implementar otros de los dos escenarios que deseamos que nuestra StoreController para controlar: Examinar y detalles.</span><span class="sxs-lookup"><span data-stu-id="a02c7-154">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="a02c7-155">Se llaman a estos métodos (índices, examinar y detalles) dentro de nuestro controlador "Acciones del controlador" y, como ya ha visto con el método de acción de HomeController.Index (), su trabajo es responder a las solicitudes de dirección URL y determinar (por lo general) el contenido se debe enviar en el explorador o el usuario que invocó la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a02c7-155">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="a02c7-156">Comenzaremos nuestra implementación StoreController cambiando el método theIndex() para devolver la cadena "Hello de Store.Index()" y vamos a agregar métodos similares para Browse() y Details():</span><span class="sxs-lookup"><span data-stu-id="a02c7-156">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="a02c7-157">Vuelva a ejecutar el proyecto y examinar las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="a02c7-157">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="a02c7-158">/ Almacén</span><span class="sxs-lookup"><span data-stu-id="a02c7-158">/Store</span></span>
- <span data-ttu-id="a02c7-159">/ Almacén/examinar</span><span class="sxs-lookup"><span data-stu-id="a02c7-159">/Store/Browse</span></span>
- <span data-ttu-id="a02c7-160">/ / Detalles del almacén</span><span class="sxs-lookup"><span data-stu-id="a02c7-160">/Store/Details</span></span>

<span data-ttu-id="a02c7-161">Obtener acceso a estas direcciones URL se invocar los métodos de acción dentro de nuestro controlador y devuelva las respuestas de la cadena:</span><span class="sxs-lookup"><span data-stu-id="a02c7-161">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="a02c7-162">Que es grande, pero se trata de cadenas constantes solo.</span><span class="sxs-lookup"><span data-stu-id="a02c7-162">That's great, but these are just constant strings.</span></span> <span data-ttu-id="a02c7-163">Vamos a darles dinamismo, lo que toman la información de la dirección URL y mostrarla en el resultado de la página.</span><span class="sxs-lookup"><span data-stu-id="a02c7-163">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="a02c7-164">En primer lugar, se cambiará el método de acción de exploración para recuperar un valor de cadena de consulta de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a02c7-164">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="a02c7-165">Podemos hacerlo mediante la adición de un parámetro de "género" para el método de acción.</span><span class="sxs-lookup"><span data-stu-id="a02c7-165">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="a02c7-166">Al hacer esto ASP.NET MVC pasará automáticamente los parámetros de envío de cadena de consulta o un formulario con el nombre "género" para el método de acción cuando se invoca.</span><span class="sxs-lookup"><span data-stu-id="a02c7-166">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="a02c7-167">*Nota: Estamos usando el método de utilidad HttpUtility.HtmlEncode al corregir la entrada del usuario. ¿Esto impide que los usuarios insertar Javascript en la vista con un vínculo como /Store/Browse? Género =&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="a02c7-167">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="a02c7-168">¿Ahora vamos a examinar para/almacén/examinar? Género = Disco</span><span class="sxs-lookup"><span data-stu-id="a02c7-168">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="a02c7-169">Vamos a cambiar a continuación la acción de detalles para leer y mostrar un parámetro de entrada con el nombre de identificador.</span><span class="sxs-lookup"><span data-stu-id="a02c7-169">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="a02c7-170">A diferencia de nuestro método anterior, no puede incrustar el valor de identificador como un parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="a02c7-170">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="a02c7-171">En su lugar, se podrá insertar directamente dentro de la propia dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a02c7-171">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="a02c7-172">Por ejemplo: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="a02c7-172">For example: /Store/Details/5.</span></span>

<span data-ttu-id="a02c7-173">ASP.NET MVC nos permite hacerlo fácilmente sin tener que configurar nada.</span><span class="sxs-lookup"><span data-stu-id="a02c7-173">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="a02c7-174">Convención de enrutamiento de ASP.NET MVC predeterminada consiste en tratar el segmento de una dirección URL después del nombre de método de acción como un parámetro denominado "ID".</span><span class="sxs-lookup"><span data-stu-id="a02c7-174">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="a02c7-175">Si el método de acción tiene un parámetro denominado Id. a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="a02c7-175">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="a02c7-176">Ejecute la aplicación y vaya a /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="a02c7-176">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="a02c7-177">Resumamos lo que hemos hecho hasta ahora:</span><span class="sxs-lookup"><span data-stu-id="a02c7-177">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="a02c7-178">Hemos creado un nuevo proyecto de MVC de ASP.NET en Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="a02c7-178">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="a02c7-179">Hemos hablado de la estructura de carpetas básica de una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a02c7-179">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="a02c7-180">Ahora sabe cómo ejecutar nuestro sitio Web mediante el servidor de desarrollo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a02c7-180">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="a02c7-181">Hemos creado dos clases de controlador: un HomeController y un StoreController</span><span class="sxs-lookup"><span data-stu-id="a02c7-181">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="a02c7-182">Hemos agregado los métodos de acción a nuestro controladores que respondan a solicitudes de dirección URL y devuelven texto en el explorador</span><span class="sxs-lookup"><span data-stu-id="a02c7-182">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="a02c7-183">[Anterior](mvc-music-store-part-1.md)
> [Siguiente](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="a02c7-183">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
