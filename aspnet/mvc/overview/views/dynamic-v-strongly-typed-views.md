---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "V dinámicos. Establecimiento inflexible de tipos vistas | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="9c450-103">V dinámicos.</span><span class="sxs-lookup"><span data-stu-id="9c450-103">Dynamic v.</span></span> <span data-ttu-id="9c450-104">Vistas fuertemente tipadas</span><span class="sxs-lookup"><span data-stu-id="9c450-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="9c450-105">Por [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9c450-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="9c450-106">Hay tres maneras para pasar información de un controlador a una vista en ASP.NET MVC 3:</span><span class="sxs-lookup"><span data-stu-id="9c450-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="9c450-107">Como un objeto de modelo fuertemente tipado.</span><span class="sxs-lookup"><span data-stu-id="9c450-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="9c450-108">Como un tipo dinámico (con @model dinámica)</span><span class="sxs-lookup"><span data-stu-id="9c450-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="9c450-109">Usar el elemento ViewBag</span><span class="sxs-lookup"><span data-stu-id="9c450-109">Using the ViewBag</span></span>

<span data-ttu-id="9c450-110">He escrito una aplicación de MVC 3 superior Blog simple para comparar y contrastar las vistas dinámicas y fuertemente tipadas.</span><span class="sxs-lookup"><span data-stu-id="9c450-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="9c450-111">El controlador se inicia con una simple lista de blogs:</span><span class="sxs-lookup"><span data-stu-id="9c450-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="9c450-112">Haga clic en el método IndexNotStonglyTyped() y agregar una vista Razor.</span><span class="sxs-lookup"><span data-stu-id="9c450-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="9c450-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9c450-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="9c450-114">Asegúrese de que el **crear una vista fuertemente tipada** casilla no está activada.</span><span class="sxs-lookup"><span data-stu-id="9c450-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="9c450-115">La vista resultante no contiene gran parte:</span><span class="sxs-lookup"><span data-stu-id="9c450-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="9c450-116">Como vamos a usar un dinámico y no una vista fuertemente tipada, intellisense no ayudarnos.</span><span class="sxs-lookup"><span data-stu-id="9c450-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="9c450-117">El código completo se muestra a continuación:</span><span class="sxs-lookup"><span data-stu-id="9c450-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="9c450-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="9c450-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="9c450-119">Ahora vamos a agregar una vista fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="9c450-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="9c450-120">Agregue el código siguiente al controlador:</span><span class="sxs-lookup"><span data-stu-id="9c450-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="9c450-121">Tenga en cuenta que es exactamente el View(topBlogs) devuelto mismo; Llame a que la vista no fuertemente tipada.</span><span class="sxs-lookup"><span data-stu-id="9c450-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="9c450-122">Haga clic con el botón secundario dentro de *StonglyTypedIndex()* y seleccione **agregar vista**.</span><span class="sxs-lookup"><span data-stu-id="9c450-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="9c450-123">Esta vez, seleccione la **Blog** clase de modelo y seleccione **lista** como la plantilla de scaffolding.</span><span class="sxs-lookup"><span data-stu-id="9c450-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="9c450-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="9c450-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="9c450-125">Dentro de la nueva plantilla de vista se obtener compatibilidad con intellisense.</span><span class="sxs-lookup"><span data-stu-id="9c450-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="9c450-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="9c450-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="9c450-127">Se puede descargar el proyecto de c# [aquí](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="9c450-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
