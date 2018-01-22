---
title: "Inspección de los métodos Details y Delete"
author: rick-anderson
description: "La vista y el método de controlador Details en una aplicación básica de ASP.NET Core MVC."
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/details
ms.openlocfilehash: 1b091cc479accfad5ad570a006d7395ef8d62a08
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="examining-the-details-and-delete-methods"></a><span data-ttu-id="f0d4b-103">Inspección de los métodos Details y Delete</span><span class="sxs-lookup"><span data-stu-id="f0d4b-103">Examining the Details and Delete methods</span></span>

<span data-ttu-id="f0d4b-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f0d4b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f0d4b-105">Abra el controlador Movie y examine el método `Details`:</span><span class="sxs-lookup"><span data-stu-id="f0d4b-105">Open the Movie controller and examine the `Details` method:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_details)]

<span data-ttu-id="f0d4b-106">El motor de scaffolding de MVC que creó este método de acción agrega un comentario en el que se muestra una solicitud HTTP que invoca el método.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-106">The MVC scaffolding engine that created this action method adds a comment showing an HTTP request that invokes the method.</span></span> <span data-ttu-id="f0d4b-107">En este caso se trata de una solicitud GET con tres segmentos de dirección URL, el controlador `Movies`, el método `Details` y un valor `id`.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-107">In this case it's a GET request with three URL segments, the `Movies` controller, the `Details` method and an `id` value.</span></span> <span data-ttu-id="f0d4b-108">Recuerde que estos segmentos se definen en *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-108">Recall these segments are defined in *Startup.cs*.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

<span data-ttu-id="f0d4b-109">EF facilita el proceso de búsqueda de datos mediante el método `SingleOrDefaultAsync`.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-109">EF makes it easy to search for data using the `SingleOrDefaultAsync` method.</span></span> <span data-ttu-id="f0d4b-110">Una característica de seguridad importante integrada en el método es que el código comprueba que el método de búsqueda haya encontrado una película antes de intentar hacer nada con ella.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-110">An important security feature built into the method is that the code verifies that the search method has found a movie before it tries to do anything with it.</span></span> <span data-ttu-id="f0d4b-111">Por ejemplo, un pirata informático podría introducir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` a algo parecido a `http://localhost:xxxx/Movies/Details/12345` (o algún otro valor que no represente una película real).</span><span class="sxs-lookup"><span data-stu-id="f0d4b-111">For example, a hacker could introduce errors into the site by changing the URL created by the links from `http://localhost:xxxx/Movies/Details/1` to something like  `http://localhost:xxxx/Movies/Details/12345` (or some other value that doesn't represent an actual movie).</span></span> <span data-ttu-id="f0d4b-112">Si no comprobara una película null, la aplicación generaría una excepción.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-112">If you did not check for a null movie, the app would throw an exception.</span></span>

<span data-ttu-id="f0d4b-113">Examine los métodos `Delete` y `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-113">Examine the `Delete` and `DeleteConfirmed` methods.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete)]

<span data-ttu-id="f0d4b-114">Tenga en cuenta que el método `HTTP GET Delete` no elimina la película especificada, sino que devuelve una vista de la película donde puede enviar (HttpPost) la eliminación.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-114">Note that the `HTTP GET Delete` method doesn't delete the specified movie, it returns a view of the movie where you can submit (HttpPost) the deletion.</span></span> <span data-ttu-id="f0d4b-115">La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-115">Performing a delete operation in response to a GET request (or for that matter, performing an edit operation, create operation, or any other operation that changes data) opens up a security hole.</span></span>

<span data-ttu-id="f0d4b-116">El método `[HttpPost]` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-116">The `[HttpPost]` method that deletes the data is named `DeleteConfirmed` to give the HTTP POST method a unique signature or name.</span></span> <span data-ttu-id="f0d4b-117">Las dos firmas de método se muestran a continuación:</span><span class="sxs-lookup"><span data-stu-id="f0d4b-117">The two method signatures are shown below:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete2)]

[!code-csharp[Main](start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_delete3)]


<span data-ttu-id="f0d4b-118">Common Language Runtime (CLR) requiere métodos sobrecargados para disponer de una firma de parámetro única (mismo nombre de método, pero lista de parámetros diferente).</span><span class="sxs-lookup"><span data-stu-id="f0d4b-118">The common language runtime (CLR) requires overloaded methods to have a unique parameter signature (same method name but different list of parameters).</span></span> <span data-ttu-id="f0d4b-119">En cambio, aquí necesita dos métodos `Delete` (uno para GET y otro para POST) que tienen la misma firma de parámetro</span><span class="sxs-lookup"><span data-stu-id="f0d4b-119">However, here you need two `Delete` methods -- one for GET and one for POST -- that both have the same parameter signature.</span></span> <span data-ttu-id="f0d4b-120">(ambos deben aceptar un número entero como parámetro).</span><span class="sxs-lookup"><span data-stu-id="f0d4b-120">(They both need to accept a single integer as a parameter.)</span></span>

<span data-ttu-id="f0d4b-121">Hay dos enfoques para este problema. Uno consiste en proporcionar nombres diferentes a los métodos,</span><span class="sxs-lookup"><span data-stu-id="f0d4b-121">There are two approaches to this problem, one is to give the methods different names.</span></span> <span data-ttu-id="f0d4b-122">que es lo que hizo el mecanismo de scaffolding en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-122">That's what the scaffolding mechanism did in the preceding example.</span></span> <span data-ttu-id="f0d4b-123">Pero esto implica un pequeño problema: ASP.NET asigna segmentos de una dirección URL a los métodos de acción por nombre y, si cambia el nombre de un método, normalmente el enrutamiento no podría encontrar ese método.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-123">However, this introduces a small problem: ASP.NET maps segments of a URL to action methods by name, and if you rename a method, routing normally wouldn't be able to find that method.</span></span> <span data-ttu-id="f0d4b-124">La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-124">The solution is what you see in the example, which is to add the `ActionName("Delete")` attribute to the `DeleteConfirmed` method.</span></span> <span data-ttu-id="f0d4b-125">Ese atributo efectúa la asignación para el sistema de enrutamiento para que una dirección URL que incluya /Delete/ para una solicitud POST busque el método `DeleteConfirmed`.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-125">That attribute performs mapping for the routing system so that a URL that includes /Delete/ for a POST request will find the `DeleteConfirmed` method.</span></span>

<span data-ttu-id="f0d4b-126">Otra solución alternativa común para los métodos que tienen nombres y firmas idénticos consiste en cambiar la firma del método POST artificialmente para incluir un parámetro adicional (sin usar).</span><span class="sxs-lookup"><span data-stu-id="f0d4b-126">Another common work around for methods that have identical names and signatures is to artificially change the signature of the POST method to include an extra (unused) parameter.</span></span> <span data-ttu-id="f0d4b-127">Es lo que hicimos en una publicación anterior, cuando agregamos el parámetro `notUsed`.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-127">That's what we did in a previous post when we added the `notUsed` parameter.</span></span> <span data-ttu-id="f0d4b-128">Podría hacer lo mismo aquí para el método `[HttpPost] Delete`:</span><span class="sxs-lookup"><span data-stu-id="f0d4b-128">You could do the same thing here for the `[HttpPost] Delete` method:</span></span>

```csharp
// POST: Movies/Delete/6
[ValidateAntiForgeryToken]
public async Task<IActionResult> Delete(int id, bool notUsed)
```

### <a name="publish-to-azure"></a><span data-ttu-id="f0d4b-129">Publicar en Azure</span><span class="sxs-lookup"><span data-stu-id="f0d4b-129">Publish to Azure</span></span>

<span data-ttu-id="f0d4b-130">Vea [Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) para obtener instrucciones sobre cómo publicar esta aplicación en Azure con Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-130">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure using Visual Studio.</span></span>  <span data-ttu-id="f0d4b-131">También se puede publicar la aplicación desde la [línea de comandos](xref:tutorials/publish-to-azure-webapp-using-cli).</span><span class="sxs-lookup"><span data-stu-id="f0d4b-131">The app can also be published from the [command line](xref:tutorials/publish-to-azure-webapp-using-cli).</span></span>

<span data-ttu-id="f0d4b-132">Gracias por seguir esta introducción a ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-132">Thanks for completing this introduction to ASP.NET Core MVC.</span></span> <span data-ttu-id="f0d4b-133">Le agradeceremos todos los comentarios que quiera hacernos.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-133">We appreciate any comments you leave.</span></span> <span data-ttu-id="f0d4b-134">[Introducción a MVC y EF Core](xref:data/ef-mvc/intro) es un excelente seguimiento de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="f0d4b-134">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f0d4b-135">Anterior</span><span class="sxs-lookup"><span data-stu-id="f0d4b-135">Previous</span></span>](validation.md)
