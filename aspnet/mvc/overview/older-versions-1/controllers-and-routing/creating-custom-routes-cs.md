---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Crear rutas personalizadas (C#) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, aprenderá a modificar la tabla de rutas predeterminadas en el archivo Global.asax.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 573b6a3360124feea92788ff7a3de363840fa1ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="c9cb8-104">Crear rutas personalizadas (C#)</span><span class="sxs-lookup"><span data-stu-id="c9cb8-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="c9cb8-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c9cb8-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c9cb8-106">Obtenga información acerca de cómo agregar rutas personalizadas a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="c9cb8-107">En este tutorial, aprenderá a modificar la tabla de rutas predeterminadas en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="c9cb8-108">En este tutorial, aprenderá a agregar una ruta personalizada a una aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="c9cb8-109">Obtenga información acerca de cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax con una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="c9cb8-110">Para muchas aplicaciones de ASP.NET MVC simples, la tabla de rutas predeterminadas funcionará correctamente.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="c9cb8-111">Sin embargo, puede que descubra que tenga necesidades de enrutamientos especiales.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="c9cb8-112">En ese caso, puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="c9cb8-113">Por ejemplo, imagine que está creando una aplicación de blog.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="c9cb8-114">Puede administrar las solicitudes entrantes que tengan un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="c9cb8-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="c9cb8-115">/ Archivo/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="c9cb8-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="c9cb8-116">Cuando un usuario escribe esta solicitud, que desea devolver la entrada de blog que corresponde a la fecha 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="c9cb8-117">Para controlar este tipo de solicitud, debe crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="c9cb8-118">El archivo Global.asax en el listado 1 contiene una ruta personalizada nueva, denominada Blog, que trata las solicitudes que se parecen /Archive/*fecha de entrada*.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="c9cb8-119">**Lista 1 - Global.asax (con una ruta personalizada)**</span><span class="sxs-lookup"><span data-stu-id="c9cb8-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="c9cb8-120">Es importante el orden de las rutas que se agregan a la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="c9cb8-121">Se agregó la nueva ruta de Blog personalizado antes de la ruta predeterminada existente.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="c9cb8-122">Si se invierte el orden, a continuación, la ruta predeterminada siempre se llamará en lugar de la ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="c9cb8-123">La ruta de Blog personalizada coincide con cualquier solicitud que se inicia con/archivo /.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="c9cb8-124">Por lo tanto, ajusta a todas las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="c9cb8-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="c9cb8-125">/ Archivo/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="c9cb8-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="c9cb8-126">/Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="c9cb8-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="c9cb8-127">/ Archivo/apple</span><span class="sxs-lookup"><span data-stu-id="c9cb8-127">/Archive/apple</span></span>

<span data-ttu-id="c9cb8-128">La ruta personalizada la solicitud entrante asigna a un controlador con el nombre de archivo e invoca la acción de Entry().</span><span class="sxs-lookup"><span data-stu-id="c9cb8-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="c9cb8-129">Cuando se llama al método de Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="c9cb8-130">Puede usar la ruta personalizada Blog con el controlador en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="c9cb8-131">**La lista 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="c9cb8-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="c9cb8-132">Observe que el método Entry() en el listado 2 acepta un parámetro de tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="c9cb8-133">El marco de MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un valor de fecha y hora automáticamente.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="c9cb8-134">Si el parámetro de fecha de entrada de la dirección URL no se puede convertir en un valor DateTime, se produce un error (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="c9cb8-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="c9cb8-135">**Figura 1: Error en la conversión de parámetros**</span><span class="sxs-lookup"><span data-stu-id="c9cb8-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="c9cb8-136">[![El cuadro de diálogo nuevo proyecto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c9cb8-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="c9cb8-137">**Figura 01**: Error en la conversión de parámetro ([haga clic aquí para ver la imagen a tamaño completo](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="c9cb8-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="c9cb8-138">Resumen</span><span class="sxs-lookup"><span data-stu-id="c9cb8-138">Summary</span></span>

<span data-ttu-id="c9cb8-139">El objetivo de este tutorial era demostrar cómo puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="c9cb8-140">Aprendido cómo agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa las entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="c9cb8-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="c9cb8-141">Se describe cómo asignar las solicitudes para las entradas de blog a un controlador denominado ArchiveController y una acción de controlador denominado Entry().</span><span class="sxs-lookup"><span data-stu-id="c9cb8-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c9cb8-142">[Anterior](aspnet-mvc-controllers-overview-cs.md)
> [Siguiente](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="c9cb8-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
