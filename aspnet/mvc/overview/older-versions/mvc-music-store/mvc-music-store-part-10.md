---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Actualizaciones finales para navegación y el diseño del sitio, conclusión | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 10 cubre actualizaciones finales de navegación y S...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: 40ed7c337e097675199ab66229095bd3315d8c8d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836815"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="22a2e-104">Parte 10: Actualizaciones finales para navegación y el diseño del sitio, conclusión</span><span class="sxs-lookup"><span data-stu-id="22a2e-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="22a2e-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="22a2e-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="22a2e-106">El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="22a2e-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="22a2e-107">El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="22a2e-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="22a2e-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="22a2e-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="22a2e-109">Parte 10 cubre actualizaciones finales de navegación y el diseño del sitio, conclusión.</span><span class="sxs-lookup"><span data-stu-id="22a2e-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="22a2e-110">Hemos completado toda la funcionalidad importante de nuestro sitio, pero aún hay algunas características para agregar a la página Examinar Store, la página principal y la navegación del sitio.</span><span class="sxs-lookup"><span data-stu-id="22a2e-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="22a2e-111">Creación de la vista parcial resumen carro de la compra</span><span class="sxs-lookup"><span data-stu-id="22a2e-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="22a2e-112">Queremos exponer el número de elementos en el carro de la compra del usuario en todo el sitio.</span><span class="sxs-lookup"><span data-stu-id="22a2e-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="22a2e-113">Se puede implementar fácilmente esto mediante la creación de una vista parcial que se agrega a nuestro Site.master.</span><span class="sxs-lookup"><span data-stu-id="22a2e-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="22a2e-114">Como se mostró anteriormente, el controlador ShoppingCart incluye un método de acción CartSummary que devuelve una vista parcial:</span><span class="sxs-lookup"><span data-stu-id="22a2e-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="22a2e-115">Para crear la vista parcial CartSummary, haga doble clic en la carpeta vistas/ShoppingCart y seleccione Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="22a2e-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="22a2e-116">Nombre de la vista CartSummary y Active la casilla "Crear una vista parcial" tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="22a2e-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="22a2e-117">La vista parcial CartSummary es muy sencilla: es solo un vínculo a la vista de índice ShoppingCart que muestra el número de elementos en el carro de compra.</span><span class="sxs-lookup"><span data-stu-id="22a2e-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="22a2e-118">El código completo de CartSummary.cshtml es como sigue:</span><span class="sxs-lookup"><span data-stu-id="22a2e-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="22a2e-119">En cualquier página del sitio, incluido al maestro de sitio, mediante el método Html.RenderAction podemos incluir una vista parcial.</span><span class="sxs-lookup"><span data-stu-id="22a2e-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="22a2e-120">RenderAction requiere que especifique el nombre de acción ("CartSummary") y el nombre del controlador ("ShoppingCart") como a continuación.</span><span class="sxs-lookup"><span data-stu-id="22a2e-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="22a2e-121">Antes de agregar esto al diseño del sitio, también crearemos el género de menú para que podamos tomar todas las actualizaciones de Site.master al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="22a2e-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="22a2e-122">Creación de la vista parcial del menú de género</span><span class="sxs-lookup"><span data-stu-id="22a2e-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="22a2e-123">Podemos lo hacemos mucho más fácil para los usuarios a navegar por el almacén mediante la adición de un menú de género que enumera todos los géneros disponibles en nuestra tienda.</span><span class="sxs-lookup"><span data-stu-id="22a2e-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="22a2e-124">Se sigue el mismo pasos también crea una vista parcial GenreMenu y, a continuación, podemos agregar ambos al patrón de sitio.</span><span class="sxs-lookup"><span data-stu-id="22a2e-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="22a2e-125">En primer lugar, agregue la siguiente acción del controlador GenreMenu a la StoreController:</span><span class="sxs-lookup"><span data-stu-id="22a2e-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="22a2e-126">Esta acción devuelve una lista de géneros que se muestra la vista parcial, que creará a continuación.</span><span class="sxs-lookup"><span data-stu-id="22a2e-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="22a2e-127">*Nota: Hemos agregado el atributo [ChildActionOnly] a esta acción de controlador, lo que indica que sólo deseamos que esta acción puede utilizarse desde una vista parcial. Este atributo evitará la acción del controlador que se ejecute, vaya a /Store/GenreMenu. Esto no es necesario para las vistas parciales, pero es una buena práctica, puesto que deseamos para asegurarse de que las acciones del controlador se usan como tenemos previsto. También nos estamos devolviendo PartialView en lugar de la vista, que permite que el motor de vistas saber que no debe usar el diseño para esta vista, ya que se incluye en otras vistas.*</span><span class="sxs-lookup"><span data-stu-id="22a2e-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="22a2e-128">Haga doble clic en la acción del controlador GenreMenu y crear una vista parcial denominada GenreMenu que está fuertemente tipada utilizando la clase de datos de vista de género, tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="22a2e-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="22a2e-129">Actualice el código de vista para la vista parcial GenreMenu mostrar los elementos con una lista desordenada como sigue.</span><span class="sxs-lookup"><span data-stu-id="22a2e-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="22a2e-130">Actualizar el diseño de sitio para mostrar nuestras vistas parciales</span><span class="sxs-lookup"><span data-stu-id="22a2e-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="22a2e-131">Podemos agregar nuestro vistas parciales al diseño del sitio (/vistas/Shared/\_Layout.cshtml) mediante una llamada a Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="22a2e-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="22a2e-132">Vamos a agregar las dos en, así como algunas marcas adicionales para mostrarlas, como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="22a2e-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="22a2e-133">Ahora Cuando ejecutemos la aplicación, veremos el género en el área de navegación izquierdo y el resumen del carro de compra en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="22a2e-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="22a2e-134">Actualizar a la página Examinar Store</span><span class="sxs-lookup"><span data-stu-id="22a2e-134">Update to the Store Browse page</span></span>

<span data-ttu-id="22a2e-135">La página Examinar Store es funcional, pero no parece muy bueno.</span><span class="sxs-lookup"><span data-stu-id="22a2e-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="22a2e-136">Podemos actualizar la página para mostrar los álbumes de un mejor diseño actualizando el código de vista (que se encuentra en /Views/Store/Browse.cshtml) como sigue:</span><span class="sxs-lookup"><span data-stu-id="22a2e-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="22a2e-137">Aquí vamos a hacer que use de Url.Action en lugar de Html.ActionLink para que podemos aplicar un formato especial al vínculo para incluir la ilustración del álbum.</span><span class="sxs-lookup"><span data-stu-id="22a2e-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="22a2e-138">*Nota: Estamos mostrando una portada del álbum genérico para estos álbumes. Esta información se almacena en la base de datos y puede modificarse mediante el Administrador de Store. Pueden agregar su propia ilustración.*</span><span class="sxs-lookup"><span data-stu-id="22a2e-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="22a2e-139">Ahora, cuando se examina un género, veremos los álbumes que se muestra en una cuadrícula con la ilustración del álbum.</span><span class="sxs-lookup"><span data-stu-id="22a2e-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="22a2e-140">Actualización de la página principal para mostrar los álbumes de venta superior</span><span class="sxs-lookup"><span data-stu-id="22a2e-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="22a2e-141">Queremos nuestro álbumes en la página principal para aumentar las ventas más vendidos de características.</span><span class="sxs-lookup"><span data-stu-id="22a2e-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="22a2e-142">Haremos algunas actualizaciones para nuestro HomeController controlar eso, y agregar en también algunos gráficos adicionales.</span><span class="sxs-lookup"><span data-stu-id="22a2e-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="22a2e-143">En primer lugar, vamos a agregar una propiedad de navegación a nuestra clase álbum para que sepa de Entity Framework que están asociados.</span><span class="sxs-lookup"><span data-stu-id="22a2e-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="22a2e-144">Las últimas líneas de nuestro **álbum** clase ahora debería parecerse a esto:</span><span class="sxs-lookup"><span data-stu-id="22a2e-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="22a2e-145">*Nota: Esto será necesario agregar el uso de una instrucción para que aparezca en el espacio de nombres System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="22a2e-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="22a2e-146">En primer lugar, vamos a agregar un campo storeDB y el MvcMusicStore.Models mediante instrucciones, como en nuestro otros controladores.</span><span class="sxs-lookup"><span data-stu-id="22a2e-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="22a2e-147">A continuación, agregaremos el método siguiente a HomeController que consulta la base de datos para encontrar los álbumes de ventas superiores según OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="22a2e-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="22a2e-148">Se trata de un método privado, puesto que no queremos que esté disponible como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="22a2e-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="22a2e-149">Incluiremos en HomeController por motivos de simplicidad, pero se recomienda para mover la lógica de negocios en clases de servicio independientes según corresponda.</span><span class="sxs-lookup"><span data-stu-id="22a2e-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="22a2e-150">Con esto en su lugar, podemos actualizar la acción de controlador de índice para consultar las principales 5 vender álbumes y devolverlos a la vista.</span><span class="sxs-lookup"><span data-stu-id="22a2e-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="22a2e-151">El código completo de HomeController actualizada es como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="22a2e-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="22a2e-152">Por último, se deberá actualizar la vista de nuestro índice de inicio para que puede mostrar una lista de álbumes, actualizar el tipo de modelo y agregue la lista de álbumes a la parte inferior.</span><span class="sxs-lookup"><span data-stu-id="22a2e-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="22a2e-153">Tomamos esta oportunidad de agregar también un título y una sección de promoción a la página.</span><span class="sxs-lookup"><span data-stu-id="22a2e-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="22a2e-154">Ahora Cuando ejecutemos la aplicación, veremos nuestra página de inicio actualizada con nuestro mensajes promocionales y álbumes de ventas superiores.</span><span class="sxs-lookup"><span data-stu-id="22a2e-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="22a2e-155">Conclusión</span><span class="sxs-lookup"><span data-stu-id="22a2e-155">Conclusion</span></span>

<span data-ttu-id="22a2e-156">Hemos visto que MVC de ASP.NET facilita la tarea para crear un sitio Web sofisticado con acceso de base de datos, suscripciones, AJAX, etcetera.</span><span class="sxs-lookup"><span data-stu-id="22a2e-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="22a2e-157">muy rápidamente.</span><span class="sxs-lookup"><span data-stu-id="22a2e-157">pretty quickly.</span></span> <span data-ttu-id="22a2e-158">Espero que este tutorial le haya proporcionado las herramientas que necesita para empezar a crear su propio ASP.NET MVC aplicaciones!</span><span class="sxs-lookup"><span data-stu-id="22a2e-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="22a2e-159">Anterior</span><span class="sxs-lookup"><span data-stu-id="22a2e-159">Previous</span></span>](mvc-music-store-part-9.md)
