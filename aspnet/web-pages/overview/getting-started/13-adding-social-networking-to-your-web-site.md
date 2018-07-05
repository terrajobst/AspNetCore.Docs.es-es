---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Agregar redes sociales para ASP.NET Web Pages (Razor) sitios | Microsoft Docs
author: tfitzmac
description: Este capítulo explica cómo integrar su sitio con servicios de redes sociales. En este capítulo, obtendrá información sobre cómo permitir a los usuarios de su sitio Web de marcador o vínculo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 4f987c9022056ddfa3cdcac746f688f562d9003e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398406"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="00fe2-104">Agregar redes sociales a ASP.NET Web Pages (Razor) sitios</span><span class="sxs-lookup"><span data-stu-id="00fe2-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="00fe2-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="00fe2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="00fe2-106">En este artículo se explica cómo agregar vínculos de redes sociales para Facebook, Twitter, Reddit y Digg a páginas en un sitio Web de ASP.NET Web Pages (Razor) y cómo incluir imágenes Gravatar, tarjetas jugadores de Xbox y fuentes de Twitter.</span><span class="sxs-lookup"><span data-stu-id="00fe2-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="00fe2-107">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="00fe2-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="00fe2-108">Cómo permitir a los usuarios de su sitio de marcador o vínculo.</span><span class="sxs-lookup"><span data-stu-id="00fe2-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="00fe2-109">Cómo agregar una fuente de Twitter.</span><span class="sxs-lookup"><span data-stu-id="00fe2-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="00fe2-110">Cómo agregar un Facebook **como** botón a las páginas.</span><span class="sxs-lookup"><span data-stu-id="00fe2-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="00fe2-111">Cómo representar imágenes Gravatar.com.</span><span class="sxs-lookup"><span data-stu-id="00fe2-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="00fe2-112">Cómo mostrar una tarjeta jugador de Xbox en su sitio.</span><span class="sxs-lookup"><span data-stu-id="00fe2-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="00fe2-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="00fe2-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="00fe2-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="00fe2-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="00fe2-115">Biblioteca auxiliar de Web ASP.NET (paquete de NuGet)</span><span class="sxs-lookup"><span data-stu-id="00fe2-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="00fe2-116">En este tutorial también funciona con 3 de ASP.NET Web Pages, excepto las partes que usan la biblioteca auxiliar de Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="00fe2-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="00fe2-117">Vinculación de su sitio Web en sitios de redes sociales</span><span class="sxs-lookup"><span data-stu-id="00fe2-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="00fe2-118">Si personas les gusta algo en su sitio, a menudo desea compartirlo con amigos.</span><span class="sxs-lookup"><span data-stu-id="00fe2-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="00fe2-119">Se puede facilitar este proceso al mostrar glifos (iconos) que las personas pueden hacer clic para compartir una página en Digg, Reddit, Facebook, Twitter o sitios similares.</span><span class="sxs-lookup"><span data-stu-id="00fe2-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="00fe2-120">Para mostrar estos glifos, agregue el `LinkSharecode` auxiliar a una página.</span><span class="sxs-lookup"><span data-stu-id="00fe2-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="00fe2-121">Las personas que visitan su página de hacer clic en un glifo individual.</span><span class="sxs-lookup"><span data-stu-id="00fe2-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="00fe2-122">Si tienen una cuenta con ese sitio de redes sociales, a continuación, puede publicar un vínculo a la página en ese sitio.</span><span class="sxs-lookup"><span data-stu-id="00fe2-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Imagen 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="00fe2-124">Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no ha agregado lo - cree una página denominada *ListLinkShare.cshtml* y agregar el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="00fe2-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="00fe2-125">En este ejemplo, cuando el `LinkShare` ejecuciones de aplicación auxiliar, el título de página se pasa como un parámetro, que a su vez pasa el título de página en el sitio de redes sociales.</span><span class="sxs-lookup"><span data-stu-id="00fe2-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="00fe2-126">Sin embargo, podría pasar en cualquier cadena que desee.</span><span class="sxs-lookup"><span data-stu-id="00fe2-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="00fe2-127">En este ejemplo también especifica qué sitios de redes sociales para incluir en la lista.</span><span class="sxs-lookup"><span data-stu-id="00fe2-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="00fe2-128">Puede especificar los sitios de redes sociales que son relevantes para su sitio.</span><span class="sxs-lookup"><span data-stu-id="00fe2-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="00fe2-129">Ejecute el *ListLinkShare.cshtml* página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="00fe2-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="00fe2-130">(Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)</span><span class="sxs-lookup"><span data-stu-id="00fe2-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="00fe2-131">Haga clic en un glifo para uno de los sitios que se están suscrito.</span><span class="sxs-lookup"><span data-stu-id="00fe2-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="00fe2-132">El vínculo le lleva a la página en el sitio de red social seleccionado en el que puede compartir un vínculo.</span><span class="sxs-lookup"><span data-stu-id="00fe2-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="00fe2-133">Por ejemplo, si hace clic en el vínculo de Reddit, pasamos a la `submit to reddit` página en el sitio Web de Reddit.</span><span class="sxs-lookup"><span data-stu-id="00fe2-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![Imagen 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="00fe2-135">Agregar una Twitter fuente</span><span class="sxs-lookup"><span data-stu-id="00fe2-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="00fe2-136">Para obtener información sobre el uso de una aplicación auxiliar de Twitter que es compatible con la versión actual de la API de Twitter, consulte [aplicación auxiliar de Twitter](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="00fe2-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="00fe2-137">En este ejemplo se muestra cómo escribir su propia aplicación auxiliar de modo que pueda reutilizar fácilmente el código de muchas páginas.</span><span class="sxs-lookup"><span data-stu-id="00fe2-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="00fe2-138">Mostrar un Facebook &quot;como&quot; botón</span><span class="sxs-lookup"><span data-stu-id="00fe2-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="00fe2-139">En algunos casos, la mejor opción es obtener el código directamente desde el proveedor de redes sociales, en lugar de depender de una aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="00fe2-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="00fe2-140">Esto es especialmente cierto si el proveedor de red social actualiza sus opciones de forma más rápida de la aplicación auxiliar se actualiza.</span><span class="sxs-lookup"><span data-stu-id="00fe2-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="00fe2-141">Para agregar características de Facebook (por ejemplo, el botón Like) a su sitio, puede recuperar fragmentos de código desde el [developers.facebook.com](https://developers.facebook.com/) sitio.</span><span class="sxs-lookup"><span data-stu-id="00fe2-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="00fe2-142">En el sitio de Facebook, usa sus herramientas para generar un fragmento de código que es relevante para el sitio.</span><span class="sxs-lookup"><span data-stu-id="00fe2-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="00fe2-143">El siguiente código resaltado es el código que se recuperó de la herramienta de botón, como en el sitio developers.facebook.com.</span><span class="sxs-lookup"><span data-stu-id="00fe2-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="00fe2-144">Debe proporcionar su propio identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="00fe2-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="00fe2-145">Representación de una imagen de Gravatar</span><span class="sxs-lookup"><span data-stu-id="00fe2-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="00fe2-146">Un *Gravatar* (un &quot;avatar globalmente reconocido&quot;) es una imagen que se puede usar en varios sitios Web como su avatar &#8212; es decir, una imagen que se representa.</span><span class="sxs-lookup"><span data-stu-id="00fe2-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="00fe2-147">Por ejemplo, un Gravatar puede identificar a una persona en un foro, en un comentario en el blog y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="00fe2-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="00fe2-148">(Puede registrar su propio Gravatar en el sitio Web de Gravatar en [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Si desea mostrar imágenes junto a los nombres de personas o direcciones de correo electrónico en su sitio Web, puede usar la aplicación auxiliar de Gravatar.</span><span class="sxs-lookup"><span data-stu-id="00fe2-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="00fe2-149">En este ejemplo, está usando un Gravatar único que representa a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="00fe2-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="00fe2-150">Otra forma de usar un Gravatar es permitir a los usuarios especificar su dirección de Gravatar al registrarse en el sitio.</span><span class="sxs-lookup"><span data-stu-id="00fe2-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="00fe2-151">(Puede aprender a permitir a los usuarios registrar en [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).) A continuación, siempre que se muestra información de ese usuario, puede agregar el Gravatar a donde se muestra el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="00fe2-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="00fe2-152">Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="00fe2-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="00fe2-153">Crear una nueva página web denominada *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="00fe2-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="00fe2-154">Agregue el marcado siguiente al archivo:</span><span class="sxs-lookup"><span data-stu-id="00fe2-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="00fe2-155">El `Gravatar.GetHtml` método muestra la imagen de Gravatar en la página.</span><span class="sxs-lookup"><span data-stu-id="00fe2-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="00fe2-156">Para cambiar el tamaño de la imagen, puede incluir un número como segundo parámetro.</span><span class="sxs-lookup"><span data-stu-id="00fe2-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="00fe2-157">El tamaño predeterminado es 80.</span><span class="sxs-lookup"><span data-stu-id="00fe2-157">The default size is 80.</span></span> <span data-ttu-id="00fe2-158">Los números de menos de 80 make la imagen más pequeños.</span><span class="sxs-lookup"><span data-stu-id="00fe2-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="00fe2-159">Un número superior a 80 ampliar la imagen.</span><span class="sxs-lookup"><span data-stu-id="00fe2-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="00fe2-160">En el `Gravatar.GetHtml` reemplazar métodos, `<Your Gravatar account here>` con la dirección de correo electrónico que usó para la cuenta de Gravatar.</span><span class="sxs-lookup"><span data-stu-id="00fe2-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="00fe2-161">(Si no tienes una cuenta de Gravatar, puede utilizar la dirección de correo electrónico de alguien que lo tenga).</span><span class="sxs-lookup"><span data-stu-id="00fe2-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="00fe2-162">Ejecute la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="00fe2-162">Run the page in your browser.</span></span> <span data-ttu-id="00fe2-163">La página muestra dos imágenes Gravatar para la dirección de correo electrónico especificada.</span><span class="sxs-lookup"><span data-stu-id="00fe2-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="00fe2-164">La segunda imagen es menor que el primero.</span><span class="sxs-lookup"><span data-stu-id="00fe2-164">The second image is smaller than the first.</span></span> 

    ![Imagen 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="00fe2-166">Mostrar una tarjeta jugador de Xbox</span><span class="sxs-lookup"><span data-stu-id="00fe2-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="00fe2-167">Cuando la gente juega Microsoft Xbox en línea, cada usuario tiene un identificador único.</span><span class="sxs-lookup"><span data-stu-id="00fe2-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="00fe2-168">Las estadísticas se mantienen para cada jugador en forma de una tarjeta de jugador, que muestra su reputación, la puntuación de jugador y juegos ejecutados recientemente.</span><span class="sxs-lookup"><span data-stu-id="00fe2-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="00fe2-169">Si es una jugador de Xbox, puede mostrar la tarjeta de jugador de páginas de su sitio mediante el `GamerCard` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="00fe2-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="00fe2-170">Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="00fe2-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="00fe2-171">Crear una nueva página denominada *XboxGamer.cshtml* y agregue el marcado siguiente.</span><span class="sxs-lookup"><span data-stu-id="00fe2-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="00fe2-172">Usa el `GamerCard.GetHtml` propiedad para especificar el alias de la tarjeta de jugador que se mostrará.</span><span class="sxs-lookup"><span data-stu-id="00fe2-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="00fe2-173">Ejecute la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="00fe2-173">Run the page in your browser.</span></span> <span data-ttu-id="00fe2-174">La página muestra la tarjeta jugador de Xbox que especificó.</span><span class="sxs-lookup"><span data-stu-id="00fe2-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Imagen 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
