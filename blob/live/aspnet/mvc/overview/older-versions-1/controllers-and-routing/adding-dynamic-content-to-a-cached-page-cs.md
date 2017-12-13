---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: "Agregar contenido dinámico a una página almacenada en caché (C#) | Documentos de Microsoft"
author: microsoft
description: "Obtenga información acerca de cómo mezclar contenido dinámico y almacenado en caché en la misma página. Sustitución tras la caché le permite mostrar contenido dinámico, como o de anuncios de pancarta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: bee7e17ee16d75419c215558b1deb7d6f0d79448
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a><span data-ttu-id="a234e-104">Agregar contenido dinámico a una página almacenada en caché (C#)</span><span class="sxs-lookup"><span data-stu-id="a234e-104">Adding Dynamic Content to a Cached Page (C#)</span></span>
====================
<span data-ttu-id="a234e-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a234e-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a234e-106">Obtenga información acerca de cómo mezclar contenido dinámico y almacenado en caché en la misma página.</span><span class="sxs-lookup"><span data-stu-id="a234e-106">Learn how to mix dynamic and cached content in the same page.</span></span> <span data-ttu-id="a234e-107">Sustitución tras la caché le permite mostrar contenido dinámico, como anuncios de pancarta o elementos de noticias, dentro de una página que se ha de salida almacenados en memoria caché.</span><span class="sxs-lookup"><span data-stu-id="a234e-107">Post-cache substitution enables you to display dynamic content, such as banner advertisements or news items, within a page that has been output cached.</span></span>


<span data-ttu-id="a234e-108">Aprovechando las ventajas del almacenamiento en caché de salida, puede mejorar considerablemente el rendimiento de una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a234e-108">By taking advantage of output caching, you can dramatically improve the performance of an ASP.NET MVC application.</span></span> <span data-ttu-id="a234e-109">En lugar de volver a una página cada vez que se solicita la página, la página se pueden generar una vez y almacena en caché en memoria para varios usuarios.</span><span class="sxs-lookup"><span data-stu-id="a234e-109">Instead of regenerating a page each and every time the page is requested, the page can be generated once and cached in memory for multiple users.</span></span>

<span data-ttu-id="a234e-110">Pero hay un problema.</span><span class="sxs-lookup"><span data-stu-id="a234e-110">But there is a problem.</span></span> <span data-ttu-id="a234e-111">¿Qué ocurre si necesita mostrar contenido dinámico en la página?</span><span class="sxs-lookup"><span data-stu-id="a234e-111">What if you need to display dynamic content in the page?</span></span> <span data-ttu-id="a234e-112">Por ejemplo, imagine que desea mostrar un anuncio en la página.</span><span class="sxs-lookup"><span data-stu-id="a234e-112">For example, imagine that you want to display a banner advertisement in the page.</span></span> <span data-ttu-id="a234e-113">No desea que el anuncio de pancarta se almacene en caché para que cada usuario ve el anuncio del mismo.</span><span class="sxs-lookup"><span data-stu-id="a234e-113">You don't want the banner advertisement to be cached so that every user sees the very same advertisement.</span></span> <span data-ttu-id="a234e-114">¡De este modo no realiza dinero!</span><span class="sxs-lookup"><span data-stu-id="a234e-114">You wouldn't make any money that way!</span></span>

<span data-ttu-id="a234e-115">Afortunadamente, hay una solución sencilla.</span><span class="sxs-lookup"><span data-stu-id="a234e-115">Fortunately, there is an easy solution.</span></span> <span data-ttu-id="a234e-116">Puede aprovechar las ventajas de una característica del marco de ASP.NET denominada *sustitución tras la caché*.</span><span class="sxs-lookup"><span data-stu-id="a234e-116">You can take advantage of a feature of the ASP.NET framework called *post-cache substitution*.</span></span> <span data-ttu-id="a234e-117">Sustitución tras la caché permite sustituir contenido dinámico en una página que se ha almacenado en caché en memoria.</span><span class="sxs-lookup"><span data-stu-id="a234e-117">Post-cache substitution enables you to substitute dynamic content in a page that has been cached in memory.</span></span>


<span data-ttu-id="a234e-118">Normalmente, cuando se caché de resultados de una página con el atributo [OutputCache], la página se almacena en caché en el servidor y el cliente (Explorador de web).</span><span class="sxs-lookup"><span data-stu-id="a234e-118">Normally, when you output cache a page by using the [OutputCache] attribute, the page is cached on both the server and the client (the web browser).</span></span> <span data-ttu-id="a234e-119">Cuando se usa la sustitución posterior a la caché, se almacena en caché una página en el servidor.</span><span class="sxs-lookup"><span data-stu-id="a234e-119">When you use post-cache substitution, a page is cached only on the server.</span></span>


#### <a name="using-post-cache-substitution"></a><span data-ttu-id="a234e-120">Uso de sustitución tras la caché</span><span class="sxs-lookup"><span data-stu-id="a234e-120">Using Post-Cache Substitution</span></span>

<span data-ttu-id="a234e-121">Uso de sustitución tras la caché, requiere dos pasos.</span><span class="sxs-lookup"><span data-stu-id="a234e-121">Using post-cache substitution requires two steps.</span></span> <span data-ttu-id="a234e-122">En primer lugar, debe definir un método que devuelve una cadena que representa el contenido dinámico que desea mostrar en la página almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="a234e-122">First, you need to define a method that returns a string that represents the dynamic content that you want to display in the cached page.</span></span> <span data-ttu-id="a234e-123">A continuación, llame al método HttpResponse.WriteSubstitution() para insertar el contenido dinámico en la página.</span><span class="sxs-lookup"><span data-stu-id="a234e-123">Next, you call the HttpResponse.WriteSubstitution() method to inject the dynamic content into the page.</span></span>

<span data-ttu-id="a234e-124">Por ejemplo, imagine que desea mostrar aleatoriamente los elementos de noticias diferente en una página almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="a234e-124">Imagine, for example, that you want to randomly display different news items in a cached page.</span></span> <span data-ttu-id="a234e-125">La clase en la lista 1 expone un solo método, denominado RenderNews(), que devuelve al azar un elemento de noticias de una lista de tres elementos de noticias.</span><span class="sxs-lookup"><span data-stu-id="a234e-125">The class in Listing 1 exposes a single method, named RenderNews(), that randomly returns one news item from a list of three news items.</span></span>

<span data-ttu-id="a234e-126">**Lista 1 – Models\News.cs**</span><span class="sxs-lookup"><span data-stu-id="a234e-126">**Listing 1 – Models\News.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

<span data-ttu-id="a234e-127">Para aprovechar las ventajas de sustitución tras la caché, se llama al método de HttpResponse.WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="a234e-127">To take advantage of post-cache substitution, you call the HttpResponse.WriteSubstitution() method.</span></span> <span data-ttu-id="a234e-128">El método WriteSubstitution() establece el código para reemplazar una región de la página en caché con contenido dinámico.</span><span class="sxs-lookup"><span data-stu-id="a234e-128">The WriteSubstitution() method sets up the code to replace a region of the cached page with dynamic content.</span></span> <span data-ttu-id="a234e-129">El método WriteSubstitution() se usa para mostrar el elemento de noticias aleatorio en la vista en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="a234e-129">The WriteSubstitution() method is used to display the random news item in the view in Listing 2.</span></span>

<span data-ttu-id="a234e-130">**La lista 2 – Views\Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="a234e-130">**Listing 2 – Views\Home\Index.aspx**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

<span data-ttu-id="a234e-131">El método RenderNews se pasa al método WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="a234e-131">The RenderNews method is passed to the WriteSubstitution() method.</span></span> <span data-ttu-id="a234e-132">Tenga en cuenta que no se llama al método de RenderNews (no hay ningún paréntesis).</span><span class="sxs-lookup"><span data-stu-id="a234e-132">Notice that the RenderNews method is not called (there are no parentheses).</span></span> <span data-ttu-id="a234e-133">En su lugar, se pasa una referencia al método a WriteSubstitution().</span><span class="sxs-lookup"><span data-stu-id="a234e-133">Instead a reference to the method is passed to WriteSubstitution().</span></span>

<span data-ttu-id="a234e-134">La vista de índice se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="a234e-134">The Index view is cached.</span></span> <span data-ttu-id="a234e-135">La vista es devuelto por el controlador en el listado 3.</span><span class="sxs-lookup"><span data-stu-id="a234e-135">The view is returned by the controller in Listing 3.</span></span> <span data-ttu-id="a234e-136">Tenga en cuenta que la acción de Index() se decora con un atributo [OutputCache] que hace que la vista de índice en la memoria caché durante 60 segundos.</span><span class="sxs-lookup"><span data-stu-id="a234e-136">Notice that the Index() action is decorated with an [OutputCache] attribute that causes the Index view to be cached for 60 seconds.</span></span>

<span data-ttu-id="a234e-137">**El listado 3 – controllers\homecontroller**</span><span class="sxs-lookup"><span data-stu-id="a234e-137">**Listing 3 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

<span data-ttu-id="a234e-138">Aunque la vista de índice se almacena en caché, se muestran elementos de noticias aleatorio diferentes cuando se solicita la página de índice.</span><span class="sxs-lookup"><span data-stu-id="a234e-138">Even though the Index view is cached, different random news items are displayed when you request the Index page.</span></span> <span data-ttu-id="a234e-139">Cuando se solicita la página de índice, el tiempo que se muestra en la página no cambia durante 60 segundos (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="a234e-139">When you request the Index page, the time displayed by the page does not change for 60 seconds (see Figure 1).</span></span> <span data-ttu-id="a234e-140">El hecho de que no cambia la hora se demuestra que la página se almacena en caché.</span><span class="sxs-lookup"><span data-stu-id="a234e-140">The fact that the time does not change proves that the page is cached.</span></span> <span data-ttu-id="a234e-141">Sin embargo, el contenido insertado por los cambios de método: el elemento de noticias aleatorio – WriteSubstitution() con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="a234e-141">However, the content injected by the WriteSubstitution() method – the random news item – changes with each request .</span></span>

<span data-ttu-id="a234e-142">**Ilustración 1: insertar elementos de noticias dinámica en una página almacenada en caché**</span><span class="sxs-lookup"><span data-stu-id="a234e-142">**Figure 1 – Injecting dynamic news items in a cached page**</span></span>

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a><span data-ttu-id="a234e-144">Uso de sustitución tras la caché en métodos auxiliares</span><span class="sxs-lookup"><span data-stu-id="a234e-144">Using Post-Cache Substitution in Helper Methods</span></span>

<span data-ttu-id="a234e-145">Una manera más fácil de aprovechar las ventajas de sustitución tras la caché consiste en encapsular la llamada al método WriteSubstitution() dentro de un método de aplicación auxiliar personalizada.</span><span class="sxs-lookup"><span data-stu-id="a234e-145">An easier way to take advantage of post-cache substitution is to encapsulate the call to the WriteSubstitution() method within a custom helper method.</span></span> <span data-ttu-id="a234e-146">Este enfoque se muestra en el método auxiliar en el listado 4.</span><span class="sxs-lookup"><span data-stu-id="a234e-146">This approach is illustrated by the helper method in Listing 4.</span></span>

<span data-ttu-id="a234e-147">**Listado 4 – AdHelper.cs**</span><span class="sxs-lookup"><span data-stu-id="a234e-147">**Listing 4 – AdHelper.cs**</span></span>

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

<span data-ttu-id="a234e-148">Listado 4 contiene una clase estática que expone dos métodos: RenderBanner() y RenderBannerInternal().</span><span class="sxs-lookup"><span data-stu-id="a234e-148">Listing 4 contains a static class that exposes two methods: RenderBanner() and RenderBannerInternal().</span></span> <span data-ttu-id="a234e-149">El método RenderBanner() representa el método auxiliar real.</span><span class="sxs-lookup"><span data-stu-id="a234e-149">The RenderBanner() method represents the actual helper method.</span></span> <span data-ttu-id="a234e-150">Este método extiende la clase HtmlHelper de MVC de ASP.NET estándar para que se puede llamar a Html.RenderBanner() en una vista al igual que cualquier otro método de aplicación auxiliar.</span><span class="sxs-lookup"><span data-stu-id="a234e-150">This method extends the standard ASP.NET MVC HtmlHelper class so that you can call Html.RenderBanner() in a view just like any other helper method.</span></span>

<span data-ttu-id="a234e-151">El método RenderBanner() llama al método de HttpResponse.WriteSubstitution() pasando al método RenderBannerInternal() al método WriteSubsitution().</span><span class="sxs-lookup"><span data-stu-id="a234e-151">The RenderBanner() method calls the HttpResponse.WriteSubstitution() method passing the RenderBannerInternal() method to the WriteSubsitution() method.</span></span>

<span data-ttu-id="a234e-152">El método RenderBannerInternal() es un método privado.</span><span class="sxs-lookup"><span data-stu-id="a234e-152">The RenderBannerInternal() method is a private method.</span></span> <span data-ttu-id="a234e-153">Este método no se expone como un método auxiliar.</span><span class="sxs-lookup"><span data-stu-id="a234e-153">This method won't be exposed as a helper method.</span></span> <span data-ttu-id="a234e-154">El método RenderBannerInternal() devuelve aleatoriamente una imagen de anuncios de pancarta en una lista de tres imágenes de anuncios de pancarta.</span><span class="sxs-lookup"><span data-stu-id="a234e-154">The RenderBannerInternal() method randomly returns one banner advertisement image from a list of three banner advertisement images.</span></span>

<span data-ttu-id="a234e-155">La vista de índice modificada en el listado 5 muestra cómo puede usar el método de aplicación auxiliar RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="a234e-155">The modified Index view in Listing 5 illustrates how you can use the RenderBanner() helper method.</span></span> <span data-ttu-id="a234e-156">Tenga en cuenta que otro &lt;% @ Import %&gt; directiva se incluye en la parte superior de la vista para importar el espacio de nombres MvcApplication1.Helpers.</span><span class="sxs-lookup"><span data-stu-id="a234e-156">Notice that an additional &lt;%@ Import %&gt; directive is included at the top of the view to import the MvcApplication1.Helpers namespace.</span></span> <span data-ttu-id="a234e-157">Si no desea importar este espacio de nombres, el método RenderBanner() no aparecerá como un método en la propiedad de Html.</span><span class="sxs-lookup"><span data-stu-id="a234e-157">If you neglect to import this namespace, then the RenderBanner() method won't appear as a method on the Html property.</span></span>

<span data-ttu-id="a234e-158">**Listado 5 – Views\Home\Index.aspx (con el método de RenderBanner())**</span><span class="sxs-lookup"><span data-stu-id="a234e-158">**Listing 5 – Views\Home\Index.aspx (with RenderBanner() method)**</span></span>

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

<span data-ttu-id="a234e-159">Cuando se solicita la página representada por la vista en el listado 5, se muestra un anuncio diferente con cada solicitud (consulte la figura 2).</span><span class="sxs-lookup"><span data-stu-id="a234e-159">When you request the page rendered by the view in Listing 5, a different banner advertisement is displayed with each request (see Figure 2).</span></span> <span data-ttu-id="a234e-160">La página se almacena en caché, pero los anuncios de pancarta se inyección dinámicamente por el método de aplicación auxiliar RenderBanner().</span><span class="sxs-lookup"><span data-stu-id="a234e-160">The page is cached, but the banner advertisement is injected dynamically by the RenderBanner() helper method.</span></span>

<span data-ttu-id="a234e-161">**Figura 2: la vista de índice muestra un anuncio aleatorio**</span><span class="sxs-lookup"><span data-stu-id="a234e-161">**Figure 2 – The Index view displaying a random banner advertisement**</span></span>

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a><span data-ttu-id="a234e-163">Resumen</span><span class="sxs-lookup"><span data-stu-id="a234e-163">Summary</span></span>

<span data-ttu-id="a234e-164">Este tutorial explica cómo puede actualizar dinámicamente el contenido de una página almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="a234e-164">This tutorial explained how you can dynamically update content in a cached page.</span></span> <span data-ttu-id="a234e-165">Aprendió a utilizar el método HttpResponse.WriteSubstitution() para habilitar el contenido dinámico que puedan insertarse en una página almacenada en caché.</span><span class="sxs-lookup"><span data-stu-id="a234e-165">You learned how to use the HttpResponse.WriteSubstitution() method to enable dynamic content to be injected in a cached page.</span></span> <span data-ttu-id="a234e-166">También aprendió encapsular la llamada al método WriteSubstitution() dentro de un método de aplicación auxiliar HTML.</span><span class="sxs-lookup"><span data-stu-id="a234e-166">You also learned how to encapsulate the call to the WriteSubstitution() method within an HTML helper method.</span></span>

<span data-ttu-id="a234e-167">Aprovechar las ventajas del almacenamiento en caché siempre que sea posible: puede tener un impacto significativo en el rendimiento de las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="a234e-167">Take advantage of caching whenever possible – it can have a dramatic impact on the performance of your web applications.</span></span> <span data-ttu-id="a234e-168">Como se explica en este tutorial, puede aprovechar las ventajas del almacenamiento en caché incluso cuando se necesitan para mostrar contenido dinámico en las páginas.</span><span class="sxs-lookup"><span data-stu-id="a234e-168">As explained in this tutorial, you can take advantage of caching even when you need to display dynamic content in your pages.</span></span>

## 

## 

>[!div class="step-by-step"]
<span data-ttu-id="a234e-169">[Anterior](improving-performance-with-output-caching-cs.md)
[Siguiente](creating-a-controller-cs.md)</span><span class="sxs-lookup"><span data-stu-id="a234e-169">[Previous](improving-performance-with-output-caching-cs.md)
[Next](creating-a-controller-cs.md)</span></span>
