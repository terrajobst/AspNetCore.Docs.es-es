---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Agregar redes sociales para ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft
author: tfitzmac
description: Este capítulo explica cómo integrar su sitio con los servicios de red sociales. En este capítulo, aprenderá cómo permitir a los usuarios o vínculo de marcador del sitio Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2d1f0074edf473c4be06adaa32598dd828a7552c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a><span data-ttu-id="f1948-104">Agregar redes sociales a ASP.NET Web Pages (Razor) sitios</span><span class="sxs-lookup"><span data-stu-id="f1948-104">Adding Social Networking to ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="f1948-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f1948-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f1948-106">En este artículo se explica cómo agregar vínculos de redes sociales de Facebook, Twitter, Reddit y Digg a páginas de un sitio Web de ASP.NET Web Pages (Razor) y cómo incluir fuentes de Twitter, tarjetas de juegos de Xbox y Gravatar imágenes.</span><span class="sxs-lookup"><span data-stu-id="f1948-106">This article explains how to add social networking links for Facebook, Twitter, Reddit, and Digg to pages in an ASP.NET Web Pages (Razor) website, and how to include Twitter feeds, Xbox gamer cards, and Gravatar images.</span></span>
> 
> <span data-ttu-id="f1948-107">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="f1948-107">What you'll learn:</span></span>
> 
> - <span data-ttu-id="f1948-108">Cómo permitir a los usuarios o vínculo de marcador su sitio.</span><span class="sxs-lookup"><span data-stu-id="f1948-108">How to let people bookmark/link your site.</span></span>
> - <span data-ttu-id="f1948-109">Cómo agregar una fuente de Twitter.</span><span class="sxs-lookup"><span data-stu-id="f1948-109">How to add a Twitter feed.</span></span>
> - <span data-ttu-id="f1948-110">Cómo agregar un Facebook **como** botón a las páginas.</span><span class="sxs-lookup"><span data-stu-id="f1948-110">How to add a Facebook **Like** button to pages.</span></span>
> - <span data-ttu-id="f1948-111">Cómo representar imágenes Gravatar.com.</span><span class="sxs-lookup"><span data-stu-id="f1948-111">How to render Gravatar.com images.</span></span>
> - <span data-ttu-id="f1948-112">Cómo mostrar una tarjeta de juegos de Xbox en su sitio.</span><span class="sxs-lookup"><span data-stu-id="f1948-112">How to display an Xbox gamer card on your site.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f1948-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="f1948-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f1948-114">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="f1948-114">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="f1948-115">Biblioteca auxiliar de Web ASP.NET (paquete de NuGet)</span><span class="sxs-lookup"><span data-stu-id="f1948-115">ASP.NET Web Helper Library (NuGet package)</span></span>
>   
> 
> <span data-ttu-id="f1948-116">Este tutorial también funciona con ASP.NET Web Pages 3, salvo los elementos que utilizan la biblioteca de aplicación auxiliar de Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1948-116">This tutorial also works with ASP.NET Web Pages 3, except for parts that use the ASP.NET Web Helper Library.</span></span>


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a><span data-ttu-id="f1948-117">Vinculación de su sitio Web en los sitios de redes sociales</span><span class="sxs-lookup"><span data-stu-id="f1948-117">Linking Your Website on Social Networking Sites</span></span>

<span data-ttu-id="f1948-118">Si las personas les gusta algo en su sitio, a menudo desea compartirlo con amigos.</span><span class="sxs-lookup"><span data-stu-id="f1948-118">If people like something on your site, they often want to share it with friends.</span></span> <span data-ttu-id="f1948-119">Puede facilitar este proceso al mostrar glifos (iconos) que se hace clic para compartir una página en Digg, Reddit, Facebook, Twitter o sitios similares.</span><span class="sxs-lookup"><span data-stu-id="f1948-119">You can make this easy by displaying glyphs (icons) that people can click to share a page on Digg, Reddit, Facebook, Twitter, or similar sites.</span></span>

<span data-ttu-id="f1948-120">Para mostrar estos glifos, agregue el `LinkSharecode` auxiliar a una página.</span><span class="sxs-lookup"><span data-stu-id="f1948-120">To display these glyphs, add the `LinkSharecode` helper to a page.</span></span> <span data-ttu-id="f1948-121">Personas que visitan la página hacer clic en un glifo individual.</span><span class="sxs-lookup"><span data-stu-id="f1948-121">People who visit your page can click an individual glyph.</span></span> <span data-ttu-id="f1948-122">Si tienen una cuenta con ese sitio de red social, a continuación, puede publicar un vínculo a la página en ese sitio.</span><span class="sxs-lookup"><span data-stu-id="f1948-122">If they have an account with that social networking site, they can then post a link to your page on that site.</span></span>

![Imagen 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. <span data-ttu-id="f1948-124">Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no ha agregado lo - crear una página denominada *ListLinkShare.cshtml* y agregar el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="f1948-124">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.- Create a page named *ListLinkShare.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    <span data-ttu-id="f1948-125">En este ejemplo, cuando el `LinkShare` auxiliar se ejecuta, el título de página se pasa como un parámetro, que a su vez pasa el título de página en el sitio de red social.</span><span class="sxs-lookup"><span data-stu-id="f1948-125">In this example, when the `LinkShare` helper runs, the page title is passed as a parameter, which in turn passes the page title to the social networking site.</span></span> <span data-ttu-id="f1948-126">Sin embargo, podría pasar en cualquier cadena que se desee.</span><span class="sxs-lookup"><span data-stu-id="f1948-126">However, you could pass in any string you want.</span></span> <span data-ttu-id="f1948-127">En este ejemplo también especifica qué sitios de redes sociales para incluir en la lista.</span><span class="sxs-lookup"><span data-stu-id="f1948-127">This example also specifies which social networking sites to include in the list.</span></span> <span data-ttu-id="f1948-128">Puede especificar los sitios de redes sociales que son relevantes para su sitio.</span><span class="sxs-lookup"><span data-stu-id="f1948-128">You can specify the social networking sites that are relevant to your site.</span></span>
2. <span data-ttu-id="f1948-129">Ejecute el *ListLinkShare.cshtml* página en un explorador.</span><span class="sxs-lookup"><span data-stu-id="f1948-129">Run the *ListLinkShare.cshtml* page in a browser.</span></span> <span data-ttu-id="f1948-130">(Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)</span><span class="sxs-lookup"><span data-stu-id="f1948-130">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
3. <span data-ttu-id="f1948-131">Haga clic en un glifo para uno de los sitios que ha iniciado sesión en.</span><span class="sxs-lookup"><span data-stu-id="f1948-131">Click a glyph for one of the sites that you're signed up for.</span></span> <span data-ttu-id="f1948-132">El vínculo le lleva a la página en el sitio de red social seleccionado donde se pueden compartir un vínculo.</span><span class="sxs-lookup"><span data-stu-id="f1948-132">The link takes you to the page on the selected social network site where you can share a link.</span></span> <span data-ttu-id="f1948-133">Por ejemplo, si hace clic en el vínculo de Reddit, llega a la `submit to reddit` página en el sitio Web Reddit.</span><span class="sxs-lookup"><span data-stu-id="f1948-133">For example, if you click the Reddit link, you're taken to the `submit to reddit` page on the Reddit website.</span></span>

     ![Imagen 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a><span data-ttu-id="f1948-135">Agregar un Twitter fuente</span><span class="sxs-lookup"><span data-stu-id="f1948-135">Adding a Twitter Feed</span></span>

<span data-ttu-id="f1948-136">Para obtener información sobre el uso de una aplicación auxiliar de Twitter que sea compatible con la versión actual de la API de Twitter, vea [aplicación auxiliar de Twitter](../ui-layouts-and-themes/twitter-helper.md).</span><span class="sxs-lookup"><span data-stu-id="f1948-136">For information about using a Twitter helper that is compatible with the current version of the Twitter API, see [Twitter helper](../ui-layouts-and-themes/twitter-helper.md).</span></span> <span data-ttu-id="f1948-137">En este ejemplo se muestra cómo escribir su propia aplicación auxiliar de modo que pueda reutilizar fácilmente el código de muchas páginas.</span><span class="sxs-lookup"><span data-stu-id="f1948-137">This example shows how to write your own helper so you can easily reuse the code from many pages.</span></span>

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a><span data-ttu-id="f1948-138">Mostrar un Facebook &quot;como&quot; botón</span><span class="sxs-lookup"><span data-stu-id="f1948-138">Displaying a Facebook &quot;Like&quot; Button</span></span>

<span data-ttu-id="f1948-139">En algunos casos, la mejor opción es obtener el código directamente desde el proveedor de redes sociales, en lugar de confiar en una aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1948-139">In some cases, your best option is to get the code directly from the social networking provider rather than relying on a helper.</span></span> <span data-ttu-id="f1948-140">Esto es especialmente cierto si el proveedor de red social actualiza sus opciones más rápidamente de lo que se actualiza la aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1948-140">This is especially true if the social network provider updates its options more quickly than the helper is updated.</span></span>

<span data-ttu-id="f1948-141">Para agregar características de Facebook (por ejemplo, el botón Like) a su sitio, puede recuperar los fragmentos de código de la [developers.facebook.com](https://developers.facebook.com/) sitio.</span><span class="sxs-lookup"><span data-stu-id="f1948-141">To add Facebook features (such as the Like button) to your site, you can retrieve code snippets from the [developers.facebook.com](https://developers.facebook.com/) site.</span></span> <span data-ttu-id="f1948-142">En el sitio de Facebook, usando sus herramientas para generar un fragmento de código que es relevante para el sitio.</span><span class="sxs-lookup"><span data-stu-id="f1948-142">On the Facebook site, you use their tools to generate a code snippet that is relevant to your site.</span></span>

<span data-ttu-id="f1948-143">El código resaltado siguiente es el código que se recuperó de la herramienta como botón en el sitio developers.facebook.com.</span><span class="sxs-lookup"><span data-stu-id="f1948-143">The following highlighted code is the code that was retrieved from the Like Button tool on the developers.facebook.com site.</span></span> <span data-ttu-id="f1948-144">Debe proporcionar su propio identificador de aplicación.</span><span class="sxs-lookup"><span data-stu-id="f1948-144">You must provide your own app ID.</span></span>

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a><span data-ttu-id="f1948-145">Representación de una imagen Gravatar</span><span class="sxs-lookup"><span data-stu-id="f1948-145">Rendering a Gravatar Image</span></span>

<span data-ttu-id="f1948-146">A *Gravatar* (un &quot;avatar globalmente reconocido&quot;) es una imagen que se puede usar en varios sitios Web como su avatar &#8212; es decir, una imagen que le represente.</span><span class="sxs-lookup"><span data-stu-id="f1948-146">A *Gravatar* (a &quot;globally recognized avatar&quot;) is an image that can be used on multiple websites as your avatar &#8212; that is, an image that represents you.</span></span> <span data-ttu-id="f1948-147">Por ejemplo, un Gravatar puede identificar a una persona con una entrada de foro, un comentario de blog y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="f1948-147">For example, a Gravatar can identify a person in a forum post, in a blog comment, and so on.</span></span> <span data-ttu-id="f1948-148">(Puede registrar su propio Gravatar en el sitio Web de Gravatar en [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Si desea mostrar imágenes junto a los nombres de personas o direcciones de correo electrónico en el sitio Web, puede usar la aplicación auxiliar Gravatar.</span><span class="sxs-lookup"><span data-stu-id="f1948-148">(You can register your own Gravatar at the Gravatar website at [http://www.gravatar.com/](http://www.gravatar.com/).) If you want to display images next to people's names or email addresses on your website, you can use the Gravatar helper.</span></span>

<span data-ttu-id="f1948-149">En este ejemplo, está usando un Gravatar único que representa a sí mismo.</span><span class="sxs-lookup"><span data-stu-id="f1948-149">In this example, you're using a single Gravatar that represents yourself.</span></span> <span data-ttu-id="f1948-150">Otra manera de usar un Gravatar es permitir a los usuarios especificar su dirección Gravatar cuando registran en su sitio.</span><span class="sxs-lookup"><span data-stu-id="f1948-150">Another way to use a Gravatar is to let people specify their Gravatar address when they register on your site.</span></span> <span data-ttu-id="f1948-151">(Puede obtener información sobre cómo permitir a los usuarios registrar en [agregar seguridad y la pertenencia a un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202904).) A continuación, cada vez que se muestra información para ese usuario, puede agregar simplemente la Gravatar a donde se muestra el nombre del usuario.</span><span class="sxs-lookup"><span data-stu-id="f1948-151">(You can learn how to let people register in [Adding Security and Membership to an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=202904).) Then whenever you display information for that user, you can just add the Gravatar to where you display the user's name.</span></span>

1. <span data-ttu-id="f1948-152">Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="f1948-152">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="f1948-153">Crear una nueva página web denominada *Gravatar.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f1948-153">Create a new web page named *Gravatar.cshtml*.</span></span>
3. <span data-ttu-id="f1948-154">Agregue el siguiente marcado al archivo:</span><span class="sxs-lookup"><span data-stu-id="f1948-154">Add the following markup to the file:</span></span> 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    <span data-ttu-id="f1948-155">El `Gravatar.GetHtml` método muestra la imagen de Gravatar en la página.</span><span class="sxs-lookup"><span data-stu-id="f1948-155">The `Gravatar.GetHtml` method displays the Gravatar image on the page.</span></span> <span data-ttu-id="f1948-156">Para cambiar el tamaño de la imagen, puede incluir un número como un segundo parámetro.</span><span class="sxs-lookup"><span data-stu-id="f1948-156">To change the size of the image, you can include a number as a second parameter.</span></span> <span data-ttu-id="f1948-157">El tamaño predeterminado es 80.</span><span class="sxs-lookup"><span data-stu-id="f1948-157">The default size is 80.</span></span> <span data-ttu-id="f1948-158">Los números de inferior a 80 hacer la imagen más pequeños.</span><span class="sxs-lookup"><span data-stu-id="f1948-158">Numbers less than 80 make the image smaller.</span></span> <span data-ttu-id="f1948-159">Un número superior a 80 ampliar la imagen.</span><span class="sxs-lookup"><span data-stu-id="f1948-159">Numbers greater than 80 make the image larger.</span></span>
4. <span data-ttu-id="f1948-160">En el `Gravatar.GetHtml` reemplazar métodos, `<Your Gravatar account here>` con la dirección de correo electrónico que usas para su cuenta de Gravatar.</span><span class="sxs-lookup"><span data-stu-id="f1948-160">In the `Gravatar.GetHtml` methods, replace `<Your Gravatar account here>` with the email address that you use for your Gravatar account.</span></span> <span data-ttu-id="f1948-161">(Si no tiene una cuenta de Gravatar, puede utilizar la dirección de correo electrónico de alguien que lo tiene).</span><span class="sxs-lookup"><span data-stu-id="f1948-161">(If you don't have a Gravatar account, you can use the email address of someone who does.)</span></span>
5. <span data-ttu-id="f1948-162">Ejecute la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f1948-162">Run the page in your browser.</span></span> <span data-ttu-id="f1948-163">La página muestra dos imágenes de Gravatar para la dirección de correo electrónico que especificó.</span><span class="sxs-lookup"><span data-stu-id="f1948-163">The page displays two Gravatar images for the email address you specified.</span></span> <span data-ttu-id="f1948-164">La segunda imagen es menor que el primero.</span><span class="sxs-lookup"><span data-stu-id="f1948-164">The second image is smaller than the first.</span></span> 

    ![Imagen 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a><span data-ttu-id="f1948-166">Mostrar una tarjeta de juegos de Xbox</span><span class="sxs-lookup"><span data-stu-id="f1948-166">Displaying an Xbox Gamer Card</span></span>

<span data-ttu-id="f1948-167">Cuando las personas jugar Microsoft Xbox en línea, cada usuario tiene un identificador único.</span><span class="sxs-lookup"><span data-stu-id="f1948-167">When people play Microsoft Xbox games online, each user has a unique ID.</span></span> <span data-ttu-id="f1948-168">Las estadísticas se mantienen para cada jugador en forma de una tarjeta jugador, que muestra su reputación, puntuación jugador y juegos ejecutados recientemente.</span><span class="sxs-lookup"><span data-stu-id="f1948-168">Statistics are kept for each player in the form of a gamer card, which shows their reputation, gamer score, and recently played games.</span></span> <span data-ttu-id="f1948-169">Si eres una jugador Xbox, puede mostrar la tarjeta jugador en páginas en el sitio mediante el `GamerCard` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="f1948-169">If you're an Xbox gamer, you can show your gamer card on pages in your site by using the `GamerCard` helper.</span></span>

1. <span data-ttu-id="f1948-170">Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si no lo ha hecho ya.</span><span class="sxs-lookup"><span data-stu-id="f1948-170">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already.</span></span>
2. <span data-ttu-id="f1948-171">Crear una nueva página denominada *XboxGamer.cshtml* y agregue el siguiente marcado.</span><span class="sxs-lookup"><span data-stu-id="f1948-171">Create a new page named *XboxGamer.cshtml* and add the following markup.</span></span>

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    <span data-ttu-id="f1948-172">Usa el `GamerCard.GetHtml` propiedad para especificar el alias de la tarjeta jugador para mostrarse.</span><span class="sxs-lookup"><span data-stu-id="f1948-172">You use the `GamerCard.GetHtml` property to specify the alias for the gamer card to be displayed.</span></span>
3. <span data-ttu-id="f1948-173">Ejecute la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="f1948-173">Run the page in your browser.</span></span> <span data-ttu-id="f1948-174">La página muestra la tarjeta de juegos de Xbox que especificó.</span><span class="sxs-lookup"><span data-stu-id="f1948-174">The page displays the Xbox gamer card that you specified.</span></span>

    ![Imagen 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
