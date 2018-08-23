---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
title: Crear rutas personalizadas (C#) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax.
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 3cd08f02-8763-490a-b625-2ac96a24b73f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1d7a25f9d257320c252408ae251e2f9f620930d8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835896"
---
<a name="creating-custom-routes-c"></a><span data-ttu-id="7f303-104">Crear rutas personalizadas (C#)</span><span class="sxs-lookup"><span data-stu-id="7f303-104">Creating Custom Routes (C#)</span></span>
====================
<span data-ttu-id="7f303-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7f303-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="7f303-106">Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7f303-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="7f303-107">En este tutorial, obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="7f303-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="7f303-108">En este tutorial, obtendrá información sobre cómo agregar una ruta personalizada a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="7f303-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="7f303-109">Obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax con una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="7f303-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="7f303-110">Para muchas aplicaciones de ASP.NET MVC simple, la tabla de enrutamiento predeterminada funcionará bien.</span><span class="sxs-lookup"><span data-stu-id="7f303-110">For many simple ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="7f303-111">Sin embargo, es posible que descubra que dispone de las necesidades de enrutamientos especializado.</span><span class="sxs-lookup"><span data-stu-id="7f303-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="7f303-112">En ese caso, puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="7f303-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="7f303-113">Por ejemplo, imagine que está creando una aplicación de blog.</span><span class="sxs-lookup"><span data-stu-id="7f303-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="7f303-114">Es posible que desee controlar las solicitudes entrantes que tienen un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="7f303-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="7f303-115">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="7f303-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="7f303-116">Cuando un usuario escribe esta solicitud, desea devolver la entrada de blog que corresponde a la fecha 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="7f303-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="7f303-117">Para controlar este tipo de solicitud, deberá crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="7f303-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="7f303-118">El archivo Global.asax en el listado 1 contiene una ruta personalizada nueva, denominada Blog, que controla las solicitudes que se parecen a /Archive/*fecha de entrada*.</span><span class="sxs-lookup"><span data-stu-id="7f303-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="7f303-119">**Listado 1 - Global.asax (con una ruta personalizada)**</span><span class="sxs-lookup"><span data-stu-id="7f303-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample1.cs)]

<span data-ttu-id="7f303-120">Es importante el orden de las rutas que se agregan a la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="7f303-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="7f303-121">Nuestra nueva ruta Blog personalizado se agrega antes de la ruta predeterminada existente.</span><span class="sxs-lookup"><span data-stu-id="7f303-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="7f303-122">Si invierte el orden, a continuación, la ruta predeterminada siempre se llamará en lugar de la ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="7f303-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="7f303-123">La ruta de blogs personalizada coincide con cualquier solicitud que empieza con/Archive /.</span><span class="sxs-lookup"><span data-stu-id="7f303-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="7f303-124">Por lo tanto, ajusta a todas las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="7f303-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="7f303-125">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="7f303-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="7f303-126">/ Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="7f303-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="7f303-127">/ Archive/apple</span><span class="sxs-lookup"><span data-stu-id="7f303-127">/Archive/apple</span></span>

<span data-ttu-id="7f303-128">La ruta personalizada la solicitud entrante asigna a un controlador con el nombre de archivo e invoca la acción Entry().</span><span class="sxs-lookup"><span data-stu-id="7f303-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="7f303-129">Cuando se llama al método Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.</span><span class="sxs-lookup"><span data-stu-id="7f303-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="7f303-130">Puede usar la ruta personalizada de Blog con el controlador en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="7f303-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="7f303-131">**Listado 2 - ArchiveController.cs**</span><span class="sxs-lookup"><span data-stu-id="7f303-131">**Listing 2 - ArchiveController.cs**</span></span>

[!code-csharp[Main](creating-custom-routes-cs/samples/sample2.cs)]

<span data-ttu-id="7f303-132">Tenga en cuenta que el método Entry() en el listado 2 acepta un parámetro de tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="7f303-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="7f303-133">El marco de MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un valor de fecha y hora automáticamente.</span><span class="sxs-lookup"><span data-stu-id="7f303-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="7f303-134">Si el parámetro de entrada de fecha de la dirección URL no puede convertirse en una fecha y hora, se produce un error (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="7f303-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="7f303-135">**Figura 1: Error en la conversión de parámetro**</span><span class="sxs-lookup"><span data-stu-id="7f303-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="7f303-136">[![El cuadro de diálogo nuevo proyecto](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7f303-136">[![The New Project dialog box](creating-custom-routes-cs/_static/image1.jpg)](creating-custom-routes-cs/_static/image1.png)</span></span>

<span data-ttu-id="7f303-137">**Figura 01**: Error en la conversión de parámetro ([haga clic aquí para ver imagen en tamaño completo](creating-custom-routes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="7f303-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-cs/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="7f303-138">Resumen</span><span class="sxs-lookup"><span data-stu-id="7f303-138">Summary</span></span>

<span data-ttu-id="7f303-139">El objetivo de este tutorial era demostrar cómo puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="7f303-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="7f303-140">Ha aprendido cómo agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa las entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="7f303-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="7f303-141">Analizamos cómo asignar las solicitudes de entradas de blog con un controlador denominado ArchiveController y una acción de controlador denominado Entry().</span><span class="sxs-lookup"><span data-stu-id="7f303-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7f303-142">[Anterior](aspnet-mvc-controllers-overview-cs.md)
> [Siguiente](creating-a-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7f303-142">[Previous](aspnet-mvc-controllers-overview-cs.md)
[Next](creating-a-route-constraint-cs.md)</span></span>
