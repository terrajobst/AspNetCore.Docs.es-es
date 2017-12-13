---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Validar los datos con un nivel de servicio (C#) | Documentos de Microsoft
author: StephenWalther
description: "Obtenga información acerca de cómo mover la lógica de validación de las acciones de controlador y almacenarla en una capa de servicio independiente. En este tutorial, Stephen Walther explica cómo se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: f36301aef4377c6c00cb4fc33dbc5c57b1c426a9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="f1c6f-104">Validar los datos con un nivel de servicio (C#)</span><span class="sxs-lookup"><span data-stu-id="f1c6f-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="f1c6f-105">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f1c6f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f1c6f-106">Obtenga información acerca de cómo mover la lógica de validación de las acciones de controlador y almacenarla en una capa de servicio independiente.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="f1c6f-107">En este tutorial, Stephen Walther explica cómo puede mantener una separación de intereses aguda al aislar el nivel de servicio de la capa de controlador.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="f1c6f-108">El objetivo de este tutorial es describir un método para realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="f1c6f-109">En este tutorial, aprenderá a mover la lógica de validación de los controladores y en una capa de servicio independiente.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="f1c6f-110">Separación de problemas</span><span class="sxs-lookup"><span data-stu-id="f1c6f-110">Separating Concerns</span></span>

<span data-ttu-id="f1c6f-111">Cuando se compila una aplicación de MVC de ASP.NET, no se debe colocar la lógica de la base de datos dentro de las acciones de controlador.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="f1c6f-112">Mezclar la lógica de base de datos y el controlador hace que la aplicación resulte más difícil mantener con el tiempo.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="f1c6f-113">La recomendación es colocar toda la lógica de base de datos en una capa de repositorio independiente.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="f1c6f-114">Por ejemplo, el listado 1 contiene un repositorio simple denominado el ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="f1c6f-115">El repositorio de producto contiene todo el código de acceso de datos para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="f1c6f-116">La lista también incluye la interfaz IProductRepository que implementa el repositorio de producto.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="f1c6f-117">**Lista 1--Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="f1c6f-118">El controlador en el listado 2 usa la capa de repositorio en sus Index() y Create() acciones.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="f1c6f-119">Tenga en cuenta que este controlador no contiene ninguna lógica de base de datos.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="f1c6f-120">Creación de una capa de repositorio le permite mantener una separación clara de intereses.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="f1c6f-121">Los controladores son responsables de lógica de control de flujo de aplicación y el repositorio es responsable de la lógica de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="f1c6f-122">**La lista 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="f1c6f-123">Creación de una capa de servicio</span><span class="sxs-lookup"><span data-stu-id="f1c6f-123">Creating a Service Layer</span></span>

<span data-ttu-id="f1c6f-124">Por lo tanto, pertenece un controlador de lógica de control de flujo de aplicación y lógica de acceso a datos pertenece a un repositorio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="f1c6f-125">¿En ese caso, su ubicación lógica de validación?</span><span class="sxs-lookup"><span data-stu-id="f1c6f-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="f1c6f-126">Una opción consiste en colocar la lógica de validación en un *capa de servicio*.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="f1c6f-127">Un nivel de servicio es una capa adicional en una aplicación de ASP.NET MVC que realiza la comunicación entre un controlador y la capa de repositorio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="f1c6f-128">El nivel de servicio contiene la lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-128">The service layer contains business logic.</span></span> <span data-ttu-id="f1c6f-129">En concreto, contiene lógica de validación.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="f1c6f-130">Por ejemplo, el nivel de servicio de producto en la lista 3 tiene un método CreateProduct().</span><span class="sxs-lookup"><span data-stu-id="f1c6f-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="f1c6f-131">El método CreateProduct() llama al método ValidateProduct() para validar un nuevo producto antes de pasar el producto en el repositorio de producto.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="f1c6f-132">**El listado 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="f1c6f-133">El controlador de producto se ha actualizado en el listado 4 para usar el nivel de servicio en lugar de la capa de repositorio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="f1c6f-134">El nivel de controlador se comunica con el nivel de servicio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="f1c6f-135">El nivel de servicio se comunica con la capa de repositorio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="f1c6f-136">Cada capa tiene una responsabilidad independiente.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="f1c6f-137">**Listado 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="f1c6f-138">Tenga en cuenta que el servicio de producto se crea en el constructor del controlador de producto.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="f1c6f-139">Cuando se crea el servicio de producto, el diccionario de Estados del modelo se pasa al servicio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="f1c6f-140">El servicio de producto usa estado del modelo para devolver mensajes de error de validación al controlador.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="f1c6f-141">Separar el nivel de servicio</span><span class="sxs-lookup"><span data-stu-id="f1c6f-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="f1c6f-142">No hemos pudimos aislar el controlador y los niveles de servicio en un sentido.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="f1c6f-143">El controlador y los niveles de servicio se comunican a través de estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="f1c6f-144">En otras palabras, el nivel de servicio tiene una dependencia en una característica determinada del marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="f1c6f-145">Queremos aislar el nivel de servicio de la capa de controlador tanto como sea posible.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="f1c6f-146">En teoría, deberíamos estar puede utilizar el nivel de servicio con cualquier tipo de aplicación y no solo una aplicación de ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="f1c6f-147">Por ejemplo, en el futuro, tengamos que queremos generar un front-end para nuestra aplicación de WPF.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="f1c6f-148">Debemos encontramos una manera de quitar la dependencia de ASP.NET MVC estado del modelo de la capa de servicio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="f1c6f-149">En el listado 5, el nivel de servicio se ha actualizado para que ya no utiliza el estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="f1c6f-150">En su lugar, utiliza cualquier clase que implementa la interfaz IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="f1c6f-151">**Listado 5 - Models\ProductService.cs (desacoplado)**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="f1c6f-152">La interfaz IValidationDictionary se define en el listado 6.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="f1c6f-153">Esta interfaz simple tiene un método único y una sola propiedad.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="f1c6f-154">**Listado 6 - Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="f1c6f-155">La clase en la lista 7, con el nombre de la clase ModelStateWrapper, implementa la interfaz IValidationDictionary.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="f1c6f-156">Puede crear una instancia de la clase ModelStateWrapper pasando un diccionario de Estados del modelo al constructor.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="f1c6f-157">**Listado 7 - Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="f1c6f-158">Por último, el controlador actualizado listado 8 usa el ModelStateWrapper al crear el nivel de servicio en su constructor.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="f1c6f-159">**Enumerar 8 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="f1c6f-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="f1c6f-160">Mediante el IValidationDictionary interfaz y la clase ModelStateWrapper nos permite aislar totalmente nuestro nivel de servicio de la capa de controlador.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="f1c6f-161">El nivel de servicio ya no es dependiente de estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="f1c6f-162">Puede pasar cualquier clase que implementa la interfaz IValidationDictionary para el nivel de servicio.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="f1c6f-163">Por ejemplo, una aplicación de WPF podría implementar la interfaz IValidationDictionary con una clase de colección simple.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="f1c6f-164">Resumen</span><span class="sxs-lookup"><span data-stu-id="f1c6f-164">Summary</span></span>

<span data-ttu-id="f1c6f-165">El objetivo de este tutorial era explicar un enfoque para realizar la validación en una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="f1c6f-166">En este tutorial, aprendió a todas las de la lógica de validación de los controladores y en una capa de servicio independiente mover.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="f1c6f-167">También aprendió a aislar su nivel de servicio de la capa de controlador mediante la creación de una clase ModelStateWrapper.</span><span class="sxs-lookup"><span data-stu-id="f1c6f-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f1c6f-168">[Anterior](validating-with-the-idataerrorinfo-interface-cs.md)
[Siguiente](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f1c6f-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
