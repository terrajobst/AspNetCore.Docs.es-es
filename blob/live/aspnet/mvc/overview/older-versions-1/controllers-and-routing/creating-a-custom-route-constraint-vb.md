---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: "Crear una restricción de ruta personalizada (VB) | Documentos de Microsoft"
author: StephenWalther
description: "Stephen Walther muestra cómo puede crear una restricción de ruta personalizados. Implementamos un sencillo personalizado restricción que impide que una ruta que se va a coincide w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 41b04c1fea267e7ee9f8a0b1c2f0d4fe4bb96d15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-custom-route-constraint-vb"></a><span data-ttu-id="ebc33-104">Crear una restricción de ruta personalizada (VB)</span><span class="sxs-lookup"><span data-stu-id="ebc33-104">Creating a Custom Route Constraint (VB)</span></span>
====================
<span data-ttu-id="ebc33-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="ebc33-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="ebc33-106">Stephen Walther muestra cómo puede crear una restricción de ruta personalizados.</span><span class="sxs-lookup"><span data-stu-id="ebc33-106">Stephen Walther demonstrates how you can create a custom route constraint.</span></span> <span data-ttu-id="ebc33-107">Implementamos una restricción personalizada simple que impide que una ruta que se va a buscar cuando se realiza una solicitud de explorador desde un equipo remoto.</span><span class="sxs-lookup"><span data-stu-id="ebc33-107">We implement a simple custom constraint that prevents a route from being matched when a browser request is made from a remote computer.</span></span>


<span data-ttu-id="ebc33-108">El objetivo de este tutorial es mostrar cómo puede crear una restricción de ruta personalizados.</span><span class="sxs-lookup"><span data-stu-id="ebc33-108">The goal of this tutorial is to demonstrate how you can create a custom route constraint.</span></span> <span data-ttu-id="ebc33-109">Una restricción de ruta personalizada le permite impedir que una ruta que se coteja a menos que se compara una condición personalizada.</span><span class="sxs-lookup"><span data-stu-id="ebc33-109">A custom route constraint enables you to prevent a route from being matched unless some custom condition is matched.</span></span>

<span data-ttu-id="ebc33-110">En este tutorial, se creará una restricción de ruta de host local.</span><span class="sxs-lookup"><span data-stu-id="ebc33-110">In this tutorial, we create a Localhost route constraint.</span></span> <span data-ttu-id="ebc33-111">La restricción de ruta de host local solo coincide con las solicitudes realizadas desde el equipo local.</span><span class="sxs-lookup"><span data-stu-id="ebc33-111">The Localhost route constraint only matches requests made from the local computer.</span></span> <span data-ttu-id="ebc33-112">No se cotejan solicitudes remotas de a través de Internet.</span><span class="sxs-lookup"><span data-stu-id="ebc33-112">Remote requests from across the Internet are not matched.</span></span>

<span data-ttu-id="ebc33-113">Implementar una restricción de ruta personalizada implementando la interfaz IRouteConstraint.</span><span class="sxs-lookup"><span data-stu-id="ebc33-113">You implement a custom route constraint by implementing the IRouteConstraint interface.</span></span> <span data-ttu-id="ebc33-114">Se trata de una interfaz muy simple que describe un único método:</span><span class="sxs-lookup"><span data-stu-id="ebc33-114">This is an extremely simple interface which describes a single method:</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

<span data-ttu-id="ebc33-115">El método devuelve un valor booleano.</span><span class="sxs-lookup"><span data-stu-id="ebc33-115">The method returns a Boolean value.</span></span> <span data-ttu-id="ebc33-116">Si devuelve False, la ruta asociada a la restricción no coincide con la solicitud del explorador.</span><span class="sxs-lookup"><span data-stu-id="ebc33-116">If you return False, the route associated with the constraint won't match the browser request.</span></span>

<span data-ttu-id="ebc33-117">La restricción de Localhost está incluida en la lista 1.</span><span class="sxs-lookup"><span data-stu-id="ebc33-117">The Localhost constraint is contained in Listing 1.</span></span>

<span data-ttu-id="ebc33-118">**Lista 1 - LocalhostConstraint.vb**</span><span class="sxs-lookup"><span data-stu-id="ebc33-118">**Listing 1 - LocalhostConstraint.vb**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

<span data-ttu-id="ebc33-119">La restricción en la lista 1 aprovecha las ventajas de la propiedad IsLocal expuesta por la clase HttpRequest.</span><span class="sxs-lookup"><span data-stu-id="ebc33-119">The constraint in Listing 1 takes advantage of the IsLocal property exposed by the HttpRequest class.</span></span> <span data-ttu-id="ebc33-120">Esta propiedad devuelve true cuando la dirección IP de la solicitud es 127.0.0.1 o cuando la dirección IP de la solicitud es el mismo que la dirección IP del servidor.</span><span class="sxs-lookup"><span data-stu-id="ebc33-120">This property returns true when the IP address of the request is either 127.0.0.1 or when the IP of the request is the same as the server's IP address.</span></span>

<span data-ttu-id="ebc33-121">Usar una restricción personalizada dentro de una ruta definida en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ebc33-121">You use a custom constraint within a route defined in the Global.asax file.</span></span> <span data-ttu-id="ebc33-122">El archivo Global.asax en el listado 2 utiliza la restricción de Localhost para impedir que alguien solicita una página de administración a menos que realice la solicitud desde el servidor local.</span><span class="sxs-lookup"><span data-stu-id="ebc33-122">The Global.asax file in Listing 2 uses the Localhost constraint to prevent anyone from requesting an Admin page unless they make the request from the local server.</span></span> <span data-ttu-id="ebc33-123">Por ejemplo, una solicitud para /Admin/DeleteAll se producirá un error cuando se realiza desde un servidor remoto.</span><span class="sxs-lookup"><span data-stu-id="ebc33-123">For example, a request for /Admin/DeleteAll will fail when made from a remote server.</span></span>

<span data-ttu-id="ebc33-124">**La lista 2 - Global.asax**</span><span class="sxs-lookup"><span data-stu-id="ebc33-124">**Listing 2 - Global.asax**</span></span>

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

<span data-ttu-id="ebc33-125">La restricción de host local se utiliza en la definición de la ruta de administración.</span><span class="sxs-lookup"><span data-stu-id="ebc33-125">The Localhost constraint is used in the definition of the Admin route.</span></span> <span data-ttu-id="ebc33-126">Esta ruta no se asocian con una solicitud de explorador remoto.</span><span class="sxs-lookup"><span data-stu-id="ebc33-126">This route won't be matched by a remote browser request.</span></span> <span data-ttu-id="ebc33-127">Sin embargo, tenga en cuenta que otras rutas definidas en Global.asax podrían coincidir con la misma solicitud.</span><span class="sxs-lookup"><span data-stu-id="ebc33-127">Realize, however, that other routes defined in Global.asax might match the same request.</span></span> <span data-ttu-id="ebc33-128">Es importante comprender que una restricción impide que una determinada ruta de una solicitud de búsqueda de coincidencias y no todas las rutas definen en el archivo Global.asax.</span><span class="sxs-lookup"><span data-stu-id="ebc33-128">It is important to understand that a constraint prevents a particular route from matching a request and not all routes defined in the Global.asax file.</span></span>

<span data-ttu-id="ebc33-129">Tenga en cuenta que la ruta predeterminada se convirtió en comentario en el archivo Global.asax en el listado 2.</span><span class="sxs-lookup"><span data-stu-id="ebc33-129">Notice that the Default route has been commented out from the Global.asax file in Listing 2.</span></span> <span data-ttu-id="ebc33-130">Si incluye la ruta predeterminada, la ruta predeterminada coincidiría con las solicitudes para el controlador de administración.</span><span class="sxs-lookup"><span data-stu-id="ebc33-130">If you include the Default route, then the Default route would match requests for the Admin controller.</span></span> <span data-ttu-id="ebc33-131">En ese caso, los usuarios remotos todavía pudieron invocar acciones del controlador de administración, aunque sus solicitudes no coinciden con la ruta de administración.</span><span class="sxs-lookup"><span data-stu-id="ebc33-131">In that case, remote users could still invoke actions of the Admin controller even though their requests wouldn't match the Admin route.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="ebc33-132">Anterior</span><span class="sxs-lookup"><span data-stu-id="ebc33-132">Previous</span></span>](creating-a-route-constraint-vb.md)
