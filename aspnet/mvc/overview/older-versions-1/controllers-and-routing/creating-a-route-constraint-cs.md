---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
title: Crear una restricción de ruta (C#) | Documentos de Microsoft
author: StephenWalther
description: En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 0bfd06b1-12d3-4fbb-9779-a82e5eb7fe7d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: 3159feb6538e3048f4f235f7d549e692604ca4e7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-c"></a><span data-ttu-id="22ac2-103">Crear una restricción de ruta (C#)</span><span class="sxs-lookup"><span data-stu-id="22ac2-103">Creating a Route Constraint (C#)</span></span>
====================
<span data-ttu-id="22ac2-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="22ac2-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="22ac2-105">En este tutorial, Stephen Walther se muestra cómo se puede controlar cómo el explorador solicita coincidencia rutas mediante la creación de restricciones de la ruta con expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="22ac2-105">In this tutorial, Stephen Walther demonstrates how you can control how browser requests match routes by creating route constraints with regular expressions.</span></span>


<span data-ttu-id="22ac2-106">Utilizar restricciones de la ruta para restringir las solicitudes del explorador que coinciden con una ruta determinada.</span><span class="sxs-lookup"><span data-stu-id="22ac2-106">You use route constraints to restrict the browser requests that match a particular route.</span></span> <span data-ttu-id="22ac2-107">Puede usar una expresión regular para especificar una restricción de ruta.</span><span class="sxs-lookup"><span data-stu-id="22ac2-107">You can use a regular expression to specify a route constraint.</span></span>

<span data-ttu-id="22ac2-108">Por ejemplo, imagine que ha definido la ruta en el listado 1 en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="22ac2-108">For example, imagine that you have defined the route in Listing 1 in your Global.asax file.</span></span>

<span data-ttu-id="22ac2-109">**Lista 1 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="22ac2-109">**Listing 1 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample1.cs)]

<span data-ttu-id="22ac2-110">Lista 1 contiene una ruta con el nombre de producto.</span><span class="sxs-lookup"><span data-stu-id="22ac2-110">Listing 1 contains a route named Product.</span></span> <span data-ttu-id="22ac2-111">Puede usar la ruta de producto para asignar las solicitudes del explorador a la ProductController contenido en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="22ac2-111">You can use the Product route to map browser requests to the ProductController contained in Listing 2.</span></span>

<span data-ttu-id="22ac2-112">**La lista 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="22ac2-112">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample2.cs)]

<span data-ttu-id="22ac2-113">Observe que la acción de Details() expuesta por el controlador del producto acepta un solo parámetro denominado productId.</span><span class="sxs-lookup"><span data-stu-id="22ac2-113">Notice that the Details() action exposed by the Product controller accepts a single parameter named productId.</span></span> <span data-ttu-id="22ac2-114">Este parámetro es un parámetro de número entero.</span><span class="sxs-lookup"><span data-stu-id="22ac2-114">This parameter is an integer parameter.</span></span>

<span data-ttu-id="22ac2-115">La ruta definida en la lista 1 coincide con cualquiera de las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="22ac2-115">The route defined in Listing 1 will match any of the following URLs:</span></span>

- <span data-ttu-id="22ac2-116">/Product/23</span><span class="sxs-lookup"><span data-stu-id="22ac2-116">/Product/23</span></span>
- <span data-ttu-id="22ac2-117">/ / De producto 7</span><span class="sxs-lookup"><span data-stu-id="22ac2-117">/Product/7</span></span>

<span data-ttu-id="22ac2-118">Desafortunadamente, la ruta también coincidirá con las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="22ac2-118">Unfortunately, the route will also match the following URLs:</span></span>

- <span data-ttu-id="22ac2-119">/ Productos/bLa, bla</span><span class="sxs-lookup"><span data-stu-id="22ac2-119">/Product/blah</span></span>
- <span data-ttu-id="22ac2-120">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="22ac2-120">/Product/apple</span></span>

<span data-ttu-id="22ac2-121">Dado que la acción Details() espera un parámetro de número entero, que realiza una solicitud que contiene un valor distinto de un valor entero se producirá un error.</span><span class="sxs-lookup"><span data-stu-id="22ac2-121">Because the Details() action expects an integer parameter, making a request that contains something other than an integer value will cause an error.</span></span> <span data-ttu-id="22ac2-122">Por ejemplo, si escribe la dirección URL /Product/apple en el explorador, a continuación, obtendrá la página de error en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="22ac2-122">For example, if you type the URL /Product/apple into your browser then you will get the error page in Figure 1.</span></span>


<span data-ttu-id="22ac2-123">[![El cuadro de diálogo nuevo proyecto](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="22ac2-123">[![The New Project dialog box](creating-a-route-constraint-cs/_static/image1.jpg)](creating-a-route-constraint-cs/_static/image1.png)</span></span>

<span data-ttu-id="22ac2-124">**Figura 01**: ver una página seccionar ([haga clic aquí para ver la imagen a tamaño completo](creating-a-route-constraint-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="22ac2-124">**Figure 01**: Seeing a page explode ([Click to view full-size image](creating-a-route-constraint-cs/_static/image2.png))</span></span>


<span data-ttu-id="22ac2-125">¿Qué desea hacer en realidad es solo coincidirá con direcciones URL que contienen un productId entero adecuado.</span><span class="sxs-lookup"><span data-stu-id="22ac2-125">What you really want to do is only match URLs that contain a proper integer productId.</span></span> <span data-ttu-id="22ac2-126">Puede usar una restricción al definir una ruta para restringir las direcciones URL que coinciden con la ruta.</span><span class="sxs-lookup"><span data-stu-id="22ac2-126">You can use a constraint when defining a route to restrict the URLs that match the route.</span></span> <span data-ttu-id="22ac2-127">La ruta de producto modificada en el listado 3 contiene una restricción de expresión regular que coincida solo con enteros.</span><span class="sxs-lookup"><span data-stu-id="22ac2-127">The modified Product route in Listing 3 contains a regular expression constraint that only matches integers.</span></span>

<span data-ttu-id="22ac2-128">**El listado 3 - Global.asax.cs**</span><span class="sxs-lookup"><span data-stu-id="22ac2-128">**Listing 3 - Global.asax.cs**</span></span>

[!code-csharp[Main](creating-a-route-constraint-cs/samples/sample3.cs)]

<span data-ttu-id="22ac2-129">La expresión regular \d+ coincide con uno o más números enteros.</span><span class="sxs-lookup"><span data-stu-id="22ac2-129">The regular expression \d+ matches one or more integers.</span></span> <span data-ttu-id="22ac2-130">Esta restricción hace que la ruta de producto para que coincida con las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="22ac2-130">This constraint causes the Product route to match the following URLs:</span></span>

- <span data-ttu-id="22ac2-131">/ Productos/3</span><span class="sxs-lookup"><span data-stu-id="22ac2-131">/Product/3</span></span>
- <span data-ttu-id="22ac2-132">/Product/8999</span><span class="sxs-lookup"><span data-stu-id="22ac2-132">/Product/8999</span></span>

<span data-ttu-id="22ac2-133">Pero no las direcciones URL siguientes:</span><span class="sxs-lookup"><span data-stu-id="22ac2-133">But not the following URLs:</span></span>

- <span data-ttu-id="22ac2-134">/Product/apple</span><span class="sxs-lookup"><span data-stu-id="22ac2-134">/Product/apple</span></span>
- <span data-ttu-id="22ac2-135">/ Producto</span><span class="sxs-lookup"><span data-stu-id="22ac2-135">/Product</span></span>

- <span data-ttu-id="22ac2-136">Estas solicitudes de explorador será procesadas por otra ruta o, si no hay ninguna ruta coincidente, un *no se pudo encontrar el recurso* se devolverá el error.</span><span class="sxs-lookup"><span data-stu-id="22ac2-136">These browser requests will be handled by another route or, if there are no matching routes, a *The resource could not be found* error will be returned.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="22ac2-137">[Anterior](creating-custom-routes-cs.md)
> [Siguiente](creating-a-custom-route-constraint-cs.md)</span><span class="sxs-lookup"><span data-stu-id="22ac2-137">[Previous](creating-custom-routes-cs.md)
[Next](creating-a-custom-route-constraint-cs.md)</span></span>
