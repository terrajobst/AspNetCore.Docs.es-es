---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Crear rutas personalizadas (VB) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminadas en el archivo Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: e827725ab7ce54c86ae9f4193d0a8a7ef4af8512
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="dc06c-104">Crear rutas personalizadas (VB)</span><span class="sxs-lookup"><span data-stu-id="dc06c-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="dc06c-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="dc06c-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="dc06c-106">Obtenga información acerca de cómo agregar rutas personalizadas a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dc06c-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="dc06c-107">En este tutorial, aprenderá a modificar la tabla de rutas predeterminadas en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="dc06c-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="dc06c-108">En este tutorial, aprenderá a agregar una ruta personalizada a una aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="dc06c-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="dc06c-109">Obtenga información acerca de cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax con una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="dc06c-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="dc06c-110">En las aplicaciones de ASP.NET MVC, la tabla de rutas predeterminadas funcionará correctamente.</span><span class="sxs-lookup"><span data-stu-id="dc06c-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="dc06c-111">Sin embargo, puede que descubra que tenga necesidades de enrutamientos especiales.</span><span class="sxs-lookup"><span data-stu-id="dc06c-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="dc06c-112">En ese caso, puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="dc06c-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="dc06c-113">Por ejemplo, imagine que está creando una aplicación de blog.</span><span class="sxs-lookup"><span data-stu-id="dc06c-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="dc06c-114">Puede administrar las solicitudes entrantes que tengan un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="dc06c-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="dc06c-115">/ Archivo/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="dc06c-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="dc06c-116">Cuando un usuario escribe esta solicitud, que desea devolver la entrada de blog que corresponde a la fecha 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="dc06c-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="dc06c-117">Para controlar este tipo de solicitud, debe crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="dc06c-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="dc06c-118">El archivo Global.asax en el listado 1 contiene una ruta personalizada nueva, denominada Blog, que trata las solicitudes que se parecen /Archive/*fecha de entrada*.</span><span class="sxs-lookup"><span data-stu-id="dc06c-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="dc06c-119">**Lista 1 - Global.asax (con una ruta personalizada)**</span><span class="sxs-lookup"><span data-stu-id="dc06c-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="dc06c-120">Es importante el orden de las rutas que se agregan a la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="dc06c-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="dc06c-121">Se agregó la nueva ruta de Blog personalizado antes de la ruta predeterminada existente.</span><span class="sxs-lookup"><span data-stu-id="dc06c-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="dc06c-122">Si se invierte el orden, a continuación, la ruta predeterminada siempre se llamará en lugar de la ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="dc06c-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="dc06c-123">La ruta de Blog personalizada coincide con cualquier solicitud que se inicia con/archivo /.</span><span class="sxs-lookup"><span data-stu-id="dc06c-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="dc06c-124">Por lo tanto, ajusta a todas las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="dc06c-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="dc06c-125">/ Archivo/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="dc06c-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="dc06c-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="dc06c-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="dc06c-127">/ Archivo/apple</span><span class="sxs-lookup"><span data-stu-id="dc06c-127">/Archive/apple</span></span>

<span data-ttu-id="dc06c-128">La ruta personalizada la solicitud entrante asigna a un controlador con el nombre de archivo e invoca la acción de Entry().</span><span class="sxs-lookup"><span data-stu-id="dc06c-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="dc06c-129">Cuando se llama al método de Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.</span><span class="sxs-lookup"><span data-stu-id="dc06c-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="dc06c-130">Puede usar la ruta personalizada Blog con el controlador en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="dc06c-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="dc06c-131">**La lista 2 - ArchiveController.vb**</span><span class="sxs-lookup"><span data-stu-id="dc06c-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="dc06c-132">Observe que el método Entry() en el listado 2 acepta un parámetro de tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="dc06c-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="dc06c-133">El marco de MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un valor de fecha y hora automáticamente.</span><span class="sxs-lookup"><span data-stu-id="dc06c-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="dc06c-134">Si el parámetro de fecha de entrada de la dirección URL no se puede convertir en un valor DateTime, se produce un error (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="dc06c-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="dc06c-135">**Figura 1: Error en la conversión de parámetros**</span><span class="sxs-lookup"><span data-stu-id="dc06c-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="dc06c-136">[![El cuadro de diálogo nuevo proyecto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dc06c-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="dc06c-137">**Figura 01**: Error en la conversión de parámetro ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="dc06c-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="dc06c-138">Resumen</span><span class="sxs-lookup"><span data-stu-id="dc06c-138">Summary</span></span>

<span data-ttu-id="dc06c-139">El objetivo de este tutorial era demostrar cómo puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="dc06c-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="dc06c-140">Aprendido cómo agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa las entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="dc06c-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="dc06c-141">Se describe cómo asignar las solicitudes para las entradas de blog a un controlador denominado ArchiveController y una acción de controlador denominado Entry().</span><span class="sxs-lookup"><span data-stu-id="dc06c-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="dc06c-142">[Anterior](asp-net-mvc-controller-overview-vb.md)
> [Siguiente](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dc06c-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
