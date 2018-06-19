---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Parte 10: Final las actualizaciones de navegación y el diseño del sitio, conclusión | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 10 cubre Final actualizaciones para navegación y S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878599"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a><span data-ttu-id="1d250-104">Parte 10: Final las actualizaciones de navegación y el diseño del sitio, conclusión</span><span class="sxs-lookup"><span data-stu-id="1d250-104">Part 10: Final Updates to Navigation and Site Design, Conclusion</span></span>
====================
<span data-ttu-id="1d250-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="1d250-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="1d250-106">La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.</span><span class="sxs-lookup"><span data-stu-id="1d250-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="1d250-107">La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.</span><span class="sxs-lookup"><span data-stu-id="1d250-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="1d250-108">Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1d250-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="1d250-109">Parte 10 cubre las actualizaciones Final de navegación y el diseño del sitio, conclusión.</span><span class="sxs-lookup"><span data-stu-id="1d250-109">Part 10 covers Final Updates to Navigation and Site Design, Conclusion.</span></span>


<span data-ttu-id="1d250-110">Hemos completado toda la funcionalidad principal de nuestro sitio, pero todavía tenemos algunas características para agregar a la exploración del sitio, la página principal y la página Examinar de almacén.</span><span class="sxs-lookup"><span data-stu-id="1d250-110">We've completed all the major functionality for our site, but we still have some features to add to the site navigation, the home page, and the Store Browse page.</span></span>

## <a name="creating-the-shopping-cart-summary-partial-view"></a><span data-ttu-id="1d250-111">Crea la vista parcial resumen carro de la compra</span><span class="sxs-lookup"><span data-stu-id="1d250-111">Creating the Shopping Cart Summary Partial View</span></span>

<span data-ttu-id="1d250-112">Debe exponer el número de elementos en el carro de la compra del usuario en todo el sitio.</span><span class="sxs-lookup"><span data-stu-id="1d250-112">We want to expose the number of items in the user's shopping cart across the entire site.</span></span>

![](mvc-music-store-part-10/_static/image1.png)

<span data-ttu-id="1d250-113">Podemos implementar con facilidad esto mediante la creación de una vista parcial que se agrega a nuestro Site.master.</span><span class="sxs-lookup"><span data-stu-id="1d250-113">We can easily implement this by creating a partial view which is added to our Site.master.</span></span>

<span data-ttu-id="1d250-114">Como se muestra anteriormente, el controlador ShoppingCart incluye un método de acción de CartSummary que devuelve una vista parcial:</span><span class="sxs-lookup"><span data-stu-id="1d250-114">As shown previously, the ShoppingCart controller includes a CartSummary action method which returns a partial view:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

<span data-ttu-id="1d250-115">Para crear la vista parcial CartSummary, haga doble clic en la carpeta vistas/ShoppingCart y seleccione Agregar vista.</span><span class="sxs-lookup"><span data-stu-id="1d250-115">To create the CartSummary partial view, right-click on the Views/ShoppingCart folder and select Add View.</span></span> <span data-ttu-id="1d250-116">Nombre de la vista CartSummary y Active la casilla "Crear una vista parcial" tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="1d250-116">Name the view CartSummary and check the "Create a partial view" checkbox as shown below.</span></span>

![](mvc-music-store-part-10/_static/image2.png)

<span data-ttu-id="1d250-117">La vista parcial CartSummary es muy sencilla: es solo un vínculo a la vista de índice ShoppingCart que muestra el número de elementos en el carro.</span><span class="sxs-lookup"><span data-stu-id="1d250-117">The CartSummary partial view is really simple - it's just a link to the ShoppingCart Index view which shows the number of items in the cart.</span></span> <span data-ttu-id="1d250-118">El código completo de CartSummary.cshtml es como sigue:</span><span class="sxs-lookup"><span data-stu-id="1d250-118">The complete code for CartSummary.cshtml is as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

<span data-ttu-id="1d250-119">Podemos incluimos una vista parcial en cualquier página del sitio, incluyendo al patrón de sitio, mediante el método Html.RenderAction.</span><span class="sxs-lookup"><span data-stu-id="1d250-119">We can include a partial view in any page in the site, including the Site master, by using the Html.RenderAction method.</span></span> <span data-ttu-id="1d250-120">RenderAction requiere que especifique el nombre de acción ("CartSummary") y el nombre del controlador ("ShoppingCart") como a continuación.</span><span class="sxs-lookup"><span data-stu-id="1d250-120">RenderAction requires us to specify the Action Name ("CartSummary") and the Controller Name ("ShoppingCart") as below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

<span data-ttu-id="1d250-121">Antes de agregar esto al sitio de diseño, también crearemos el menú de género para que podamos hacer todas nuestras actualizaciones Site.master al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="1d250-121">Before adding this to the site Layout, we will also create the Genre Menu so we can make all of our Site.master updates at one time.</span></span>

## <a name="creating-the-genre-menu-partial-view"></a><span data-ttu-id="1d250-122">Crea la vista parcial del menú de género</span><span class="sxs-lookup"><span data-stu-id="1d250-122">Creating the Genre Menu Partial View</span></span>

<span data-ttu-id="1d250-123">Podemos hacer mucho más fácil para los usuarios a navegar por el almacén mediante la adición de un menú de género que enumera todos los géneros disponibles en el almacén.</span><span class="sxs-lookup"><span data-stu-id="1d250-123">We can make it a lot easier for our users to navigate through the store by adding a Genre Menu which lists all the Genres available in our store.</span></span>

![](mvc-music-store-part-10/_static/image3.png)

<span data-ttu-id="1d250-124">Se sigue los mismos pasos también crea una vista parcial GenreMenu y, a continuación, podemos agregar ambos con el maestro de sitio.</span><span class="sxs-lookup"><span data-stu-id="1d250-124">We will follow the same steps also create a GenreMenu partial view, and then we can add them both to the Site master.</span></span> <span data-ttu-id="1d250-125">En primer lugar, agregue la siguiente acción del controlador GenreMenu a la StoreController:</span><span class="sxs-lookup"><span data-stu-id="1d250-125">First, add the following GenreMenu controller action to the StoreController:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

<span data-ttu-id="1d250-126">Esta acción devuelve una lista de géneros que mostrará la vista parcial, que se creará a continuación.</span><span class="sxs-lookup"><span data-stu-id="1d250-126">This action returns a list of Genres which will be displayed by the partial view, which we will create next.</span></span>

<span data-ttu-id="1d250-127">*Nota: Hemos agregado el atributo [ChildActionOnly] para esta acción de controlador, lo que indica que deseamos solo esta acción puede utilizarse desde una vista parcial. Este atributo evitará que la acción de controlador desde la que se está ejecutando examinando /Store/GenreMenu. Esto no es necesario para las vistas parciales, pero es una buena práctica, puesto que deseamos para asegurarse de que las acciones del controlador se usan como tenemos previsto. También nos estamos devolver PartialView en lugar de la vista, que permite que el motor de vista saber que no debe utilizar el diseño para esta vista, tal y como incluirlo en otras vistas.*</span><span class="sxs-lookup"><span data-stu-id="1d250-127">*Note: We have added the [ChildActionOnly] attribute to this controller action, which indicates that we only want this action to be used from a Partial View. This attribute will prevent the controller action from being executed by browsing to /Store/GenreMenu. This isn't required for partial views, but it is a good practice, since we want to make sure our controller actions are used as we intend. We are also returning PartialView rather than View, which lets the view engine know that it shouldn't use the Layout for this view, as it is being included in other views.*</span></span>

<span data-ttu-id="1d250-128">Haga doble clic en la acción del controlador GenreMenu y crear una vista parcial denominada GenreMenu que está fuertemente tipado mediante la clase de datos de vista de género, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="1d250-128">Right-click on the GenreMenu controller action and create a partial view named GenreMenu which is strongly typed using the Genre view data class as shown below.</span></span>

![](mvc-music-store-part-10/_static/image4.png)

<span data-ttu-id="1d250-129">Actualice el código de la vista para la vista parcial GenreMenu mostrar los elementos mediante una lista sin ordenar como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="1d250-129">Update the view code for the GenreMenu partial view to display the items using an unordered list as follows.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a><span data-ttu-id="1d250-130">Actualizar el diseño de sitio para mostrar nuestro vistas parciales</span><span class="sxs-lookup"><span data-stu-id="1d250-130">Updating Site Layout to display our Partial Views</span></span>

<span data-ttu-id="1d250-131">Podemos agregar nuestro vistas parciales para el diseño del sitio (ovistas/Shared/\_Layout.cshtml) mediante una llamada a Html.RenderAction().</span><span class="sxs-lookup"><span data-stu-id="1d250-131">We can add our partial views to the Site Layout (/Views/Shared/\_Layout.cshtml) by calling Html.RenderAction().</span></span> <span data-ttu-id="1d250-132">Vamos a agregar los dos en, así como algunas marcas adicionales para que se muestren, tal y como se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="1d250-132">We'll add them both in, as well as some additional markup to display them, as shown below:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

<span data-ttu-id="1d250-133">Ahora cuando se ejecuta la aplicación, se verá el género en el área de navegación izquierdo y el resumen de carro en la parte superior.</span><span class="sxs-lookup"><span data-stu-id="1d250-133">Now when we run the application, we will see the Genre in the left navigation area and the Cart Summary at the top.</span></span>

## <a name="update-to-the-store-browse-page"></a><span data-ttu-id="1d250-134">Actualizar a la página Examinar de almacén</span><span class="sxs-lookup"><span data-stu-id="1d250-134">Update to the Store Browse page</span></span>

<span data-ttu-id="1d250-135">La página Examinar de almacén es funcional, pero no presente un buen aspecto.</span><span class="sxs-lookup"><span data-stu-id="1d250-135">The Store Browse page is functional, but doesn't look very good.</span></span> <span data-ttu-id="1d250-136">Podemos actualizar la página para mostrar los álbumes de un mejor diseño actualizando el código de vista (que se encuentra en /Views/Store/Browse.cshtml) como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="1d250-136">We can update the page to show the albums in a better layout by updating the view code (found in /Views/Store/Browse.cshtml) as follows:</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

<span data-ttu-id="1d250-137">Aquí estamos realizando empleo de Url.Action en lugar de Html.ActionLink por lo que podemos aplicar un formato especial al vínculo para incluir la ilustración del álbum.</span><span class="sxs-lookup"><span data-stu-id="1d250-137">Here we are making use of Url.Action rather than Html.ActionLink so that we can apply special formatting to the link to include the album artwork.</span></span>

<span data-ttu-id="1d250-138">*Nota: Estamos mostrando una portada del álbum genérico para estos álbumes. Esta información se almacena en la base de datos y se puede editar mediante el Administrador de almacén. Pueden agregar su propia ilustración.*</span><span class="sxs-lookup"><span data-stu-id="1d250-138">*Note: We are displaying a generic album cover for these albums. This information is stored in the database and is editable via the Store Manager. You are welcome to add your own artwork.*</span></span>

<span data-ttu-id="1d250-139">Ahora cuando se vaya a un género, veremos los álbumes que se muestra en una cuadrícula con la ilustración del álbum.</span><span class="sxs-lookup"><span data-stu-id="1d250-139">Now when we browse to a Genre, we will see the albums shown in a grid with the album artwork.</span></span>

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a><span data-ttu-id="1d250-140">Actualizar la página de inicio para mostrar la parte superior se venden álbumes</span><span class="sxs-lookup"><span data-stu-id="1d250-140">Updating the Home Page to show Top Selling Albums</span></span>

<span data-ttu-id="1d250-141">Deseamos ofrecer nuestro mayor venta álbumes en la página de inicio para aumentar las ventas.</span><span class="sxs-lookup"><span data-stu-id="1d250-141">We want to feature our top selling albums on the home page to increase sales.</span></span> <span data-ttu-id="1d250-142">Para nuestro HomeController para ocuparse de esto y agregar en algunos gráficos adicionales también nos aseguraremos de hacer algunas actualizaciones.</span><span class="sxs-lookup"><span data-stu-id="1d250-142">We'll make some updates to our HomeController to handle that, and add in some additional graphics as well.</span></span>

<span data-ttu-id="1d250-143">En primer lugar, vamos a agregar una propiedad de navegación a nuestra clase álbum para que Entity Framework sepa que está asociados.</span><span class="sxs-lookup"><span data-stu-id="1d250-143">First, we'll add a navigation property to our Album class so that EntityFramework knows that they're associated.</span></span> <span data-ttu-id="1d250-144">Las últimas líneas de nuestro **álbum** clase debe tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="1d250-144">The last few lines of our **Album** class should now look like this:</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

<span data-ttu-id="1d250-145">*Nota: Será necesario agregar un uso de la instrucción que se va a poner en el espacio de nombres System.Collections.Generic.*</span><span class="sxs-lookup"><span data-stu-id="1d250-145">*Note: This will require adding a using statement to bring in the System.Collections.Generic namespace.*</span></span>

<span data-ttu-id="1d250-146">En primer lugar, vamos a agregar un campo de storeDB y la MvcMusicStore.Models mediante instrucciones, como en nuestros otros controladores.</span><span class="sxs-lookup"><span data-stu-id="1d250-146">First, we'll add a storeDB field and the MvcMusicStore.Models using statements, as in our other controllers.</span></span> <span data-ttu-id="1d250-147">A continuación, agregaremos el método siguiente a HomeController que consulta nuestra base de datos para buscar los álbumes de ventas superiores según OrderDetails.</span><span class="sxs-lookup"><span data-stu-id="1d250-147">Next, we'll add the following method to the HomeController which queries our database to find top selling albums according to OrderDetails.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

<span data-ttu-id="1d250-148">Se trata de un método privado, ya que no queremos que esté disponible como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="1d250-148">This is a private method, since we don't want to make it available as a controller action.</span></span> <span data-ttu-id="1d250-149">Incluiremos en HomeController por motivos de simplicidad, pero se recomienda mover la lógica de negocios en clases de servicio independiente según corresponda.</span><span class="sxs-lookup"><span data-stu-id="1d250-149">We are including it in the HomeController for simplicity, but you are encouraged to move your business logic into separate service classes as appropriate.</span></span>

<span data-ttu-id="1d250-150">Teniendo esto en su lugar, podemos actualizar la acción de controlador de índice para consultar las principales 5 venta álbumes y devolverlos a la vista.</span><span class="sxs-lookup"><span data-stu-id="1d250-150">With that in place, we can update the Index controller action to query the top 5 selling albums and return them to the view.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

<span data-ttu-id="1d250-151">El código completo de HomeController actualizado es tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="1d250-151">The complete code for the updated HomeController is as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

<span data-ttu-id="1d250-152">Por último, necesitaremos actualizar la vista de índice de inicio para que pueda mostrar una lista de álbumes actualizando el tipo de modelo y agregar la lista de álbumes a la parte inferior.</span><span class="sxs-lookup"><span data-stu-id="1d250-152">Finally, we'll need to update our Home Index view so that it can display a list of albums by updating the Model type and adding the album list to the bottom.</span></span> <span data-ttu-id="1d250-153">Tomaremos esta oportunidad para agregar un encabezado y una sección de promoción a la página.</span><span class="sxs-lookup"><span data-stu-id="1d250-153">We will take this opportunity to also add a heading and a promotion section to the page.</span></span>

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

<span data-ttu-id="1d250-154">Ahora, cuando se ejecuta la aplicación, veremos nuestra página de principal actualizada con álbumes de ventas superiores y el mensaje de la promoción.</span><span class="sxs-lookup"><span data-stu-id="1d250-154">Now when we run the application, we'll see our updated home page with top selling albums and our promotional message.</span></span>

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a><span data-ttu-id="1d250-155">Conclusión</span><span class="sxs-lookup"><span data-stu-id="1d250-155">Conclusion</span></span>

<span data-ttu-id="1d250-156">Hemos visto que ASP.NET MVC facilita el proceso para crear un sitio Web sofisticado con acceso de base de datos de pertenencia, AJAX, etcetera.</span><span class="sxs-lookup"><span data-stu-id="1d250-156">We've seen that ASP.NET MVC makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="1d250-157">muy rápidamente.</span><span class="sxs-lookup"><span data-stu-id="1d250-157">pretty quickly.</span></span> <span data-ttu-id="1d250-158">Espero que este tutorial le ha concedido las herramientas que necesita para empezar a compilar las aplicaciones de su propia ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="1d250-158">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET MVC applications!</span></span>


> [!div class="step-by-step"]
> [<span data-ttu-id="1d250-159">Anterior</span><span class="sxs-lookup"><span data-stu-id="1d250-159">Previous</span></span>](mvc-music-store-part-9.md)
