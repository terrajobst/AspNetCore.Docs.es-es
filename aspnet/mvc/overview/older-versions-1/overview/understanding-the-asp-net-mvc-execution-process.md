---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Descripción del proceso de ejecución de ASP.NET MVC | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo el marco de MVC de ASP.NET procesa una solicitud de explorador paso a paso.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 3a3bcf4b78e3b19fb4d293f67b68c3a790d05221
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838147"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a><span data-ttu-id="68ade-103">Descripción del proceso de ejecución de ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="68ade-103">Understanding the ASP.NET MVC Execution Process</span></span>
====================
<span data-ttu-id="68ade-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="68ade-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="68ade-105">Obtenga información sobre cómo el marco de MVC de ASP.NET procesa una solicitud de explorador paso a paso.</span><span class="sxs-lookup"><span data-stu-id="68ade-105">Learn how the ASP.NET MVC framework processes a browser request step-by-step.</span></span>


<span data-ttu-id="68ade-106">Las solicitudes a una aplicación Web basada en ASP.NET MVC que primero pasan por la **UrlRoutingModule** objeto, que es un módulo HTTP.</span><span class="sxs-lookup"><span data-stu-id="68ade-106">Requests to an ASP.NET MVC-based Web application first pass through the **UrlRoutingModule** object, which is an HTTP module.</span></span> <span data-ttu-id="68ade-107">Este módulo analiza la solicitud y realiza la selección de la ruta.</span><span class="sxs-lookup"><span data-stu-id="68ade-107">This module parses the request and performs route selection.</span></span> <span data-ttu-id="68ade-108">El **UrlRoutingModule** objeto selecciona el primer objeto de ruta que coincida con la solicitud actual.</span><span class="sxs-lookup"><span data-stu-id="68ade-108">The **UrlRoutingModule** object selects the first route object that matches the current request.</span></span> <span data-ttu-id="68ade-109">(Un objeto de ruta es una clase que implementa **RouteBase**, y normalmente es una instancia de la **ruta** clase.) Si ninguna ruta coincide, el **UrlRoutingModule** objeto no hace nada y permite que la solicitud recurrir a la solicitud ASP.NET o IIS normal al procesamiento.</span><span class="sxs-lookup"><span data-stu-id="68ade-109">(A route object is a class that implements **RouteBase**, and is typically an instance of the **Route** class.) If no routes match, the **UrlRoutingModule** object does nothing and lets the request fall back to the regular ASP.NET or IIS request processing.</span></span>

<span data-ttu-id="68ade-110">Desde seleccionado **ruta** objeto, el **UrlRoutingModule** objeto obtiene la **IRouteHandler** objeto que está asociado el **ruta**objeto.</span><span class="sxs-lookup"><span data-stu-id="68ade-110">From the selected **Route** object, the **UrlRoutingModule** object obtains the **IRouteHandler** object that is associated with the **Route** object.</span></span> <span data-ttu-id="68ade-111">Normalmente, en una aplicación MVC, esto será una instancia de **MvcRouteHandler**.</span><span class="sxs-lookup"><span data-stu-id="68ade-111">Typically, in an MVC application, this will be an instance of **MvcRouteHandler**.</span></span> <span data-ttu-id="68ade-112">El **IRouteHandler** instancia crea un **IHttpHandler** objeto y lo pasa la **IHttpContext** objeto.</span><span class="sxs-lookup"><span data-stu-id="68ade-112">The **IRouteHandler** instance creates an **IHttpHandler** object and passes it the **IHttpContext** object.</span></span> <span data-ttu-id="68ade-113">De forma predeterminada, el **IHttpHandler** instancia para MVC es el **MvcHandler** objeto.</span><span class="sxs-lookup"><span data-stu-id="68ade-113">By default, the **IHttpHandler** instance for MVC is the **MvcHandler** object.</span></span> <span data-ttu-id="68ade-114">El **MvcHandler** objeto, a continuación, selecciona el controlador que en última instancia controlará la solicitud.</span><span class="sxs-lookup"><span data-stu-id="68ade-114">The **MvcHandler** object then selects the controller that will ultimately handle the request.</span></span>

> [!NOTE]
> <span data-ttu-id="68ade-115">Cuando se ejecuta una aplicación Web de MVC de ASP.NET en IIS 7.0, extensión de nombre de archivo no es necesaria para los proyectos de MVC.</span><span class="sxs-lookup"><span data-stu-id="68ade-115">When an ASP.NET MVC Web application runs in IIS 7.0, no file name extension is required for MVC projects.</span></span> <span data-ttu-id="68ade-116">Sin embargo, en IIS 6.0, el controlador requiere que asigne la extensión de nombre de archivo .mvc a la DLL de ISAPI de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="68ade-116">However, in IIS 6.0, the handler requires that you map the .mvc file name extension to the ASP.NET ISAPI DLL.</span></span>


<span data-ttu-id="68ade-117">El módulo y el controlador son los puntos de entrada para el marco de MVC de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="68ade-117">The module and handler are the entry points to the ASP.NET MVC framework.</span></span> <span data-ttu-id="68ade-118">Se realizan las siguientes acciones:</span><span class="sxs-lookup"><span data-stu-id="68ade-118">They perform the following actions:</span></span>

- <span data-ttu-id="68ade-119">Seleccionar el controlador adecuado en una aplicación Web MVC.</span><span class="sxs-lookup"><span data-stu-id="68ade-119">Select the appropriate controller in an MVC Web application.</span></span>
- <span data-ttu-id="68ade-120">Obtener una instancia de un controlador específico.</span><span class="sxs-lookup"><span data-stu-id="68ade-120">Obtain a specific controller instance.</span></span>
- <span data-ttu-id="68ade-121">Llamar a del controlador **Execute** método.</span><span class="sxs-lookup"><span data-stu-id="68ade-121">Call the controller's **Execute** method.</span></span>

<span data-ttu-id="68ade-122">A continuación enumeran las fases de ejecución para un proyecto Web de MVC:</span><span class="sxs-lookup"><span data-stu-id="68ade-122">The following lists the stages of execution for an MVC Web project:</span></span>

- <span data-ttu-id="68ade-123">Recibir la primera solicitud para la aplicación</span><span class="sxs-lookup"><span data-stu-id="68ade-123">Receive first request for the application</span></span> 

    - <span data-ttu-id="68ade-124">En el archivo Global.asax, **ruta** se agregan objetos a la **RouteTable** objeto.</span><span class="sxs-lookup"><span data-stu-id="68ade-124">In the Global.asax file, **Route** objects are added to the **RouteTable** object.</span></span>
- <span data-ttu-id="68ade-125">Realizar el enrutamiento</span><span class="sxs-lookup"><span data-stu-id="68ade-125">Perform routing</span></span> 

    - <span data-ttu-id="68ade-126">El **UrlRoutingModule** módulo usa la primera coincidencia **ruta** objeto en el **RouteTable** colección para crear el **RouteData** objeto que usa entonces para crear un **RequestContext** (**IHttpContext**) objetos.</span><span class="sxs-lookup"><span data-stu-id="68ade-126">The **UrlRoutingModule** module uses the first matching **Route** object in the **RouteTable** collection to create the **RouteData** object, which it then uses to create a **RequestContext** (**IHttpContext**) object.</span></span>
- <span data-ttu-id="68ade-127">Crear el controlador de solicitudes de MVC</span><span class="sxs-lookup"><span data-stu-id="68ade-127">Create MVC request handler</span></span> 

    - <span data-ttu-id="68ade-128">El **MvcRouteHandler** objeto crea una instancia de la **MvcHandler** clase y le pasa el **RequestContext** instancia.</span><span class="sxs-lookup"><span data-stu-id="68ade-128">The **MvcRouteHandler** object creates an instance of the **MvcHandler** class and passes it the **RequestContext** instance.</span></span>
- <span data-ttu-id="68ade-129">Crear el controlador</span><span class="sxs-lookup"><span data-stu-id="68ade-129">Create controller</span></span> 

    - <span data-ttu-id="68ade-130">El **MvcHandler** de objeto usa la **RequestContext** instancia para identificar el **IControllerFactory** objeto (normalmente una instancia de la  **DefaultControllerFactory** clase) para crear la instancia del controlador.</span><span class="sxs-lookup"><span data-stu-id="68ade-130">The **MvcHandler** object uses the **RequestContext** instance to identify the **IControllerFactory** object (typically an instance of the **DefaultControllerFactory** class) to create the controller instance with.</span></span>
- <span data-ttu-id="68ade-131">Controlador Execute: el **MvcHandler** instancia invoca el controlador s **Execute** método.</span><span class="sxs-lookup"><span data-stu-id="68ade-131">Execute controller - The **MvcHandler** instance calls the controller s **Execute** method.</span></span> |
- <span data-ttu-id="68ade-132">Invocar acción</span><span class="sxs-lookup"><span data-stu-id="68ade-132">Invoke action</span></span> 

    - <span data-ttu-id="68ade-133">La mayoría de los controladores hereda de la **controlador** clase base.</span><span class="sxs-lookup"><span data-stu-id="68ade-133">Most controllers inherit from the **Controller** base class.</span></span> <span data-ttu-id="68ade-134">Para los controladores que lo haga, la **ControllerActionInvoker** objeto que está asociado con el controlador determina qué método de acción de la clase de controlador para llamar a y, a continuación, llama a ese método.</span><span class="sxs-lookup"><span data-stu-id="68ade-134">For controllers that do so, the **ControllerActionInvoker** object that is associated with the controller determines which action method of the controller class to call, and then calls that method.</span></span>
- <span data-ttu-id="68ade-135">Resultado de ejecución</span><span class="sxs-lookup"><span data-stu-id="68ade-135">Execute result</span></span> 

    - <span data-ttu-id="68ade-136">Un método de acción típico podría recibir entradas del usuario, preparar los datos de la respuesta adecuada y, a continuación, ejecute el resultado devolviendo un tipo de resultado.</span><span class="sxs-lookup"><span data-stu-id="68ade-136">A typical action method might receive user input, prepare the appropriate response data, and then execute the result by returning a result type.</span></span> <span data-ttu-id="68ade-137">Los tipos de resultado integrados que se pueden ejecutar incluyen los siguientes: **ViewResult** (que presenta una vista y es el tipo de resultado utilizado con mayor frecuencia), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, y **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="68ade-137">The built-in result types that can be executed include the following: **ViewResult** (which renders a view and is the most-often used result type), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**, and **EmptyResult**.</span></span>
