---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Con devolución de datos automática CascadingDropDown (VB) | Documentos de Microsoft
author: wenz
description: El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores de anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871381"
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a><span data-ttu-id="ac963-103">Uso de devolución de datos automática con CascadingDropDown (VB)</span><span class="sxs-lookup"><span data-stu-id="ac963-103">Using Auto-Postback with CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="ac963-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ac963-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ac963-105">[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ac963-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)</span></span>

> <span data-ttu-id="ac963-106">El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="ac963-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="ac963-107">Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de NET DropDownList no funciona, ya que la carga de forma asincrónica datos en la lista genera una respuesta (innecesario) por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="ac963-107">However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="ac963-108">Con algún código de JavaScript, se puede evitar este efecto.</span><span class="sxs-lookup"><span data-stu-id="ac963-108">With some JavaScript code, this effect can be avoided.</span></span>


## <a name="overview"></a><span data-ttu-id="ac963-109">Información general</span><span class="sxs-lookup"><span data-stu-id="ac963-109">Overview</span></span>

<span data-ttu-id="ac963-110">El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="ac963-110">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="ac963-111">(Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) Sin embargo cuando se usa el control CascadingDropDown, ASP. Característica de AutoPostBack del control de NET DropDownList no funciona, ya que la carga de forma asincrónica datos en la lista genera una respuesta (innecesario) por sí mismo.</span><span class="sxs-lookup"><span data-stu-id="ac963-111">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) However when using the CascadingDropDown control, ASP.NET's DropDownList control's AutoPostBack feature does not work, since asynchronously loading data into the list generates an (unnecessary) postback itself.</span></span> <span data-ttu-id="ac963-112">Con algún código de JavaScript, se puede evitar este efecto.</span><span class="sxs-lookup"><span data-stu-id="ac963-112">With some JavaScript code, this effect can be avoided.</span></span>

## <a name="steps"></a><span data-ttu-id="ac963-113">Pasos</span><span class="sxs-lookup"><span data-stu-id="ac963-113">Steps</span></span>

<span data-ttu-id="ac963-114">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el &lt; `form` &gt; elemento):</span><span class="sxs-lookup"><span data-stu-id="ac963-114">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="ac963-115">A continuación, se requiere un control de DropDownList:</span><span class="sxs-lookup"><span data-stu-id="ac963-115">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="ac963-116">Para esta lista, se agrega un extensor CascadingDropDown, proporcionar información de dirección URL y el método de servicio web:</span><span class="sxs-lookup"><span data-stu-id="ac963-116">For this list, a CascadingDropDown extender is added, providing web service URL and method information:</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="ac963-117">El extensor CascadingDropDown, a continuación, llama de forma asincrónica un servicio web con la siguiente firma de método:</span><span class="sxs-lookup"><span data-stu-id="ac963-117">The CascadingDropDown extender then asynchronously calls a web service with the following method signature:</span></span>

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="ac963-118">El método devuelve una matriz de tipo de valor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="ac963-118">The method returns an array of type CascadingDropDown value.</span></span> <span data-ttu-id="ac963-119">El constructor del tipo espera primero título de la entrada de lista y, a continuación, el valor (HTML `value` atributo).</span><span class="sxs-lookup"><span data-stu-id="ac963-119">The type's constructor expects first the list entry's caption and then the value (HTML `value` attribute).</span></span>

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="ac963-120">Cargar la página en el explorador, se rellenará la lista desplegable con los tres proveedores, la segunda se está preseleccionada.</span><span class="sxs-lookup"><span data-stu-id="ac963-120">Loading the page in the browser will fill the dropdown list with three vendors, the second one being preselected.</span></span> <span data-ttu-id="ac963-121">Además, ASP.NET define la `__doPostBack()` método JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ac963-121">Also, ASP.NET defines the `__doPostBack()` JavaScript method.</span></span> <span data-ttu-id="ac963-122">Una vez que se ha cargado la página, esta llamada de JavaScript se agrega a la lista desplegable, pero solo si hay elementos en ella.</span><span class="sxs-lookup"><span data-stu-id="ac963-122">Once the page has been loaded, this JavaScript call is added to the dropdown list, but only if there are elements in it.</span></span> <span data-ttu-id="ac963-123">Si no hay ningún elemento en la lista, el Kit de herramientas de Control está cargando actualmente, por lo que el código de JavaScript usa un tiempo de espera y trata de nuevo en un medio segundo.</span><span class="sxs-lookup"><span data-stu-id="ac963-123">If there are no elements in the list, the Control Toolkit is currently loading them, so the JavaScript code uses a timeout and tries again in a half second.</span></span>

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

<span data-ttu-id="ac963-124">De esta manera, una devolución de datos solo se ejecuta cuando hay elementos realmente en la lista y el usuario selecciona una entrada.</span><span class="sxs-lookup"><span data-stu-id="ac963-124">This way, a postback is only executed when there are actually elements in the list and the user selects an entry.</span></span>


<span data-ttu-id="ac963-125">[![Al seleccionar un elemento de lista, una devolución de datos](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ac963-125">[![Selecting a list element causes a postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="ac963-126">Al seleccionar un elemento de lista, una devolución de datos ([haga clic aquí para ver la imagen a tamaño completo](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ac963-126">Selecting a list element causes a postback ([Click to view full-size image](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ac963-127">Anterior</span><span class="sxs-lookup"><span data-stu-id="ac963-127">Previous</span></span>](presetting-list-entries-with-cascadingdropdown-vb.md)
