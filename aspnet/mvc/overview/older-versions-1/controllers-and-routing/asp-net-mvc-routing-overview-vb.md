---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: Introducción al enrutamiento de ASP.NET MVC (VB) | Microsoft Docs
author: StephenWalther
description: En este tutorial, Stephen Walther explica cómo el marco de MVC de ASP.NET asigna las solicitudes del explorador a acciones del controlador.
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 7511bc98667ac3acfccf78ea74b1369cf47bdc3b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813396"
---
<a name="aspnet-mvc-routing-overview-vb"></a><span data-ttu-id="2fb6d-103">Introducción al enrutamiento de ASP.NET MVC (VB)</span><span class="sxs-lookup"><span data-stu-id="2fb6d-103">ASP.NET MVC Routing Overview (VB)</span></span>
====================
<span data-ttu-id="2fb6d-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2fb6d-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="2fb6d-105">En este tutorial, Stephen Walther explica cómo el marco de MVC de ASP.NET asigna las solicitudes del explorador a acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-105">In this tutorial, Stephen Walther shows how the ASP.NET MVC framework maps browser requests to controller actions.</span></span>


<span data-ttu-id="2fb6d-106">En este tutorial, se le presenta una característica importante de cada aplicación de ASP.NET MVC llama *enrutamiento de ASP.NET*.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-106">In this tutorial, you are introduced to an important feature of every ASP.NET MVC application called *ASP.NET Routing*.</span></span> <span data-ttu-id="2fb6d-107">El módulo de enrutamiento de ASP.NET es responsable de asignar las solicitudes entrantes a acciones del controlador MVC concreto.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-107">The ASP.NET Routing module is responsible for mapping incoming browser requests to particular MVC controller actions.</span></span> <span data-ttu-id="2fb6d-108">Al final de este tutorial, comprenderá cómo la tabla de enrutamiento estándar asigna las solicitudes a acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-108">By the end of this tutorial, you will understand how the standard route table maps requests to controller actions.</span></span>

## <a name="using-the-default-route-table"></a><span data-ttu-id="2fb6d-109">Uso de la tabla de ruta predeterminada</span><span class="sxs-lookup"><span data-stu-id="2fb6d-109">Using the Default Route Table</span></span>

<span data-ttu-id="2fb6d-110">Cuando se crea una nueva aplicación de ASP.NET MVC, la aplicación ya está configurada para usar el enrutamiento de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-110">When you create a new ASP.NET MVC application, the application is already configured to use ASP.NET Routing.</span></span> <span data-ttu-id="2fb6d-111">El enrutamiento de ASP.NET es el programa de instalación en dos lugares.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-111">ASP.NET Routing is setup in two places.</span></span>

<span data-ttu-id="2fb6d-112">En primer lugar, el enrutamiento de ASP.NET está habilitado en el archivo de configuración de la aplicación Web (archivo Web.config).</span><span class="sxs-lookup"><span data-stu-id="2fb6d-112">First, ASP.NET Routing is enabled in your application's Web configuration file (Web.config file).</span></span> <span data-ttu-id="2fb6d-113">Hay cuatro secciones en el archivo de configuración que son relevantes para el enrutamiento: la sección system.web.httpModules, la sección system.web.httpHandlers, la sección system.webserver.modules y la sección system.webserver.handlers.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-113">There are four sections in the configuration file that are relevant to routing: the system.web.httpModules section, the system.web.httpHandlers section, the system.webserver.modules section, and the system.webserver.handlers section.</span></span> <span data-ttu-id="2fb6d-114">Tenga cuidado de no eliminar estas secciones porque sin estas secciones enrutamiento dejarán de funcionar.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-114">Be careful not to delete these sections because without these sections routing will no longer work.</span></span>

<span data-ttu-id="2fb6d-115">En segundo lugar y más importante aún, se crea una tabla de rutas en el archivo Global.asax de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-115">Second, and more importantly, a route table is created in the application's Global.asax file.</span></span> <span data-ttu-id="2fb6d-116">El archivo Global.asax es un archivo especial que contiene los controladores de eventos para eventos de ciclo de vida de aplicación de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-116">The Global.asax file is a special file that contains event handlers for ASP.NET application lifecycle events.</span></span> <span data-ttu-id="2fb6d-117">Durante el evento de inicio de la aplicación se crea la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-117">The route table is created during the Application Start event.</span></span>

<span data-ttu-id="2fb6d-118">El archivo en el listado 1 contiene el archivo Global.asax de forma predeterminada para una aplicación ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-118">The file in Listing 1 contains the default Global.asax file for an ASP.NET MVC application.</span></span>

<span data-ttu-id="2fb6d-119">**Listado 1 - Global.asax.vb**</span><span class="sxs-lookup"><span data-stu-id="2fb6d-119">**Listing 1 - Global.asax.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

<span data-ttu-id="2fb6d-120">Cuando se inicia por primera vez una aplicación MVC, la aplicación\_se llama al método Start().</span><span class="sxs-lookup"><span data-stu-id="2fb6d-120">When an MVC application first starts, the Application\_Start() method is called.</span></span> <span data-ttu-id="2fb6d-121">Este método, a su vez, llama al método de RegisterRoutes().</span><span class="sxs-lookup"><span data-stu-id="2fb6d-121">This method, in turn, calls the RegisterRoutes() method.</span></span> <span data-ttu-id="2fb6d-122">El método RegisterRoutes() crea la tabla de rutas.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-122">The RegisterRoutes() method creates the route table.</span></span>

<span data-ttu-id="2fb6d-123">La tabla de enrutamiento predeterminada contiene una única ruta (denominada de forma predeterminada).</span><span class="sxs-lookup"><span data-stu-id="2fb6d-123">The default route table contains a single route (named Default).</span></span> <span data-ttu-id="2fb6d-124">La ruta predeterminada asigna el primer segmento de una dirección URL a un nombre de controlador, el segundo segmento de una dirección URL a una acción de controlador y el tercer segmento a un parámetro denominado **id**.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-124">The Default route maps the first segment of a URL to a controller name, the second segment of a URL to a controller action, and the third segment to a parameter named **id**.</span></span>

<span data-ttu-id="2fb6d-125">Imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador web:</span><span class="sxs-lookup"><span data-stu-id="2fb6d-125">Imagine that you enter the following URL into your web browser's address bar:</span></span>

<span data-ttu-id="2fb6d-126">/ Home/Index/3</span><span class="sxs-lookup"><span data-stu-id="2fb6d-126">/Home/Index/3</span></span>

<span data-ttu-id="2fb6d-127">La ruta predeterminada asigna esta dirección URL a los siguientes parámetros:</span><span class="sxs-lookup"><span data-stu-id="2fb6d-127">The Default route maps this URL to the following parameters:</span></span>

- <span data-ttu-id="2fb6d-128">controlador = Home</span><span class="sxs-lookup"><span data-stu-id="2fb6d-128">controller = Home</span></span>

- <span data-ttu-id="2fb6d-129">acción = índice</span><span class="sxs-lookup"><span data-stu-id="2fb6d-129">action = Index</span></span>

- <span data-ttu-id="2fb6d-130">Id. = 3</span><span class="sxs-lookup"><span data-stu-id="2fb6d-130">id = 3</span></span>

<span data-ttu-id="2fb6d-131">Cuando se solicita/Home de dirección URL/índice/3, se ejecuta el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="2fb6d-131">When you request the URL /Home/Index/3, the following code is executed:</span></span>

<span data-ttu-id="2fb6d-132">HomeController.Index(3)</span><span class="sxs-lookup"><span data-stu-id="2fb6d-132">HomeController.Index(3)</span></span>

<span data-ttu-id="2fb6d-133">La ruta predeterminada incluye valores predeterminados para los tres parámetros.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-133">The Default route includes defaults for all three parameters.</span></span> <span data-ttu-id="2fb6d-134">Si no se proporciona un controlador, a continuación, el parámetro de controlador tiene como valor predeterminado el valor **inicio**.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-134">If you don't supply a controller, then the controller parameter defaults to the value **Home**.</span></span> <span data-ttu-id="2fb6d-135">Si no se proporciona una acción, el parámetro de acción tiene como valor predeterminado el valor **índice**.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-135">If you don't supply an action, the action parameter defaults to the value **Index**.</span></span> <span data-ttu-id="2fb6d-136">Por último, si no proporciona un identificador, el parámetro de Id. predeterminado es una cadena vacía.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-136">Finally, if you don't supply an id, the id parameter defaults to an empty string.</span></span>

<span data-ttu-id="2fb6d-137">Echemos un vistazo a algunos ejemplos de cómo la ruta predeterminada asigna las direcciones URL a acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-137">Let's look at a few examples of how the Default route maps URLs to controller actions.</span></span> <span data-ttu-id="2fb6d-138">Imagine que escriba la dirección URL siguiente en la barra de direcciones del explorador:</span><span class="sxs-lookup"><span data-stu-id="2fb6d-138">Imagine that you enter the following URL into your browser address bar:</span></span>

<span data-ttu-id="2fb6d-139">/ Home</span><span class="sxs-lookup"><span data-stu-id="2fb6d-139">/Home</span></span>

<span data-ttu-id="2fb6d-140">Debido a los valores predeterminados de parámetros de ruta de forma predeterminada, al escribir esta dirección URL hará que el método de Index() de la clase HomeController en el listado 2 para llamarlo.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-140">Because of the Default route parameter defaults, entering this URL will cause the Index() method of the HomeController class in Listing 2 to be called.</span></span>

<span data-ttu-id="2fb6d-141">**Listado 2 - HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="2fb6d-141">**Listing 2 - HomeController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

<span data-ttu-id="2fb6d-142">En el listado 2, la clase HomeController incluye un método denominado Index() que acepta un solo parámetro denominado identificador. El/Home URL hace que el método Index() se llama con el valor Nothing como el valor del parámetro del identificador.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-142">In Listing 2, the HomeController class includes a method named Index() that accepts a single parameter named Id. The URL /Home causes the Index() method to be called with the value Nothing as the value of the Id parameter.</span></span>

<span data-ttu-id="2fb6d-143">Debido al modo en que el marco de MVC invoca las acciones de controlador, el/Home URL también coincide con el método Index() de la clase HomeController en el listado 3.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-143">Because of the way that the MVC framework invokes controller actions, the URL /Home also matches the Index() method of the HomeController class in Listing 3.</span></span>

<span data-ttu-id="2fb6d-144">**Listado 3 - HomeController.vb (acción del índice sin ningún parámetro)**</span><span class="sxs-lookup"><span data-stu-id="2fb6d-144">**Listing 3 - HomeController.vb (Index action with no parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

<span data-ttu-id="2fb6d-145">El método Index() en el listado 3 no acepta ningún parámetro.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-145">The Index() method in Listing 3 does not accept any parameters.</span></span> <span data-ttu-id="2fb6d-146">El/Home URL hará que este método Index() al que llamar.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-146">The URL /Home will cause this Index() method to be called.</span></span> <span data-ttu-id="2fb6d-147">/ Home de dirección URL/índice/3 también invoca este método (el Id. se omite).</span><span class="sxs-lookup"><span data-stu-id="2fb6d-147">The URL /Home/Index/3 also invokes this method (the Id is ignored).</span></span>

<span data-ttu-id="2fb6d-148">El/Home URL también coincide con el método Index() de la clase HomeController en el listado 4.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-148">The URL /Home also matches the Index() method of the HomeController class in Listing 4.</span></span>

<span data-ttu-id="2fb6d-149">**Listado 4 - HomeController.vb (acción del índice con el parámetro que acepta valores NULL)**</span><span class="sxs-lookup"><span data-stu-id="2fb6d-149">**Listing 4 - HomeController.vb (Index action with nullable parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

<span data-ttu-id="2fb6d-150">En el listado 4, el método Index() tiene un parámetro de entero.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-150">In Listing 4, the Index() method has one Integer parameter.</span></span> <span data-ttu-id="2fb6d-151">Dado que el parámetro es un parámetro que acepta valores null (puede tener el valor Nothing), se puede llamar el Index() sin provocar un error.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-151">Because the parameter is a nullable parameter (can have the value Nothing), the Index() can be called without raising an error.</span></span>

<span data-ttu-id="2fb6d-152">Por último, al invocar el método Index() en el listado 5 con la/Home URL provoca una excepción desde el parámetro Id *no* un parámetro que acepta valores NULL.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-152">Finally, invoking the Index() method in Listing 5 with the URL /Home causes an exception since the Id parameter *is not* a nullable parameter.</span></span> <span data-ttu-id="2fb6d-153">Si se intenta invocar el método Index() obtendrá el error mostrado en la figura 1.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-153">If you attempt to invoke the Index() method then you get the error displayed in Figure 1.</span></span>

<span data-ttu-id="2fb6d-154">**Listado 5 - HomeController.vb (acción del índice con el parámetro de Id.)**</span><span class="sxs-lookup"><span data-stu-id="2fb6d-154">**Listing 5 - HomeController.vb (Index action with Id parameter)**</span></span>

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


<span data-ttu-id="2fb6d-155">[![Invocar una acción de controlador que espera un valor de parámetro](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2fb6d-155">[![Invoking a controller action that expects a parameter value](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)</span></span>

<span data-ttu-id="2fb6d-156">**Figura 01**: invocar una acción de controlador que espera un valor de parámetro ([haga clic aquí para ver imagen en tamaño completo](asp-net-mvc-routing-overview-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="2fb6d-156">**Figure 01**: Invoking a controller action that expects a parameter value ([Click to view full-size image](asp-net-mvc-routing-overview-vb/_static/image2.png))</span></span>


<span data-ttu-id="2fb6d-157">/ Home de dirección URL/índice/3, por otro lado, funciona bien con la acción de controlador de índice en el listado 5.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-157">The URL /Home/Index/3, on the other hand, works just fine with the Index controller action in Listing 5.</span></span> <span data-ttu-id="2fb6d-158">La solicitud /Home/Index/3 hace que el método Index() se llamen con un parámetro de identificador que tiene el valor 3.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-158">The request /Home/Index/3 causes the Index() method to be called with an Id parameter that has the value 3.</span></span>

## <a name="summary"></a><span data-ttu-id="2fb6d-159">Resumen</span><span class="sxs-lookup"><span data-stu-id="2fb6d-159">Summary</span></span>

<span data-ttu-id="2fb6d-160">El objetivo de este tutorial era para proporcionarle una breve introducción al enrutamiento de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-160">The goal of this tutorial was to provide you with a brief introduction to ASP.NET Routing.</span></span> <span data-ttu-id="2fb6d-161">Se examina la tabla de rutas predeterminadas que se obtiene con una nueva aplicación MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-161">We examined the default route table that you get with a new ASP.NET MVC application.</span></span> <span data-ttu-id="2fb6d-162">Ha aprendido cómo la ruta predeterminada asigna las direcciones URL a acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="2fb6d-162">You learned how the default route maps URLs to controller actions.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2fb6d-163">[Anterior](creating-an-action-cs.md)
> [Siguiente](understanding-action-filters-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2fb6d-163">[Previous](creating-an-action-cs.md)
[Next](understanding-action-filters-vb.md)</span></span>
