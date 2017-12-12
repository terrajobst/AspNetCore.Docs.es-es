---
uid: ajax/cdn/overview
title: Red de entrega de contenido de Microsoft Ajax | Documentos de Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="b6ce4-102">Red de entrega de contenido de Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="b6ce4-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="b6ce4-103">Nota: La CDN de Microsoft Ajax no tiene ningún SLA más allá del uso de una CDN de Azure.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="b6ce4-104">Tabla de contenido</span><span class="sxs-lookup"><span data-stu-id="b6ce4-104">Table of Contents</span></span>

<span data-ttu-id="b6ce4-105">**[AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="b6ce4-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="b6ce4-106">**[Compatibilidad de Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="b6ce4-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="b6ce4-107">**[Uso de Ajax de ASP.NET de la red CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="b6ce4-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="b6ce4-108">**[Mediante jQuery desde la red CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="b6ce4-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="b6ce4-109">**[Mediante jQuery UI de la red CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="b6ce4-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="b6ce4-110">**[Archivos de terceros en la red CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="b6ce4-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="b6ce4-111">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="b6ce4-112">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="b6ce4-113">Versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="b6ce4-114">Versiones de la validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="b6ce4-115">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="b6ce4-116">Versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="b6ce4-117">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="b6ce4-118">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="b6ce4-119">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="b6ce4-120">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="b6ce4-121">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="b6ce4-122">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="b6ce4-123">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="b6ce4-124">Versiones de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="b6ce4-125">Versiones de TouchCarousel de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="b6ce4-126">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="b6ce4-127">Formularios Web Forms ASP.NET y Ajax versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="b6ce4-128">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="b6ce4-129">ASP.NET SignalR libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="b6ce4-130">La red contenido de entrega de Microsoft Ajax (CDN) hospeda bibliotecas de JavaScript de terceros populares como jQuery y le permite fácilmente agregarlas a sus aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="b6ce4-131">Por ejemplo, puede empezar a usar jQuery que se hospeda en esta red CDN mediante la adición de un &lt;script&gt; etiqueta a la página que señala a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="b6ce4-132">Aprovechando las ventajas de la red CDN, puede mejorar notablemente el rendimiento de las aplicaciones Ajax.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="b6ce4-133">El contenido de la red CDN se almacenan en caché en servidores ubicados en todo el mundo.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="b6ce4-134">Además, la red CDN permite exploradores reutilicen los archivos de JavaScript de terceros en caché para los sitios web que se encuentran en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="b6ce4-135">La red CDN es compatible con SSL (HTTPS), en caso de que necesite servir una página web con la capa de Sockets seguros.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="b6ce4-136">La red CDN hospeda las siguientes bibliotecas de scripts de terceros que se han cargado y que están autorizadas, los propietarios de dichas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="b6ce4-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="b6ce4-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="b6ce4-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="b6ce4-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="b6ce4-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="b6ce4-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="b6ce4-140">jQuery validación (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="b6ce4-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="b6ce4-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="b6ce4-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="b6ce4-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="b6ce4-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="b6ce4-143">La CDN de Microsoft Ajax también incluye las siguientes bibliotecas que se han cargado por Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="b6ce4-144">Ajax de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="b6ce4-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="b6ce4-145">Archivos de JavaScript de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="b6ce4-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="b6ce4-146">Archivos ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="b6ce4-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="b6ce4-147">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-148">Los propietarios del copyright de las bibliotecas licencias estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="b6ce4-149">Se conceden los derechos que tendrá que descargar y usar estas bibliotecas únicamente por los propietarios del copyright respectivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="b6ce4-150">Dado que estos no son las bibliotecas de Microsoft, Microsoft no proporciona ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patentes implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="b6ce4-151">Si desea volver a enviar la biblioteca de JavaScript y la biblioteca es una de las bibliotecas de JavaScript (como se muestra en http://trends.builtwith.com) o extensiones y complementos a estas bibliotecas que se superior (a) populares; o (b) útil para usar en ASP.NET, a continuación, póngase en contacto con AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="b6ce4-152">AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="b6ce4-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="b6ce4-153">La red CDN permite usar el nombre de dominio microsoft.com y se ha cambiado para usar el nombre de dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="b6ce4-154">Este cambio se realizó para aumentar el rendimiento porque cuando un explorador al que hace referencia el dominio microsoft.com podría enviar ninguna cookie de ese dominio a través de la conexión con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="b6ce4-155">Al cambiar el nombre a un nombre de dominio que no sea de microsoft.com puede aumentar rendimiento tanta a 25%.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="b6ce4-156">Tenga en cuenta ajax.microsoft.com continuarán funcionando pero se recomienda ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="b6ce4-157">Formato antiguo: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="b6ce4-158">Nuevo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="b6ce4-159">Compatibilidad de Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="b6ce4-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="b6ce4-160">Para utilizar correctamente los archivos de .vsdoc con Visual Studio 2008 debe asegurarse de que dispone de VS 2008 SP1 instalado, y la revisión para archivos de vsdoc instalada.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="b6ce4-161">Puede obtener desde aquí:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-161">You can get these from here:</span></span>

- [<span data-ttu-id="b6ce4-162">Descargar Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "descargar Visual Studio 2008 SP1")
- [<span data-ttu-id="b6ce4-163">Descargar la revisión de .vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "descargar la revisión de .vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="b6ce4-164">Visual Studio 2010 es compatible con archivos de .vsdoc sin las revisiones adicionales.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="b6ce4-165">Uso de Ajax de ASP.NET de la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="b6ce4-166">Cuando se usa ASP.NET 4, puede redirigir todas las solicitudes de scripts del marco ASP.NET a la red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="b6ce4-167">Recuperar las secuencias de comandos de la red CDN en lugar de su servidor web local esencialmente puede mejorar el rendimiento de sitios Web ASP.NET públicos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="b6ce4-168">Utilice la propiedad ScriptManager EnableCDN para redirigir todas las solicitudes de secuencia de comandos de marco de trabajo ASP.NET a la CDN de Ajax de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="b6ce4-169">Mediante jQuery desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="b6ce4-170">Puede usar scripts de jQuery hospedados en la red CDN en su aplicación Web agregando el siguiente elemento de secuencia de comandos a una página:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="b6ce4-171">La red CDN también incluye la versión reducida de la secuencia de comandos de jQuery, que puede obtener mediante el siguiente elemento:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="b6ce4-172">Para permitir que la página de retroceder a la carga de jQuery desde una ruta local en su propio sitio Web si se produce la CDN no esté disponible, agregue el siguiente elemento inmediatamente después del elemento que hacen referencia a la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="b6ce4-173">La página de ejemplo siguiente usa la versión de la red CDN de la biblioteca de jQuery (con reserva de una copia local) para mostrar el contenido de un elemento div cuando se hace clic en un botón.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="b6ce4-174">Puede obtener más información acerca de jQuery y descargar una copia local de jQuery visitando la [jQuery](http://jquery.com/) sitio Web.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="b6ce4-175">Mediante jQuery UI de la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="b6ce4-176">La red CDN también hospeda la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="b6ce4-177">La biblioteca de interfaz de usuario jQuery incluye un amplio conjunto de widgets y los efectos que puede usar en las aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="b6ce4-178">Por ejemplo, en la página siguiente se muestra cómo puede usar jQuery Datepicker de interfaz de usuario en el contexto de una aplicación de formularios Web Forms de ASP.NET para mostrar un calendario emergente:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="b6ce4-179">Al mover el foco en el cuadro de texto utilizando el teclado, se muestra un calendario:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario emergente creado con Datepicker](overview/_static/image1.png)

<span data-ttu-id="b6ce4-181">Tenga en cuenta que debe incluir tres archivos de la red CDN en el código anterior:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="b6ce4-182">La biblioteca de jQuery &mdash; la biblioteca de interfaz de usuario de jQuery depende de la biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="b6ce4-183">Debe agregar la biblioteca de jQuery a la página antes de agregar la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="b6ce4-184">La biblioteca de interfaz de usuario jQuery &mdash; la biblioteca de interfaz de usuario jQuery contiene todos los efectos de la interfaz de usuario de jQuery y widgets, como el widget Datepicker utilizado en la página anterior.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="b6ce4-185">Un tema de la interfaz de usuario jQuery &mdash; jQuery UI admite distintos temas.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="b6ce4-186">La página anterior incluye un vínculo a un archivo CSS para importar el tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="b6ce4-187">Todos los temas de la interfaz de usuario jQuery estándar se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="b6ce4-188">[Visite esta página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en CDN de Microsoft Ajax") para ver vistas en miniatura para cada tema.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="b6ce4-189">Para obtener más información sobre la biblioteca de interfaz de usuario jQuery, visite el oficial [sitio Web de interfaz de usuario de jQuery](http://jQueryUI.com "sitio Web de interfaz de usuario de jQuery").</span><span class="sxs-lookup"><span data-stu-id="b6ce4-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="b6ce4-190">Archivos de terceros en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="b6ce4-191">La red CDN hospeda algunas de las bibliotecas de JavaScript de terceros más populares.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="b6ce4-192">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-193">Los propietarios del copyright de las bibliotecas licencias estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="b6ce4-194">Se conceden los derechos que tendrá que descargar y usar estas bibliotecas únicamente por los propietarios del copyright respectivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="b6ce4-195">Dado que estos no son las bibliotecas de Microsoft, Microsoft no proporciona ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patentes implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-196">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-197">Las versiones siguientes de jQuery se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="b6ce4-198">versión de jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="b6ce4-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="b6ce4-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="b6ce4-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="b6ce4-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="b6ce4-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="b6ce4-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="b6ce4-205">versión de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="b6ce4-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="b6ce4-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="b6ce4-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="b6ce4-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="b6ce4-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="b6ce4-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="b6ce4-212">versión de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="b6ce4-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="b6ce4-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="b6ce4-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="b6ce4-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="b6ce4-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="b6ce4-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="b6ce4-219">versión de jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="b6ce4-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="b6ce4-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="b6ce4-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="b6ce4-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="b6ce4-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="b6ce4-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="b6ce4-226">versión de jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="b6ce4-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="b6ce4-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="b6ce4-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="b6ce4-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="b6ce4-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="b6ce4-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="b6ce4-233">versión de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="b6ce4-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="b6ce4-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="b6ce4-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="b6ce4-237">versión de jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="b6ce4-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="b6ce4-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="b6ce4-240">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="b6ce4-241">versión de jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="b6ce4-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="b6ce4-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="b6ce4-244">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="b6ce4-245">versión 2.2.1 del jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="b6ce4-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="b6ce4-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="b6ce4-248">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="b6ce4-249">versión de jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="b6ce4-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="b6ce4-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="b6ce4-252">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="b6ce4-253">versión de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="b6ce4-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="b6ce4-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="b6ce4-256">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="b6ce4-257">versión de jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="b6ce4-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="b6ce4-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="b6ce4-260">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="b6ce4-261">versión de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="b6ce4-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="b6ce4-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="b6ce4-264">versión de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="b6ce4-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="b6ce4-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="b6ce4-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="b6ce4-268">versión de jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="b6ce4-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="b6ce4-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="b6ce4-271">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="b6ce4-273">versión de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="b6ce4-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="b6ce4-275">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="b6ce4-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="b6ce4-278">versión de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="b6ce4-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="b6ce4-280">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="b6ce4-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="b6ce4-283">versión de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="b6ce4-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="b6ce4-285">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="b6ce4-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="b6ce4-288">versión de jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="b6ce4-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="b6ce4-290">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="b6ce4-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="b6ce4-293">versión de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="b6ce4-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="b6ce4-295">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="b6ce4-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="b6ce4-297">versión de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="b6ce4-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="b6ce4-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="b6ce4-300">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="b6ce4-301">versión de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="b6ce4-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="b6ce4-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="b6ce4-304">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="b6ce4-305">versión de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="b6ce4-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="b6ce4-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="b6ce4-308">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="b6ce4-309">versión de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="b6ce4-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="b6ce4-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="b6ce4-312">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="b6ce4-313">versión de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="b6ce4-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="b6ce4-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="b6ce4-316">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="b6ce4-317">versión de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="b6ce4-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="b6ce4-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="b6ce4-320">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="b6ce4-321">versión de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="b6ce4-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="b6ce4-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="b6ce4-324">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="b6ce4-325">versión de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="b6ce4-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="b6ce4-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="b6ce4-328">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="b6ce4-330">versión de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="b6ce4-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="b6ce4-332">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="b6ce4-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="b6ce4-335">versión de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="b6ce4-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="b6ce4-337">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="b6ce4-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="b6ce4-340">versión de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="b6ce4-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="b6ce4-342">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="b6ce4-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="b6ce4-345">versión de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="b6ce4-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="b6ce4-347">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="b6ce4-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="b6ce4-350">versión de jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="b6ce4-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="b6ce4-352">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="b6ce4-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="b6ce4-355">versión de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="b6ce4-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="b6ce4-357">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="b6ce4-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="b6ce4-359">versión de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="b6ce4-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="b6ce4-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="b6ce4-362">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="b6ce4-363">versión de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="b6ce4-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="b6ce4-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="b6ce4-366">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="b6ce4-367">versión de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="b6ce4-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="b6ce4-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="b6ce4-370">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="b6ce4-371">versión de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="b6ce4-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="b6ce4-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="b6ce4-374">versión de jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="b6ce4-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="b6ce4-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="b6ce4-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="b6ce4-378">jQuery versión 1.7</span><span class="sxs-lookup"><span data-stu-id="b6ce4-378">jQuery version 1.7</span></span>

- <span data-ttu-id="b6ce4-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="b6ce4-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="b6ce4-381">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="b6ce4-382">versión de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="b6ce4-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="b6ce4-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="b6ce4-385">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="b6ce4-386">versión de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="b6ce4-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="b6ce4-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="b6ce4-389">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="b6ce4-390">versión de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="b6ce4-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="b6ce4-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="b6ce4-393">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="b6ce4-394">versión de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="b6ce4-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="b6ce4-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="b6ce4-397">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="b6ce4-398">jQuery versión 1.6</span><span class="sxs-lookup"><span data-stu-id="b6ce4-398">jQuery version 1.6</span></span>

- <span data-ttu-id="b6ce4-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="b6ce4-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="b6ce4-401">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="b6ce4-402">versión de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="b6ce4-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="b6ce4-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="b6ce4-405">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="b6ce4-406">versión de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="b6ce4-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="b6ce4-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="b6ce4-409">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="b6ce4-410">versión de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="b6ce4-410">jQuery version 1.5</span></span>

- <span data-ttu-id="b6ce4-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="b6ce4-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="b6ce4-413">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="b6ce4-414">versión de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="b6ce4-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="b6ce4-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="b6ce4-417">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="b6ce4-418">versión de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="b6ce4-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="b6ce4-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="b6ce4-421">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="b6ce4-422">versión de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="b6ce4-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="b6ce4-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="b6ce4-425">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="b6ce4-426">versión de jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="b6ce4-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="b6ce4-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="b6ce4-429">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="b6ce4-430">versión de jQuery 1.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-430">jQuery version 1.4</span></span>

- <span data-ttu-id="b6ce4-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="b6ce4-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="b6ce4-433">versión de jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="b6ce4-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="b6ce4-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="b6ce4-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="b6ce4-437">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-438">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-439">Las versiones siguientes de jQuery migrar se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="b6ce4-440">jQuery migrar versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="b6ce4-441">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="b6ce4-442">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="b6ce4-443">jQuery migrar versión 1.2.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="b6ce4-444">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="b6ce4-445">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="b6ce4-446">jQuery migrar versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="b6ce4-447">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="b6ce4-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="b6ce4-449">jQuery migrar versión 1.1.1.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="b6ce4-450">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="b6ce4-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="b6ce4-452">jQuery migrar versión 1.1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="b6ce4-453">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="b6ce4-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="b6ce4-455">Migrar la versión 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="b6ce4-456">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="b6ce4-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-458">Versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-459">Las siguientes versiones de la biblioteca de interfaz de usuario de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-460">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b6ce4-461">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-462">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-463">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-464">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-465">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-466">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-467">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-468">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-469">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-470">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-471">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-472">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-473">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-474">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-475">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-476">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="b6ce4-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-477">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="b6ce4-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-478">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="b6ce4-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-479">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="b6ce4-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-480">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="b6ce4-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-481">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="b6ce4-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-482">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="b6ce4-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-483">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="b6ce4-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-484">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="b6ce4-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-485">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="b6ce4-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-486">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="b6ce4-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-487">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="b6ce4-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-488">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="b6ce4-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-489">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="b6ce4-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-490">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="b6ce4-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-491">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="b6ce4-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-492">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="b6ce4-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-493">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="b6ce4-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-494">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="b6ce4-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-495">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="b6ce4-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-496">Versiones de la validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-497">Las siguientes versiones de la biblioteca de validación de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-498">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b6ce4-499">jQuery validar 1.17.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery validación 1.17.0")
- [<span data-ttu-id="b6ce4-500">jQuery validar 1.16.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery validación 1.16.0")
- [<span data-ttu-id="b6ce4-501">jQuery validar 1.15.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery validación 1.15.1")
- [<span data-ttu-id="b6ce4-502">jQuery validar 1.15.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery validación 1.15.0")
- [<span data-ttu-id="b6ce4-503">jQuery validar 1.14.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery validación 1.14.0")
- [<span data-ttu-id="b6ce4-504">jQuery validar 1.13.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery validación 1.13.1")
- [<span data-ttu-id="b6ce4-505">jQuery validar 1.13.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery validación 1.13.0")
- [<span data-ttu-id="b6ce4-506">jQuery validar 1.12.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery validación 1.12.0")
- [<span data-ttu-id="b6ce4-507">jQuery validar 1.11.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery validación 1.11.1")
- [<span data-ttu-id="b6ce4-508">jQuery validar 1.11.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery validación 1.11.0")
- [<span data-ttu-id="b6ce4-509">jQuery validar 1.10.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery validación 1.10.0")
- [<span data-ttu-id="b6ce4-510">jQuery validar 1.9</span><span class="sxs-lookup"><span data-stu-id="b6ce4-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versión 1.9")
- [<span data-ttu-id="b6ce4-511">jQuery validar 1.8.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versión 1.8.1")
- [<span data-ttu-id="b6ce4-512">jQuery validar 1.8</span><span class="sxs-lookup"><span data-stu-id="b6ce4-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versión 1.8")
- [<span data-ttu-id="b6ce4-513">jQuery validar 1.7</span><span class="sxs-lookup"><span data-stu-id="b6ce4-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versión 1.7")
- [<span data-ttu-id="b6ce4-514">jQuery validar 1.6</span><span class="sxs-lookup"><span data-stu-id="b6ce4-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validar 1.6")
- [<span data-ttu-id="b6ce4-515">jQuery validar 1.5.5</span><span class="sxs-lookup"><span data-stu-id="b6ce4-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validar 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-516">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-517">Las siguientes versiones de la biblioteca de jQuery móvil se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-518">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b6ce4-519">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="b6ce4-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-520">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-521">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-522">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-523">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-524">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-525">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-526">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-527">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-528">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-529">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-530">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-531">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-532">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 en la CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-533">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-534">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 de CDN de Microsoft Ajax")
- [<span data-ttu-id="b6ce4-535">versión beta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 en la CDN de Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-536">Versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-537">Las siguientes versiones del complemento de plantillas de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-538">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b6ce4-539">jQuery plantillas Beta 1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery plantillas Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-540">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="b6ce4-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-541">Las siguientes versiones del complemento de ciclo de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-542">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b6ce4-543">jQuery ciclo 2,99</span><span class="sxs-lookup"><span data-stu-id="b6ce4-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [<span data-ttu-id="b6ce4-544">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="b6ce4-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery ciclo 2.94")
- [<span data-ttu-id="b6ce4-545">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="b6ce4-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-546">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-547">Las siguientes versiones del complemento DataTables de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="b6ce4-548">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b6ce4-549">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="b6ce4-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="b6ce4-550">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="b6ce4-551">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="b6ce4-552">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="b6ce4-553">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="b6ce4-554">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="b6ce4-555">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="b6ce4-556">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-557">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-558">Los siguientes versiones de [Modernizr](http://www.modernizr.com "Modernizr") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="b6ce4-559">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="b6ce4-560">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="b6ce4-561">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="b6ce4-562">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="b6ce4-563">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="b6ce4-564">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-565">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-566">Los siguientes versiones de [JSHint](http://www.jshint.com "JSHint") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="b6ce4-567">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-568">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-569">Los siguientes versiones de [Knockout](http://www.knockoutjs.com "Knockout") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="b6ce4-570">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="b6ce4-571">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="b6ce4-572">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="b6ce4-573">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="b6ce4-574">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="b6ce4-575">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="b6ce4-576">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="b6ce4-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="b6ce4-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="b6ce4-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="b6ce4-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="b6ce4-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="b6ce4-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="b6ce4-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="b6ce4-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="b6ce4-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="b6ce4-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="b6ce4-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="b6ce4-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="b6ce4-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-590">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-591">Los siguientes versiones de [Globalize](https://github.com/jquery/globalize "Globalize") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="b6ce4-592">Globalizar la versión 1.0.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="b6ce4-593">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="b6ce4-594">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="b6ce4-595">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="b6ce4-596">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="b6ce4-597">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="b6ce4-598">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="b6ce4-599">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="b6ce4-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-time.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="b6ce4-601">Globalizar versión 0.1.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="b6ce4-602">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="b6ce4-603">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="b6ce4-604">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Cultures.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="b6ce4-605">todas las referencias culturales</span><span class="sxs-lookup"><span data-stu-id="b6ce4-605">all cultures</span></span>
- <span data-ttu-id="b6ce4-606">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Culture. {código de referencia cultural} .js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="b6ce4-607">Reemplace "{-código de referencia cultural}" por el código de la referencia cultural deseada, por ejemplo, Microsoft globalize.culture.en GB.js== archivos en la red CDN == estas bibliotecas cargadas por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-608">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-609">Los siguientes versiones de [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") responden se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="b6ce4-610">Responder versión 1.4.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="b6ce4-611">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="b6ce4-612">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="b6ce4-613">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="b6ce4-614">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="b6ce4-615">Responder versión 1.4.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="b6ce4-616">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="b6ce4-617">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="b6ce4-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="b6ce4-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="b6ce4-620">Responder versión 1.4.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="b6ce4-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="b6ce4-622">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="b6ce4-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="b6ce4-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="b6ce4-625">Responder versión 1.3.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="b6ce4-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="b6ce4-627">Responder versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="b6ce4-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-629">Versiones de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-630">Los siguientes versiones de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="b6ce4-631">Versión de arranque 3.3.7</span><span class="sxs-lookup"><span data-stu-id="b6ce4-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="b6ce4-632">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-633">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-634">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-635">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-636">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-637">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-638">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b6ce4-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="b6ce4-645">Versión de arranque 3.3.6</span><span class="sxs-lookup"><span data-stu-id="b6ce4-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="b6ce4-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-652">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b6ce4-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="b6ce4-659">Arranque versión 3.3.5</span><span class="sxs-lookup"><span data-stu-id="b6ce4-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="b6ce4-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-666">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b6ce4-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="b6ce4-673">Arranque versión 3.3.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="b6ce4-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-680">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b6ce4-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="b6ce4-687">Versión de arranque 3.3.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="b6ce4-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-694">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b6ce4-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="b6ce4-701">Versión de arranque 3.3.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="b6ce4-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-708">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="b6ce4-714">Versión de arranque 3.3.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="b6ce4-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-721">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="b6ce4-727">Versión de arranque 3.2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="b6ce4-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-734">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="b6ce4-740">Versión de arranque 3.1.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="b6ce4-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-747">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="b6ce4-753">Arranque versión 3.1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="b6ce4-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b6ce4-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-760">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b6ce4-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="b6ce4-766">Arranque versión 3.0.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="b6ce4-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-773">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="b6ce4-777">Arranque versión 3.0.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="b6ce4-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-784">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="b6ce4-788">Arranque versión 3.0.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="b6ce4-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-795">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="b6ce4-799">Arranque versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="b6ce4-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b6ce4-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b6ce4-806">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="b6ce4-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b6ce4-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b6ce4-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b6ce4-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b6ce4-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b6ce4-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="b6ce4-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="b6ce4-810">Arranque versión 2.3.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="b6ce4-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="b6ce4-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="b6ce4-817">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/IMG/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="b6ce4-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="b6ce4-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/IMG/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="b6ce4-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="b6ce4-819">Arranque versión 2.3.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="b6ce4-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="b6ce4-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b6ce4-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b6ce4-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b6ce4-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="b6ce4-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="b6ce4-826">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/IMG/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="b6ce4-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="b6ce4-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/IMG/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="b6ce4-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-828">Versiones de TouchCarousel de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-829">Los siguientes versiones de [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel Bootstrap versiones se hospedan en la red CDN :</span><span class="sxs-lookup"><span data-stu-id="b6ce4-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="b6ce4-830">Arranque TouchCarousel versión 0.8.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="b6ce4-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/CSS/Bootstrap-Touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="b6ce4-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="b6ce4-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/js/Bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-833">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-834">Los siguientes versiones de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versiones se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="b6ce4-835">Hammer.js versión 2.0.4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="b6ce4-836">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="b6ce4-837">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="b6ce4-838">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.Map</span><span class="sxs-lookup"><span data-stu-id="b6ce4-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-839">Formularios Web Forms ASP.NET y Ajax versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-840">Las siguientes versiones de la biblioteca de Ajax de ASP.NET se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="b6ce4-841">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="b6ce4-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b6ce4-842">Formularios Web Forms ASP.NET y Ajax versión 4.5.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularios Web Forms de ASP.NET y Ajax 4.5.2")
- [<span data-ttu-id="b6ce4-843">Versión de formularios Web Forms ASP.NET y Ajax 4</span><span class="sxs-lookup"><span data-stu-id="b6ce4-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularios Web Forms de ASP.NET y Ajax 4")
- [<span data-ttu-id="b6ce4-844">Ajax de ASP.NET versión 3.5</span><span class="sxs-lookup"><span data-stu-id="b6ce4-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax de ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-845">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-846">Los siguientes archivos JavaScript de ASP.NET MVC se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="b6ce4-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="b6ce4-848">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b6ce4-849">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="b6ce4-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="b6ce4-851">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b6ce4-852">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="b6ce4-853">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="b6ce4-854">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b6ce4-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="b6ce4-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="b6ce4-857">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b6ce4-858">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="b6ce4-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="b6ce4-860">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="b6ce4-861">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="b6ce4-862">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b6ce4-863">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="b6ce4-864">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="b6ce4-865">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="b6ce4-866">MVC DE ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="b6ce4-867">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="b6ce4-868">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="b6ce4-869">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="b6ce4-870">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="b6ce4-871">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="b6ce4-872">ASP.NET SignalR libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="b6ce4-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="b6ce4-873">Los siguientes archivos de ASP.NET SignalR JavaScript se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="b6ce4-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="b6ce4-874">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="b6ce4-875">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="b6ce4-876">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="b6ce4-877">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="b6ce4-878">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="b6ce4-879">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="b6ce4-880">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="b6ce4-881">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="b6ce4-882">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="b6ce4-883">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="b6ce4-884">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="b6ce4-885">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="b6ce4-886">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="b6ce4-887">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="b6ce4-888">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="b6ce4-889">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="b6ce4-890">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="b6ce4-891">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="b6ce4-892">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="b6ce4-893">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="b6ce4-894">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="b6ce4-895">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="b6ce4-896">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="b6ce4-897">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="b6ce4-898">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="b6ce4-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="b6ce4-899">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="b6ce4-900">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="b6ce4-901">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="b6ce4-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="b6ce4-902">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="b6ce4-903">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="b6ce4-904">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="b6ce4-905">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="b6ce4-906">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="b6ce4-907">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="b6ce4-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="b6ce4-908">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="b6ce4-909">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="b6ce4-910">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="b6ce4-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="b6ce4-911">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="b6ce4-912">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="b6ce4-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="b6ce4-913">Para obtener información acerca de los términos de uso de la red CDN, vea [Microsoft Ajax CDN términos de uso](https://www.asp.net/terms-of-use "Microsoft Ajax CDN términos de uso").</span><span class="sxs-lookup"><span data-stu-id="b6ce4-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
