---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: Información de visitante (análisis) de seguimiento de ASP.NET Web Pages (Razor) sitio | Microsoft Docs
author: tfitzmac
description: Una vez que ha obtenido su sitio Web va, puede analizar el tráfico del sitio Web.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 48782606083b4aa1e32adf6163bcb3f2d9828bc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387142"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="a5583-103">Información de seguimiento visitante (análisis) para un sitio Web de ASP.NET Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="a5583-103">Tracking Visitor Information (Analytics) for an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="a5583-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a5583-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a5583-105">En este artículo se describe cómo usar una aplicación auxiliar para agregar análisis de sitios Web a las páginas de un sitio Web de ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="a5583-105">This article describes how to use a helper to add website analytics to pages in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="a5583-106">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="a5583-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a5583-107">Cómo enviar información sobre el tráfico del sitio Web a un proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="a5583-107">How to send information about your website traffic to an analytics provider.</span></span>
> 
> <span data-ttu-id="a5583-108">Estas son las características introducidas en el artículo de programación de ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="a5583-108">These are the ASP.NET programming features introduced in the article:</span></span>
> 
> - <span data-ttu-id="a5583-109">El `Analytics` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="a5583-109">The `Analytics` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a5583-110">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="a5583-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a5583-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="a5583-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="a5583-112">ASP.NET Web Helpers Library (paquete de NuGet)</span><span class="sxs-lookup"><span data-stu-id="a5583-112">ASP.NET Web Helpers Library (NuGet package)</span></span>


<span data-ttu-id="a5583-113">Analytics es un término general para la tecnología que mide el tráfico en su sitio Web para que pueda comprender cómo se usa el sitio.</span><span class="sxs-lookup"><span data-stu-id="a5583-113">Analytics is a general term for technology that measures traffic on your website so you can understand how people use the site.</span></span> <span data-ttu-id="a5583-114">Muchos de los servicios de análisis están disponibles, incluidos los servicios de Google, Yahoo, StatCounter y otros.</span><span class="sxs-lookup"><span data-stu-id="a5583-114">Many analytics services are available, including services from Google, Yahoo, StatCounter, and others.</span></span>

<span data-ttu-id="a5583-115">El análisis de manera funciona es que suscribirse a una cuenta con el proveedor de análisis, donde se registra el sitio que desean realizar un seguimiento. El proveedor envía un fragmento de código de JavaScript que incluye un identificador o código de la cuenta de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a5583-115">The way analytics works is that you sign up for an account with the analytics provider, where you register the site that you want to track. The provider sends you a snippet of JavaScript code that includes an ID or tracking code for your account.</span></span> <span data-ttu-id="a5583-116">Agregue el fragmento de código de JavaScript a las páginas web en el sitio que desea realizar un seguimiento. (Normalmente, agregar el fragmento de código de análisis para una página de diseño o de pie de página o de otro elemento de marcado HTML que aparece en todas las páginas del sitio.) Cuando los usuarios solicitan una página que contiene uno de estos fragmentos de código de JavaScript, el fragmento de código envía información sobre la página actual para el proveedor de análisis, que registra los diversos detalles acerca de la página.</span><span class="sxs-lookup"><span data-stu-id="a5583-116">You add the JavaScript snippet to the web pages on the site that you want to track. (You typically add the analytics snippet to a footer or layout page or other HTML markup that appears on every page in your site.) When users request a page that contains one of these JavaScript snippets, the snippet sends information about the current page to the analytics provider, who records various details about the page.</span></span>

<span data-ttu-id="a5583-117">Cuando desee echar un vistazo a las estadísticas del sitio, inicie sesión en el sitio Web del proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="a5583-117">When you want to have a look at your site statistics, you log into the analytics provider's website.</span></span> <span data-ttu-id="a5583-118">A continuación, puede ver a todo tipo de informes sobre su sitio, como:</span><span class="sxs-lookup"><span data-stu-id="a5583-118">You can then view all sorts of reports about your site, like:</span></span>

- <span data-ttu-id="a5583-119">El número de vistas de página para páginas individuales.</span><span class="sxs-lookup"><span data-stu-id="a5583-119">The number of page views for individual pages.</span></span> <span data-ttu-id="a5583-120">Esto indica a (aproximadamente) el número de personas que visitan el sitio, y qué páginas del sitio son los más populares.</span><span class="sxs-lookup"><span data-stu-id="a5583-120">This tells you (roughly) how many people are visiting the site, and which pages on your site are the most popular.</span></span>
- <span data-ttu-id="a5583-121">¿Durante cuánto tiempo trabajadores emplean normalmente en páginas específicas.</span><span class="sxs-lookup"><span data-stu-id="a5583-121">How long people spend on specific pages.</span></span> <span data-ttu-id="a5583-122">Esto puede indicar cosas como si la página principal es mantener el interés de las personas.</span><span class="sxs-lookup"><span data-stu-id="a5583-122">This can tell you things like whether your home page is keeping people's interest.</span></span>
- <span data-ttu-id="a5583-123">Lo que los usuarios sitios estaban en antes de que visiten su sitio.</span><span class="sxs-lookup"><span data-stu-id="a5583-123">What sites people were on before they visited your site.</span></span> <span data-ttu-id="a5583-124">Esto le ayudará a comprender si el tráfico procede de vínculos desde las búsquedas y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="a5583-124">This helps you understand whether your traffic is coming from links, from searches, and so on.</span></span>
- <span data-ttu-id="a5583-125">Cuando las personas visitan su sitio y cuánto tiempo permanecen.</span><span class="sxs-lookup"><span data-stu-id="a5583-125">When people visit your site and how long they stay.</span></span>
- <span data-ttu-id="a5583-126">¿Qué países son de los visitantes de.</span><span class="sxs-lookup"><span data-stu-id="a5583-126">What countries your visitors are from.</span></span>
- <span data-ttu-id="a5583-127">¿Qué sistemas operativos y exploradores utilizan los visitantes.</span><span class="sxs-lookup"><span data-stu-id="a5583-127">What browsers and operating systems your visitors are using.</span></span>

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a><span data-ttu-id="a5583-129">Uso de una aplicación auxiliar para agregar análisis a una página</span><span class="sxs-lookup"><span data-stu-id="a5583-129">Using a Helper to Add Analytics to a Page</span></span>

<span data-ttu-id="a5583-130">Las páginas Web ASP.NET incluye varias aplicaciones auxiliares de análisis (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, y `Analytics.GetStatCounterHtml`) que facilitan administrar los fragmentos de código de JavaScript que se usan para el análisis.</span><span class="sxs-lookup"><span data-stu-id="a5583-130">ASP.NET Web Pages includes several analytics helpers (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, and `Analytics.GetStatCounterHtml`) that make it easy to manage the JavaScript snippets used for analytics.</span></span> <span data-ttu-id="a5583-131">En lugar de averiguar cómo y dónde colocar el código de JavaScript, lo único que debe hacer es agregar la aplicación auxiliar a una página.</span><span class="sxs-lookup"><span data-stu-id="a5583-131">Instead of figuring out how and where to put the JavaScript code, all you have to do is add the helper to a page.</span></span> <span data-ttu-id="a5583-132">La única información que debe proporcionar es el nombre de la cuenta, identificador o código de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a5583-132">The only information you need to provide is your account name, ID, or tracking code.</span></span> <span data-ttu-id="a5583-133">(Para StatCounter, también debe proporcionar algunos valores adicionales.)</span><span class="sxs-lookup"><span data-stu-id="a5583-133">(For StatCounter, you also have to provide a few additional values.)</span></span>

<span data-ttu-id="a5583-134">En este procedimiento, creará una página de diseño que usa el `GetGoogleHtml` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="a5583-134">In this procedure, you'll create a layout page that uses the `GetGoogleHtml` helper.</span></span> <span data-ttu-id="a5583-135">Si ya tiene una cuenta con uno de los otros proveedores de análisis, puede utilizar esa cuenta en su lugar y realizar pequeñas modificaciones, según sea necesario.</span><span class="sxs-lookup"><span data-stu-id="a5583-135">If you already have an account with one of the other analytics providers, you can use that account instead and make slight adjustments as needed.</span></span>

> [!NOTE]
> <span data-ttu-id="a5583-136">Cuando se crea una cuenta de análisis, registrar la dirección URL del sitio que desea realizar el seguimiento.</span><span class="sxs-lookup"><span data-stu-id="a5583-136">When you create an analytics account, you register the URL of the site that you want to be tracking.</span></span> <span data-ttu-id="a5583-137">Si va a probar todo en el equipo local, no se seguimiento tráfico real (es el único tráfico), por lo que no será capaz de registrar y ver las estadísticas del sitio.</span><span class="sxs-lookup"><span data-stu-id="a5583-137">If you're testing everything on your local computer, you won't be tracking actual traffic (the only traffic is you), so you won't be able to record and view site statistics.</span></span> <span data-ttu-id="a5583-138">Pero este procedimiento muestra cómo agregar una aplicación auxiliar de analytics a una página.</span><span class="sxs-lookup"><span data-stu-id="a5583-138">But this procedure shows how you add an analytics helper to a page.</span></span> <span data-ttu-id="a5583-139">Al publicar su sitio, el sitio activo enviará información a su proveedor de análisis.</span><span class="sxs-lookup"><span data-stu-id="a5583-139">When you publish your site, the live site will send information to your analytics provider.</span></span>


1. <span data-ttu-id="a5583-140">Agregue la ASP.NET Web Helpers Library a su sitio Web, como se describe en [las aplicaciones auxiliares de la instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), si aún no lo ha agregado.</span><span class="sxs-lookup"><span data-stu-id="a5583-140">Add the ASP.NET Web Helpers Library to your website as described in [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372), if you haven't already added it.</span></span>
2. <span data-ttu-id="a5583-141">Cree una cuenta con Google Analytics y registre el nombre de cuenta.</span><span class="sxs-lookup"><span data-stu-id="a5583-141">Create an account with Google Analytics and record the account name.</span></span>
3. <span data-ttu-id="a5583-142">Cree una página de diseño denominada *Analytics.cshtml* y agregue el siguiente marcado:</span><span class="sxs-lookup"><span data-stu-id="a5583-142">Create a layout page named *Analytics.cshtml* and add the following markup:</span></span>

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="a5583-143">Debe colocar la llamada a la `Analytics` auxiliar en el cuerpo de la página web (antes de la `</body>` etiqueta).</span><span class="sxs-lookup"><span data-stu-id="a5583-143">You must place the call to the `Analytics` helper in the body of your web page (before the `</body>` tag).</span></span> <span data-ttu-id="a5583-144">En caso contrario, el explorador no ejecutará el script.</span><span class="sxs-lookup"><span data-stu-id="a5583-144">Otherwise, the browser will not run the script.</span></span>

    <span data-ttu-id="a5583-145">Si usa un proveedor de análisis diferentes, use una de las siguientes aplicaciones auxiliares en su lugar:</span><span class="sxs-lookup"><span data-stu-id="a5583-145">If you're using a different analytics provider, use one of the following helpers instead:</span></span>

    - <span data-ttu-id="a5583-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span><span class="sxs-lookup"><span data-stu-id="a5583-146">(Yahoo) `@Analytics.GetYahooHtml("myaccount")`</span></span>
    - <span data-ttu-id="a5583-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span><span class="sxs-lookup"><span data-stu-id="a5583-147">(StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`</span></span>
4. <span data-ttu-id="a5583-148">Reemplace `myaccount` con el nombre de la cuenta, identificador o código de seguimiento que creó en el paso 1.</span><span class="sxs-lookup"><span data-stu-id="a5583-148">Replace `myaccount` with the name of the account, ID, or tracking code that you created in step 1.</span></span>
5. <span data-ttu-id="a5583-149">Ejecute la página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="a5583-149">Run the page in the browser.</span></span> <span data-ttu-id="a5583-150">(Asegúrese de que la página está seleccionada en el **archivos** área de trabajo antes de ejecutarlo.)</span><span class="sxs-lookup"><span data-stu-id="a5583-150">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
6. <span data-ttu-id="a5583-151">En el explorador, vea el origen de la página.</span><span class="sxs-lookup"><span data-stu-id="a5583-151">In the browser, view the page source.</span></span> <span data-ttu-id="a5583-152">Podrá ver el código representado analytics:</span><span class="sxs-lookup"><span data-stu-id="a5583-152">You'll be able to see the rendered analytics code:</span></span>

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. <span data-ttu-id="a5583-153">Inicie sesión en el sitio de Google Analytics y examinar las estadísticas para el sitio.</span><span class="sxs-lookup"><span data-stu-id="a5583-153">Log onto the Google Analytics site and examine the statistics for your site.</span></span> <span data-ttu-id="a5583-154">Si ejecuta la página en un sitio activo, verá una entrada que registra la visita a la página.</span><span class="sxs-lookup"><span data-stu-id="a5583-154">If you're running the page on a live site, you see an entry that logs the visit to your page.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a5583-155">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a5583-155">Additional Resources</span></span>

- [<span data-ttu-id="a5583-156">Sitio de Google Analytics</span><span class="sxs-lookup"><span data-stu-id="a5583-156">Google Analytics site</span></span>](https://www.google.com/analytics/)
- [<span data-ttu-id="a5583-157">Yahoo! Sitio de Web Analytics</span><span class="sxs-lookup"><span data-stu-id="a5583-157">Yahoo! Web Analytics site</span></span>](http://help.yahoo.com/l/us/yahoo/ywa/)
- [<span data-ttu-id="a5583-158">Sitio StatCounter</span><span class="sxs-lookup"><span data-stu-id="a5583-158">StatCounter site</span></span>](http://statcounter.com/)
