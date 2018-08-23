---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Vistas dinámicas frente a Vistas de establecimiento inflexible de tipos | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/27/2011
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 41a47f0f6e5fd900e8bf35c37ed98566fe416ace
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828768"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="8fafd-103">Vistas dinámicas frente a</span><span class="sxs-lookup"><span data-stu-id="8fafd-103">Dynamic v.</span></span> <span data-ttu-id="8fafd-104">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="8fafd-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="8fafd-105">por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="8fafd-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="8fafd-106">Hay tres maneras de pasar información de un controlador a una vista en ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="8fafd-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="8fafd-107">Como un objeto de modelo fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="8fafd-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="8fafd-108">Como un tipo dinámico (mediante @model dinámico)</span><span class="sxs-lookup"><span data-stu-id="8fafd-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="8fafd-109">Utilizando el elemento ViewBag</span><span class="sxs-lookup"><span data-stu-id="8fafd-109">Using the ViewBag</span></span>

<span data-ttu-id="8fafd-110">He escrito una aplicación de MVC 3 principales Blog sencilla para comparar y contrastar las vistas dinámicas y fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="8fafd-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="8fafd-111">El controlador comienza con una simple lista de blogs:</span><span class="sxs-lookup"><span data-stu-id="8fafd-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="8fafd-112">Haga clic con el botón derecho en el método IndexNotStonglyTyped() y agregar una vista de Razor.</span><span class="sxs-lookup"><span data-stu-id="8fafd-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="8fafd-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8fafd-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="8fafd-114">Asegúrese de que el **crear una vista fuertemente tipada** no esté marcada la casilla.</span><span class="sxs-lookup"><span data-stu-id="8fafd-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="8fafd-115">La vista resultante no contiene gran parte:</span><span class="sxs-lookup"><span data-stu-id="8fafd-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="8fafd-116">Dado que usamos un dinámico y no una vista fuertemente tipada, intellisense no ayudarnos.</span><span class="sxs-lookup"><span data-stu-id="8fafd-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="8fafd-117">El código completo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="8fafd-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="8fafd-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="8fafd-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="8fafd-119">Ahora vamos a agregar una vista fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="8fafd-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="8fafd-120">Agregue el código siguiente al controlador:</span><span class="sxs-lookup"><span data-stu-id="8fafd-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="8fafd-121">Tenga en cuenta es exactamente el View(topBlogs) devuelto mismo; Llame a que la vista que no son fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="8fafd-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="8fafd-122">Haga clic con el botón derecho dentro de *StonglyTypedIndex()* y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="8fafd-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="8fafd-123">Esta vez seleccione el **Blog** clase de modelo y seleccione **lista** como plantilla Scaffold.</span><span class="sxs-lookup"><span data-stu-id="8fafd-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="8fafd-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="8fafd-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="8fafd-125">Dentro de la nueva plantilla de vista se obtiene compatibilidad con intellisense.</span><span class="sxs-lookup"><span data-stu-id="8fafd-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="8fafd-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8fafd-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="8fafd-127">Se puede descargar el proyecto de c# [aquí](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="8fafd-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
