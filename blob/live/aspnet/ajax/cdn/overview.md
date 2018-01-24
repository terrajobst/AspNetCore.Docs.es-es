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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="a8012-102">Red de entrega de contenido de Microsoft Ajax</span><span class="sxs-lookup"><span data-stu-id="a8012-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="a8012-103">Nota: La CDN de Microsoft Ajax no tiene ningún SLA más allá del uso de una CDN de Azure.</span><span class="sxs-lookup"><span data-stu-id="a8012-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="a8012-104">Tabla de contenido</span><span class="sxs-lookup"><span data-stu-id="a8012-104">Table of Contents</span></span>

<span data-ttu-id="a8012-105">**[AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="a8012-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="a8012-106">**[Compatibilidad de Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="a8012-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="a8012-107">**[Uso de Ajax de ASP.NET de la red CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="a8012-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="a8012-108">**[Mediante jQuery desde la red CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="a8012-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="a8012-109">**[Mediante jQuery UI de la red CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="a8012-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="a8012-110">**[Archivos de terceros en la red CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="a8012-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="a8012-111">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="a8012-112">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="a8012-113">Versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="a8012-114">Versiones de la validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="a8012-115">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="a8012-116">Versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="a8012-117">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="a8012-118">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="a8012-119">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="a8012-120">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="a8012-121">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="a8012-122">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="a8012-123">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="a8012-124">Versiones de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="a8012-125">Versiones de TouchCarousel de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="a8012-126">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="a8012-127">Formularios Web Forms ASP.NET y Ajax versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="a8012-128">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="a8012-129">ASP.NET SignalR libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="a8012-130">La red contenido de entrega de Microsoft Ajax (CDN) hospeda bibliotecas de JavaScript de terceros populares como jQuery y le permite fácilmente agregarlas a sus aplicaciones Web.</span><span class="sxs-lookup"><span data-stu-id="a8012-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="a8012-131">Por ejemplo, puede empezar a usar jQuery que se hospeda en esta red CDN mediante la adición de un &lt;script&gt; etiqueta a la página que señala a ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="a8012-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="a8012-132">Aprovechando las ventajas de la red CDN, puede mejorar notablemente el rendimiento de las aplicaciones Ajax.</span><span class="sxs-lookup"><span data-stu-id="a8012-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="a8012-133">El contenido de la red CDN se almacenan en caché en servidores ubicados en todo el mundo.</span><span class="sxs-lookup"><span data-stu-id="a8012-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="a8012-134">Además, la red CDN permite exploradores reutilicen los archivos de JavaScript de terceros en caché para los sitios web que se encuentran en dominios diferentes.</span><span class="sxs-lookup"><span data-stu-id="a8012-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="a8012-135">La red CDN es compatible con SSL (HTTPS), en caso de que necesite servir una página web con la capa de Sockets seguros.</span><span class="sxs-lookup"><span data-stu-id="a8012-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="a8012-136">La red CDN hospeda las siguientes bibliotecas de scripts de terceros que se han cargado y que están autorizadas, los propietarios de dichas bibliotecas:</span><span class="sxs-lookup"><span data-stu-id="a8012-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="a8012-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="a8012-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="a8012-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="a8012-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="a8012-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="a8012-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="a8012-140">jQuery validación (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="a8012-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="a8012-141">jQuery ciclo (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="a8012-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="a8012-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="a8012-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="a8012-143">La CDN de Microsoft Ajax también incluye las siguientes bibliotecas que se han cargado por Microsoft:</span><span class="sxs-lookup"><span data-stu-id="a8012-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="a8012-144">Ajax de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="a8012-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="a8012-145">Archivos de JavaScript de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="a8012-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="a8012-146">Archivos ASP.NET SignalR JavaScript</span><span class="sxs-lookup"><span data-stu-id="a8012-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="a8012-147">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="a8012-148">Los propietarios del copyright de las bibliotecas licencias estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="a8012-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="a8012-149">Se conceden los derechos que tendrá que descargar y usar estas bibliotecas únicamente por los propietarios del copyright respectivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="a8012-150">Dado que estos no son las bibliotecas de Microsoft, Microsoft no proporciona ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patentes implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="a8012-151">Si desea volver a enviar la biblioteca de JavaScript y la biblioteca es una de las bibliotecas de JavaScript (como se muestra en http://trends.builtwith.com) o extensiones y complementos a estas bibliotecas que se superior (a) populares; o (b) útil para usar en ASP.NET, a continuación, póngase en contacto con AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="a8012-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="a8012-152">AJAX.Microsoft.com ha cambiado a ajax.aspnetcdn.com</span><span class="sxs-lookup"><span data-stu-id="a8012-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="a8012-153">La red CDN permite usar el nombre de dominio microsoft.com y se ha cambiado para usar el nombre de dominio aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="a8012-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="a8012-154">Este cambio se realizó para aumentar el rendimiento porque cuando un explorador al que hace referencia el dominio microsoft.com podría enviar ninguna cookie de ese dominio a través de la conexión con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8012-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="a8012-155">Al cambiar el nombre a un nombre de dominio que no sea de microsoft.com puede aumentar rendimiento tanta a 25%.</span><span class="sxs-lookup"><span data-stu-id="a8012-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="a8012-156">Tenga en cuenta ajax.microsoft.com continuarán funcionando pero se recomienda ajax.aspnetcdn.com.</span><span class="sxs-lookup"><span data-stu-id="a8012-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="a8012-157">Formato antiguo: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="a8012-158">Nuevo formato: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="a8012-159">Compatibilidad de Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="a8012-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="a8012-160">Para utilizar correctamente los archivos de .vsdoc con Visual Studio 2008 debe asegurarse de que dispone de VS 2008 SP1 instalado, y la revisión para archivos de vsdoc instalada.</span><span class="sxs-lookup"><span data-stu-id="a8012-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="a8012-161">Puede obtener desde aquí:</span><span class="sxs-lookup"><span data-stu-id="a8012-161">You can get these from here:</span></span>

- [<span data-ttu-id="a8012-162">Descargar Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="a8012-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "descargar Visual Studio 2008 SP1")
- [<span data-ttu-id="a8012-163">Descargar la revisión de .vsdoc para Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="a8012-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 "descargar la revisión de .vsdoc para Visual Studio 2008 SP1")

<span data-ttu-id="a8012-164">Visual Studio 2010 es compatible con archivos de .vsdoc sin las revisiones adicionales.</span><span class="sxs-lookup"><span data-stu-id="a8012-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="a8012-165">Uso de Ajax de ASP.NET de la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="a8012-166">Cuando se usa ASP.NET 4, puede redirigir todas las solicitudes de scripts del marco ASP.NET a la red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="a8012-167">Recuperar las secuencias de comandos de la red CDN en lugar de su servidor web local esencialmente puede mejorar el rendimiento de sitios Web ASP.NET públicos.</span><span class="sxs-lookup"><span data-stu-id="a8012-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="a8012-168">Utilice la propiedad ScriptManager EnableCDN para redirigir todas las solicitudes de secuencia de comandos de marco de trabajo ASP.NET a la CDN de Ajax de Microsoft:</span><span class="sxs-lookup"><span data-stu-id="a8012-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="a8012-169">Mediante jQuery desde la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="a8012-170">Puede usar scripts de jQuery hospedados en la red CDN en su aplicación Web agregando el siguiente elemento de secuencia de comandos a una página:</span><span class="sxs-lookup"><span data-stu-id="a8012-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="a8012-171">La red CDN también incluye la versión reducida de la secuencia de comandos de jQuery, que puede obtener mediante el siguiente elemento:</span><span class="sxs-lookup"><span data-stu-id="a8012-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="a8012-172">Para permitir que la página de retroceder a la carga de jQuery desde una ruta local en su propio sitio Web si se produce la CDN no esté disponible, agregue el siguiente elemento inmediatamente después del elemento que hacen referencia a la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="a8012-173">La página de ejemplo siguiente usa la versión de la red CDN de la biblioteca de jQuery (con reserva de una copia local) para mostrar el contenido de un elemento div cuando se hace clic en un botón.</span><span class="sxs-lookup"><span data-stu-id="a8012-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="a8012-174">Puede obtener más información acerca de jQuery y descargar una copia local de jQuery visitando la [jQuery](http://jquery.com/) sitio Web.</span><span class="sxs-lookup"><span data-stu-id="a8012-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="a8012-175">Mediante jQuery UI de la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="a8012-176">La red CDN también hospeda la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="a8012-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="a8012-177">La biblioteca de interfaz de usuario jQuery incluye un amplio conjunto de widgets y los efectos que puede usar en las aplicaciones ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="a8012-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="a8012-178">Por ejemplo, en la página siguiente se muestra cómo puede usar jQuery Datepicker de interfaz de usuario en el contexto de una aplicación de formularios Web Forms de ASP.NET para mostrar un calendario emergente:</span><span class="sxs-lookup"><span data-stu-id="a8012-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="a8012-179">Al mover el foco en el cuadro de texto utilizando el teclado, se muestra un calendario:</span><span class="sxs-lookup"><span data-stu-id="a8012-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Calendario emergente creado con Datepicker](overview/_static/image1.png)

<span data-ttu-id="a8012-181">Tenga en cuenta que debe incluir tres archivos de la red CDN en el código anterior:</span><span class="sxs-lookup"><span data-stu-id="a8012-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="a8012-182">La biblioteca de jQuery &mdash; la biblioteca de interfaz de usuario de jQuery depende de la biblioteca de jQuery.</span><span class="sxs-lookup"><span data-stu-id="a8012-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="a8012-183">Debe agregar la biblioteca de jQuery a la página antes de agregar la biblioteca de interfaz de usuario de jQuery.</span><span class="sxs-lookup"><span data-stu-id="a8012-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="a8012-184">La biblioteca de interfaz de usuario jQuery &mdash; la biblioteca de interfaz de usuario jQuery contiene todos los efectos de la interfaz de usuario de jQuery y widgets, como el widget Datepicker utilizado en la página anterior.</span><span class="sxs-lookup"><span data-stu-id="a8012-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="a8012-185">Un tema de la interfaz de usuario jQuery &mdash; jQuery UI admite distintos temas.</span><span class="sxs-lookup"><span data-stu-id="a8012-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="a8012-186">La página anterior incluye un vínculo a un archivo CSS para importar el tema de Redmond.</span><span class="sxs-lookup"><span data-stu-id="a8012-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="a8012-187">Todos los temas de la interfaz de usuario jQuery estándar se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="a8012-188">[Visite esta página](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en CDN de Microsoft Ajax") para ver vistas en miniatura para cada tema.</span><span class="sxs-lookup"><span data-stu-id="a8012-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="a8012-189">Para obtener más información sobre la biblioteca de interfaz de usuario jQuery, visite el oficial [sitio Web de interfaz de usuario de jQuery](http://jQueryUI.com "sitio Web de interfaz de usuario de jQuery").</span><span class="sxs-lookup"><span data-stu-id="a8012-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="a8012-190">Archivos de terceros en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="a8012-191">La red CDN hospeda algunas de las bibliotecas de JavaScript de terceros más populares.</span><span class="sxs-lookup"><span data-stu-id="a8012-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="a8012-192">Microsoft no reclama la propiedad de las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="a8012-193">Los propietarios del copyright de las bibliotecas licencias estas bibliotecas para usted.</span><span class="sxs-lookup"><span data-stu-id="a8012-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="a8012-194">Se conceden los derechos que tendrá que descargar y usar estas bibliotecas únicamente por los propietarios del copyright respectivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="a8012-195">Dado que estos no son las bibliotecas de Microsoft, Microsoft no proporciona ninguna garantía ni licencias de derechos de propiedad intelectual (no incluido derechos de patentes implícitas) para las bibliotecas de terceros hospedadas en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="a8012-196">Versiones de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="a8012-197">Las versiones siguientes de jQuery se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="a8012-198">versión de jQuery 3.3.1</span><span class="sxs-lookup"><span data-stu-id="a8012-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="a8012-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="a8012-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="a8012-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="a8012-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="a8012-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="a8012-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="a8012-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="a8012-205">versión de jQuery 3.2.1</span><span class="sxs-lookup"><span data-stu-id="a8012-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="a8012-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="a8012-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="a8012-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="a8012-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="a8012-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="a8012-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="a8012-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="a8012-212">versión de jQuery 3.2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="a8012-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="a8012-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="a8012-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="a8012-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="a8012-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="a8012-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="a8012-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="a8012-219">versión de jQuery 3.1.1</span><span class="sxs-lookup"><span data-stu-id="a8012-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="a8012-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="a8012-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="a8012-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="a8012-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="a8012-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="a8012-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="a8012-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="a8012-226">versión de jQuery 3.1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="a8012-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="a8012-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="a8012-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="a8012-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="a8012-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="a8012-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="a8012-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="a8012-233">versión de jQuery 3.0.0</span><span class="sxs-lookup"><span data-stu-id="a8012-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="a8012-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="a8012-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="a8012-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="a8012-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="a8012-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="a8012-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="a8012-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="a8012-240">versión de jQuery 2.2.4</span><span class="sxs-lookup"><span data-stu-id="a8012-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="a8012-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="a8012-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="a8012-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="a8012-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="a8012-244">versión de jQuery 2.2.3</span><span class="sxs-lookup"><span data-stu-id="a8012-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="a8012-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="a8012-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="a8012-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="a8012-248">versión de jQuery 2.2.2</span><span class="sxs-lookup"><span data-stu-id="a8012-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="a8012-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="a8012-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="a8012-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="a8012-252">versión 2.2.1 del jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="a8012-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="a8012-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="a8012-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="a8012-256">versión de jQuery 2.2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="a8012-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="a8012-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="a8012-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="a8012-260">versión de jQuery 2.1.4</span><span class="sxs-lookup"><span data-stu-id="a8012-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="a8012-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="a8012-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="a8012-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="a8012-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="a8012-264">versión de jQuery 2.1.3</span><span class="sxs-lookup"><span data-stu-id="a8012-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="a8012-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="a8012-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="a8012-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="a8012-268">versión de jQuery 2.1.2</span><span class="sxs-lookup"><span data-stu-id="a8012-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="a8012-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="a8012-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="a8012-271">versión de jQuery 2.1.1</span><span class="sxs-lookup"><span data-stu-id="a8012-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="a8012-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="a8012-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="a8012-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="a8012-275">versión de jQuery 2.1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="a8012-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="a8012-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="a8012-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="a8012-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="a8012-280">versión de jQuery 2.0.3</span><span class="sxs-lookup"><span data-stu-id="a8012-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="a8012-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="a8012-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="a8012-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="a8012-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="a8012-285">versión de jQuery 2.0.2</span><span class="sxs-lookup"><span data-stu-id="a8012-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="a8012-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="a8012-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="a8012-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="a8012-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="a8012-290">versión de jQuery 2.0.1</span><span class="sxs-lookup"><span data-stu-id="a8012-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="a8012-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="a8012-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="a8012-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="a8012-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="a8012-295">versión de jQuery 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a8012-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="a8012-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="a8012-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="a8012-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="a8012-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="a8012-300">versión de jQuery 1.12.4</span><span class="sxs-lookup"><span data-stu-id="a8012-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="a8012-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="a8012-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="a8012-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="a8012-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="a8012-304">versión de jQuery 1.12.3</span><span class="sxs-lookup"><span data-stu-id="a8012-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="a8012-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="a8012-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="a8012-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="a8012-308">versión de jQuery 1.12.2</span><span class="sxs-lookup"><span data-stu-id="a8012-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="a8012-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="a8012-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="a8012-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="a8012-312">versión de jQuery 1.12.1</span><span class="sxs-lookup"><span data-stu-id="a8012-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="a8012-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="a8012-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="a8012-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="a8012-316">versión de jQuery 1.12.0</span><span class="sxs-lookup"><span data-stu-id="a8012-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="a8012-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="a8012-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="a8012-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="a8012-320">versión de jQuery 1.11.3</span><span class="sxs-lookup"><span data-stu-id="a8012-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="a8012-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="a8012-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="a8012-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="a8012-324">versión de jQuery 1.11.2</span><span class="sxs-lookup"><span data-stu-id="a8012-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="a8012-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="a8012-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="a8012-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="a8012-328">versión de jQuery 1.11.1</span><span class="sxs-lookup"><span data-stu-id="a8012-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="a8012-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="a8012-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="a8012-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="a8012-332">versión de jQuery 1.11.0</span><span class="sxs-lookup"><span data-stu-id="a8012-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="a8012-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="a8012-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="a8012-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="a8012-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="a8012-337">versión de jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="a8012-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="a8012-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="a8012-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="a8012-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="a8012-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="a8012-342">versión de jQuery 1.10.1</span><span class="sxs-lookup"><span data-stu-id="a8012-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="a8012-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="a8012-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="a8012-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="a8012-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="a8012-347">versión de jQuery 1.10.0</span><span class="sxs-lookup"><span data-stu-id="a8012-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="a8012-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="a8012-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="a8012-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="a8012-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="a8012-352">versión de jQuery 1.9.1</span><span class="sxs-lookup"><span data-stu-id="a8012-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="a8012-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="a8012-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="a8012-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="a8012-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="a8012-357">versión de jQuery 1.9.0</span><span class="sxs-lookup"><span data-stu-id="a8012-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="a8012-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="a8012-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="a8012-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="a8012-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="a8012-362">versión de jQuery 1.8.3</span><span class="sxs-lookup"><span data-stu-id="a8012-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="a8012-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="a8012-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="a8012-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="a8012-366">versión de jQuery 1.8.2</span><span class="sxs-lookup"><span data-stu-id="a8012-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="a8012-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="a8012-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="a8012-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="a8012-370">versión de jQuery 1.8.1</span><span class="sxs-lookup"><span data-stu-id="a8012-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="a8012-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="a8012-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="a8012-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="a8012-374">versión de jQuery 1.8.0</span><span class="sxs-lookup"><span data-stu-id="a8012-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="a8012-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="a8012-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="a8012-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="a8012-378">versión de jQuery 1.7.2</span><span class="sxs-lookup"><span data-stu-id="a8012-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="a8012-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="a8012-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="a8012-381">versión de jQuery 1.7.1</span><span class="sxs-lookup"><span data-stu-id="a8012-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="a8012-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="a8012-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="a8012-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="a8012-385">jQuery versión 1.7</span><span class="sxs-lookup"><span data-stu-id="a8012-385">jQuery version 1.7</span></span>

- <span data-ttu-id="a8012-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="a8012-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="a8012-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="a8012-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="a8012-389">versión de jQuery 1.6.4</span><span class="sxs-lookup"><span data-stu-id="a8012-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="a8012-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="a8012-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="a8012-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="a8012-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="a8012-393">versión de jQuery 1.6.3</span><span class="sxs-lookup"><span data-stu-id="a8012-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="a8012-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="a8012-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="a8012-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="a8012-397">versión de jQuery 1.6.2</span><span class="sxs-lookup"><span data-stu-id="a8012-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="a8012-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="a8012-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="a8012-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="a8012-401">versión de jQuery 1.6.1</span><span class="sxs-lookup"><span data-stu-id="a8012-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="a8012-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="a8012-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="a8012-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="a8012-405">jQuery versión 1.6</span><span class="sxs-lookup"><span data-stu-id="a8012-405">jQuery version 1.6</span></span>

- <span data-ttu-id="a8012-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="a8012-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="a8012-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="a8012-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="a8012-409">versión de jQuery 1.5.2</span><span class="sxs-lookup"><span data-stu-id="a8012-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="a8012-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="a8012-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="a8012-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="a8012-413">versión de jQuery 1.5.1</span><span class="sxs-lookup"><span data-stu-id="a8012-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="a8012-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="a8012-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="a8012-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="a8012-417">versión de jQuery 1.5</span><span class="sxs-lookup"><span data-stu-id="a8012-417">jQuery version 1.5</span></span>

- <span data-ttu-id="a8012-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="a8012-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="a8012-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="a8012-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="a8012-421">versión de jQuery 1.4.4</span><span class="sxs-lookup"><span data-stu-id="a8012-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="a8012-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="a8012-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="a8012-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="a8012-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="a8012-425">versión de jQuery 1.4.3</span><span class="sxs-lookup"><span data-stu-id="a8012-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="a8012-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="a8012-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="a8012-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="a8012-429">versión de jQuery 1.4.2</span><span class="sxs-lookup"><span data-stu-id="a8012-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="a8012-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="a8012-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="a8012-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="a8012-433">versión de jQuery 1.4.1</span><span class="sxs-lookup"><span data-stu-id="a8012-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="a8012-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="a8012-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="a8012-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="a8012-437">versión de jQuery 1.4</span><span class="sxs-lookup"><span data-stu-id="a8012-437">jQuery version 1.4</span></span>

- <span data-ttu-id="a8012-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="a8012-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="a8012-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="a8012-440">versión de jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="a8012-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="a8012-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="a8012-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="a8012-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="a8012-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="a8012-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="a8012-445">Versiones de migrar de jQuery en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="a8012-446">Las versiones siguientes de jQuery migrar se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="a8012-447">jQuery migrar versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="a8012-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="a8012-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="a8012-449">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="a8012-450">jQuery migrar versión 1.2.1</span><span class="sxs-lookup"><span data-stu-id="a8012-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="a8012-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="a8012-452">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="a8012-453">jQuery migrar versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="a8012-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="a8012-455">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="a8012-456">jQuery migrar versión 1.1.1.</span><span class="sxs-lookup"><span data-stu-id="a8012-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="a8012-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="a8012-458">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="a8012-459">jQuery migrar versión 1.1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="a8012-460">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="a8012-461">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="a8012-462">Migrar la versión 1.0.0 de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="a8012-463">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="a8012-464">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="a8012-465">Versiones de la interfaz de usuario en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="a8012-466">Las siguientes versiones de la biblioteca de interfaz de usuario de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="a8012-467">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a8012-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="a8012-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="a8012-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="a8012-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="a8012-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="a8012-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="a8012-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="a8012-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="a8012-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="a8012-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="a8012-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery UI 1.10.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="a8012-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="a8012-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="a8012-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="a8012-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-482">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="a8012-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="a8012-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="a8012-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="a8012-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="a8012-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="a8012-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="a8012-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="a8012-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="a8012-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="a8012-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="a8012-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="a8012-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="a8012-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="a8012-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="a8012-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="a8012-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="a8012-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="a8012-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="a8012-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="a8012-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="a8012-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="a8012-503">Versiones de la validación en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="a8012-504">Las siguientes versiones de la biblioteca de validación de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="a8012-505">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a8012-506">jQuery validar 1.17.0</span><span class="sxs-lookup"><span data-stu-id="a8012-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery validación 1.17.0")
- [<span data-ttu-id="a8012-507">jQuery validar 1.16.0</span><span class="sxs-lookup"><span data-stu-id="a8012-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery validación 1.16.0")
- [<span data-ttu-id="a8012-508">jQuery validar 1.15.1</span><span class="sxs-lookup"><span data-stu-id="a8012-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery validación 1.15.1")
- [<span data-ttu-id="a8012-509">jQuery validar 1.15.0</span><span class="sxs-lookup"><span data-stu-id="a8012-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery validación 1.15.0")
- [<span data-ttu-id="a8012-510">jQuery validar 1.14.0</span><span class="sxs-lookup"><span data-stu-id="a8012-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery validación 1.14.0")
- [<span data-ttu-id="a8012-511">jQuery validar 1.13.1</span><span class="sxs-lookup"><span data-stu-id="a8012-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery validación 1.13.1")
- [<span data-ttu-id="a8012-512">jQuery validar 1.13.0</span><span class="sxs-lookup"><span data-stu-id="a8012-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery validación 1.13.0")
- [<span data-ttu-id="a8012-513">jQuery validar 1.12.0</span><span class="sxs-lookup"><span data-stu-id="a8012-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery validación 1.12.0")
- [<span data-ttu-id="a8012-514">jQuery validar 1.11.1</span><span class="sxs-lookup"><span data-stu-id="a8012-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery validación 1.11.1")
- [<span data-ttu-id="a8012-515">jQuery validar 1.11.0</span><span class="sxs-lookup"><span data-stu-id="a8012-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery validación 1.11.0")
- [<span data-ttu-id="a8012-516">jQuery validar 1.10.0</span><span class="sxs-lookup"><span data-stu-id="a8012-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery validación 1.10.0")
- [<span data-ttu-id="a8012-517">jQuery validar 1.9</span><span class="sxs-lookup"><span data-stu-id="a8012-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate versión 1.9")
- [<span data-ttu-id="a8012-518">jQuery validar 1.8.1</span><span class="sxs-lookup"><span data-stu-id="a8012-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate versión 1.8.1")
- [<span data-ttu-id="a8012-519">jQuery validar 1.8</span><span class="sxs-lookup"><span data-stu-id="a8012-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate versión 1.8")
- [<span data-ttu-id="a8012-520">jQuery validar 1.7</span><span class="sxs-lookup"><span data-stu-id="a8012-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate versión 1.7")
- [<span data-ttu-id="a8012-521">jQuery validar 1.6</span><span class="sxs-lookup"><span data-stu-id="a8012-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery validar 1.6")
- [<span data-ttu-id="a8012-522">jQuery validar 1.5.5</span><span class="sxs-lookup"><span data-stu-id="a8012-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery validar 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="a8012-523">jQuery Mobile versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="a8012-524">Las siguientes versiones de la biblioteca de jQuery móvil se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="a8012-525">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a8012-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="a8012-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="a8012-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="a8012-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="a8012-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="a8012-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="a8012-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="a8012-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="a8012-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="a8012-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="a8012-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="a8012-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 en la CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="a8012-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 en CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="a8012-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 de CDN de Microsoft Ajax")
- [<span data-ttu-id="a8012-542">versión beta 3 de jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 en la CDN de Microsoft Ajax")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="a8012-543">Versiones de plantillas en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="a8012-544">Las siguientes versiones del complemento de plantillas de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="a8012-545">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a8012-546">jQuery plantillas Beta 1</span><span class="sxs-lookup"><span data-stu-id="a8012-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery plantillas Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="a8012-547">Ciclo de versiones en la red CDN de jQuery</span><span class="sxs-lookup"><span data-stu-id="a8012-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="a8012-548">Las siguientes versiones del complemento de ciclo de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="a8012-549">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a8012-550">jQuery ciclo 2,99</span><span class="sxs-lookup"><span data-stu-id="a8012-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery 2,99 ciclo")
- [<span data-ttu-id="a8012-551">jQuery ciclo 2.94</span><span class="sxs-lookup"><span data-stu-id="a8012-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery ciclo 2.94")
- [<span data-ttu-id="a8012-552">jQuery ciclo 2,88</span><span class="sxs-lookup"><span data-stu-id="a8012-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery 2,88 ciclo")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="a8012-553">jQuery DataTables versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="a8012-554">Las siguientes versiones del complemento DataTables de jQuery se hospedan en esta red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="a8012-555">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a8012-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="a8012-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="a8012-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="a8012-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="a8012-558">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="a8012-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="a8012-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="a8012-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="a8012-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="a8012-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="a8012-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="a8012-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="a8012-562">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="a8012-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="a8012-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="a8012-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="a8012-564">Versiones de Modernizr en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="a8012-565">Los siguientes versiones de [Modernizr](http://www.modernizr.com "Modernizr") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="a8012-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="a8012-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="a8012-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="a8012-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="a8012-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="a8012-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="a8012-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="a8012-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="a8012-572">Versiones de JSHint en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="a8012-573">Los siguientes versiones de [JSHint](http://www.jshint.com "JSHint") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="a8012-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="a8012-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="a8012-575">Versiones de cobertura en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="a8012-576">Los siguientes versiones de [Knockout](http://www.knockoutjs.com "Knockout") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="a8012-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="a8012-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="a8012-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="a8012-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="a8012-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="a8012-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="a8012-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="a8012-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="a8012-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="a8012-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="a8012-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="a8012-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="a8012-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="a8012-590">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="a8012-591">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="a8012-592">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="a8012-593">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="a8012-594">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="a8012-595">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="a8012-596">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="a8012-597">Globalizar versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="a8012-598">Los siguientes versiones de [Globalize](https://github.com/jquery/globalize "Globalize") se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="a8012-599">Globalizar la versión 1.0.0</span><span class="sxs-lookup"><span data-stu-id="a8012-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="a8012-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="a8012-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="a8012-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js</span><span class="sxs-lookup"><span data-stu-id="a8012-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="a8012-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="a8012-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="a8012-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="a8012-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="a8012-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="a8012-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="a8012-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="a8012-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="a8012-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="a8012-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="a8012-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-time.js</span><span class="sxs-lookup"><span data-stu-id="a8012-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="a8012-608">Globalizar versión 0.1.1</span><span class="sxs-lookup"><span data-stu-id="a8012-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="a8012-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="a8012-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="a8012-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="a8012-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Cultures.js</span><span class="sxs-lookup"><span data-stu-id="a8012-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="a8012-612">todas las referencias culturales</span><span class="sxs-lookup"><span data-stu-id="a8012-612">all cultures</span></span>
- <span data-ttu-id="a8012-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Culture. {código de referencia cultural} .js</span><span class="sxs-lookup"><span data-stu-id="a8012-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="a8012-614">Reemplace "{-código de referencia cultural}" por el código de la referencia cultural deseada, por ejemplo, Microsoft globalize.culture.en GB.js== archivos en la red CDN == estas bibliotecas cargadas por Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a8012-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="a8012-615">Responder a las versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="a8012-616">Los siguientes versiones de [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") responden se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="a8012-617">Responder versión 1.4.2</span><span class="sxs-lookup"><span data-stu-id="a8012-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="a8012-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="a8012-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="a8012-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="a8012-620">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="a8012-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="a8012-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="a8012-622">Responder versión 1.4.1</span><span class="sxs-lookup"><span data-stu-id="a8012-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="a8012-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="a8012-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="a8012-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="a8012-625">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="a8012-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="a8012-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="a8012-627">Responder versión 1.4.0</span><span class="sxs-lookup"><span data-stu-id="a8012-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="a8012-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="a8012-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="a8012-629">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="a8012-630">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="a8012-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="a8012-631">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="a8012-632">Responder versión 1.3.0</span><span class="sxs-lookup"><span data-stu-id="a8012-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="a8012-633">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="a8012-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="a8012-634">Responder versión 1.2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="a8012-635">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="a8012-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="a8012-636">Versiones de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="a8012-637">Los siguientes versiones de [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="a8012-638">Versión de arranque 3.3.7</span><span class="sxs-lookup"><span data-stu-id="a8012-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="a8012-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="a8012-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-645">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a8012-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a8012-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="a8012-652">Versión de arranque 3.3.6</span><span class="sxs-lookup"><span data-stu-id="a8012-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="a8012-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="a8012-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-659">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a8012-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a8012-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="a8012-666">Arranque versión 3.3.5</span><span class="sxs-lookup"><span data-stu-id="a8012-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="a8012-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="a8012-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-673">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a8012-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a8012-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="a8012-680">Arranque versión 3.3.4</span><span class="sxs-lookup"><span data-stu-id="a8012-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="a8012-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="a8012-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-687">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a8012-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a8012-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="a8012-694">Versión de arranque 3.3.2</span><span class="sxs-lookup"><span data-stu-id="a8012-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="a8012-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="a8012-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-701">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="a8012-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-regular.woff2</span><span class="sxs-lookup"><span data-stu-id="a8012-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="a8012-708">Versión de arranque 3.3.1</span><span class="sxs-lookup"><span data-stu-id="a8012-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="a8012-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="a8012-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-714">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="a8012-721">Versión de arranque 3.3.0</span><span class="sxs-lookup"><span data-stu-id="a8012-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="a8012-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="a8012-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-727">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="a8012-734">Versión de arranque 3.2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="a8012-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="a8012-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-740">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="a8012-747">Versión de arranque 3.1.1</span><span class="sxs-lookup"><span data-stu-id="a8012-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="a8012-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="a8012-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-753">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="a8012-760">Arranque versión 3.1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="a8012-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="a8012-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="a8012-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-766">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="a8012-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="a8012-773">Arranque versión 3.0.3</span><span class="sxs-lookup"><span data-stu-id="a8012-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="a8012-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="a8012-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-777">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="a8012-784">Arranque versión 3.0.2</span><span class="sxs-lookup"><span data-stu-id="a8012-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="a8012-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="a8012-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-788">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="a8012-795">Arranque versión 3.0.1</span><span class="sxs-lookup"><span data-stu-id="a8012-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="a8012-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="a8012-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-799">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="a8012-806">Arranque versión 3.0.0</span><span class="sxs-lookup"><span data-stu-id="a8012-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="a8012-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="a8012-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-810">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="a8012-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="a8012-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.eot</span><span class="sxs-lookup"><span data-stu-id="a8012-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="a8012-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.SVG</span><span class="sxs-lookup"><span data-stu-id="a8012-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="a8012-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.ttf</span><span class="sxs-lookup"><span data-stu-id="a8012-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="a8012-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-regular.woff</span><span class="sxs-lookup"><span data-stu-id="a8012-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="a8012-817">Arranque versión 2.3.2</span><span class="sxs-lookup"><span data-stu-id="a8012-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="a8012-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="a8012-819">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="a8012-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="a8012-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/IMG/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="a8012-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="a8012-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/IMG/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="a8012-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="a8012-826">Arranque versión 2.3.1</span><span class="sxs-lookup"><span data-stu-id="a8012-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="a8012-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="a8012-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="a8012-828">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/bootstrap.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="a8012-829">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="a8012-830">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/bootstrap.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="a8012-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="a8012-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.min.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="a8012-833">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/IMG/glyphicons-halflings.png</span><span class="sxs-lookup"><span data-stu-id="a8012-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="a8012-834">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/IMG/glyphicons-halflings-White.png</span><span class="sxs-lookup"><span data-stu-id="a8012-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="a8012-835">Versiones de TouchCarousel de arranque en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="a8012-836">Los siguientes versiones de [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") TouchCarousel Bootstrap versiones se hospedan en la red CDN :</span><span class="sxs-lookup"><span data-stu-id="a8012-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="a8012-837">Arranque TouchCarousel versión 0.8.0</span><span class="sxs-lookup"><span data-stu-id="a8012-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="a8012-838">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/CSS/Bootstrap-Touch-carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="a8012-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="a8012-839">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/js/Bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="a8012-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="a8012-840">Versiones de hammer.js en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="a8012-841">Los siguientes versiones de [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js versiones se hospedan en la red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="a8012-842">Hammer.js versión 2.0.4</span><span class="sxs-lookup"><span data-stu-id="a8012-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="a8012-843">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="a8012-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="a8012-844">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="a8012-845">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.min.Map</span><span class="sxs-lookup"><span data-stu-id="a8012-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="a8012-846">Formularios Web Forms ASP.NET y Ajax versiones en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="a8012-847">Las siguientes versiones de la biblioteca de Ajax de ASP.NET se hospedan en la red CDN.</span><span class="sxs-lookup"><span data-stu-id="a8012-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="a8012-848">Haga clic en cada vínculo para ver la lista real de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8012-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="a8012-849">Formularios Web Forms ASP.NET y Ajax versión 4.5.2</span><span class="sxs-lookup"><span data-stu-id="a8012-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "formularios Web Forms de ASP.NET y Ajax 4.5.2")
- [<span data-ttu-id="a8012-850">Versión de formularios Web Forms ASP.NET y Ajax 4</span><span class="sxs-lookup"><span data-stu-id="a8012-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "formularios Web Forms de ASP.NET y Ajax 4")
- [<span data-ttu-id="a8012-851">Ajax de ASP.NET versión 3.5</span><span class="sxs-lookup"><span data-stu-id="a8012-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "Ajax de ASP.NET 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="a8012-852">ASP.NET MVC se libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="a8012-853">Los siguientes archivos JavaScript de ASP.NET MVC se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="a8012-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="a8012-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="a8012-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a8012-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a8012-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="a8012-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="a8012-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="a8012-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a8012-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a8012-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="a8012-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="a8012-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="a8012-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a8012-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a8012-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="a8012-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="a8012-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="a8012-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a8012-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a8012-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="a8012-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="a8012-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="a8012-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="a8012-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="a8012-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="a8012-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="a8012-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="a8012-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="a8012-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="a8012-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="a8012-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="a8012-873">MVC DE ASP.NET 2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="a8012-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="a8012-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="a8012-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="a8012-876">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="a8012-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="a8012-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="a8012-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="a8012-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="a8012-879">ASP.NET SignalR libera en la red CDN</span><span class="sxs-lookup"><span data-stu-id="a8012-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="a8012-880">Los siguientes archivos de ASP.NET SignalR JavaScript se hospedan en esta red CDN:</span><span class="sxs-lookup"><span data-stu-id="a8012-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="a8012-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="a8012-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="a8012-882">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="a8012-883">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="a8012-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="a8012-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="a8012-885">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="a8012-886">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="a8012-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="a8012-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="a8012-888">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="a8012-889">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="a8012-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="a8012-891">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="a8012-892">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="a8012-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="a8012-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="a8012-894">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="a8012-895">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="a8012-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="a8012-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="a8012-897">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="a8012-898">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="a8012-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="a8012-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="a8012-900">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="a8012-901">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="a8012-902">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a8012-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="a8012-903">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="a8012-904">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="a8012-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="a8012-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="a8012-906">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="a8012-907">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="a8012-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="a8012-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="a8012-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="a8012-909">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="a8012-910">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="a8012-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="a8012-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="a8012-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="a8012-912">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="a8012-913">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="a8012-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="a8012-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="a8012-915">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="a8012-916">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="a8012-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="a8012-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="a8012-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="a8012-918">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.min.js</span><span class="sxs-lookup"><span data-stu-id="a8012-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="a8012-919">http://AJAX.aspnetcdn.com/AJAX/signalr/jQuery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="a8012-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="a8012-920">Para obtener información acerca de los términos de uso de la red CDN, vea [Microsoft Ajax CDN términos de uso](https://www.asp.net/terms-of-use "Microsoft Ajax CDN términos de uso").</span><span class="sxs-lookup"><span data-stu-id="a8012-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
