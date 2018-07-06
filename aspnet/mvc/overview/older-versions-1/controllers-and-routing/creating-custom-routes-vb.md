---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
title: Crear rutas personalizadas (VB) | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC. En este tutorial, obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax.
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: 6ac5758b-6199-42af-adcb-21954b864951
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-custom-routes-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d6125f93d99acc695be319bd000ff65ab4a1159
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829705"
---
<a name="creating-custom-routes-vb"></a><span data-ttu-id="a928d-104">Crear rutas personalizadas (VB)</span><span class="sxs-lookup"><span data-stu-id="a928d-104">Creating Custom Routes (VB)</span></span>
====================
<span data-ttu-id="a928d-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a928d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a928d-106">Obtenga información sobre cómo agregar rutas personalizadas a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a928d-106">Learn how to add custom routes to an ASP.NET MVC application.</span></span> <span data-ttu-id="a928d-107">En este tutorial, obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="a928d-107">In this tutorial, you learn how to modify the default route table in the Global.asax file.</span></span>


<span data-ttu-id="a928d-108">En este tutorial, obtendrá información sobre cómo agregar una ruta personalizada a una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="a928d-108">In this tutorial, you learn how to add a custom route to an ASP.NET MVC application.</span></span> <span data-ttu-id="a928d-109">Obtendrá información sobre cómo modificar la tabla de rutas predeterminadas en el archivo Global.asax con una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="a928d-109">You learn how to modify the default route table in the Global.asax file with a custom route.</span></span>

<span data-ttu-id="a928d-110">En las aplicaciones de ASP.NET MVC, la tabla de enrutamiento predeterminada funcionará bien.</span><span class="sxs-lookup"><span data-stu-id="a928d-110">In ASP.NET MVC applications, the default route table will work just fine.</span></span> <span data-ttu-id="a928d-111">Sin embargo, es posible que descubra que dispone de las necesidades de enrutamientos especializado.</span><span class="sxs-lookup"><span data-stu-id="a928d-111">However, you might discover that you have specialized routing needs.</span></span> <span data-ttu-id="a928d-112">En ese caso, puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="a928d-112">In that case, you can create a custom route.</span></span>

<span data-ttu-id="a928d-113">Por ejemplo, imagine que está creando una aplicación de blog.</span><span class="sxs-lookup"><span data-stu-id="a928d-113">Imagine, for example, that you are building a blog application.</span></span> <span data-ttu-id="a928d-114">Es posible que desee controlar las solicitudes entrantes que tienen un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a928d-114">You might want to handle incoming requests that look like this:</span></span>

<span data-ttu-id="a928d-115">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="a928d-115">/Archive/12-25-2009</span></span>

<span data-ttu-id="a928d-116">Cuando un usuario escribe esta solicitud, desea devolver la entrada de blog que corresponde a la fecha 25/12/2009.</span><span class="sxs-lookup"><span data-stu-id="a928d-116">When a user enters this request, you want to return the blog entry that corresponds to the date 12/25/2009.</span></span> <span data-ttu-id="a928d-117">Para controlar este tipo de solicitud, deberá crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="a928d-117">In order to handle this type of request, you need to create a custom route.</span></span>

<span data-ttu-id="a928d-118">El archivo Global.asax en el listado 1 contiene una ruta personalizada nueva, denominada Blog, que controla las solicitudes que se parecen a /Archive/*fecha de entrada*.</span><span class="sxs-lookup"><span data-stu-id="a928d-118">The Global.asax file in Listing 1 contains a new custom route, named Blog, which handles requests that look like /Archive/*entry date*.</span></span>

<span data-ttu-id="a928d-119">**Listado 1 - Global.asax (con una ruta personalizada)**</span><span class="sxs-lookup"><span data-stu-id="a928d-119">**Listing 1 - Global.asax (with custom route)**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample1.vb)]

<span data-ttu-id="a928d-120">Es importante el orden de las rutas que se agregan a la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="a928d-120">The order of the routes that you add to the route table is important.</span></span> <span data-ttu-id="a928d-121">Nuestra nueva ruta Blog personalizado se agrega antes de la ruta predeterminada existente.</span><span class="sxs-lookup"><span data-stu-id="a928d-121">Our new custom Blog route is added before the existing Default route.</span></span> <span data-ttu-id="a928d-122">Si invierte el orden, a continuación, la ruta predeterminada siempre se llamará en lugar de la ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="a928d-122">If you reversed the order, then the Default route always will get called instead of the custom route.</span></span>

<span data-ttu-id="a928d-123">La ruta de blogs personalizada coincide con cualquier solicitud que empieza con/Archive /.</span><span class="sxs-lookup"><span data-stu-id="a928d-123">The custom Blog route matches any request that starts with /Archive/.</span></span> <span data-ttu-id="a928d-124">Por lo tanto, ajusta a todas las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="a928d-124">So, it matches all of the following URLs:</span></span>

- <span data-ttu-id="a928d-125">/ Archive/12-25-2009</span><span class="sxs-lookup"><span data-stu-id="a928d-125">/Archive/12-25-2009</span></span>

- <span data-ttu-id="a928d-126">/ Archive/10-6-2004</span><span class="sxs-lookup"><span data-stu-id="a928d-126">/Archive/10-6-2004</span></span>

- <span data-ttu-id="a928d-127">/ Archive/apple</span><span class="sxs-lookup"><span data-stu-id="a928d-127">/Archive/apple</span></span>

<span data-ttu-id="a928d-128">La ruta personalizada la solicitud entrante asigna a un controlador con el nombre de archivo e invoca la acción Entry().</span><span class="sxs-lookup"><span data-stu-id="a928d-128">The custom route maps the incoming request to a controller named Archive and invokes the Entry() action.</span></span> <span data-ttu-id="a928d-129">Cuando se llama al método Entry(), la fecha de entrada se pasa como un parámetro denominado entryDate.</span><span class="sxs-lookup"><span data-stu-id="a928d-129">When the Entry() method is called, the entry date is passed as a parameter named entryDate.</span></span>

<span data-ttu-id="a928d-130">Puede usar la ruta personalizada de Blog con el controlador en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="a928d-130">You can use the Blog custom route with the controller in Listing 2.</span></span>

<span data-ttu-id="a928d-131">**Listado 2 - ArchiveController.vb**</span><span class="sxs-lookup"><span data-stu-id="a928d-131">**Listing 2 - ArchiveController.vb**</span></span>

[!code-vb[Main](creating-custom-routes-vb/samples/sample2.vb)]

<span data-ttu-id="a928d-132">Tenga en cuenta que el método Entry() en el listado 2 acepta un parámetro de tipo DateTime.</span><span class="sxs-lookup"><span data-stu-id="a928d-132">Notice that the Entry() method in Listing 2 accepts a parameter of type DateTime.</span></span> <span data-ttu-id="a928d-133">El marco de MVC es lo suficientemente inteligente como para convertir la fecha de entrada de la dirección URL en un valor de fecha y hora automáticamente.</span><span class="sxs-lookup"><span data-stu-id="a928d-133">The MVC framework is smart enough to convert the entry date from the URL into a DateTime value automatically.</span></span> <span data-ttu-id="a928d-134">Si el parámetro de entrada de fecha de la dirección URL no puede convertirse en una fecha y hora, se produce un error (consulte la figura 1).</span><span class="sxs-lookup"><span data-stu-id="a928d-134">If the entry date parameter from the URL cannot be converted to a DateTime, an error is raised (see Figure 1).</span></span>

<span data-ttu-id="a928d-135">**Figura 1: Error en la conversión de parámetro**</span><span class="sxs-lookup"><span data-stu-id="a928d-135">**Figure 1 - Error from converting parameter**</span></span>


<span data-ttu-id="a928d-136">[![El cuadro de diálogo nuevo proyecto](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a928d-136">[![The New Project dialog box](creating-custom-routes-vb/_static/image1.jpg)](creating-custom-routes-vb/_static/image1.png)</span></span>

<span data-ttu-id="a928d-137">**Figura 01**: Error en la conversión de parámetro ([haga clic aquí para ver imagen en tamaño completo](creating-custom-routes-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a928d-137">**Figure 01**: Error from converting parameter ([Click to view full-size image](creating-custom-routes-vb/_static/image2.png))</span></span>


## <a name="summary"></a><span data-ttu-id="a928d-138">Resumen</span><span class="sxs-lookup"><span data-stu-id="a928d-138">Summary</span></span>

<span data-ttu-id="a928d-139">El objetivo de este tutorial era demostrar cómo puede crear una ruta personalizada.</span><span class="sxs-lookup"><span data-stu-id="a928d-139">The goal of this tutorial was to demonstrate how you can create a custom route.</span></span> <span data-ttu-id="a928d-140">Ha aprendido cómo agregar una ruta personalizada a la tabla de rutas en el archivo Global.asax que representa las entradas de blog.</span><span class="sxs-lookup"><span data-stu-id="a928d-140">You learned how to add a custom route to the route table in the Global.asax file that represents blog entries.</span></span> <span data-ttu-id="a928d-141">Analizamos cómo asignar las solicitudes de entradas de blog con un controlador denominado ArchiveController y una acción de controlador denominado Entry().</span><span class="sxs-lookup"><span data-stu-id="a928d-141">We discussed how to map requests for blog entries to a controller named ArchiveController and a controller action named Entry().</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a928d-142">[Anterior](asp-net-mvc-controller-overview-vb.md)
> [Siguiente](creating-a-route-constraint-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a928d-142">[Previous](asp-net-mvc-controller-overview-vb.md)
[Next](creating-a-route-constraint-vb.md)</span></span>
