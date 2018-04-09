---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Crear una acción (C#) | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo agregar una nueva acción a un controlador de MVC de ASP.NET. Obtenga información acerca de los requisitos para un método como una acción.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-c"></a><span data-ttu-id="fed95-104">Crear una acción (C#)</span><span class="sxs-lookup"><span data-stu-id="fed95-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="fed95-105">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="fed95-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="fed95-106">Obtenga información acerca de cómo agregar una nueva acción a un controlador de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="fed95-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="fed95-107">Obtenga información acerca de los requisitos para un método como una acción.</span><span class="sxs-lookup"><span data-stu-id="fed95-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="fed95-108">El objetivo de este tutorial es explicar cómo puede crear una nueva acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="fed95-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="fed95-109">Conocer los requisitos de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="fed95-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="fed95-110">También aprenderá cómo impedir que un método que se expone como una acción.</span><span class="sxs-lookup"><span data-stu-id="fed95-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="fed95-111">Agregar una acción a un controlador</span><span class="sxs-lookup"><span data-stu-id="fed95-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="fed95-112">Agregar una nueva acción a un controlador mediante la adición de un nuevo método al controlador.</span><span class="sxs-lookup"><span data-stu-id="fed95-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="fed95-113">Por ejemplo, el controlador en el listado 1 contiene una acción denominada Index() y una acción denominada SayHello().</span><span class="sxs-lookup"><span data-stu-id="fed95-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="fed95-114">Ambos métodos se exponen como acciones.</span><span class="sxs-lookup"><span data-stu-id="fed95-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="fed95-115">**Lista 1 - controllers\homecontroller**</span><span class="sxs-lookup"><span data-stu-id="fed95-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="fed95-116">Con el fin de exponer al universo como una acción, un método debe cumplir ciertos requisitos:</span><span class="sxs-lookup"><span data-stu-id="fed95-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="fed95-117">El método debe ser público.</span><span class="sxs-lookup"><span data-stu-id="fed95-117">The method must be public.</span></span>
- <span data-ttu-id="fed95-118">El método no puede ser un método estático.</span><span class="sxs-lookup"><span data-stu-id="fed95-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="fed95-119">El método no puede ser un método de extensión.</span><span class="sxs-lookup"><span data-stu-id="fed95-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="fed95-120">El método no puede ser un constructor, un captador o un establecedor.</span><span class="sxs-lookup"><span data-stu-id="fed95-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="fed95-121">El método no puede tener los tipos genéricos abiertos.</span><span class="sxs-lookup"><span data-stu-id="fed95-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="fed95-122">El método no es un método de la clase base del controlador.</span><span class="sxs-lookup"><span data-stu-id="fed95-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="fed95-123">El método no puede contener **ref** o **out** parámetros.</span><span class="sxs-lookup"><span data-stu-id="fed95-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="fed95-124">Tenga en cuenta que no hay ninguna restricción en el tipo de valor devuelto de una acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="fed95-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="fed95-125">Una acción de controlador puede devolver una cadena, un valor DateTime, una instancia de la clase Random, o nulo.</span><span class="sxs-lookup"><span data-stu-id="fed95-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="fed95-126">El marco de MVC de ASP.NET convertirá cualquier tipo de valor devuelto no es un resultado de acción en una cadena y procesar la cadena incluida en el explorador.</span><span class="sxs-lookup"><span data-stu-id="fed95-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="fed95-127">Cuando se agrega cualquier método que no infrinja estos requisitos a un controlador, el método se expone como una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="fed95-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="fed95-128">Tenga cuidado aquí.</span><span class="sxs-lookup"><span data-stu-id="fed95-128">Be careful here.</span></span> <span data-ttu-id="fed95-129">Cualquier persona conectada a Internet puede invocar una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="fed95-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="fed95-130">No, por ejemplo, cree una acción de controlador DeleteMyWebsite().</span><span class="sxs-lookup"><span data-stu-id="fed95-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="fed95-131">Impide que se está llamando a un método público</span><span class="sxs-lookup"><span data-stu-id="fed95-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="fed95-132">Si necesita crear un método público en una clase de controlador y no desea exponer el método como una acción de controlador, a continuación, se puede evitar que el método que se invoca mediante el atributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="fed95-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="fed95-133">Por ejemplo, el controlador en el listado 2 contiene un método público denominado CompanySecrets() que se decora con el atributo [NonAction].</span><span class="sxs-lookup"><span data-stu-id="fed95-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="fed95-134">**La lista 2 - Controllers\WorkController.cs**</span><span class="sxs-lookup"><span data-stu-id="fed95-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="fed95-135">Si se intenta invocar la acción del controlador CompanySecrets() escribiendo /Work/CompanySecrets en la barra de direcciones del explorador, a continuación, obtendrá el mensaje de error en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="fed95-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="fed95-136">[![Invocar un método NonAction](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fed95-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="fed95-137">**Figura 01**: invocar un método NonAction ([haga clic aquí para ver la imagen a tamaño completo](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="fed95-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="fed95-138">[Anterior](creating-a-controller-cs.md)
> [Siguiente](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fed95-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
