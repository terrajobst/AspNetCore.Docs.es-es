---
title: "Migrar controladores HTTP y módulos ASP.NET Core middleware"
author: rick-anderson
description: 
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: f104c9116cfaa4a82ac88e4a83b4b6f172eb2aa1
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="migrating-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="1243f-102">Migrar controladores HTTP y módulos ASP.NET Core middleware</span><span class="sxs-lookup"><span data-stu-id="1243f-102">Migrating HTTP handlers and modules to ASP.NET Core middleware</span></span> 

<span data-ttu-id="1243f-103">Por [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="1243f-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="1243f-104">Este artículo muestra cómo migrar ASP.NET existente [módulos HTTP y controladores de system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) a ASP.NET Core [middleware](../fundamentals/middleware.md).</span><span class="sxs-lookup"><span data-stu-id="1243f-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](https://docs.microsoft.com/iis/configuration/system.webserver/) to ASP.NET Core [middleware](../fundamentals/middleware.md).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="1243f-105">Módulos y controladores revisan</span><span class="sxs-lookup"><span data-stu-id="1243f-105">Modules and handlers revisited</span></span>

<span data-ttu-id="1243f-106">Antes de proceder con middleware de ASP.NET Core, primero Resumamos cómo funcionan los controladores y módulos HTTP:</span><span class="sxs-lookup"><span data-stu-id="1243f-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![Controlador de módulos](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="1243f-108">**Los controladores son:**</span><span class="sxs-lookup"><span data-stu-id="1243f-108">**Handlers are:**</span></span>

   * <span data-ttu-id="1243f-109">Las clases que implementan [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="1243f-109">Classes that implement [IHttpHandler](https://docs.microsoft.com/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="1243f-110">Utilizado para controlar solicitudes con un nombre de archivo especificado o una extensión, como *informes*</span><span class="sxs-lookup"><span data-stu-id="1243f-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="1243f-111">[Configurar](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) en *Web.config*</span><span class="sxs-lookup"><span data-stu-id="1243f-111">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="1243f-112">**Los módulos son:**</span><span class="sxs-lookup"><span data-stu-id="1243f-112">**Modules are:**</span></span>

   * <span data-ttu-id="1243f-113">Las clases que implementan [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="1243f-113">Classes that implement [IHttpModule](https://docs.microsoft.com/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="1243f-114">Se invoca para cada solicitud</span><span class="sxs-lookup"><span data-stu-id="1243f-114">Invoked for every request</span></span>

   * <span data-ttu-id="1243f-115">Capaz de cortocircuito (detener el procesamiento de una solicitud)</span><span class="sxs-lookup"><span data-stu-id="1243f-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="1243f-116">Puede agregar a la respuesta HTTP, o crear sus propios</span><span class="sxs-lookup"><span data-stu-id="1243f-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="1243f-117">[Configurar](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) en *Web.config*</span><span class="sxs-lookup"><span data-stu-id="1243f-117">[Configured](https://docs.microsoft.com//iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="1243f-118">**El orden en el que los módulos de procesan las solicitudes entrantes viene determinado por:**</span><span class="sxs-lookup"><span data-stu-id="1243f-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="1243f-119">El [ciclo de vida de aplicación](https://msdn.microsoft.com/library/ms227673.aspx), que es un eventos serie desencadenados por ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etcetera. Cada módulo puede crear un controlador para uno o varios eventos.</span><span class="sxs-lookup"><span data-stu-id="1243f-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](https://docs.microsoft.com/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="1243f-120">Para el mismo evento, el orden en el que está configurados en *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="1243f-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="1243f-121">Además de los módulos, puede agregar controladores para los eventos de ciclo de vida a sus *Global.asax.cs* archivo.</span><span class="sxs-lookup"><span data-stu-id="1243f-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="1243f-122">Estos controladores se ejecutan después de los controladores en los módulos configurados.</span><span class="sxs-lookup"><span data-stu-id="1243f-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="1243f-123">De controladores y módulos middleware</span><span class="sxs-lookup"><span data-stu-id="1243f-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="1243f-124">**Middleware son más sencillas que controladores y módulos HTTP:**</span><span class="sxs-lookup"><span data-stu-id="1243f-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="1243f-125">Módulos, controladores, *Global.asax.cs*, *Web.config* (excepto para la configuración de IIS) y el ciclo de vida de la aplicación han desaparecido</span><span class="sxs-lookup"><span data-stu-id="1243f-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="1243f-126">Los roles de los módulos y los controladores se hayan realizado el middleware</span><span class="sxs-lookup"><span data-stu-id="1243f-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="1243f-127">Middleware se configuran mediante código en lugar de en *Web.config*</span><span class="sxs-lookup"><span data-stu-id="1243f-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="1243f-128">[Bifurcación de canalización](../fundamentals/middleware.md#middleware-run-map-use) le permite enviar las solicitudes de middleware específico, según no solo la dirección URL sino también en los encabezados de solicitud, las cadenas de consulta, etcetera.</span><span class="sxs-lookup"><span data-stu-id="1243f-128">[Pipeline branching](../fundamentals/middleware.md#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="1243f-129">**Middleware son muy similares a los módulos:**</span><span class="sxs-lookup"><span data-stu-id="1243f-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="1243f-130">Se invoca en la entidad de seguridad para todas las solicitudes</span><span class="sxs-lookup"><span data-stu-id="1243f-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="1243f-131">Capaz de una solicitud de cortocircuito por [no pasar la solicitud al siguiente middleware](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="1243f-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="1243f-132">Puede crear su propia respuesta HTTP</span><span class="sxs-lookup"><span data-stu-id="1243f-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="1243f-133">**El software intermedio y módulos se procesan en un orden diferente:**</span><span class="sxs-lookup"><span data-stu-id="1243f-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="1243f-134">Orden de middleware se basa en el orden en el que se insertan en la canalización de solicitudes, mientras que el orden de los módulos se basa principalmente en [ciclo de vida de aplicación](https://msdn.microsoft.com/library/ms227673.aspx) eventos</span><span class="sxs-lookup"><span data-stu-id="1243f-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="1243f-135">Orden de middleware para las respuestas es el inverso de la para las solicitudes, mientras que el orden de los módulos es el mismo para las solicitudes y respuestas</span><span class="sxs-lookup"><span data-stu-id="1243f-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="1243f-136">Vea [crear una canalización de middleware con IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="1243f-136">See [Creating a middleware pipeline with IApplicationBuilder](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![Software intermedio](http-modules/_static/middleware.png)

<span data-ttu-id="1243f-138">Tenga en cuenta cómo en la imagen anterior, el middleware de autenticación había cortocircuitado la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1243f-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="1243f-139">Migrar código del módulo middleware</span><span class="sxs-lookup"><span data-stu-id="1243f-139">Migrating module code to middleware</span></span>

<span data-ttu-id="1243f-140">Un módulo HTTP existente tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1243f-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="1243f-141">Como se muestra en el [Middleware](../fundamentals/middleware.md) página, un middleware de ASP.NET Core es una clase que expone un `Invoke` tomar método un `HttpContext` y devolver un `Task`.</span><span class="sxs-lookup"><span data-stu-id="1243f-141">As shown in the [Middleware](../fundamentals/middleware.md) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="1243f-142">El middleware de nuevo tendrá un aspecto similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1243f-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="1243f-143">La plantilla de middleware anterior se realizó desde la sección de [escribir middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span><span class="sxs-lookup"><span data-stu-id="1243f-143">The above middleware template was taken from the section on [writing middleware](../fundamentals/middleware.md#middleware-writing-middleware).</span></span>

<span data-ttu-id="1243f-144">El *MyMiddlewareExtensions* clase auxiliar resulta más fácil de configurar el middleware en su `Startup` clase.</span><span class="sxs-lookup"><span data-stu-id="1243f-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="1243f-145">El `UseMyMiddleware` método agrega la clase de middleware a la canalización de solicitudes.</span><span class="sxs-lookup"><span data-stu-id="1243f-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="1243f-146">Servicios requeridos por el middleware obtengan insertados en el constructor del middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="1243f-147">El módulo podría terminar una solicitud, por ejemplo, si el usuario no autorizado:</span><span class="sxs-lookup"><span data-stu-id="1243f-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="1243f-148">Un middleware encarga de ello llamando no `Invoke` en el siguiente middleware en la canalización.</span><span class="sxs-lookup"><span data-stu-id="1243f-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="1243f-149">Tenga en cuenta que esto no finaliza completamente la solicitud, porque middlewares anterior aún se invocará cuando la respuesta realiza su forma de volver a través de la canalización.</span><span class="sxs-lookup"><span data-stu-id="1243f-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="1243f-150">Cuando se migra la funcionalidad de su módulo el middleware de nuevo, es posible que el código no se compila porque la `HttpContext` clase ha cambiado significativamente en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1243f-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="1243f-151">[Más adelante en](#migrating-to-the-new-httpcontext), verá cómo migrar a la nueva HttpContext principales de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1243f-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="1243f-152">Migrar inserción de módulo en la canalización de solicitud</span><span class="sxs-lookup"><span data-stu-id="1243f-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="1243f-153">Módulos HTTP normalmente se agregan a la canalización de solicitud con *Web.config*:</span><span class="sxs-lookup"><span data-stu-id="1243f-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="1243f-154">Convertir esto por [agregar su middleware nueva](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) a la canalización de solicitud en su `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="1243f-154">Convert this by [adding your new middleware](../fundamentals/middleware.md#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="1243f-155">El lugar exacto de la canalización donde insertar el middleware nueva depende de los eventos que tratan como un módulo (`BeginRequest`, `EndRequest`, etc.) y su orden en la lista de módulos de *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="1243f-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="1243f-156">Como anteriormente indicó, no hay ningún ciclo de vida de la aplicación de ASP.NET Core y el orden en que se procesan las respuestas middleware distinto del orden usado por módulos.</span><span class="sxs-lookup"><span data-stu-id="1243f-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="1243f-157">Esto podría tomar una decisión de ordenación más complejo.</span><span class="sxs-lookup"><span data-stu-id="1243f-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="1243f-158">Si la ordenación se convierte en un problema, puede dividir el módulo de varios componentes de software intermedio que se pueden ordenar de forma independiente.</span><span class="sxs-lookup"><span data-stu-id="1243f-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="1243f-159">Migrar código del controlador de middleware</span><span class="sxs-lookup"><span data-stu-id="1243f-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="1243f-160">Un controlador HTTP es algo parecido a esto:</span><span class="sxs-lookup"><span data-stu-id="1243f-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="1243f-161">En el proyecto de ASP.NET Core, se traduciría en un middleware similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1243f-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="1243f-162">Este middleware es muy similar del middleware correspondiente a los módulos.</span><span class="sxs-lookup"><span data-stu-id="1243f-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="1243f-163">La única diferencia es que aquí no hay ninguna llamada a `_next.Invoke(context)`.</span><span class="sxs-lookup"><span data-stu-id="1243f-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="1243f-164">Que tiene sentido, porque el controlador no es el final de la canalización de solicitud, por lo que no habrá ningún siguiente middleware para invocar.</span><span class="sxs-lookup"><span data-stu-id="1243f-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="1243f-165">Migrar inserción de controlador en la canalización de solicitud</span><span class="sxs-lookup"><span data-stu-id="1243f-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="1243f-166">La configuración de un controlador HTTP se realiza *Web.config* y es algo parecido a esto:</span><span class="sxs-lookup"><span data-stu-id="1243f-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[Main](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="1243f-167">Puede convertir este agregando el middleware de controlador nuevo a la canalización de solicitud en su `Startup` (clase), similar a middleware convertido a partir de los módulos.</span><span class="sxs-lookup"><span data-stu-id="1243f-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="1243f-168">El problema con este enfoque es que enviaría todas las solicitudes para el middleware de controlador nuevo.</span><span class="sxs-lookup"><span data-stu-id="1243f-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="1243f-169">Sin embargo, sólo desea que las solicitudes con una extensión específica para alcanzar el middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="1243f-170">Esto le proporcionaría la misma funcionalidad que no tenga el controlador HTTP.</span><span class="sxs-lookup"><span data-stu-id="1243f-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="1243f-171">Una solución es crear una bifurcación de la canalización de solicitudes con una extensión determinada, mediante el `MapWhen` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="1243f-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="1243f-172">Para ello, en el mismo `Configure` método donde se agrega el middleware de otro:</span><span class="sxs-lookup"><span data-stu-id="1243f-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="1243f-173">`MapWhen`toma estos parámetros:</span><span class="sxs-lookup"><span data-stu-id="1243f-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="1243f-174">Una expresión lambda que toma el `HttpContext` y devuelve `true` si la solicitud debe pasar a la rama.</span><span class="sxs-lookup"><span data-stu-id="1243f-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="1243f-175">Esto significa que puede crear una bifurcación de solicitudes no solo en función de su extensión, sino también en los encabezados de solicitud, los parámetros de cadena de consulta, etcetera.</span><span class="sxs-lookup"><span data-stu-id="1243f-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="1243f-176">Una expresión lambda que toma un `IApplicationBuilder` y agrega el software intermedio para la bifurcación.</span><span class="sxs-lookup"><span data-stu-id="1243f-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="1243f-177">Esto significa que puede agregar middleware adicional a la bifurcación delante el middleware de controlador.</span><span class="sxs-lookup"><span data-stu-id="1243f-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="1243f-178">Software intermedio se agrega a la canalización antes de que se invocará la bifurcación en todas las solicitudes; la bifurcación no tendrá ningún impacto en ellos.</span><span class="sxs-lookup"><span data-stu-id="1243f-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="1243f-179">Opciones de middleware usando el patrón de opciones de carga</span><span class="sxs-lookup"><span data-stu-id="1243f-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="1243f-180">Algunos controladores y módulos tienen opciones de configuración que se almacenan en *Web.config*. Sin embargo, en ASP.NET Core un nuevo modelo de configuración se utiliza en lugar de *Web.config*.</span><span class="sxs-lookup"><span data-stu-id="1243f-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="1243f-181">El nuevo [sistema de configuración](xref:fundamentals/configuration/index) ofrece las siguientes opciones para resolver este problema:</span><span class="sxs-lookup"><span data-stu-id="1243f-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="1243f-182">Insertar directamente las opciones en el middleware, como se muestra en el [próxima sección](#loading-middleware-options-through-direct-injection).</span><span class="sxs-lookup"><span data-stu-id="1243f-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="1243f-183">Use la [patrón opciones](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="1243f-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1.  <span data-ttu-id="1243f-184">Cree una clase para contener las opciones de middleware, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1243f-184">Create a class to hold your middleware options, for example:</span></span>

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2.  <span data-ttu-id="1243f-185">Almacenar los valores de opción</span><span class="sxs-lookup"><span data-stu-id="1243f-185">Store the option values</span></span>

    <span data-ttu-id="1243f-186">El sistema de configuración le permite almacenar valores de opción en cualquier lugar que desee.</span><span class="sxs-lookup"><span data-stu-id="1243f-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="1243f-187">Sin embargo, los sitios más uso *appSettings.JSON que se*, por lo que le llevaremos a ese método:</span><span class="sxs-lookup"><span data-stu-id="1243f-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

    <span data-ttu-id="1243f-188">*MyMiddlewareOptionsSection* aquí es un nombre de sección.</span><span class="sxs-lookup"><span data-stu-id="1243f-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="1243f-189">No tiene que ser el mismo que el nombre de la clase de opciones.</span><span class="sxs-lookup"><span data-stu-id="1243f-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="1243f-190">Asociar a los valores de opción de la clase de opciones</span><span class="sxs-lookup"><span data-stu-id="1243f-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="1243f-191">El patrón de opciones usa framework de inyección de dependencia de ASP.NET Core para asociar el tipo de opciones (como `MyMiddlewareOptions`) con un `MyMiddlewareOptions` objeto que tiene las opciones reales.</span><span class="sxs-lookup"><span data-stu-id="1243f-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="1243f-192">Actualización de su `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="1243f-192">Update your `Startup` class:</span></span>

    1.  <span data-ttu-id="1243f-193">Si usas *appSettings.JSON que se*, agregar para el generador de configuración de la `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="1243f-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

    2.  <span data-ttu-id="1243f-194">Configurar el servicio de opciones:</span><span class="sxs-lookup"><span data-stu-id="1243f-194">Configure the options service:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    3.  <span data-ttu-id="1243f-195">Asocie las opciones de la clase de opciones:</span><span class="sxs-lookup"><span data-stu-id="1243f-195">Associate your options with your options class:</span></span>

      [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4.  <span data-ttu-id="1243f-196">Insertar las opciones en el constructor de middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="1243f-197">Esto es similar a insertar opciones en un controlador.</span><span class="sxs-lookup"><span data-stu-id="1243f-197">This is similar to injecting options into a controller.</span></span>

  [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

  <span data-ttu-id="1243f-198">El [UseMiddleware](#http-modules-usemiddleware) método de extensión que agrega el middleware para la `IApplicationBuilder` se encarga de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="1243f-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

  <span data-ttu-id="1243f-199">Esto no se limita a `IOptions` objetos.</span><span class="sxs-lookup"><span data-stu-id="1243f-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="1243f-200">Puede insertar cualquier otro objeto que requiere el middleware de esta manera.</span><span class="sxs-lookup"><span data-stu-id="1243f-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="1243f-201">Opciones de middleware a través de inyección directa de carga</span><span class="sxs-lookup"><span data-stu-id="1243f-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="1243f-202">El patrón de opciones tiene la ventaja de que crea un acoplamiento entre los valores de las opciones y sus consumidores flexible.</span><span class="sxs-lookup"><span data-stu-id="1243f-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="1243f-203">Una vez que haya asociado una clase de opciones con los valores de opciones real, cualquier otra clase puede obtener acceso a las opciones a través del marco de inyección de dependencia.</span><span class="sxs-lookup"><span data-stu-id="1243f-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="1243f-204">No es necesario pasar valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="1243f-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="1243f-205">Este modo se divide aunque si desea utilizar el mismo middleware dos veces, con diferentes opciones.</span><span class="sxs-lookup"><span data-stu-id="1243f-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="1243f-206">Por ejemplo un middleware de autorización utilizado en distintas ramas que permite a distintos roles.</span><span class="sxs-lookup"><span data-stu-id="1243f-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="1243f-207">No se puede asociar dos objetos diferentes opciones con la clase de uno opciones.</span><span class="sxs-lookup"><span data-stu-id="1243f-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="1243f-208">La solución consiste en obtener los objetos de opciones con los valores de opciones real en su `Startup` clase y las pasará directamente a cada instancia de su middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1.  <span data-ttu-id="1243f-209">Agregar una segunda clave para *appSettings.JSON que se*</span><span class="sxs-lookup"><span data-stu-id="1243f-209">Add a second key to *appsettings.json*</span></span>

    <span data-ttu-id="1243f-210">Para agregar un segundo conjunto de opciones para la *appSettings.JSON que se* de archivos, use una nueva clave para identificar de forma exclusiva:</span><span class="sxs-lookup"><span data-stu-id="1243f-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

    [!code-json[Main](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2.  <span data-ttu-id="1243f-211">Recuperar valores de las opciones y pasarlos a middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="1243f-212">El `Use...` método de extensión (que agrega el middleware a la canalización) es un punto lógico para pasar los valores de opción:</span><span class="sxs-lookup"><span data-stu-id="1243f-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

    [!code-csharp[Main](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

4.  <span data-ttu-id="1243f-213">Habilita el middleware tomar un parámetro de opciones.</span><span class="sxs-lookup"><span data-stu-id="1243f-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="1243f-214">Proporcionar una sobrecarga de la `Use...` método de extensión (que toma el parámetro options y lo pasa a `UseMiddleware`).</span><span class="sxs-lookup"><span data-stu-id="1243f-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="1243f-215">Cuando `UseMiddleware` se llama con parámetros, pasa los parámetros al constructor de middleware cuando crea una instancia del objeto de middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

    [!code-csharp[Main](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

    <span data-ttu-id="1243f-216">Tenga en cuenta cómo se ajusta el objeto de opciones en un `OptionsWrapper` objeto.</span><span class="sxs-lookup"><span data-stu-id="1243f-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="1243f-217">Esto implementa `IOptions`, tal y como se esperaba el constructor de middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="1243f-218">Migrar a la nueva HttpContext</span><span class="sxs-lookup"><span data-stu-id="1243f-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="1243f-219">Se ha visto anteriormente que el `Invoke` método en el middleware toma un parámetro de tipo `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="1243f-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="1243f-220">`HttpContext`ha cambiado significativamente en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1243f-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="1243f-221">Esta sección muestra cómo traducir las propiedades más utilizadas de [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) al nuevo `Microsoft.AspNetCore.Http.HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="1243f-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](https://docs.microsoft.com/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="1243f-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="1243f-222">HttpContext</span></span>

<span data-ttu-id="1243f-223">**HttpContext.Items** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="1243f-224">**Identificador único de la solicitud (ningún homólogo System.Web.HttpContext)**</span><span class="sxs-lookup"><span data-stu-id="1243f-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="1243f-225">Proporciona un identificador único para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="1243f-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="1243f-226">Resulta muy útil para incluir en los registros.</span><span class="sxs-lookup"><span data-stu-id="1243f-226">Very useful to include in your logs.</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="1243f-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="1243f-227">HttpContext.Request</span></span>

<span data-ttu-id="1243f-228">**HttpContext.Request.HttpMethod** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="1243f-229">**HttpContext.Request.QueryString** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="1243f-230">**HttpContext.Request.Url** y **HttpContext.Request.RawUrl** traducir al:</span><span class="sxs-lookup"><span data-stu-id="1243f-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="1243f-231">**HttpContext.Request.IsSecureConnection** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="1243f-232">**HttpContext.Request.UserHostAddress** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="1243f-233">**HttpContext.Request.Cookies** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="1243f-234">**HttpContext.Request.RequestContext.RouteData** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="1243f-235">**HttpContext.Request.Headers** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="1243f-236">**HttpContext.Request.UserAgent** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="1243f-237">**HttpContext.Request.UrlReferrer** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="1243f-238">**HttpContext.Request.ContentType** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="1243f-239">**HttpContext.Request.Form** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="1243f-240">Leer valores de formulario sólo si el tipo de contenido sub es *x--www-form-urlencoded* o *datos del formulario*.</span><span class="sxs-lookup"><span data-stu-id="1243f-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="1243f-241">**HttpContext.Request.InputStream** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="1243f-242">Use este código solo en un middleware de tipo de controlador, al final de una canalización.</span><span class="sxs-lookup"><span data-stu-id="1243f-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="1243f-243">Puede leer el cuerpo sin formato, como se indicó anteriormente una sola vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="1243f-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="1243f-244">Middleware de intentar leer el cuerpo después de la primera lectura leerá un cuerpo vacío.</span><span class="sxs-lookup"><span data-stu-id="1243f-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="1243f-245">Esto no se aplica a un formulario de lectura como se muestra anteriormente, ya proviene de un búfer.</span><span class="sxs-lookup"><span data-stu-id="1243f-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="1243f-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="1243f-246">HttpContext.Response</span></span>

<span data-ttu-id="1243f-247">**HttpContext.Response.Status** y **HttpContext.Response.StatusDescription** traducir al:</span><span class="sxs-lookup"><span data-stu-id="1243f-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="1243f-248">**HttpContext.Response.ContentEncoding** y **HttpContext.Response.ContentType** traducir al:</span><span class="sxs-lookup"><span data-stu-id="1243f-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="1243f-249">**HttpContext.Response.ContentType** en su propio también se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="1243f-250">**HttpContext.Response.Output** se convierte en:</span><span class="sxs-lookup"><span data-stu-id="1243f-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="1243f-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="1243f-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="1243f-252">Servir el archivo se analiza [aquí](../fundamentals/request-features.md#middleware-and-request-features).</span><span class="sxs-lookup"><span data-stu-id="1243f-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="1243f-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="1243f-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="1243f-254">Enviar encabezados de respuesta se ve complicado por el hecho de que si se establece después de que algo se ha escrito en el cuerpo de respuesta, no se enviará.</span><span class="sxs-lookup"><span data-stu-id="1243f-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="1243f-255">La solución consiste en establecer un método de devolución de llamada que se llamará derecha antes de escribir en la respuesta se inicia.</span><span class="sxs-lookup"><span data-stu-id="1243f-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="1243f-256">Es mejor hacerlo al principio de la `Invoke` método en el middleware.</span><span class="sxs-lookup"><span data-stu-id="1243f-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="1243f-257">Es este método de devolución de llamada que establece los encabezados de respuesta.</span><span class="sxs-lookup"><span data-stu-id="1243f-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="1243f-258">El código siguiente define un método de devolución de llamada llama `SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="1243f-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="1243f-259">El `SetHeaders` método de devolución de llamada sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1243f-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="1243f-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="1243f-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="1243f-261">Las cookies de viaje en el explorador en un *Set-Cookie* encabezado de respuesta.</span><span class="sxs-lookup"><span data-stu-id="1243f-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="1243f-262">Como resultado, al enviar cookies, requiere la misma devolución de llamada que se usan para enviar los encabezados de respuesta:</span><span class="sxs-lookup"><span data-stu-id="1243f-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="1243f-263">El `SetCookies` método de devolución de llamada sería similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="1243f-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[Main](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="1243f-264">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1243f-264">Additional resources</span></span>

* [<span data-ttu-id="1243f-265">Información general de los módulos HTTP y controladores HTTP</span><span class="sxs-lookup"><span data-stu-id="1243f-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="1243f-266">Configuración</span><span class="sxs-lookup"><span data-stu-id="1243f-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="1243f-267">Inicio de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="1243f-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="1243f-268">Middleware</span><span class="sxs-lookup"><span data-stu-id="1243f-268">Middleware</span></span>](xref:fundamentals/middleware)
