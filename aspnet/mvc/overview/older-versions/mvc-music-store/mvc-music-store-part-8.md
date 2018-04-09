---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
title: 'Parte 8: Carrito con las actualizaciones de Ajax | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 8 cubre el carro de la compra con las actualizaciones de Ajax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 26b2f55e-ed42-4277-89b0-c941eb754145
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-8
msc.type: authoredcontent
ms.openlocfilehash: 195c01ff0d71b2bfd0c00e71244d47a166330921
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-8-shopping-cart-with-ajax-updates"></a><span data-ttu-id="ac44e-104">Parte 8: Carro de la compra con las actualizaciones de Ajax</span><span class="sxs-lookup"><span data-stu-id="ac44e-104">Part 8: Shopping Cart with Ajax Updates</span></span>
====================
<span data-ttu-id="ac44e-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ac44e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ac44e-106">La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="ac44e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ac44e-107">La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="ac44e-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ac44e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ac44e-109">Parte 8 cubre el carro de la compra con las actualizaciones de Ajax.</span><span class="sxs-lookup"><span data-stu-id="ac44e-109">Part 8 covers Shopping Cart with Ajax Updates.</span></span>


<span data-ttu-id="ac44e-110">Se permitirá a los usuarios que coloquen álbumes en su carro sin registrar, pero tendrá que registrar como invitados consultar completa.</span><span class="sxs-lookup"><span data-stu-id="ac44e-110">We'll allow users to place albums in their cart without registering, but they'll need to register as guests to complete checkout.</span></span> <span data-ttu-id="ac44e-111">El proceso de compra y desprotección se dividirá en dos controladores: un controlador de ShoppingCart que permite de forma anónima agregar elementos a una cesta de y un controlador de desprotección que controla el proceso de pago.</span><span class="sxs-lookup"><span data-stu-id="ac44e-111">The shopping and checkout process will be separated into two controllers: a ShoppingCart Controller which allows anonymously adding items to a cart, and a Checkout Controller which handles the checkout process.</span></span> <span data-ttu-id="ac44e-112">Podrá iniciar con Shopping Cart en esta sección, después, se crea un proceso de retirada en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="ac44e-112">We'll start with the Shopping Cart in this section, then build the Checkout process in the following section.</span></span>

## <a name="adding-the-cart-order-and-orderdetail-model-classes"></a><span data-ttu-id="ac44e-113">Agregar las clases del modelo carro, Order y OrderDetail</span><span class="sxs-lookup"><span data-stu-id="ac44e-113">Adding the Cart, Order, and OrderDetail model classes</span></span>

<span data-ttu-id="ac44e-114">Nuestros procesos Shopping Cart y desprotección hará que el uso de algunas nuevas clases.</span><span class="sxs-lookup"><span data-stu-id="ac44e-114">Our Shopping Cart and Checkout processes will make use of some new classes.</span></span> <span data-ttu-id="ac44e-115">Haga clic en la carpeta de modelos y agregue una clase de carro (Cart.cs) con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="ac44e-115">Right-click the Models folder and add a Cart class (Cart.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample1.cs)]

<span data-ttu-id="ac44e-116">Esta clase es muy similar a otras personas que hemos usado hasta ahora, excepto el atributo [Key] para la propiedad de identificador de registro.</span><span class="sxs-lookup"><span data-stu-id="ac44e-116">This class is pretty similar to others we've used so far, with the exception of the [Key] attribute for the RecordId property.</span></span> <span data-ttu-id="ac44e-117">Nuestros artículos del carro tendrá un identificador de cadena denominado CartID para permitir la compra anónima, pero la tabla incluye una clave principal de entero denominada RecordId.</span><span class="sxs-lookup"><span data-stu-id="ac44e-117">Our Cart items will have a string identifier named CartID to allow anonymous shopping, but the table includes an integer primary key named RecordId.</span></span> <span data-ttu-id="ac44e-118">Por convención, Entity Framework Code-First espera que la clave principal para una tabla denominada carro será CartId o Id., pero, podemos fácilmente reemplazamos a través de las anotaciones o código si lo deseamos.</span><span class="sxs-lookup"><span data-stu-id="ac44e-118">By convention, Entity Framework Code-First expects that the primary key for a table named Cart will be either CartId or ID, but we can easily override that via annotations or code if we want.</span></span> <span data-ttu-id="ac44e-119">Este es un ejemplo de cómo podemos utilizar las convenciones simples en Entity Framework Code-First cuando ajuste a nosotros, pero no nos estamos limitados por ellos cuando no lo hacen.</span><span class="sxs-lookup"><span data-stu-id="ac44e-119">This is an example of how we can use the simple conventions in Entity Framework Code-First when they suit us, but we're not constrained by them when they don't.</span></span>

<span data-ttu-id="ac44e-120">A continuación, agregue una clase Order (Order.cs) con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="ac44e-120">Next, add an Order class (Order.cs) with the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample2.cs)]

<span data-ttu-id="ac44e-121">Esta clase realiza un seguimiento de información de resumen y la entrega de un pedido.</span><span class="sxs-lookup"><span data-stu-id="ac44e-121">This class tracks summary and delivery information for an order.</span></span> <span data-ttu-id="ac44e-122">**No se compilará aún**porque tiene una propiedad de navegación de OrderDetails que depende de una clase que no hemos creado aún.</span><span class="sxs-lookup"><span data-stu-id="ac44e-122">**It won't compile yet**, because it has an OrderDetails navigation property which depends on a class we haven't created yet.</span></span> <span data-ttu-id="ac44e-123">Vamos a corregir que ahora mediante la adición de una clase denominada OrderDetail.cs, agregue el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="ac44e-123">Let's fix that now by adding a class named OrderDetail.cs, adding the following code.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample3.cs)]

<span data-ttu-id="ac44e-124">Nos aseguraremos de hacer una actualización de última a nuestra clase MusicStoreEntities para incluir DbSets que exponen las nuevas clases de modelo, también incluye un DbSet&lt;intérprete&gt;.</span><span class="sxs-lookup"><span data-stu-id="ac44e-124">We'll make one last update to our MusicStoreEntities class to include DbSets which expose those new Model classes, also including a DbSet&lt;Artist&gt;.</span></span> <span data-ttu-id="ac44e-125">La clase MusicStoreEntities actualizada aparece como a continuación.</span><span class="sxs-lookup"><span data-stu-id="ac44e-125">The updated MusicStoreEntities class appears as below.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample4.cs)]

## <a name="managing-the-shopping-cart-business-logic"></a><span data-ttu-id="ac44e-126">Administrar la lógica de negocios de carro de la compra</span><span class="sxs-lookup"><span data-stu-id="ac44e-126">Managing the Shopping Cart business logic</span></span>

<span data-ttu-id="ac44e-127">A continuación, vamos a crear la clase ShoppingCart en la carpeta de modelos.</span><span class="sxs-lookup"><span data-stu-id="ac44e-127">Next, we'll create the ShoppingCart class in the Models folder.</span></span> <span data-ttu-id="ac44e-128">El modelo ShoppingCart controla el acceso a los datos a la tabla de carro.</span><span class="sxs-lookup"><span data-stu-id="ac44e-128">The ShoppingCart model handles data access to the Cart table.</span></span> <span data-ttu-id="ac44e-129">Además, controlará la lógica de negocios a para agregar y quitar elementos del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-129">Additionally, it will handle the business logic to for adding and removing items from the shopping cart.</span></span>

<span data-ttu-id="ac44e-130">Puesto que no desea obligar a los usuarios iniciar sesión en una cuenta agregar elementos al carro de la compra, se le asignarán los usuarios un identificador temporal (mediante un GUID o identificador único global) al tener acceso al carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-130">Since we don't want to require users to sign up for an account just to add items to their shopping cart, we will assign users a temporary unique identifier (using a GUID, or globally unique identifier) when they access the shopping cart.</span></span> <span data-ttu-id="ac44e-131">Almacenaremos este identificador mediante la clase de sesión de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ac44e-131">We'll store this ID using the ASP.NET Session class.</span></span>

<span data-ttu-id="ac44e-132">*Nota: La sesión de ASP.NET es un lugar conveniente para almacenar información específica del usuario que expirará después de que atraviesan el sitio. Mientras un uso incorrecto del estado de sesión puede tener implicaciones de rendimiento en sitios de mayor tamaño, nuestro uso claro funcionarán bien con fines de demostración.*</span><span class="sxs-lookup"><span data-stu-id="ac44e-132">*Note: The ASP.NET Session is a convenient place to store user-specific information which will expire after they leave the site. While misuse of session state can have performance implications on larger sites, our light use will work well for demonstration purposes.*</span></span>

<span data-ttu-id="ac44e-133">La clase ShoppingCart expone los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="ac44e-133">The ShoppingCart class exposes the following methods:</span></span>

<span data-ttu-id="ac44e-134">**AddToCart** toma un álbum como parámetro y lo agrega al carro de la del usuario.</span><span class="sxs-lookup"><span data-stu-id="ac44e-134">**AddToCart** takes an Album as a parameter and adds it to the user's cart.</span></span> <span data-ttu-id="ac44e-135">Puesto que la tabla de carro realiza un seguimiento de cantidad para cada álbum, incluye una lógica para crear una nueva fila si es necesario o simplemente aumentará la cantidad si el usuario ya ha solicitado una copia del álbum.</span><span class="sxs-lookup"><span data-stu-id="ac44e-135">Since the Cart table tracks quantity for each album, it includes logic to create a new row if needed or just increment the quantity if the user has already ordered one copy of the album.</span></span>

<span data-ttu-id="ac44e-136">**RemoveFromCart** toma un identificador de álbum y lo quita del carro de la del usuario.</span><span class="sxs-lookup"><span data-stu-id="ac44e-136">**RemoveFromCart** takes an Album ID and removes it from the user's cart.</span></span> <span data-ttu-id="ac44e-137">Si el usuario sólo tuviera una copia del álbum en su carro, se quita la fila.</span><span class="sxs-lookup"><span data-stu-id="ac44e-137">If the user only had one copy of the album in their cart, the row is removed.</span></span>

<span data-ttu-id="ac44e-138">**EmptyCart** quita todos los elementos del carro de la compra de un usuario.</span><span class="sxs-lookup"><span data-stu-id="ac44e-138">**EmptyCart** removes all items from a user's shopping cart.</span></span>

<span data-ttu-id="ac44e-139">**GetCartItems** recupera una lista de CartItems para mostrar o procesamiento.</span><span class="sxs-lookup"><span data-stu-id="ac44e-139">**GetCartItems** retrieves a list of CartItems for display or processing.</span></span>

<span data-ttu-id="ac44e-140">**GetCount** recupera un número total de álbumes de un usuario tiene en su carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-140">**GetCount** retrieves a the total number of albums a user has in their shopping cart.</span></span>

<span data-ttu-id="ac44e-141">**GetTotal** calcula el costo total de todos los elementos en el carro.</span><span class="sxs-lookup"><span data-stu-id="ac44e-141">**GetTotal** calculates the total cost of all items in the cart.</span></span>

<span data-ttu-id="ac44e-142">**CreateOrder** convierte el carro de la compra en un pedido durante la fase de desprotección.</span><span class="sxs-lookup"><span data-stu-id="ac44e-142">**CreateOrder** converts the shopping cart to an order during the checkout phase.</span></span>

<span data-ttu-id="ac44e-143">**GetCart** es un método estático que permite a nuestros controladores obtener un objeto de carro.</span><span class="sxs-lookup"><span data-stu-id="ac44e-143">**GetCart** is a static method which allows our controllers to obtain a cart object.</span></span> <span data-ttu-id="ac44e-144">Usa el **GetCartId** método para controlar leía el CartId la sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="ac44e-144">It uses the **GetCartId** method to handle reading the CartId from the user's session.</span></span> <span data-ttu-id="ac44e-145">El método GetCartId requiere el objeto HttpContextBase para que pueda leer CartId del usuario de la sesión del usuario.</span><span class="sxs-lookup"><span data-stu-id="ac44e-145">The GetCartId method requires the HttpContextBase so that it can read the user's CartId from user's session.</span></span>

<span data-ttu-id="ac44e-146">Aquí es el conjunto completo **clase ShoppingCart**:</span><span class="sxs-lookup"><span data-stu-id="ac44e-146">Here's the complete **ShoppingCart class**:</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample5.cs)]

## <a name="viewmodels"></a><span data-ttu-id="ac44e-147">ViewModels</span><span class="sxs-lookup"><span data-stu-id="ac44e-147">ViewModels</span></span>

<span data-ttu-id="ac44e-148">Nuestro controlador de carro de la compra deberá comunicarse cierta información a sus vistas compleja que no se asigna correctamente a los objetos de modelo.</span><span class="sxs-lookup"><span data-stu-id="ac44e-148">Our Shopping Cart Controller will need to communicate some complex information to its views which doesn't map cleanly to our Model objects.</span></span> <span data-ttu-id="ac44e-149">No queremos modificar nuestros modelos para adaptarlo a nuestro vistas; Clases de modelo deberían representar el dominio, no en la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="ac44e-149">We don't want to modify our Models to suit our views; Model classes should represent our domain, not the user interface.</span></span> <span data-ttu-id="ac44e-150">Una solución sería pasar la información a nuestro vistas mediante la clase de elemento ViewBag, tal y como hicimos con la información de la lista desplegable de Store Manager, pero pasando una gran cantidad de información a través de ViewBag obtiene difícil de administrar.</span><span class="sxs-lookup"><span data-stu-id="ac44e-150">One solution would be to pass the information to our Views using the ViewBag class, as we did with the Store Manager dropdown information, but passing a lot of information via ViewBag gets hard to manage.</span></span>

<span data-ttu-id="ac44e-151">Una solución consiste en utilizar el *ViewModel* patrón.</span><span class="sxs-lookup"><span data-stu-id="ac44e-151">A solution to this is to use the *ViewModel* pattern.</span></span> <span data-ttu-id="ac44e-152">Si utiliza este modelo, crear clases fuertemente tipadas que están optimizados para cuatro escenarios de vista específicos, y que exponen propiedades de los valores o contenido dinámico necesita nuestras plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="ac44e-152">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="ac44e-153">Las clases de controlador, a continuación, pueden rellenar y pasar estas clases con optimización para ver a nuestra plantilla de vista que se usará.</span><span class="sxs-lookup"><span data-stu-id="ac44e-153">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="ac44e-154">Esto permite la seguridad de tipos, comprobación en tiempo de compilación y editor IntelliSense dentro de las plantillas de vista.</span><span class="sxs-lookup"><span data-stu-id="ac44e-154">This enables type-safety, compile-time checking, and editor IntelliSense within view templates.</span></span>

<span data-ttu-id="ac44e-155">Vamos a crear dos modelos de vista para su uso en el controlador de carro de la compra: el ShoppingCartViewModel contendrá el contenido del carro de la compra del usuario y la ShoppingCartRemoveViewModel se usará para mostrar información de confirmación cuando un usuario quita algo en su carro.</span><span class="sxs-lookup"><span data-stu-id="ac44e-155">We'll create two View Models for use in our Shopping Cart controller: the ShoppingCartViewModel will hold the contents of the user's shopping cart, and the ShoppingCartRemoveViewModel will be used to display confirmation information when a user removes something from their cart.</span></span>

<span data-ttu-id="ac44e-156">Vamos a crear una nueva carpeta ViewModels en la raíz de nuestro proyecto de mantener las cosas organizadas.</span><span class="sxs-lookup"><span data-stu-id="ac44e-156">Let's create a new ViewModels folder in the root of our project to keep things organized.</span></span> <span data-ttu-id="ac44e-157">Haga clic en el proyecto, seleccione Agregar / nueva carpeta.</span><span class="sxs-lookup"><span data-stu-id="ac44e-157">Right-click the project, select Add / New Folder.</span></span>

![](mvc-music-store-part-8/_static/image1.jpg)

<span data-ttu-id="ac44e-158">Nombre de la carpeta ViewModels.</span><span class="sxs-lookup"><span data-stu-id="ac44e-158">Name the folder ViewModels.</span></span>

![](mvc-music-store-part-8/_static/image1.png)

<span data-ttu-id="ac44e-159">A continuación, agregue la clase ShoppingCartViewModel en la carpeta de ViewModels.</span><span class="sxs-lookup"><span data-stu-id="ac44e-159">Next, add the ShoppingCartViewModel class in the ViewModels folder.</span></span> <span data-ttu-id="ac44e-160">Tiene dos propiedades: una lista de artículos del carro y un valor decimal para contener el precio total para todos los elementos en el carro.</span><span class="sxs-lookup"><span data-stu-id="ac44e-160">It has two properties: a list of Cart items, and a decimal value to hold the total price for all items in the cart.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample6.cs)]

<span data-ttu-id="ac44e-161">Ahora agregue el ShoppingCartRemoveViewModel a la carpeta ViewModels, con las siguientes cuatro propiedades.</span><span class="sxs-lookup"><span data-stu-id="ac44e-161">Now add the ShoppingCartRemoveViewModel to the ViewModels folder, with the following four properties.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample7.cs)]

## <a name="the-shopping-cart-controller"></a><span data-ttu-id="ac44e-162">El controlador de carro de la compra</span><span class="sxs-lookup"><span data-stu-id="ac44e-162">The Shopping Cart Controller</span></span>

<span data-ttu-id="ac44e-163">El controlador de carro de la compra tiene tres objetivos principales: agregar elementos a una cesta de, quitar elementos de la cesta y ven los elementos en el carro de la.</span><span class="sxs-lookup"><span data-stu-id="ac44e-163">The Shopping Cart controller has three main purposes: adding items to a cart, removing items from the cart, and viewing items in the cart.</span></span> <span data-ttu-id="ac44e-164">Hará que el uso de las tres clases se acaba de crear: ShoppingCartViewModel, ShoppingCartRemoveViewModel y ShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="ac44e-164">It will make use of the three classes we just created: ShoppingCartViewModel, ShoppingCartRemoveViewModel, and ShoppingCart.</span></span> <span data-ttu-id="ac44e-165">Como en los StoreController y StoreManagerController, vamos a agregar un campo que contenga una instancia de MusicStoreEntities.</span><span class="sxs-lookup"><span data-stu-id="ac44e-165">As in the StoreController and StoreManagerController, we'll add a field to hold an instance of MusicStoreEntities.</span></span>

<span data-ttu-id="ac44e-166">Agrega un nuevo controlador de carro de la compra para el proyecto mediante la plantilla de controlador en blanco.</span><span class="sxs-lookup"><span data-stu-id="ac44e-166">Add a new Shopping Cart controller to the project using the Empty controller template.</span></span>

![](mvc-music-store-part-8/_static/image2.png)

<span data-ttu-id="ac44e-167">Aquí es el controlador de ShoppingCart completa.</span><span class="sxs-lookup"><span data-stu-id="ac44e-167">Here's the complete ShoppingCart Controller.</span></span> <span data-ttu-id="ac44e-168">Las acciones de índice y Agregar controlador deben ser muy familiares.</span><span class="sxs-lookup"><span data-stu-id="ac44e-168">The Index and Add Controller actions should look very familiar.</span></span> <span data-ttu-id="ac44e-169">Las acciones de controlador Remove y CartSummary tratar dos casos especiales, que se abordará más adelante en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="ac44e-169">The Remove and CartSummary controller actions handle two special cases, which we'll discuss in the following section.</span></span>

[!code-csharp[Main](mvc-music-store-part-8/samples/sample8.cs)]

## <a name="ajax-updates-with-jquery"></a><span data-ttu-id="ac44e-170">Actualizaciones de AJAX con jQuery</span><span class="sxs-lookup"><span data-stu-id="ac44e-170">Ajax Updates with jQuery</span></span>

<span data-ttu-id="ac44e-171">A continuación, vamos a crear una página de índice de carro de la compra que está fuertemente tipada en el ShoppingCartViewModel y usa la plantilla de vista de lista con el mismo método que antes.</span><span class="sxs-lookup"><span data-stu-id="ac44e-171">We'll next create a Shopping Cart Index page that is strongly typed to the ShoppingCartViewModel and uses the List View template using the same method as before.</span></span>

![](mvc-music-store-part-8/_static/image3.png)

<span data-ttu-id="ac44e-172">Sin embargo, en lugar de usar un Html.ActionLink para quitar elementos del carro, usaremos jQuery "conectar" el evento click para todos los vínculos en esta vista que tiene la clase HTML RemoveLink.</span><span class="sxs-lookup"><span data-stu-id="ac44e-172">However, instead of using an Html.ActionLink to remove items from the cart, we'll use jQuery to "wire up" the click event for all links in this view which have the HTML class RemoveLink.</span></span> <span data-ttu-id="ac44e-173">En lugar de enviar el formulario, este controlador de eventos click solo realizará una devolución de llamada de AJAX en nuestro RemoveFromCart de la acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="ac44e-173">Rather than posting the form, this click event handler will just make an AJAX callback to our RemoveFromCart controller action.</span></span> <span data-ttu-id="ac44e-174">El RemoveFromCart devuelve un resultado JSON serializado, que la devolución de llamada de jQuery, a continuación, analiza y realiza cuatro actualizaciones rápidas a la página mediante jQuery:</span><span class="sxs-lookup"><span data-stu-id="ac44e-174">The RemoveFromCart returns a JSON serialized result, which our jQuery callback then parses and performs four quick updates to the page using jQuery:</span></span>

- 1. <span data-ttu-id="ac44e-175">Quita el álbum eliminado de la lista</span><span class="sxs-lookup"><span data-stu-id="ac44e-175">Removes the deleted album from the list</span></span>
- 2. <span data-ttu-id="ac44e-176">Actualiza el recuento de carro en el encabezado</span><span class="sxs-lookup"><span data-stu-id="ac44e-176">Updates the cart count in the header</span></span>
- 3. <span data-ttu-id="ac44e-177">Muestra un mensaje de actualización al usuario</span><span class="sxs-lookup"><span data-stu-id="ac44e-177">Displays an update message to the user</span></span>
- 4. <span data-ttu-id="ac44e-178">Actualiza el precio total de carro</span><span class="sxs-lookup"><span data-stu-id="ac44e-178">Updates the cart total price</span></span>

<span data-ttu-id="ac44e-179">Puesto que se está controlando el escenario de quitar una devolución de llamada de Ajax en la vista de índice, no necesitamos una vista adicional para la acción de RemoveFromCart.</span><span class="sxs-lookup"><span data-stu-id="ac44e-179">Since the remove scenario is being handled by an Ajax callback within the Index view, we don't need an additional view for RemoveFromCart action.</span></span> <span data-ttu-id="ac44e-180">Este es el código completo para la vista de /ShoppingCart/Index:</span><span class="sxs-lookup"><span data-stu-id="ac44e-180">Here is the complete code for the /ShoppingCart/Index view:</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample9.cshtml)]

<span data-ttu-id="ac44e-181">Para probarlo, necesitamos poder agregar elementos a nuestro carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-181">In order to test this out, we need to be able to add items to our shopping cart.</span></span> <span data-ttu-id="ac44e-182">Actualizaremos nuestra **detalles del almacén de** vista incluya un botón "Agregar al carro".</span><span class="sxs-lookup"><span data-stu-id="ac44e-182">We'll update our **Store Details** view to include an "Add to cart" button.</span></span> <span data-ttu-id="ac44e-183">Mientras se encuentra en él, puede incluir parte de la información adicional de álbum que hemos agregado desde que se actualizó por última vez esta vista: género, intérprete, precio y carátulas de álbum.</span><span class="sxs-lookup"><span data-stu-id="ac44e-183">While we're at it, we can include some of the Album additional information which we've added since we last updated this view: Genre, Artist, Price, and Album Art.</span></span> <span data-ttu-id="ac44e-184">El código de la vista de detalles del almacén actualizado aparece tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="ac44e-184">The updated Store Details view code appears as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-8/samples/sample10.cshtml)]

<span data-ttu-id="ac44e-185">Ahora podemos haga clic en a través de la tienda y probar agregando y quitando álbumes a y desde nuestro carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-185">Now we can click through the store and test adding and removing Albums to and from our shopping cart.</span></span> <span data-ttu-id="ac44e-186">Ejecute la aplicación y busque el índice de almacén.</span><span class="sxs-lookup"><span data-stu-id="ac44e-186">Run the application and browse to the Store Index.</span></span>

![](mvc-music-store-part-8/_static/image4.png)

<span data-ttu-id="ac44e-187">A continuación, haga clic en un género para ver una lista de álbumes.</span><span class="sxs-lookup"><span data-stu-id="ac44e-187">Next, click on a Genre to view a list of albums.</span></span>

![](mvc-music-store-part-8/_static/image5.png)

<span data-ttu-id="ac44e-188">Al hacer clic en un título de álbum ahora muestra nuestra vista Detalles del álbum actualizada, incluido el botón "Agregar al carro".</span><span class="sxs-lookup"><span data-stu-id="ac44e-188">Clicking on an Album title now shows our updated Album Details view, including the "Add to cart" button.</span></span>

![](mvc-music-store-part-8/_static/image6.png)

<span data-ttu-id="ac44e-189">Al hacer clic en el botón "Agregar al carro" muestra la vista de índice de carro de la compra con la lista de resumen de carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-189">Clicking the "Add to cart" button shows our Shopping Cart Index view with the shopping cart summary list.</span></span>

![](mvc-music-store-part-8/_static/image7.png)

<span data-ttu-id="ac44e-190">Tras la carga de su carro de la compra, puede hacer clic en la quitar del vínculo del carro para ver la actualización de Ajax al carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="ac44e-190">After loading up your shopping cart, you can click on the Remove from cart link to see the Ajax update to your shopping cart.</span></span>

![](mvc-music-store-part-8/_static/image8.png)

<span data-ttu-id="ac44e-191">Hemos desarrollado una carro que permite a los usuarios no registrados agregar elementos a su carro de la compra y en funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="ac44e-191">We've built out a working shopping cart which allows unregistered users to add items to their cart.</span></span> <span data-ttu-id="ac44e-192">En la sección siguiente, permitimos que puedan registrar y realizar el proceso de pago.</span><span class="sxs-lookup"><span data-stu-id="ac44e-192">In the following section, we'll allow them to register and complete the checkout process.</span></span>


> [!div class="step-by-step"]
> <span data-ttu-id="ac44e-193">[Anterior](mvc-music-store-part-7.md)
> [Siguiente](mvc-music-store-part-9.md)</span><span class="sxs-lookup"><span data-stu-id="ac44e-193">[Previous](mvc-music-store-part-7.md)
[Next](mvc-music-store-part-9.md)</span></span>
