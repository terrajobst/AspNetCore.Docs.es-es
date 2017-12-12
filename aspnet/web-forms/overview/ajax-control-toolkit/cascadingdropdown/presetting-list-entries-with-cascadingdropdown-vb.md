---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: "Preconfiguración de entradas de la lista con CascadingDropDown (VB) | Documentos de Microsoft"
author: wenz
description: El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores de anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: c28c7893c39d9ba9f828c34da7ffdce525ee248e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a><span data-ttu-id="30c95-103">Entradas de lista de preconfiguración CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="30c95-103">Presetting List Entries with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="30c95-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="30c95-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="30c95-105">[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="30c95-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)</span></span>

> <span data-ttu-id="30c95-106">El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="30c95-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="30c95-107">Con un poco de código es posible que un elemento de lista es el valor una vez que los datos se han cargado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="30c95-107">With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>


## <a name="overview"></a><span data-ttu-id="30c95-108">Información general</span><span class="sxs-lookup"><span data-stu-id="30c95-108">Overview</span></span>

<span data-ttu-id="30c95-109">El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="30c95-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="30c95-110">(Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) Con un poco de código es posible que un elemento de lista es el valor una vez que los datos se han cargado dinámicamente.</span><span class="sxs-lookup"><span data-stu-id="30c95-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) With a little bit of code it is possible that a list element is preselected once the data has been dynamically loaded.</span></span>

## <a name="steps"></a><span data-ttu-id="30c95-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="30c95-111">Steps</span></span>

<span data-ttu-id="30c95-112">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="30c95-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="30c95-113">A continuación, se requiere un control de DropDownList:</span><span class="sxs-lookup"><span data-stu-id="30c95-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="30c95-114">Para esta lista, se agrega un extensor CascadingDropDown, proporcionar información de dirección URL y el método de servicio web:</span><span class="sxs-lookup"><span data-stu-id="30c95-114">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="30c95-115">El extensor CascadingDropDown, a continuación, llama de forma asincrónica un servicio web con la siguiente firma de método:</span><span class="sxs-lookup"><span data-stu-id="30c95-115">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="30c95-116">El método devuelve una matriz de tipo de valor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="30c95-116">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="30c95-117">El constructor del tipo espera primero título de la entrada de lista y, a continuación, el valor (HTML `value` atributo).</span><span class="sxs-lookup"><span data-stu-id="30c95-117">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span> <span data-ttu-id="30c95-118">Si el tercer argumento se establece en true, la lista de elemento se selecciona automáticamente en el explorador.</span><span class="sxs-lookup"><span data-stu-id="30c95-118">If the third argument is set to true, the list element is automatically selected in the browser.</span></span>

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="30c95-119">Cargar la página en el explorador, se rellenará la lista desplegable con los tres proveedores, la segunda se está preseleccionada.</span><span class="sxs-lookup"><span data-stu-id="30c95-119">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span>


<span data-ttu-id="30c95-120">[![La lista se rellena y se preselecciona automáticamente](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="30c95-120">[![The list is filled and preselected automatically](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="30c95-121">La lista se rellena y se preselecciona automáticamente ([haga clic aquí para ver la imagen a tamaño completo](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="30c95-121">The list is filled and preselected automatically ([Click to view full-size image](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="30c95-122">[Anterior](using-cascadingdropdown-with-a-database-vb.md)
[Siguiente](using-auto-postback-with-cascadingdropdown-vb.md)</span><span class="sxs-lookup"><span data-stu-id="30c95-122">[Previous](using-cascadingdropdown-with-a-database-vb.md)
[Next](using-auto-postback-with-cascadingdropdown-vb.md)</span></span>
