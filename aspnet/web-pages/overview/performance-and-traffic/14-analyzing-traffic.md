---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "Seguimiento de la información de visitante (análisis) para ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft"
author: tfitzmac
description: "Después de que ha llegado el sitio Web va, puede analizar el tráfico del sitio Web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="88f09-103">Seguimiento de la información de visitante (análisis) para un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="88f09-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="88f09-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="88f09-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="88f09-105">Este artículo describe cómo utilizar una aplicación auxiliar para agregar el análisis de sitio Web a las páginas en un sitio Web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="88f09-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="88f09-106">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="88f09-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="88f09-107">Cómo enviar información sobre el tráfico del sitio Web a un proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="88f09-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="88f09-108">Se trata de la programación de características introducidas en el artículo de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="88f09-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="88f09-109">El `Analytics` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="88f09-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="88f09-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="88f09-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="88f09-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="88f09-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="88f09-112">ASP.NET Web Helpers Library (paquete de NuGet)</span><span class="sxs-lookup"><span data-stu-id="88f09-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="88f09-113">El análisis es un término general para las tecnologías que mide el tráfico en el sitio Web de manera que pueda comprender cómo se utiliza el sitio.</span><span class="sxs-lookup"><span data-stu-id="88f09-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="88f09-114">Muchos servicios de análisis están disponibles, incluidos los servicios de Google, Yahoo, StatCounter y otros usuarios.</span><span class="sxs-lookup"><span data-stu-id="88f09-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="88f09-115">El análisis de manera funciona es la que se registra para una cuenta con el proveedor de análisis, en el que registra el sitio que desea realizar un seguimiento. El proveedor envía un fragmento de código de JavaScript que incluye un identificador o código de la cuenta de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="88f09-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="88f09-116">Agregar el fragmento de código de JavaScript a las páginas web en el sitio que desea realizar un seguimiento. (Se suele agrega el fragmento de código de análisis a un diseño o un pie de página u otro formato HTML que aparece en cada página en el sitio.) Cuando los usuarios solicitan una página que contiene uno de estos fragmentos de código de JavaScript, el fragmento de código envía información sobre la página actual para el proveedor de análisis, que registra los detalles acerca de la página.</span><span class="sxs-lookup"><span data-stu-id="88f09-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="88f09-117">Cuando desea un vistazo a las estadísticas del sitio, inicia sesión en el sitio Web del proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="88f09-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="88f09-118">A continuación, puede ver a todos los tipos de informes sobre su sitio, como:</span><span class="sxs-lookup"><span data-stu-id="88f09-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="88f09-119">El número de vistas de página para las páginas individuales.</span><span class="sxs-lookup"><span data-stu-id="88f09-119">The number of page views for individual pages.</span></span> <span data-ttu-id="88f09-120">Esto indica a (aproximadamente) el número de personas que visitan el sitio y las páginas en el sitio son las más populares.</span><span class="sxs-lookup"><span data-stu-id="88f09-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="88f09-121">¿Durante cuánto tiempo personas dedican en páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="88f09-121">How long people spend on specific pages.</span></span> <span data-ttu-id="88f09-122">Esto puede indicar cosas como si la página principal se mantiene interés popular.</span><span class="sxs-lookup"><span data-stu-id="88f09-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="88f09-123">¿Qué sitios personas estaban en antes de que visita el sitio.</span><span class="sxs-lookup"><span data-stu-id="88f09-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="88f09-124">Esto le ayudará a comprender si el tráfico procede de vínculos de búsquedas y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="88f09-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="88f09-125">Cuando los usuarios visitan su sitio y cuánto tiempo se mantienen.</span><span class="sxs-lookup"><span data-stu-id="88f09-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="88f09-126">¿Qué países son gran los visitantes de.</span><span class="sxs-lookup"><span data-stu-id="88f09-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="88f09-127">¿Qué sistemas operativos y exploradores utilizan los visitantes.</span><span class="sxs-lookup"><span data-stu-id="88f09-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="88f09-129">Uso de una aplicación auxiliar para agregar análisis a una página</span><span class="sxs-lookup"><span data-stu-id="88f09-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="88f09-130">Las páginas Web ASP.NET incluye varias aplicaciones auxiliares de análisis (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, y `Analytics.GetStatCounterHtml`) que resulte sencillo administrar los fragmentos de código de JavaScript que se usan para el análisis.</span><span class="sxs-lookup"><span data-stu-id="88f09-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="88f09-131">En lugar de pensar en cómo y dónde colocar el código de JavaScript, lo único que debe hacer es agregar la aplicación auxiliar a una página.</span><span class="sxs-lookup"><span data-stu-id="88f09-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="88f09-132">La única información que debe proporcionar es el nombre de la cuenta, el identificador o el código de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="88f09-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="88f09-133">(Para StatCounter, también tendrá que proporcione algunos valores adicionales.)</span><span class="sxs-lookup"><span data-stu-id="88f09-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="88f09-134">En este procedimiento, creará una página de diseño que usa el `GetGoogleHtml` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="88f09-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="88f09-135">Si ya tiene una cuenta con uno de los otros proveedores de análisis, puede utilizar esa cuenta en su lugar y realizar pequeñas modificaciones según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="88f09-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="88f09-136">Al crear una cuenta de análisis, se registra la dirección URL del sitio que desea realizar el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="88f09-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="88f09-137">Si va a probar todos los elementos en el equipo local, no a seguimiento tráfico real (el único tráfico es usted), por lo que no podrá registrar y ver las estadísticas del sitio.</span><span class="sxs-lookup"><span data-stu-id="88f09-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="88f09-138">Pero este procedimiento muestra cómo agregar una aplicación auxiliar de análisis a una página.</span><span class="sxs-lookup"><span data-stu-id="88f09-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="88f09-139">Cuando se publica un sitio, el sitio activo enviará información a su proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="88f09-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="88f09-140">Agregar ASP.NET Web Helpers Library a su sitio Web, como se describe en [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no se ha agregado.</span><span class="sxs-lookup"><span data-stu-id="88f09-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="88f09-141">Crear una cuenta con Google Analytics y registre el nombre de cuenta.</span><span class="sxs-lookup"><span data-stu-id="88f09-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="88f09-142">Crear una página de diseño denominada *Analytics.cshtml* y agregue el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="88f09-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="88f09-143">Debe colocar la llamada a la `Analytics` auxiliar en el cuerpo de la página web (antes de la `</body>` etiqueta).</span><span class="sxs-lookup"><span data-stu-id="88f09-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="88f09-144">En caso contrario, el explorador no ejecutará la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="88f09-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="88f09-145">Si usa un proveedor de análisis diferentes, utilice en su lugar uno de los elementos auxiliares siguientes:</span><span class="sxs-lookup"><span data-stu-id="88f09-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="88f09-146">(Yahoo)`@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="88f09-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="88f09-147">(StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="88f09-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="88f09-148">Reemplace `myaccount` con el nombre de la cuenta, el identificador o el código de seguimiento que creó en el paso 1.</span><span class="sxs-lookup"><span data-stu-id="88f09-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="88f09-149">Ejecute la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="88f09-149">Run the page in the browser.</span></span> <span data-ttu-id="88f09-150">(Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)</span><span class="sxs-lookup"><span data-stu-id="88f09-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="88f09-151">En el explorador, vea el origen de la página.</span><span class="sxs-lookup"><span data-stu-id="88f09-151">In the browser, view the page source.</span></span> <span data-ttu-id="88f09-152">Podrá ver el código de análisis representado:</span><span class="sxs-lookup"><span data-stu-id="88f09-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="88f09-153">Inicie sesión en el sitio de Google Analytics y examine las estadísticas para el sitio.</span><span class="sxs-lookup"><span data-stu-id="88f09-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="88f09-154">Si estás ejecutando la página en un sitio en vivo, verá una entrada que registra la visita a la página.</span><span class="sxs-lookup"><span data-stu-id="88f09-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="88f09-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="88f09-155">Additional Resources</span></span>

- [<span data-ttu-id="88f09-156">Sitio de Google Analytics</span><span class="sxs-lookup"><span data-stu-id="88f09-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="88f09-157">Yahoo! Sitio Web de análisis</span><span class="sxs-lookup"><span data-stu-id="88f09-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="88f09-158">Sitio de StatCounter</span><span class="sxs-lookup"><span data-stu-id="88f09-158">StatCounter site</span></span>](http://statcounter.com/)
