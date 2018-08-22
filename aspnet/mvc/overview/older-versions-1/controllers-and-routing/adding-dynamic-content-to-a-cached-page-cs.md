---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Agregar contenido dinámico a una página almacenada en caché (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo mezclar contenido dinámico y almacenado en caché en la misma página. La substitución posterior a la caché le permite mostrar contenido dinámico, como o los anuncios de pancarta...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: a03f943b936c68215d65dca92e62431642226993
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827776"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="bf79a-104">Agregar contenido dinámico a una página almacenada en caché (C#)</span><span class="sxs-lookup"><span data-stu-id="bf79a-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="bf79a-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bf79a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="bf79a-106">Obtenga información sobre cómo mezclar contenido dinámico y almacenado en caché en la misma página.</span><span class="sxs-lookup"><span data-stu-id="bf79a-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="bf79a-107">La substitución posterior a la caché le permite mostrar contenido dinámico, como anuncios de pancarta o elementos de noticias, dentro de una página que ha sido de salida almacenados en caché.</span><span class="sxs-lookup"><span data-stu-id="bf79a-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="bf79a-108">Aprovechando las ventajas de la caché de resultados, puede mejorar considerablemente el rendimiento de una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="bf79a-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="bf79a-109">En lugar de volver a generar una página cada vez que se solicita la página, la página se puede generar una vez y almacena en caché en memoria para varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="bf79a-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="bf79a-110">Pero hay un problema.</span><span class="sxs-lookup"><span data-stu-id="bf79a-110">But there is a problem.</span></span> <span data-ttu-id="bf79a-111">¿Qué ocurre si necesita mostrar contenido dinámico en la página?</span><span class="sxs-lookup"><span data-stu-id="bf79a-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="bf79a-112">Por ejemplo, imagine que desea mostrar un anuncio de banner en la página.</span><span class="sxs-lookup"><span data-stu-id="bf79a-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="bf79a-113">No desea que el anuncio de banner en la memoria caché para que cada usuario ve el anuncio mismo.</span><span class="sxs-lookup"><span data-stu-id="bf79a-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="bf79a-114">No generar dinero de este modo.</span><span class="sxs-lookup"><span data-stu-id="bf79a-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="bf79a-115">Afortunadamente, hay una solución sencilla.</span><span class="sxs-lookup"><span data-stu-id="bf79a-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="bf79a-116">Puede aprovechar una característica de ASP.NET framework llama *sustitución tras la caché*.</span><span class="sxs-lookup"><span data-stu-id="bf79a-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="bf79a-117">La substitución posterior a la caché permite sustituir el contenido dinámico en una página que se ha almacenado en memoria.</span><span class="sxs-lookup"><span data-stu-id="bf79a-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="bf79a-118">Normalmente, cuando la salida de una página de caché mediante el atributo [OutputCache], la página se almacena en caché en el servidor y el cliente (el explorador web).</span><span class="sxs-lookup"><span data-stu-id="bf79a-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="bf79a-119">Cuando se usa la sustitución posterior a la caché, se almacena en caché una página solo en el servidor.</span><span class="sxs-lookup"><span data-stu-id="bf79a-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="bf79a-120">Uso de la sustitución posterior a la caché</span><span class="sxs-lookup"><span data-stu-id="bf79a-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="bf79a-121">Uso de la sustitución posterior a la caché, requiere dos pasos.</span><span class="sxs-lookup"><span data-stu-id="bf79a-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="bf79a-122">En primer lugar, deberá definir un método que devuelve una cadena que representa el contenido dinámico que desea mostrar en la página en caché.</span><span class="sxs-lookup"><span data-stu-id="bf79a-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="bf79a-123">A continuación, llame al método HttpResponse.WriteSubstitution() para insertar el contenido dinámico en la página.</span><span class="sxs-lookup"><span data-stu-id="bf79a-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="bf79a-124">Por ejemplo, imagínese que desea mostrar los elementos de noticias diferente aleatoriamente en una página almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="bf79a-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="bf79a-125">La clase en el listado 1 expone un solo método, denominado RenderNews(), que devuelve aleatoriamente un artículo de noticias de una lista de tres elementos de noticias.</span><span class="sxs-lookup"><span data-stu-id="bf79a-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="bf79a-126">**Listado 1 – Models\News.cs**</span><span class="sxs-lookup"><span data-stu-id="bf79a-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="bf79a-127">Para aprovechar las ventajas de la sustitución posterior a la caché, se llama al método de HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="bf79a-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="bf79a-128">El método WriteSubstitution() establece el código para reemplazar una región de la página en caché con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="bf79a-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="bf79a-129">El método WriteSubstitution() se usa para mostrar el elemento de noticias aleatorio en la vista en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="bf79a-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="bf79a-130">**Listado 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="bf79a-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="bf79a-131">El método RenderNews se pasa al método WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="bf79a-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="bf79a-132">Tenga en cuenta que no se llama al método RenderNews (no hay ningún paréntesis).</span><span class="sxs-lookup"><span data-stu-id="bf79a-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="bf79a-133">En su lugar, se pasa una referencia al método a WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="bf79a-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="bf79a-134">La vista de índice se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="bf79a-134">The Index view is cached.</span></span> <span data-ttu-id="bf79a-135">Se devuelve la vista por el controlador en el listado 3.</span><span class="sxs-lookup"><span data-stu-id="bf79a-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="bf79a-136">Tenga en cuenta que la acción de Index() está decorada con un atributo [OutputCache] que hace que la vista de índice en la memoria caché durante 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="bf79a-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="bf79a-137">**Listado 3 – controllers\homecontroller. cs**</span><span class="sxs-lookup"><span data-stu-id="bf79a-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="bf79a-138">Aunque la vista de índice se almacena en caché, se muestran elementos de noticias aleatorio diferentes cuando se solicita la página de índice.</span><span class="sxs-lookup"><span data-stu-id="bf79a-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="bf79a-139">Cuando se solicita la página de índice, la hora mostrada por la página no cambia durante 60 segundos (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="bf79a-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="bf79a-140">El hecho de que no cambia la hora demuestra que la página se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="bf79a-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="bf79a-141">Sin embargo, el contenido insertado por los cambios de método: el elemento aleatorio noticias – WriteSubstitution() con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="bf79a-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="bf79a-142">**Ilustración 1: inserción de elementos de noticias dinámica en una página almacenada en caché**</span><span class="sxs-lookup"><span data-stu-id="bf79a-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="bf79a-144">Uso de la sustitución posterior a la caché en métodos auxiliares</span><span class="sxs-lookup"><span data-stu-id="bf79a-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="bf79a-145">Aprovechar las ventajas de la sustitución posterior a la caché de una manera más fácil consiste en encapsular la llamada al método WriteSubstitution() dentro de un método auxiliar personalizada.</span><span class="sxs-lookup"><span data-stu-id="bf79a-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="bf79a-146">Este enfoque se ilustra el método auxiliar en el listado 4.</span><span class="sxs-lookup"><span data-stu-id="bf79a-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="bf79a-147">**Listado 4 – AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="bf79a-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="bf79a-148">Listado 4 contiene una clase estática que expone dos métodos: RenderBanner() y RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="bf79a-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="bf79a-149">El método RenderBanner() representa el método auxiliar real.</span><span class="sxs-lookup"><span data-stu-id="bf79a-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="bf79a-150">Este método extiende la clase HtmlHelper de MVC de ASP.NET estándar para que pueda llamar Html.RenderBanner() en una vista al igual que cualquier otro método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="bf79a-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="bf79a-151">El método RenderBanner() llama al método de HttpResponse.WriteSubstitution() pasando el método RenderBannerInternal() WriteSubsitution() al método.</span><span class="sxs-lookup"><span data-stu-id="bf79a-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="bf79a-152">El método RenderBannerInternal() es un método privado.</span><span class="sxs-lookup"><span data-stu-id="bf79a-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="bf79a-153">Este método no se exponen como un método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="bf79a-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="bf79a-154">El método RenderBannerInternal() aleatoriamente devuelve una imagen del anuncio banner de una lista de tres imágenes de anuncios de pancarta.</span><span class="sxs-lookup"><span data-stu-id="bf79a-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="bf79a-155">La vista de índice modificada en el listado 5 muestra cómo puede usar el método auxiliar RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="bf79a-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="bf79a-156">Tenga en cuenta que adicional &lt;% @ Import %&gt; directiva se incluye en la parte superior de la vista para importar el espacio de nombres MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="bf79a-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="bf79a-157">Si no importa este espacio de nombres, el método RenderBanner() no aparecerá como un método en la propiedad Html.</span><span class="sxs-lookup"><span data-stu-id="bf79a-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="bf79a-158">**Listado 5 – Views\Home\Index.aspx (con el método RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="bf79a-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="bf79a-159">Cuando se solicita la página representada por la vista en el listado 5, se muestra un anuncio diferente de pancarta con cada solicitud (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="bf79a-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="bf79a-160">La página se almacena en caché, pero el anuncio se inserta dinámicamente mediante el método auxiliar RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="bf79a-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="bf79a-161">**Ilustración 2: la vista de índice muestra un anuncio aleatorio**</span><span class="sxs-lookup"><span data-stu-id="bf79a-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="bf79a-163">Resumen</span><span class="sxs-lookup"><span data-stu-id="bf79a-163">Summary</span></span>

<span data-ttu-id="bf79a-164">En este tutorial se explica cómo actualizar dinámicamente el contenido de una página almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="bf79a-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="bf79a-165">Ha aprendido a usar el método HttpResponse.WriteSubstitution() para habilitar el contenido dinámico para insertarse en una página almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="bf79a-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="bf79a-166">También ha aprendido cómo encapsular la llamada al método WriteSubstitution() dentro de un método de aplicación auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="bf79a-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="bf79a-167">Aprovechar las ventajas del almacenamiento en caché siempre que sea posible: puede tener un impacto significativo en el rendimiento de las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="bf79a-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="bf79a-168">Como se explica en este tutorial, puede beneficiarse del almacenamiento en caché incluso cuando necesite mostrar contenido dinámico en las páginas.</span><span class="sxs-lookup"><span data-stu-id="bf79a-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

> [!div class="step-by-step"]
> <span data-ttu-id="bf79a-169">[Anterior](improving-performance-with-output-caching-cs.md)
> [Siguiente](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="bf79a-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
