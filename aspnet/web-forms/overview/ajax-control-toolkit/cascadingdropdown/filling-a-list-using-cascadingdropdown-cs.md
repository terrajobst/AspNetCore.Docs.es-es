---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: Rellenar una lista con CascadingDropDown (C#) | Documentos de Microsoft
author: wenz
description: El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores de anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: c9e47f6484e49013004bf15084f98440ee67558e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="filling-a-list-using-cascadingdropdown-c"></a><span data-ttu-id="911ca-103">Rellenar una lista con CascadingDropDown (C#)</span><span class="sxs-lookup"><span data-stu-id="911ca-103">Filling a List Using CascadingDropDown (C#)</span></span>
====================
<span data-ttu-id="911ca-104">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="911ca-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="911ca-105">[Descargar código](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) o [descarga de PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="911ca-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)</span></span>

> <span data-ttu-id="911ca-106">El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="911ca-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="911ca-107">(Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) El primer desafío para resolver es realmente rellenar una lista desplegable con este control.</span><span class="sxs-lookup"><span data-stu-id="911ca-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="911ca-108">Información general</span><span class="sxs-lookup"><span data-stu-id="911ca-108">Overview</span></span>

<span data-ttu-id="911ca-109">El control CascadingDropDown en el Kit de herramientas de Control de AJAX extiende un control DropDownList para que los cambios en una carga de DropDownList asociados valores en otra DropDownList.</span><span class="sxs-lookup"><span data-stu-id="911ca-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="911ca-110">(Por ejemplo, una lista proporciona una lista de US Estados y la lista siguiente, a continuación, se rellena con ciudades importantes en ese estado.) El primer desafío para resolver es realmente rellenar una lista desplegable con este control.</span><span class="sxs-lookup"><span data-stu-id="911ca-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="911ca-111">Pasos</span><span class="sxs-lookup"><span data-stu-id="911ca-111">Steps</span></span>

<span data-ttu-id="911ca-112">Para activar la funcionalidad de AJAX de ASP.NET y el Kit de herramientas de Control, el `ScriptManager` control se debe colocar en cualquier sitio en la página (pero en el `<form>` elemento):</span><span class="sxs-lookup"><span data-stu-id="911ca-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

<span data-ttu-id="911ca-113">A continuación, se requiere un control de DropDownList:</span><span class="sxs-lookup"><span data-stu-id="911ca-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

<span data-ttu-id="911ca-114">Para esta lista, se agrega un extensor CascadingDropDown.</span><span class="sxs-lookup"><span data-stu-id="911ca-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="911ca-115">Enviará una solicitud asincrónica a un servicio web que devolverá una lista de entradas que se mostrará en la lista.</span><span class="sxs-lookup"><span data-stu-id="911ca-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="911ca-116">Para que funcione, deben establecerse los atributos CascadingDropDown siguientes:</span><span class="sxs-lookup"><span data-stu-id="911ca-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="911ca-117">`ServicePath`: Dirección URL de un servicio web entrega las entradas de lista</span><span class="sxs-lookup"><span data-stu-id="911ca-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="911ca-118">`ServiceMethod`: Método web entrega de las entradas de lista</span><span class="sxs-lookup"><span data-stu-id="911ca-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="911ca-119">`TargetControlID`: Id. de la lista desplegable</span><span class="sxs-lookup"><span data-stu-id="911ca-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="911ca-120">`Category`: Información de categoría que se envía al método web cuando se llama</span><span class="sxs-lookup"><span data-stu-id="911ca-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="911ca-121">`PromptText`: Texto que aparece cuando asincrónicamente Cargando datos de la lista desde el servidor</span><span class="sxs-lookup"><span data-stu-id="911ca-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="911ca-122">Este es el marcado para la `CascadingDropDown` elemento.</span><span class="sxs-lookup"><span data-stu-id="911ca-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="911ca-123">La única diferencia entre C# y VB es el nombre del servicio web asociado:</span><span class="sxs-lookup"><span data-stu-id="911ca-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

<span data-ttu-id="911ca-124">El código de JavaScript procedente de la `CascadingDropDown` extender llama a un método de servicio web con la siguiente firma:</span><span class="sxs-lookup"><span data-stu-id="911ca-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

<span data-ttu-id="911ca-125">Por lo que el aspecto importante es que el método debe devolver una matriz de tipo `CascadingDropDownNameValue` (definido por el Kit de herramientas de Control de AJAX de ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="911ca-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="911ca-126">En el `CascadingDropDownNameValue` constructor, primero deben proporcionar el texto de la entrada de lista y, a continuación, su valor, al igual que `<option value="VALUE">NAME</option>` sería hacerlo en HTML.</span><span class="sxs-lookup"><span data-stu-id="911ca-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="911ca-127">Le mostramos algunos datos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="911ca-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

<span data-ttu-id="911ca-128">Cargar la página en el explorador se activará la lista que se debe satisfacer con tres proveedores.</span><span class="sxs-lookup"><span data-stu-id="911ca-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="911ca-129">[![La lista se rellena automáticamente](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="911ca-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)</span></span>

<span data-ttu-id="911ca-130">La lista se rellena automáticamente ([haga clic aquí para ver la imagen a tamaño completo](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="911ca-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="911ca-131">Siguiente</span><span class="sxs-lookup"><span data-stu-id="911ca-131">Next</span></span>](using-cascadingdropdown-with-a-database-cs.md)
