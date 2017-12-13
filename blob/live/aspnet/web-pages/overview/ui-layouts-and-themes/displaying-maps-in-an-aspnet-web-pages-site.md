---
uid: web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
title: Mostrar mapas en un ASP.NET Web Pages (Razor) sitio | Documentos de Microsoft
author: tfitzmac
description: "Este artículo explica cómo mostrar los mapas interactivos en páginas en un sitio Web de ASP.NET Web Pages (Razor) en función de asignación de servicios proporcionados por Bing, Google, Ma..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: b5c268dd-ca6a-4562-b94c-a220fcf01f58
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/displaying-maps-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: a802e4fed81fadca195c8aa83c37c7100ac5cbec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="displaying-maps-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="69490-103">Mostrar mapas en un sitio de ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="69490-103">Displaying Maps in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="69490-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="69490-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="69490-105">En este artículo se explica cómo mostrar los mapas interactivos en páginas en un sitio Web de ASP.NET Web Pages (Razor) en función de asignación de servicios proporcionados por Bing, Google, MapQuest y Yahoo.</span><span class="sxs-lookup"><span data-stu-id="69490-105">This article explains how to display interactive maps on pages in an ASP.NET Web Pages (Razor) website based on mapping services provided by Bing, Google, MapQuest, and Yahoo.</span></span>
> 
> <span data-ttu-id="69490-106">Lo que aprenderá:</span><span class="sxs-lookup"><span data-stu-id="69490-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="69490-107">Cómo generar un mapa basándose en una dirección.</span><span class="sxs-lookup"><span data-stu-id="69490-107">How to generate a map based on an address.</span></span>
> - <span data-ttu-id="69490-108">Cómo generar un mapa basado en coordenadas de latitud y longitud.</span><span class="sxs-lookup"><span data-stu-id="69490-108">How to generate a map based on latitude and longitude coordinates.</span></span>
> - <span data-ttu-id="69490-109">Cómo registrar una cuenta de desarrollador de mapas de Bing y obtener una clave para usarla con Bing Maps.</span><span class="sxs-lookup"><span data-stu-id="69490-109">How to register a Bing Maps Developer Account and get a key to use with Bing Maps.</span></span>
> 
> <span data-ttu-id="69490-110">Se trata de la característica ASP.NET que se introdujo en el artículo:</span><span class="sxs-lookup"><span data-stu-id="69490-110">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="69490-111">El `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="69490-111">The `Maps` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="69490-112">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="69490-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="69490-113">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="69490-113">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="69490-114">WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="69490-114">WebMatrix 2</span></span>
>   
> 
> <span data-ttu-id="69490-115">Este tutorial también funciona con 3 de WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="69490-115">This tutorial also works with WebMatrix 3.</span></span>


<span data-ttu-id="69490-116">En las páginas Web, puede mostrar los mapas en una página mediante el uso de `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="69490-116">In Web Pages, you can display maps on a page by using `Maps` helper.</span></span> <span data-ttu-id="69490-117">Puede generar las asignaciones basadas en una dirección o en un conjunto de coordenadas de longitud y latitud.</span><span class="sxs-lookup"><span data-stu-id="69490-117">You can generate maps based either on an address or on a set of longitude and latitude coordinates.</span></span> <span data-ttu-id="69490-118">La `Maps` clase permite llamar a motores de mapa populares como Bing, Google, MapQuest y Yahoo.</span><span class="sxs-lookup"><span data-stu-id="69490-118">The `Maps` class lets you call into popular map engines including Bing, Google, MapQuest, and Yahoo.</span></span>

<span data-ttu-id="69490-119">Los pasos para agregar la asignación a una página son los mismos independientemente de cuál de los motores de mapa se llama a.</span><span class="sxs-lookup"><span data-stu-id="69490-119">The steps for adding mapping to a page are the same regardless of which of the map engines you call.</span></span> <span data-ttu-id="69490-120">Solo tiene que agregar una referencia de archivo de JavaScript que habilita los métodos disponibles para mostrar el mapa y, a continuación, llamar a métodos de la `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="69490-120">You just add a JavaScript file reference that makes available methods to display the map, and then you call methods of the `Maps` helper.</span></span>

<span data-ttu-id="69490-121">Elija un servicio de mapa en función del `Maps` método auxiliar que se utiliza.</span><span class="sxs-lookup"><span data-stu-id="69490-121">You choose a map service based on which `Maps` helper method you use.</span></span> <span data-ttu-id="69490-122">Puede utilizar cualquiera de ellos:</span><span class="sxs-lookup"><span data-stu-id="69490-122">You can use any of these:</span></span>

- `Maps.GetBingHtml`
- `Maps.GetGoogleHtml`
- `Maps.GetYahooHtml`
- `Maps.GetMapQuestHtml`

## <a name="installing-the-pieces-you-need"></a><span data-ttu-id="69490-123">Instalar los elementos que necesarios</span><span class="sxs-lookup"><span data-stu-id="69490-123">Installing the Pieces You Need</span></span>

<span data-ttu-id="69490-124">Para mostrar los mapas, necesita estos fragmentos:</span><span class="sxs-lookup"><span data-stu-id="69490-124">To display maps, you need these pieces:</span></span>

- <span data-ttu-id="69490-125">El `Maps` auxiliar.</span><span class="sxs-lookup"><span data-stu-id="69490-125">The `Maps` helper.</span></span> <span data-ttu-id="69490-126">Esta aplicación auxiliar es en la versión 2 de ASP.NET Web Helpers Library.</span><span class="sxs-lookup"><span data-stu-id="69490-126">This helper is in version 2 of the ASP.NET Web Helpers Library.</span></span> <span data-ttu-id="69490-127">Si aún no ha agregado la biblioteca, puede instalarlo en el sitio como un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="69490-127">If you haven't already added the library, you can install it in your site as a NuGet package.</span></span> <span data-ttu-id="69490-128">Para obtener más información, consulte [aplicaciones auxiliares de instalación en un sitio de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372).</span><span class="sxs-lookup"><span data-stu-id="69490-128">For details, see [Installing Helpers in an ASP.NET Web Pages Site](https://go.microsoft.com/fwlink/?LinkId=252372).</span></span> <span data-ttu-id="69490-129">(En la galería, busque el `microsoft-web-helpers` paquete.)</span><span class="sxs-lookup"><span data-stu-id="69490-129">(In the Gallery, search for the `microsoft-web-helpers` package.)</span></span>
- <span data-ttu-id="69490-130">La biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="69490-130">The jQuery library.</span></span> <span data-ttu-id="69490-131">Varias de las plantillas de sitio de WebMatrix ya incluyen bibliotecas de jQuery en sus *Script* carpetas.</span><span class="sxs-lookup"><span data-stu-id="69490-131">Several of the WebMatrix site templates already include jQuery libraries in their *Script* folders.</span></span> <span data-ttu-id="69490-132">Si no tiene estas bibliotecas, puede descargar la biblioteca de jQuery más reciente directamente desde el [jQuery.org](http://jQuery.org) sitio.</span><span class="sxs-lookup"><span data-stu-id="69490-132">If you do not have these libraries, you can download the latest jQuery library directly from the [jQuery.org](http://jQuery.org) site.</span></span> <span data-ttu-id="69490-133">O bien puede crear un sitio nuevo con una plantilla (por ejemplo, el **Starter Site** plantilla) y, a continuación, copie los archivos de jQuery desde ese sitio al sitio actual.</span><span class="sxs-lookup"><span data-stu-id="69490-133">Or you can create a new site using a template (for example, the **Starter Site** template) and then copy the jQuery files from that site to your current site.</span></span>

<span data-ttu-id="69490-134">Por último, si desea usar Bing maps, primero debe crear una cuenta (gratis) y obtener una clave.</span><span class="sxs-lookup"><span data-stu-id="69490-134">Finally, if you want to use Bing maps, you must first create a (free) account and get a key.</span></span> <span data-ttu-id="69490-135">Para obtener una clave, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="69490-135">To get a key, follow these steps:</span></span>

1. <span data-ttu-id="69490-136">Crear una cuenta en el [cuenta de desarrollador de mapas de Bing](https://www.microsoft.com/maps/developers/web.aspx).</span><span class="sxs-lookup"><span data-stu-id="69490-136">Create an account on the [Bing Maps Developer Account](https://www.microsoft.com/maps/developers/web.aspx).</span></span> <span data-ttu-id="69490-137">Debe tener también una cuenta de Microsoft (Windows Live ID).</span><span class="sxs-lookup"><span data-stu-id="69490-137">You must have a Microsoft account (Windows Live ID) as well.</span></span>

    <span data-ttu-id="69490-138">Puede especificar que desea usar la clave de **evaluación y pruebas**.</span><span class="sxs-lookup"><span data-stu-id="69490-138">You can specify that you want to use the key for **Evaluation/Test**.</span></span> <span data-ttu-id="69490-139">Si va a probar la función de asignación en su propio equipo con WebMatrix y IIS Express, vaya el **sitio** área de trabajo y anote la dirección URL del sitio (por ejemplo, `http://localhost:50408`, aunque el número de puerto probablemente serán diferente).</span><span class="sxs-lookup"><span data-stu-id="69490-139">If you are testing the mapping function on your own computer using WebMatrix and IIS Express, go the **Site** workspace and note the URL of your site (for example, `http://localhost:50408`, although your port number will probably be different).</span></span> <span data-ttu-id="69490-140">Puede utilizar esta *localhost* dirección que el sitio al registrar.</span><span class="sxs-lookup"><span data-stu-id="69490-140">You can use this *localhost* address as the site when you register.</span></span>
2. <span data-ttu-id="69490-141">Después de haber registrado para una cuenta, vaya al centro de cuentas de mapas de Bing y haga clic en **permite crear o ver las claves**:</span><span class="sxs-lookup"><span data-stu-id="69490-141">After you have registered for an account, go to the Bing Maps Account Center and click **Create or view keys**:</span></span>

    ![asignación-2](displaying-maps-in-an-aspnet-web-pages-site/_static/image1.png)
3. <span data-ttu-id="69490-143">Registre la clave que se crea de Bing.</span><span class="sxs-lookup"><span data-stu-id="69490-143">Record the key that Bing creates.</span></span>

## <a name="creating-a-map-based-on-an-address-using-google"></a><span data-ttu-id="69490-144">Creación de un mapa basándose en una dirección (con Google)</span><span class="sxs-lookup"><span data-stu-id="69490-144">Creating a Map Based on an Address (Using Google)</span></span>

<span data-ttu-id="69490-145">En el ejemplo siguiente se muestra cómo crear una página que representa un mapa basándose en una dirección.</span><span class="sxs-lookup"><span data-stu-id="69490-145">The following example shows how to create a page that renders a map based on an address.</span></span> <span data-ttu-id="69490-146">Este ejemplo muestra cómo usar Google Maps.</span><span class="sxs-lookup"><span data-stu-id="69490-146">This example shows how to use Google Maps.</span></span>

1. <span data-ttu-id="69490-147">Cree un archivo denominado *MapAddress.cshtml* en la raíz del sitio.</span><span class="sxs-lookup"><span data-stu-id="69490-147">Create a file named *MapAddress.cshtml* in the root of the site.</span></span> <span data-ttu-id="69490-148">Esta página generará un mapa basándose en una dirección que se pasa a él.</span><span class="sxs-lookup"><span data-stu-id="69490-148">This page will generate a map based on an address that you pass to it.</span></span>
2. <span data-ttu-id="69490-149">Copie el código siguiente en el archivo, sobrescribiendo el contenido existente.</span><span class="sxs-lookup"><span data-stu-id="69490-149">Copy the following code into the file, overwriting the existing content.</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="69490-150">Tenga en cuenta las siguientes características de la página:</span><span class="sxs-lookup"><span data-stu-id="69490-150">Notice the following features of the page:</span></span>

    - <span data-ttu-id="69490-151">El `<script>` elemento en el `<head>` elemento.</span><span class="sxs-lookup"><span data-stu-id="69490-151">The `<script>` element in the `<head>` element.</span></span> <span data-ttu-id="69490-152">En el ejemplo, el `<script>` referencias del elemento de la *1.6.4.min.js jquery* archivo, que es una versión reducida (comprimida) de la biblioteca de jQuery, versión 1.6.4.</span><span class="sxs-lookup"><span data-stu-id="69490-152">In the example, the `<script>` element references the *jquery-1.6.4.min.js* file, which is a minified (compressed) version of the jQuery library, version 1.6.4.</span></span> <span data-ttu-id="69490-153">Tenga en cuenta que la referencia se da por supuesto que el *.js* archivo se encuentra en la *Scripts* carpeta de su sitio.</span><span class="sxs-lookup"><span data-stu-id="69490-153">Note that the reference assumes that the *.js* file is in the *Scripts* folder of your site.</span></span> 

        > [!NOTE]
        > <span data-ttu-id="69490-154">Si usa una versión diferente de la biblioteca de jQuery, asegúrese de que está señalando a esa versión correctamente.</span><span class="sxs-lookup"><span data-stu-id="69490-154">If you're using a different version of the jQuery library, just make sure that you're pointing to that version correctly.</span></span>
    - <span data-ttu-id="69490-155">La llamada a la `@Maps.GetGoogleHtml` en el cuerpo de la página.</span><span class="sxs-lookup"><span data-stu-id="69490-155">The call to the `@Maps.GetGoogleHtml` in the body of the page.</span></span> <span data-ttu-id="69490-156">Para asignar una dirección, debe pasar una cadena de dirección.</span><span class="sxs-lookup"><span data-stu-id="69490-156">To map an address, you must pass an address string.</span></span> <span data-ttu-id="69490-157">Los métodos para el resto de los motores de asignación de trabajo de una manera similar (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span><span class="sxs-lookup"><span data-stu-id="69490-157">The methods for the other map engines work in a similar way (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).</span></span>
- <span data-ttu-id="69490-158">Ejecute la página e introduzca una dirección.</span><span class="sxs-lookup"><span data-stu-id="69490-158">Run the page and enter an address.</span></span> <span data-ttu-id="69490-159">La página muestra una asignación, en función de Google Maps, que muestra la ubicación que especificó.</span><span class="sxs-lookup"><span data-stu-id="69490-159">The page displays a map, based on Google Maps, that shows the location that you specified.</span></span>

    ![asignación-1](displaying-maps-in-an-aspnet-web-pages-site/_static/image2.png)

## <a name="creating-a-map-based-on-latitude-and-longitude-coordinates-using-bing"></a><span data-ttu-id="69490-161">Creación de un mapa en función de latitud y longitud coordina (con Bing)</span><span class="sxs-lookup"><span data-stu-id="69490-161">Creating a Map Based on Latitude and Longitude Coordinates (Using Bing)</span></span>

<span data-ttu-id="69490-162">En este ejemplo se muestra cómo crear un mapa en función de coordenadas.</span><span class="sxs-lookup"><span data-stu-id="69490-162">This example shows how to create a map based on coordinates.</span></span> <span data-ttu-id="69490-163">En este ejemplo se muestra cómo usar Bing maps y cómo incluir la clave de Bing.</span><span class="sxs-lookup"><span data-stu-id="69490-163">This example shows how to use Bing maps and how to include your Bing key.</span></span> <span data-ttu-id="69490-164">(Puede crear un mapa en función de coordenadas con el resto de los motores de mapa también, sin usar una clave de Bing.)</span><span class="sxs-lookup"><span data-stu-id="69490-164">(You can create a map based on coordinates using the other map engines also, without using a Bing key.)</span></span>

1. <span data-ttu-id="69490-165">Cree un archivo denominado *MapCoordinates.cshtml* en la raíz del sitio y reemplazar el contenido existente con el código y el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="69490-165">Create a file named *MapCoordinates.cshtml* in the root of the site and replace the existing content with the following code and markup:</span></span>

    [!code-cshtml[Main](displaying-maps-in-an-aspnet-web-pages-site/samples/sample2.cshtml)]
2. <span data-ttu-id="69490-166">Reemplace `your-key-here` con la clave de mapas de Bing que ha generado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="69490-166">Replace `your-key-here` with the Bing Maps key that you generated earlier.</span></span>
3. <span data-ttu-id="69490-167">Ejecute el *MapCoordinates.cshtml* página, escriba las coordenadas de latitud y longitud y, a continuación, haga clic en el **Map It!**</span><span class="sxs-lookup"><span data-stu-id="69490-167">Run the *MapCoordinates.cshtml* page, enter latitude and longitude coordinates, and then click the **Map It!**</span></span> <span data-ttu-id="69490-168">.</span><span class="sxs-lookup"><span data-stu-id="69490-168">button.</span></span> <span data-ttu-id="69490-169">(Si no conoce las coordenadas, intente lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="69490-169">(If you don't know any coordinates, try the following.</span></span> <span data-ttu-id="69490-170">Esta es una ubicación en la sede de Redmond de Microsoft.)</span><span class="sxs-lookup"><span data-stu-id="69490-170">This is a location on the Microsoft Redmond campus.)</span></span>

    - <span data-ttu-id="69490-171">Latitud: 47.6781005859375</span><span class="sxs-lookup"><span data-stu-id="69490-171">Latitude: 47.6781005859375</span></span>
    - <span data-ttu-id="69490-172">Longitud:-122.158317565918</span><span class="sxs-lookup"><span data-stu-id="69490-172">Longitude: -122.158317565918</span></span>

    <span data-ttu-id="69490-173">Se abrirá la página utilizando las coordenadas que especificó.</span><span class="sxs-lookup"><span data-stu-id="69490-173">The page is displayed using the coordinates that you specified.</span></span>

    ![asignación-3](displaying-maps-in-an-aspnet-web-pages-site/_static/image3.png)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="69490-175">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="69490-175">Additional Resources</span></span>


[<span data-ttu-id="69490-176">Referencia de la API de Microsoft.Maps</span><span class="sxs-lookup"><span data-stu-id="69490-176">Microsoft.Maps API Reference</span></span>](https://msdn.microsoft.com/en-us/library/gg427611.aspx)
