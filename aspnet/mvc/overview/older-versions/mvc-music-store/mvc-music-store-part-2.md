---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 2 trata los controladores.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: b88aa22ccef04ab03b3a16c42e0a30e45ad11901
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829385"
---
<a name="part-2-controllers"></a><span data-ttu-id="711b3-104">Parte 2: controladores</span><span class="sxs-lookup"><span data-stu-id="711b3-104">Part 2: Controllers</span></span>
====================
<span data-ttu-id="711b3-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="711b3-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="711b3-106">El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="711b3-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="711b3-107">El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="711b3-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="711b3-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="711b3-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="711b3-109">Parte 2 trata los controladores.</span><span class="sxs-lookup"><span data-stu-id="711b3-109">Part 2 covers Controllers.</span></span>


<span data-ttu-id="711b3-110">Con marcos web tradicionales, las direcciones URL entrantes suelen asignarse a los archivos en disco.</span><span class="sxs-lookup"><span data-stu-id="711b3-110">With traditional web frameworks, incoming URLs are typically mapped to files on disk.</span></span> <span data-ttu-id="711b3-111">Por ejemplo: una solicitud para una dirección URL como "/ Products.aspx" o "/ Products.php" podría ser procesados por un archivo "Products.aspx" o "Products.php".</span><span class="sxs-lookup"><span data-stu-id="711b3-111">For example: a request for a URL like "/Products.aspx" or "/Products.php" might be processed by a "Products.aspx" or "Products.php" file.</span></span>

<span data-ttu-id="711b3-112">Marcos basados en Web MVC asignan las direcciones URL al código del servidor de forma ligeramente diferente.</span><span class="sxs-lookup"><span data-stu-id="711b3-112">Web-based MVC frameworks map URLs to server code in a slightly different way.</span></span> <span data-ttu-id="711b3-113">En lugar de asignar direcciones URL entrantes a los archivos, en su lugar asignar URL a métodos en clases.</span><span class="sxs-lookup"><span data-stu-id="711b3-113">Instead of mapping incoming URLs to files, they instead map URLs to methods on classes.</span></span> <span data-ttu-id="711b3-114">Estas clases se denominan "Controladores" y son responsables de procesar las solicitudes HTTP entrantes, control de entrada del usuario, recuperar y guardar los datos y determinar la respuesta enviada al cliente (Mostrar HTML, descargar un archivo, redirigir a otro Dirección URL, etcetera).</span><span class="sxs-lookup"><span data-stu-id="711b3-114">These classes are called "Controllers" and they are responsible for processing incoming HTTP requests, handling user input, retrieving and saving data, and determining the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span>

## <a name="adding-a-homecontroller"></a><span data-ttu-id="711b3-115">Agregar un HomeController</span><span class="sxs-lookup"><span data-stu-id="711b3-115">Adding a HomeController</span></span>

<span data-ttu-id="711b3-116">Empezaremos a nuestra aplicación de MVC Music Store mediante la adición de una clase de controlador que controlará las direcciones URL a la página principal de nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="711b3-116">We'll begin our MVC Music Store application by adding a Controller class that will handle URLs to the Home page of our site.</span></span> <span data-ttu-id="711b3-117">Comenzaremos siguen las convenciones de nomenclatura predeterminado de ASP.NET MVC y llámelo HomeController.</span><span class="sxs-lookup"><span data-stu-id="711b3-117">We'll follow the default naming conventions of ASP.NET MVC and call it HomeController.</span></span>

<span data-ttu-id="711b3-118">Haga clic en la carpeta "Controladores" en el Explorador de soluciones y seleccione "Agregar" y, a continuación, el comando "controlador":</span><span class="sxs-lookup"><span data-stu-id="711b3-118">Right-click the "Controllers" folder within the Solution Explorer and select "Add", and then the "Controller…" command:</span></span>

![](mvc-music-store-part-2/_static/image1.jpg)

<span data-ttu-id="711b3-119">Se abrirá el cuadro de diálogo "Agregar controlador".</span><span class="sxs-lookup"><span data-stu-id="711b3-119">This will bring up the "Add Controller" dialog.</span></span> <span data-ttu-id="711b3-120">Nombre del controlador "HomeController" y presione el botón Agregar.</span><span class="sxs-lookup"><span data-stu-id="711b3-120">Name the controller "HomeController" and press the Add button.</span></span>

![](mvc-music-store-part-2/_static/image1.png)

<span data-ttu-id="711b3-121">Esto creará un nuevo archivo, HomeController.cs, con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="711b3-121">This will create a new file, HomeController.cs, with the following code:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

<span data-ttu-id="711b3-122">Para iniciar tan simple como sea posible, vamos a reemplazar el método Index con un método sencillo que simplemente devuelve una cadena.</span><span class="sxs-lookup"><span data-stu-id="711b3-122">To start as simply as possible, let's replace the Index method with a simple method that just returns a string.</span></span> <span data-ttu-id="711b3-123">Haremos dos cambios:</span><span class="sxs-lookup"><span data-stu-id="711b3-123">We'll make two changes:</span></span>

- <span data-ttu-id="711b3-124">Cambiar el método para devolver una cadena en lugar de un ActionResult</span><span class="sxs-lookup"><span data-stu-id="711b3-124">Change the method to return a string instead of an ActionResult</span></span>
- <span data-ttu-id="711b3-125">Cambie la instrucción return para devolver "Hello de hogar"</span><span class="sxs-lookup"><span data-stu-id="711b3-125">Change the return statement to return "Hello from Home"</span></span>

<span data-ttu-id="711b3-126">El método ahora un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="711b3-126">The method should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a><span data-ttu-id="711b3-127">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="711b3-127">Running the Application</span></span>

<span data-ttu-id="711b3-128">Ahora vamos a ejecutar el sitio.</span><span class="sxs-lookup"><span data-stu-id="711b3-128">Now let's run the site.</span></span> <span data-ttu-id="711b3-129">Podemos iniciar nuestro servidor web y pruebe el sitio mediante cualquiera de las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="711b3-129">We can start our web-server and try out the site using any of the following::</span></span>

- <span data-ttu-id="711b3-130">Elija el elemento de menú Iniciar depuración ⇨ de depuración</span><span class="sxs-lookup"><span data-stu-id="711b3-130">Choose the Debug ⇨ Start Debugging menu item</span></span>
- <span data-ttu-id="711b3-131">Haga clic en el botón de flecha verde en la barra de herramientas ![](mvc-music-store-part-2/_static/image2.jpg)</span><span class="sxs-lookup"><span data-stu-id="711b3-131">Click the Green arrow button in the toolbar ![](mvc-music-store-part-2/_static/image2.jpg)</span></span>
- <span data-ttu-id="711b3-132">Utilice el método abreviado F5.</span><span class="sxs-lookup"><span data-stu-id="711b3-132">Use the keyboard shortcut, F5.</span></span>

<span data-ttu-id="711b3-133">Con cualquiera de los pasos anteriores compilar el proyecto y, a continuación, hacer que el servidor de desarrollo de ASP.NET que está integrada en Visual Web Developer para iniciar.</span><span class="sxs-lookup"><span data-stu-id="711b3-133">Using any of the above steps will compile our project, and then cause the ASP.NET Development Server that is built-into Visual Web Developer to start.</span></span> <span data-ttu-id="711b3-134">Una notificación aparecerá en la esquina inferior de la pantalla para indicar que el servidor de desarrollo de ASP.NET haya iniciado y mostrará al número de puerto que se está ejecutando bajo.</span><span class="sxs-lookup"><span data-stu-id="711b3-134">A notification will appear in the bottom corner of the screen to indicate that the ASP.NET Development Server has started up, and will show the port number that it is running under.</span></span>

![](mvc-music-store-part-2/_static/image2.png)

<span data-ttu-id="711b3-135">Visual Web Developer, a continuación, se abrirá una ventana del explorador cuya dirección URL apunta a nuestro servidor web automáticamente.</span><span class="sxs-lookup"><span data-stu-id="711b3-135">Visual Web Developer will then automatically open a browser window whose URL points to our web-server.</span></span> <span data-ttu-id="711b3-136">Esto nos permitirá probar rápidamente nuestra aplicación web:</span><span class="sxs-lookup"><span data-stu-id="711b3-136">This will allow us to quickly try out our web application:</span></span>

![](mvc-music-store-part-2/_static/image3.png)

<span data-ttu-id="711b3-137">Bien, que era bastante rápido, que creamos un nuevo sitio Web, ha agregado una función de tres líneas, y tenemos texto en un explorador.</span><span class="sxs-lookup"><span data-stu-id="711b3-137">Okay, that was pretty quick – we created a new website, added a three line function, and we've got text in a browser.</span></span> <span data-ttu-id="711b3-138">Rocket no ciencia, pero es un comienzo.</span><span class="sxs-lookup"><span data-stu-id="711b3-138">Not rocket science, but it's a start.</span></span>

<span data-ttu-id="711b3-139">*Nota: Visual Web Developer incluye el servidor de desarrollo de ASP.NET, que se ejecutará el sitio Web en un número aleatorio libre "puerto". En la captura de pantalla anterior, se está ejecutando en el sitio `http://localhost:26641/`, por lo que está utilizando el puerto 26641. El número de puerto será diferente. Cuando hablamos sobre como /Store/Browse la dirección URL en este tutorial, que pasará después del número de puerto. Suponiendo que un número de puerto de 26641, vaya a/Store/examinar significa ir a `http://localhost:26641/Store/Browse`.*</span><span class="sxs-lookup"><span data-stu-id="711b3-139">*Note: Visual Web Developer includes the ASP.NET Development Server, which will run your website on a random free "port" number. In the screenshot above, the site is running at `http://localhost:26641/`, so it's using port 26641. Your port number will be different. When we talk about URL's like /Store/Browse in this tutorial, that will go after the port number. Assuming a port number of 26641, browsing to /Store/Browse will mean browsing to `http://localhost:26641/Store/Browse`.*</span></span>

## <a name="adding-a-storecontroller"></a><span data-ttu-id="711b3-140">Agregar un StoreController</span><span class="sxs-lookup"><span data-stu-id="711b3-140">Adding a StoreController</span></span>

<span data-ttu-id="711b3-141">Se ha agregado un HomeController simple que implementa la página principal de nuestro sitio.</span><span class="sxs-lookup"><span data-stu-id="711b3-141">We added a simple HomeController that implements the Home Page of our site.</span></span> <span data-ttu-id="711b3-142">Ahora agreguemos otro controlador que vamos a usar para implementar la funcionalidad de exploración de nuestra tienda de música.</span><span class="sxs-lookup"><span data-stu-id="711b3-142">Let's now add another controller that we'll use to implement the browsing functionality of our music store.</span></span> <span data-ttu-id="711b3-143">Nuestro controlador de almacén será compatible con tres escenarios:</span><span class="sxs-lookup"><span data-stu-id="711b3-143">Our store controller will support three scenarios:</span></span>

- <span data-ttu-id="711b3-144">Una página de listado de los géneros de música en nuestra tienda de música</span><span class="sxs-lookup"><span data-stu-id="711b3-144">A listing page of the music genres in our music store</span></span>
- <span data-ttu-id="711b3-145">Una página de exploración que enumera todos los álbumes de música de un género determinado</span><span class="sxs-lookup"><span data-stu-id="711b3-145">A browse page that lists all of the music albums in a particular genre</span></span>
- <span data-ttu-id="711b3-146">Una página de detalles que muestra información sobre un álbum de música específicos</span><span class="sxs-lookup"><span data-stu-id="711b3-146">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="711b3-147">Comenzaremos por agregar una nueva clase StoreController...</span><span class="sxs-lookup"><span data-stu-id="711b3-147">We'll start by adding a new StoreController class..</span></span> <span data-ttu-id="711b3-148">Si no lo ha hecho ya, detenga la ejecución de la aplicación, ya sea cerrando el explorador o seleccionando el elemento de menú Detener depuración ⇨ de depuración.</span><span class="sxs-lookup"><span data-stu-id="711b3-148">If you haven't already, stop running the application either by closing the browser or selecting the Debug ⇨ Stop Debugging menu item.</span></span>

<span data-ttu-id="711b3-149">Ahora, agregue un nuevo StoreController.</span><span class="sxs-lookup"><span data-stu-id="711b3-149">Now add a new StoreController.</span></span> <span data-ttu-id="711b3-150">Tal como se hizo con HomeController, lo haremos con el botón secundario en la carpeta "Controladores" en el Explorador de soluciones y eligiendo agregar -&gt;elemento de menú del controlador</span><span class="sxs-lookup"><span data-stu-id="711b3-150">Just like we did with HomeController, we'll do this by right-clicking on the "Controllers" folder within the Solution Explorer and choosing the Add-&gt;Controller menu item</span></span>

![](mvc-music-store-part-2/_static/image4.png)

<span data-ttu-id="711b3-151">Nuestro nuevo StoreController ya tiene un método "Index".</span><span class="sxs-lookup"><span data-stu-id="711b3-151">Our new StoreController already has an "Index" method.</span></span> <span data-ttu-id="711b3-152">Este método "Index" vamos a usar para implementar nuestra página de lista que se enumera todos los géneros en nuestra tienda de música.</span><span class="sxs-lookup"><span data-stu-id="711b3-152">We'll use this "Index" method to implement our listing page that lists all genres in our music store.</span></span> <span data-ttu-id="711b3-153">También agregaremos dos métodos adicionales para implementar otros de los dos escenarios que deseamos que nuestro StoreController para controlar: Examinar y detalles.</span><span class="sxs-lookup"><span data-stu-id="711b3-153">We'll also add two additional methods to implement the two other scenarios we want our StoreController to handle: Browse and Details.</span></span>

<span data-ttu-id="711b3-154">Estos métodos (índice, examinar y detalles) dentro de nuestro controlador se denominan "Controlador de acciones" y como ya hemos visto con el método de acción HomeController.Index (), su trabajo es responder a las solicitudes de dirección URL y (general) determinar qué contenido se debe enviar al explorador o usuario que invocó la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="711b3-154">These methods (Index, Browse and Details) within our Controller are called "Controller Actions", and as you've already seen with the HomeController.Index()action method, their job is to respond to URL requests and (generally speaking) determine what content should be sent back to the browser or user that invoked the URL.</span></span>

<span data-ttu-id="711b3-155">Comenzaremos a nuestra implementación StoreController cambiando theIndex() al método para devolver la cadena "Hello de Store.Index()" y agregaremos métodos similares para Browse() y Details():</span><span class="sxs-lookup"><span data-stu-id="711b3-155">We'll start our StoreController implementation by changing theIndex() method to return the string "Hello from Store.Index()" and we'll add similar methods for Browse() and Details():</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

<span data-ttu-id="711b3-156">Vuelva a ejecutar el proyecto y examinar las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="711b3-156">Run the project again and browse the following URLs:</span></span>

- <span data-ttu-id="711b3-157">/ Store</span><span class="sxs-lookup"><span data-stu-id="711b3-157">/Store</span></span>
- <span data-ttu-id="711b3-158">/ Store/examinar</span><span class="sxs-lookup"><span data-stu-id="711b3-158">/Store/Browse</span></span>
- <span data-ttu-id="711b3-159">/ Store/detalles</span><span class="sxs-lookup"><span data-stu-id="711b3-159">/Store/Details</span></span>

<span data-ttu-id="711b3-160">Obtener acceso a estas direcciones URL se invocan los métodos de acción dentro de nuestro controlador y devolver las respuestas de cadena:</span><span class="sxs-lookup"><span data-stu-id="711b3-160">Accessing these URLs will invoke the action methods within our Controller and return string responses:</span></span>

![](mvc-music-store-part-2/_static/image5.png)

<span data-ttu-id="711b3-161">Eso está muy bien, pero estas son las cadenas constantes solo.</span><span class="sxs-lookup"><span data-stu-id="711b3-161">That's great, but these are just constant strings.</span></span> <span data-ttu-id="711b3-162">Aumentemos su dinámica, por lo que toman la información de la dirección URL y mostrarlos en el resultado de la página.</span><span class="sxs-lookup"><span data-stu-id="711b3-162">Let's make them dynamic, so they take information from the URL and display it in the page output.</span></span>

<span data-ttu-id="711b3-163">Primero, cambiaremos el método de acción de exploración para recuperar un valor de cadena de consulta de la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="711b3-163">First we'll change the Browse action method to retrieve a querystring value from the URL.</span></span> <span data-ttu-id="711b3-164">Esto se logra mediante la adición de un parámetro de "género" a nuestro método de acción.</span><span class="sxs-lookup"><span data-stu-id="711b3-164">We can do this by adding a "genre" parameter to our action method.</span></span> <span data-ttu-id="711b3-165">Al hacer esto ASP.NET MVC pasará automáticamente los parámetros post de cadena de consulta o formulario denominados "género" a nuestro método de acción cuando se invoca.</span><span class="sxs-lookup"><span data-stu-id="711b3-165">When we do this ASP.NET MVC will automatically pass any querystring or form post parameters named "genre" to our action method when it is invoked.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

<span data-ttu-id="711b3-166">*Nota: Vamos a usar el método de utilidad HttpUtility.HtmlEncode para sanear la entrada del usuario. ¿Esto impide que los usuarios de inyección de Javascript en nuestra vista con un vínculo como /Store/Browse? Género =&lt;script&gt;Window.Location = "http://hackersite.com'&lt;/script&gt;.*</span><span class="sxs-lookup"><span data-stu-id="711b3-166">*Note: We're using the HttpUtility.HtmlEncode utility method to sanitize the user input. This prevents users from injecting Javascript into our View with a link like /Store/Browse?Genre=&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*</span></span>

<span data-ttu-id="711b3-167">¿Ahora vamos a examinar para/Store/examinar? Género = Disco</span><span class="sxs-lookup"><span data-stu-id="711b3-167">Now let's browse to /Store/Browse?Genre=Disco</span></span>

![](mvc-music-store-part-2/_static/image6.png)

<span data-ttu-id="711b3-168">Vamos a próximo cambio de la acción de detalles para leer y mostrar un parámetro de entrada denominado Id.</span><span class="sxs-lookup"><span data-stu-id="711b3-168">Let's next change the Details action to read and display an input parameter named ID.</span></span> <span data-ttu-id="711b3-169">A diferencia de nuestro método anterior, no se puede incrustar el valor de identificador como un parámetro de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="711b3-169">Unlike our previous method, we won't be embedding the ID value as a querystring parameter.</span></span> <span data-ttu-id="711b3-170">En su lugar, se insertará directamente dentro de la propia dirección URL.</span><span class="sxs-lookup"><span data-stu-id="711b3-170">Instead we'll embed it directly within the URL itself.</span></span> <span data-ttu-id="711b3-171">Por ejemplo: /Store/Details/5.</span><span class="sxs-lookup"><span data-stu-id="711b3-171">For example: /Store/Details/5.</span></span>

<span data-ttu-id="711b3-172">ASP.NET MVC nos permite hacerlo fácilmente sin tener que configurar nada.</span><span class="sxs-lookup"><span data-stu-id="711b3-172">ASP.NET MVC lets us easily do this without having to configure anything.</span></span> <span data-ttu-id="711b3-173">Convención de enrutamiento predeterminado de ASP.NET MVC es tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado "ID".</span><span class="sxs-lookup"><span data-stu-id="711b3-173">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named "ID".</span></span> <span data-ttu-id="711b3-174">Si el método de acción tiene un parámetro denominado Id. a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="711b3-174">If your action method has a parameter named ID then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span>

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

<span data-ttu-id="711b3-175">Ejecute la aplicación y vaya a /Store/Details/5:</span><span class="sxs-lookup"><span data-stu-id="711b3-175">Run the application and browse to /Store/Details/5:</span></span>

![](mvc-music-store-part-2/_static/image7.png)

<span data-ttu-id="711b3-176">Resumamos lo que hemos hecho hasta ahora:</span><span class="sxs-lookup"><span data-stu-id="711b3-176">Let's recap what we've done so far:</span></span>

- <span data-ttu-id="711b3-177">Hemos creado un nuevo proyecto de ASP.NET MVC en Visual Web Developer</span><span class="sxs-lookup"><span data-stu-id="711b3-177">We've created a new ASP.NET MVC project in Visual Web Developer</span></span>
- <span data-ttu-id="711b3-178">Hemos analizado la estructura de carpetas básica de una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="711b3-178">We've discussed the basic folder structure of an ASP.NET MVC application</span></span>
- <span data-ttu-id="711b3-179">Nos hemos aprendido a ejecutar nuestro sitio Web mediante el servidor de desarrollo de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="711b3-179">We've learned how to run our website using the ASP.NET Development Server</span></span>
- <span data-ttu-id="711b3-180">Hemos creado dos clases de controlador: un HomeController y un StoreController</span><span class="sxs-lookup"><span data-stu-id="711b3-180">We've created two Controller classes: a HomeController and a StoreController</span></span>
- <span data-ttu-id="711b3-181">Hemos agregado métodos de acción a nuestro controladores que responden a solicitudes de dirección URL y devuelven texto en el explorador</span><span class="sxs-lookup"><span data-stu-id="711b3-181">We've added Action Methods to our controllers which respond to URL requests and return text to the browser</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="711b3-182">[Anterior](mvc-music-store-part-1.md)
> [Siguiente](mvc-music-store-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="711b3-182">[Previous](mvc-music-store-part-1.md)
[Next](mvc-music-store-part-3.md)</span></span>
