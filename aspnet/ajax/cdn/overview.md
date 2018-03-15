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
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="69d5e-102">Red de entrega de contenido de Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="69d5e-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="69d5e-103">Nota: La CDN de Microsoft Ajax no tiene ningún SLA más allá del uso de una CDN de Azure.</span><span class="sxs-lookup"><span data-stu-id="69d5e-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="69d5e-104">Tabla de contenido</span><span class="sxs-lookup"><span data-stu-id="69d5e-104">Table of Contents</span></span>

<span data-ttu-id="69d5e-105">**[AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="69d5e-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="69d5e-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="69d5e-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="69d5e-107">**[Uso de Ajax de ASP.NET de la red CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="69d5e-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="69d5e-108">**[Mediante jQuery desde la red CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="69d5e-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="69d5e-109">**[Mediante jQuery UI de la red CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="69d5e-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="69d5e-110">**[Archivos de terceros en la red CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="69d5e-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="69d5e-111">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="69d5e-112">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="69d5e-113">Versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="69d5e-114">Versiones de la validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="69d5e-115">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="69d5e-116">Versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="69d5e-117">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="69d5e-118">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="69d5e-119">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="69d5e-120">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="69d5e-121">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="69d5e-122">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="69d5e-123">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="69d5e-124">Versiones de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="69d5e-125">Versiones de TouchCarousel de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="69d5e-126">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="69d5e-127">Formularios Web Forms ASP.NET y Ajax versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="69d5e-128">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="69d5e-129">ASP.NET SignalR libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="69d5e-130">La red contenido de entrega de Microsoft Ajax (CDN) hospeda bibliotecas de JavaScript de terceros populares como jQuery y le permite fácilmente agregarlas a sus aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="69d5e-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="69d5e-131">Por ejemplo, puede empezar a usar jQuery que se hospeda en esta red CDN mediante la adición de un &lt;script&gt; etiqueta a la página que señala a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="69d5e-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="69d5e-132">Aprovechando las ventajas de la red CDN, puede mejorar notablemente el rendimiento de las aplicaciones Ajax.</span><span class="sxs-lookup"><span data-stu-id="69d5e-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="69d5e-133">El contenido de la red CDN se almacenan en caché en servidores ubicados en todo el mundo.</span><span class="sxs-lookup"><span data-stu-id="69d5e-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="69d5e-134">Además, la red CDN permite exploradores reutilicen los archivos de JavaScript de terceros en caché para los sitios web que se encuentran en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="69d5e-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="69d5e-135">La red CDN es compatible con SSL (HTTPS), en caso de que necesite servir una página web con la capa de Sockets seguros.</span><span class="sxs-lookup"><span data-stu-id="69d5e-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="69d5e-136">La red CDN hospeda las siguientes bibliotecas de scripts de terceros que se han cargado y que están autorizadas, los propietarios de dichas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="69d5e-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="69d5e-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="69d5e-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="69d5e-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="69d5e-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="69d5e-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="69d5e-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="69d5e-140">jQuery validación (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="69d5e-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="69d5e-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="69d5e-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="69d5e-142">jQuery (DataTableshttp://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="69d5e-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="69d5e-143">La CDN de Microsoft Ajax también incluye las siguientes bibliotecas que se han cargado por Microsoft:</span><span class="sxs-lookup"><span data-stu-id="69d5e-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="69d5e-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="69d5e-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="69d5e-145">Archivos de JavaScript de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="69d5e-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="69d5e-146">Archivos ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="69d5e-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="69d5e-147">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="69d5e-148">Los propietarios del copyright de las bibliotecas licencias estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="69d5e-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="69d5e-149">Se conceden los derechos que tendrá que descargar y usar estas bibliotecas únicamente por los propietarios del copyright respectivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="69d5e-150">Dado que estos no son las bibliotecas de Microsoft, Microsoft no proporciona ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patentes implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="69d5e-151">Si desea volver a enviar la biblioteca de JavaScript y la biblioteca es una de las bibliotecas de JavaScript superiores (como se muestra en http://trends.builtwith.com) o extensiones o complementos para estas bibliotecas que son (a) populares; o (b) útil para su uso en ASP.NET, a continuación, póngase en contacto con AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="69d5e-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="69d5e-152">AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="69d5e-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="69d5e-153">La red CDN permite usar el nombre de dominio microsoft.com y se ha cambiado para usar el nombre de dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="69d5e-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="69d5e-154">Este cambio se realizó para aumentar el rendimiento porque cuando un explorador al que hace referencia el dominio microsoft.com podría enviar ninguna cookie de ese dominio a través de la conexión con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="69d5e-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="69d5e-155">Al cambiar el nombre a un nombre de dominio que no sea de microsoft.com puede aumentar rendimiento tanta a 25%.</span><span class="sxs-lookup"><span data-stu-id="69d5e-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="69d5e-156">Tenga en cuenta ajax.microsoft.com continuarán funcionando pero se recomienda ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="69d5e-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="69d5e-157">Formato anterior: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="69d5e-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="69d5e-158">Nuevo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="69d5e-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="69d5e-159">Visual Studio .vsdoc Support</span><span class="sxs-lookup"><span data-stu-id="69d5e-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="69d5e-160">Para utilizar correctamente los archivos de .vsdoc con Visual Studio 2008 debe asegurarse de que dispone de VS 2008 SP1 instalado, y la revisión para archivos de vsdoc instalada.</span><span class="sxs-lookup"><span data-stu-id="69d5e-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="69d5e-161">Puede obtener desde aquí:</span><span class="sxs-lookup"><span data-stu-id="69d5e-161">You can get these from here:</span></span>

- [<span data-ttu-id="69d5e-162">Descargar Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="69d5e-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "descargar Visual Studio 2008 SP1")
- [<span data-ttu-id="69d5e-163">Descargar la revisión de .vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="69d5e-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "descargar la revisión de .vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="69d5e-164">Visual Studio 2010 es compatible con archivos de .vsdoc sin las revisiones adicionales.</span><span class="sxs-lookup"><span data-stu-id="69d5e-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="69d5e-165">Uso de Ajax de ASP.NET de la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="69d5e-166">Cuando se usa ASP.NET 4, puede redirigir todas las solicitudes de scripts del marco ASP.NET a la red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="69d5e-167">Recuperar las secuencias de comandos de la red CDN en lugar de su servidor web local esencialmente puede mejorar el rendimiento de sitios Web ASP.NET públicos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="69d5e-168">Utilice la propiedad ScriptManager EnableCDN para redirigir todas las solicitudes de secuencia de comandos de marco de trabajo ASP.NET a la CDN de Ajax de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="69d5e-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="69d5e-169">Mediante jQuery desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="69d5e-170">Puede usar scripts de jQuery hospedados en la red CDN en su aplicación Web agregando el siguiente elemento de secuencia de comandos a una página:</span><span class="sxs-lookup"><span data-stu-id="69d5e-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="69d5e-171">La red CDN también incluye la versión reducida de la secuencia de comandos de jQuery, que puede obtener mediante el siguiente elemento:</span><span class="sxs-lookup"><span data-stu-id="69d5e-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="69d5e-172">Para permitir que la página de retroceder a la carga de jQuery desde una ruta local en su propio sitio Web si se produce la CDN no esté disponible, agregue el siguiente elemento inmediatamente después del elemento que hacen referencia a la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="69d5e-173">La página de ejemplo siguiente usa la versión de la red CDN de la biblioteca de jQuery (con reserva de una copia local) para mostrar el contenido de un elemento div cuando se hace clic en un botón.</span><span class="sxs-lookup"><span data-stu-id="69d5e-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="69d5e-174">Puede obtener más información acerca de jQuery y descargar una copia local de jQuery visitando la [jQuery](http://jquery.com/) sitio Web.</span><span class="sxs-lookup"><span data-stu-id="69d5e-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="69d5e-175">Mediante jQuery UI de la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="69d5e-176">La red CDN también hospeda la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="69d5e-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="69d5e-177">La biblioteca de interfaz de usuario jQuery incluye un amplio conjunto de widgets y los efectos que puede usar en las aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="69d5e-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="69d5e-178">Por ejemplo, en la página siguiente se muestra cómo puede usar jQuery Datepicker de interfaz de usuario en el contexto de una aplicación de formularios Web Forms de ASP.NET para mostrar un calendario emergente:</span><span class="sxs-lookup"><span data-stu-id="69d5e-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="69d5e-179">Al mover el foco en el cuadro de texto utilizando el teclado, se muestra un calendario:</span><span class="sxs-lookup"><span data-stu-id="69d5e-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario emergente creado con Datepicker](overview/_static/image1.png)

<span data-ttu-id="69d5e-181">Tenga en cuenta que debe incluir tres archivos de la red CDN en el código anterior:</span><span class="sxs-lookup"><span data-stu-id="69d5e-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="69d5e-182">La biblioteca de jQuery &mdash; la biblioteca de interfaz de usuario de jQuery depende de la biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="69d5e-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="69d5e-183">Debe agregar la biblioteca de jQuery a la página antes de agregar la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="69d5e-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="69d5e-184">La biblioteca de interfaz de usuario jQuery &mdash; la biblioteca de interfaz de usuario jQuery contiene todos los efectos de la interfaz de usuario de jQuery y widgets, como el widget Datepicker utilizado en la página anterior.</span><span class="sxs-lookup"><span data-stu-id="69d5e-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="69d5e-185">Un tema de la interfaz de usuario jQuery &mdash; jQuery UI admite distintos temas.</span><span class="sxs-lookup"><span data-stu-id="69d5e-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="69d5e-186">La página anterior incluye un vínculo a un archivo CSS para importar el tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="69d5e-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="69d5e-187">Todos los temas de la interfaz de usuario jQuery estándar se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="69d5e-188">[Visite esta página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en CDN de Microsoft Ajax") para ver vistas en miniatura para cada tema.</span><span class="sxs-lookup"><span data-stu-id="69d5e-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="69d5e-189">Para obtener más información sobre la biblioteca de interfaz de usuario jQuery, visite el oficial [sitio Web de interfaz de usuario de jQuery](http://jQueryUI.com "sitio Web de interfaz de usuario de jQuery").</span><span class="sxs-lookup"><span data-stu-id="69d5e-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="69d5e-190">Archivos de terceros en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="69d5e-191">La red CDN hospeda algunas de las bibliotecas de JavaScript de terceros más populares.</span><span class="sxs-lookup"><span data-stu-id="69d5e-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="69d5e-192">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="69d5e-193">Los propietarios del copyright de las bibliotecas licencias estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="69d5e-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="69d5e-194">Se conceden los derechos que tendrá que descargar y usar estas bibliotecas únicamente por los propietarios del copyright respectivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="69d5e-195">Dado que estos no son las bibliotecas de Microsoft, Microsoft no proporciona ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patentes implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="69d5e-196">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="69d5e-197">Las versiones siguientes de jQuery se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="69d5e-198">versión de jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="69d5e-199">versión de jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="69d5e-200">versión de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="69d5e-201">versión de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="69d5e-202">versión de jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="69d5e-203">versión de jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="69d5e-204">versión de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="69d5e-205">versión de jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="69d5e-206">versión de jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="69d5e-207">versión 2.2.1 del jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="69d5e-208">versión de jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="69d5e-209">versión de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="69d5e-210">versión de jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="69d5e-211">versión de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="69d5e-212">versión de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="69d5e-213">versión de jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="69d5e-214">versión de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="69d5e-215">versión de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="69d5e-216">versión de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="69d5e-217">versión de jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="69d5e-218">versión de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="69d5e-219">versión de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="69d5e-220">versión de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="69d5e-221">versión de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="69d5e-222">versión de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="69d5e-223">versión de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="69d5e-224">versión de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="69d5e-225">versión de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="69d5e-226">versión de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="69d5e-227">versión de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="69d5e-228">versión de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="69d5e-229">versión de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="69d5e-230">versión de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="69d5e-231">versión de jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="69d5e-232">versión de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="69d5e-233">versión de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="69d5e-234">versión de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="69d5e-235">versión de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="69d5e-236">versión de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="69d5e-237">versión de jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="69d5e-238">jQuery versión 1.7</span><span class="sxs-lookup"><span data-stu-id="69d5e-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="69d5e-239">versión de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="69d5e-240">versión de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="69d5e-241">versión de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="69d5e-242">versión de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="69d5e-243">jQuery versión 1.6</span><span class="sxs-lookup"><span data-stu-id="69d5e-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="69d5e-244">versión de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="69d5e-245">versión de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="69d5e-246">versión de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="69d5e-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="69d5e-247">versión de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="69d5e-248">versión de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="69d5e-249">versión de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="69d5e-250">versión de jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="69d5e-251">versión de jQuery 1.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="69d5e-252">versión de jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="69d5e-253">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="69d5e-254">Las versiones siguientes de jQuery migrar se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="69d5e-255">jQuery migrar versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="69d5e-256">jQuery migrar versión 1.2.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="69d5e-257">jQuery migrar versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="69d5e-258">jQuery migrar versión 1.1.1.</span><span class="sxs-lookup"><span data-stu-id="69d5e-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="69d5e-259">jQuery migrar versión 1.1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="69d5e-260">Migrar la versión 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="69d5e-261">Versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="69d5e-262">Las siguientes versiones de la biblioteca de interfaz de usuario de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="69d5e-263">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="69d5e-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-278">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="69d5e-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="69d5e-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="69d5e-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="69d5e-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="69d5e-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="69d5e-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="69d5e-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="69d5e-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="69d5e-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="69d5e-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="69d5e-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="69d5e-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="69d5e-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="69d5e-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="69d5e-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="69d5e-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="69d5e-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="69d5e-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="69d5e-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="69d5e-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="69d5e-299">Versiones de la validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="69d5e-300">Las siguientes versiones de la biblioteca de validación de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="69d5e-301">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="69d5e-302">jQuery validar 1.17.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery validación 1.17.0")
- [<span data-ttu-id="69d5e-303">jQuery validar 1.16.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery validación 1.16.0")
- [<span data-ttu-id="69d5e-304">jQuery validar 1.15.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery validación 1.15.1")
- [<span data-ttu-id="69d5e-305">jQuery validar 1.15.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery validación 1.15.0")
- [<span data-ttu-id="69d5e-306">jQuery validar 1.14.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery validación 1.14.0")
- [<span data-ttu-id="69d5e-307">jQuery validar 1.13.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery validación 1.13.1")
- [<span data-ttu-id="69d5e-308">jQuery validar 1.13.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery validación 1.13.0")
- [<span data-ttu-id="69d5e-309">jQuery validar 1.12.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery validación 1.12.0")
- [<span data-ttu-id="69d5e-310">jQuery validar 1.11.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery validación 1.11.1")
- [<span data-ttu-id="69d5e-311">jQuery validar 1.11.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery validación 1.11.0")
- [<span data-ttu-id="69d5e-312">jQuery validar 1.10.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery validación 1.10.0")
- [<span data-ttu-id="69d5e-313">jQuery validar 1.9</span><span class="sxs-lookup"><span data-stu-id="69d5e-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versión 1.9")
- [<span data-ttu-id="69d5e-314">jQuery validar 1.8.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versión 1.8.1")
- [<span data-ttu-id="69d5e-315">jQuery validar 1.8</span><span class="sxs-lookup"><span data-stu-id="69d5e-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versión 1.8")
- [<span data-ttu-id="69d5e-316">jQuery validar 1.7</span><span class="sxs-lookup"><span data-stu-id="69d5e-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versión 1.7")
- [<span data-ttu-id="69d5e-317">jQuery validar 1.6</span><span class="sxs-lookup"><span data-stu-id="69d5e-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validar 1.6")
- [<span data-ttu-id="69d5e-318">jQuery validar 1.5.5</span><span class="sxs-lookup"><span data-stu-id="69d5e-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validar 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="69d5e-319">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="69d5e-320">Las siguientes versiones de la biblioteca de jQuery móvil se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="69d5e-321">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="69d5e-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="69d5e-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-328">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="69d5e-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 en la CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="69d5e-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="69d5e-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 de CDN de Microsoft Ajax")
- [<span data-ttu-id="69d5e-338">versión beta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 en la CDN de Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="69d5e-339">Versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="69d5e-340">Las siguientes versiones del complemento de plantillas de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="69d5e-341">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="69d5e-342">jQuery plantillas Beta 1</span><span class="sxs-lookup"><span data-stu-id="69d5e-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery plantillas Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="69d5e-343">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="69d5e-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="69d5e-344">Las siguientes versiones del complemento de ciclo de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="69d5e-345">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="69d5e-346">jQuery ciclo 2,99</span><span class="sxs-lookup"><span data-stu-id="69d5e-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [<span data-ttu-id="69d5e-347">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="69d5e-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery ciclo 2.94")
- [<span data-ttu-id="69d5e-348">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="69d5e-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="69d5e-349">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="69d5e-350">Las siguientes versiones del complemento DataTables de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="69d5e-351">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="69d5e-352">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="69d5e-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="69d5e-353">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="69d5e-354">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="69d5e-355">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="69d5e-356">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="69d5e-357">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="69d5e-358">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="69d5e-359">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="69d5e-360">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="69d5e-361">Los siguientes versiones de [Modernizr](http://www.modernizr.com "Modernizr") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="69d5e-362">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="69d5e-363">Los siguientes versiones de [JSHint](http://www.jshint.com "JSHint") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="69d5e-364">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="69d5e-365">Los siguientes versiones de [Knockout](http://www.knockoutjs.com "Knockout") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="69d5e-366">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="69d5e-367">Los siguientes versiones de [Globalize](https://github.com/jquery/globalize "Globalize") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="69d5e-368">Globalizar la versión 1.0.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="69d5e-369">Globalizar versión 0.1.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="69d5e-370">todas las referencias culturales</span><span class="sxs-lookup"><span data-stu-id="69d5e-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="69d5e-371">Reemplace "{-código de referencia cultural}" por el código de la referencia cultural deseada, por ejemplo, Microsoft globalize.culture.en GB.js== archivos en la red CDN == estas bibliotecas cargadas por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="69d5e-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="69d5e-372">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="69d5e-373">Los siguientes versiones de [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") responden se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="69d5e-374">Responder versión 1.4.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="69d5e-375">Responder versión 1.4.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="69d5e-376">Responder versión 1.4.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="69d5e-377">Responder versión 1.3.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="69d5e-378">Responder versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="69d5e-379">Versiones de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="69d5e-380">Los siguientes versiones de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="69d5e-381">Arranque versión 4.0.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="69d5e-382">Versión de arranque 3.3.7</span><span class="sxs-lookup"><span data-stu-id="69d5e-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="69d5e-383">Versión de arranque 3.3.6</span><span class="sxs-lookup"><span data-stu-id="69d5e-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="69d5e-384">Arranque versión 3.3.5</span><span class="sxs-lookup"><span data-stu-id="69d5e-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="69d5e-385">Arranque versión 3.3.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="69d5e-386">Versión de arranque 3.3.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="69d5e-387">Versión de arranque 3.3.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="69d5e-388">Versión de arranque 3.3.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="69d5e-389">Versión de arranque 3.2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="69d5e-390">Versión de arranque 3.1.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="69d5e-391">Arranque versión 3.1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="69d5e-392">Arranque versión 3.0.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="69d5e-393">Arranque versión 3.0.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="69d5e-394">Arranque versión 3.0.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="69d5e-395">Arranque versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="69d5e-396">Arranque versión 2.3.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="69d5e-397">Arranque versión 2.3.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="69d5e-398">Versiones de TouchCarousel de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="69d5e-399">Los siguientes versiones de [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") TouchCarousel Bootstrap versiones se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="69d5e-400">Arranque TouchCarousel versión 0.8.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="69d5e-401">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="69d5e-402">Los siguientes versiones de [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js versiones se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="69d5e-403">Hammer.js versión 2.0.4</span><span class="sxs-lookup"><span data-stu-id="69d5e-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="69d5e-404">Formularios Web Forms ASP.NET y Ajax versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="69d5e-405">Las siguientes versiones de la biblioteca de Ajax de ASP.NET se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="69d5e-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="69d5e-406">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="69d5e-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="69d5e-407">Formularios Web Forms ASP.NET y Ajax versión 4.5.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularios Web Forms de ASP.NET y Ajax 4.5.2")
- [<span data-ttu-id="69d5e-408">Versión de formularios Web Forms ASP.NET y Ajax 4</span><span class="sxs-lookup"><span data-stu-id="69d5e-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularios Web Forms de ASP.NET y Ajax 4")
- [<span data-ttu-id="69d5e-409">Ajax de ASP.NET versión 3.5</span><span class="sxs-lookup"><span data-stu-id="69d5e-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax de ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="69d5e-410">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="69d5e-411">Los siguientes archivos JavaScript de ASP.NET MVC se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="69d5e-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="69d5e-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="69d5e-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="69d5e-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="69d5e-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="69d5e-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="69d5e-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="69d5e-419">ASP.NET SignalR libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="69d5e-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="69d5e-420">Los siguientes archivos de ASP.NET SignalR JavaScript se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="69d5e-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="69d5e-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="69d5e-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="69d5e-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="69d5e-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="69d5e-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="69d5e-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="69d5e-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="69d5e-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="69d5e-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="69d5e-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="69d5e-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="69d5e-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="69d5e-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="69d5e-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="69d5e-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="69d5e-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="69d5e-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="69d5e-434">Para obtener información acerca de los términos de uso de la red CDN, vea [Microsoft Ajax CDN términos de uso](https://www.asp.net/terms-of-use "Microsoft Ajax CDN términos de uso").</span><span class="sxs-lookup"><span data-stu-id="69d5e-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
