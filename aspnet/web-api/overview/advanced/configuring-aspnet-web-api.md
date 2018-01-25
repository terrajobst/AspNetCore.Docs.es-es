---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "Configuración de ASP.NET Web API 2 | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f9b471fe2afdce278869a2e4d9b693a78030324b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="configuring-aspnet-web-api-2"></a><span data-ttu-id="befe6-102">Configuración de ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="befe6-102">Configuring ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="befe6-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="befe6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="befe6-104">En este tema se describe cómo configurar ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="befe6-104">This topic describes how to configure ASP.NET Web API.</span></span>

- [<span data-ttu-id="befe6-105">Opciones de configuración</span><span class="sxs-lookup"><span data-stu-id="befe6-105">Configuration Settings</span></span>](#settings)
- [<span data-ttu-id="befe6-106">Configuración de Web API con el hospedaje de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="befe6-106">Configuring Web API with ASP.NET Hosting</span></span>](#webhost)
- [<span data-ttu-id="befe6-107">Configuración de Web API con OWIN hospeda a sí mismo</span><span class="sxs-lookup"><span data-stu-id="befe6-107">Configuring Web API with OWIN Self-Hosting</span></span>](#selfhost)
- [<span data-ttu-id="befe6-108">Servicios de API Web global</span><span class="sxs-lookup"><span data-stu-id="befe6-108">Global Web API Services</span></span>](#services)
- [<span data-ttu-id="befe6-109">Configuración por controlador</span><span class="sxs-lookup"><span data-stu-id="befe6-109">Per-Controller Configuration</span></span>](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a><span data-ttu-id="befe6-110">Opciones de configuración</span><span class="sxs-lookup"><span data-stu-id="befe6-110">Configuration Settings</span></span>

<span data-ttu-id="befe6-111">Configuración de la configuración de Web API se define en el [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) clase.</span><span class="sxs-lookup"><span data-stu-id="befe6-111">Web API configuration setttings are defined in the [HttpConfiguration](https://msdn.microsoft.com/library/system.web.http.httpconfiguration.aspx) class.</span></span>

| <span data-ttu-id="befe6-112">Miembro</span><span class="sxs-lookup"><span data-stu-id="befe6-112">Member</span></span> | <span data-ttu-id="befe6-113">Descripción</span><span class="sxs-lookup"><span data-stu-id="befe6-113">Description</span></span> |
| --- | --- |
| <span data-ttu-id="befe6-114">**DependencyResolver**</span><span class="sxs-lookup"><span data-stu-id="befe6-114">**DependencyResolver**</span></span> | <span data-ttu-id="befe6-115">Permite la inserción de dependencias para los controladores.</span><span class="sxs-lookup"><span data-stu-id="befe6-115">Enables dependency injection for controllers.</span></span> <span data-ttu-id="befe6-116">Vea [mediante la resolución de dependencia de la API Web](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-116">See [Using the Web API Dependency Resolver](dependency-injection.md).</span></span> |
| <span data-ttu-id="befe6-117">**Filtros**</span><span class="sxs-lookup"><span data-stu-id="befe6-117">**Filters**</span></span> | <span data-ttu-id="befe6-118">Filtros de acción.</span><span class="sxs-lookup"><span data-stu-id="befe6-118">Action filters.</span></span> |
| <span data-ttu-id="befe6-119">**Formateadores**</span><span class="sxs-lookup"><span data-stu-id="befe6-119">**Formatters**</span></span> | <span data-ttu-id="befe6-120">[Formateadores de tipo de medio](../formats-and-model-binding/media-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-120">[Media-type formatters](../formats-and-model-binding/media-formatters.md).</span></span> |
| <span data-ttu-id="befe6-121">**IncludeErrorDetailPolicy**</span><span class="sxs-lookup"><span data-stu-id="befe6-121">**IncludeErrorDetailPolicy**</span></span> | <span data-ttu-id="befe6-122">Especifica si el servidor debe incluir detalles del error, como los mensajes de excepción y los seguimientos de pila, en mensajes de respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="befe6-122">Specifies whether the server should include error details, such as exception messages and stack traces, in HTTP response messages.</span></span> <span data-ttu-id="befe6-123">Vea [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span><span class="sxs-lookup"><span data-stu-id="befe6-123">See [IncludeErrorDetailPolicy](https://msdn.microsoft.com/library/system.web.http.includeerrordetailpolicy(v=vs.108)).</span></span> |
| <span data-ttu-id="befe6-124">**Initializer**</span><span class="sxs-lookup"><span data-stu-id="befe6-124">**Initializer**</span></span> | <span data-ttu-id="befe6-125">Una función que realiza la inicialización final de la **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="befe6-125">A function that performs final initialization of the **HttpConfiguration**.</span></span> |
| <span data-ttu-id="befe6-126">**MessageHandlers**</span><span class="sxs-lookup"><span data-stu-id="befe6-126">**MessageHandlers**</span></span> | <span data-ttu-id="befe6-127">[Controladores de mensajes HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-127">[HTTP message handlers](http-message-handlers.md).</span></span> |
| <span data-ttu-id="befe6-128">**ParameterBindingRules**</span><span class="sxs-lookup"><span data-stu-id="befe6-128">**ParameterBindingRules**</span></span> | <span data-ttu-id="befe6-129">Una colección de reglas para enlazar parámetros en acciones del controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-129">A collection of rules for binding parameters on controller actions.</span></span> |
| <span data-ttu-id="befe6-130">**Propiedades**</span><span class="sxs-lookup"><span data-stu-id="befe6-130">**Properties**</span></span> | <span data-ttu-id="befe6-131">Un contenedor de propiedades genéricas.</span><span class="sxs-lookup"><span data-stu-id="befe6-131">A generic property bag.</span></span> |
| <span data-ttu-id="befe6-132">**Rutas**</span><span class="sxs-lookup"><span data-stu-id="befe6-132">**Routes**</span></span> | <span data-ttu-id="befe6-133">La colección de rutas.</span><span class="sxs-lookup"><span data-stu-id="befe6-133">The collection of routes.</span></span> <span data-ttu-id="befe6-134">Vea [enrutar en ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-134">See [Routing in ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="befe6-135">**Servicios**</span><span class="sxs-lookup"><span data-stu-id="befe6-135">**Services**</span></span> | <span data-ttu-id="befe6-136">La colección de servicios.</span><span class="sxs-lookup"><span data-stu-id="befe6-136">The collection of services.</span></span> <span data-ttu-id="befe6-137">Vea [Services](#services).</span><span class="sxs-lookup"><span data-stu-id="befe6-137">See [Services](#services).</span></span> |


## <a name="prerequisites"></a><span data-ttu-id="befe6-138">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="befe6-138">Prerequisites</span></span>

<span data-ttu-id="befe6-139">[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition.</span><span class="sxs-lookup"><span data-stu-id="befe6-139">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition.</span></span>

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a><span data-ttu-id="befe6-140">Configuración de Web API con el hospedaje de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="befe6-140">Configuring Web API with ASP.NET Hosting</span></span>

<span data-ttu-id="befe6-141">En una aplicación ASP.NET, configurar la API Web mediante una llamada a [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) en el **aplicación\_iniciar** método.</span><span class="sxs-lookup"><span data-stu-id="befe6-141">In an ASP.NET application, configure Web API by calling [GlobalConfiguration.Configure](https://msdn.microsoft.com/library/system.web.http.globalconfiguration.configure.aspx) in the **Application\_Start** method.</span></span> <span data-ttu-id="befe6-142">El **configurar** método toma un delegado con un único parámetro de tipo **HttpConfiguration**.</span><span class="sxs-lookup"><span data-stu-id="befe6-142">The **Configure** method takes a delegate with a single parameter of type **HttpConfiguration**.</span></span> <span data-ttu-id="befe6-143">Realizar todas las de su configuración en el delegado.</span><span class="sxs-lookup"><span data-stu-id="befe6-143">Perform all of your configuation inside the delegate.</span></span>

<span data-ttu-id="befe6-144">Este es un ejemplo de cómo utilizar a un delegado anónimo:</span><span class="sxs-lookup"><span data-stu-id="befe6-144">Here is an example using an anonymous delegate:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="befe6-145">En Visual Studio de 2017, la plantilla de proyecto de "Aplicación Web de ASP.NET" configura automáticamente el código de configuración, si selecciona "API Web" en la **nuevo proyecto ASP.NET** cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="befe6-145">In Visual Studio 2017, the "ASP.NET Web Application" project template automatically sets up the configuration code, if you select "Web API" in the **New ASP.NET Project** dialog.</span></span>

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

<span data-ttu-id="befe6-146">La plantilla de proyecto crea un archivo denominado WebApiConfig.cs dentro de la aplicación\_carpeta de inicio.</span><span class="sxs-lookup"><span data-stu-id="befe6-146">The project template creates a file named WebApiConfig.cs inside the App\_Start folder.</span></span> <span data-ttu-id="befe6-147">Este archivo de código define al delegado que se debe colocar el código de configuración de API Web.</span><span class="sxs-lookup"><span data-stu-id="befe6-147">This code file defines the delegate where you should put your Web API configuration code.</span></span>

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

<span data-ttu-id="befe6-148">La plantilla de proyecto también agrega el código que llama al delegado de **aplicación\_iniciar**.</span><span class="sxs-lookup"><span data-stu-id="befe6-148">The project template also adds the code that calls the delegate from **Application\_Start**.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a><span data-ttu-id="befe6-149">Configuración de Web API con OWIN hospeda a sí mismo</span><span class="sxs-lookup"><span data-stu-id="befe6-149">Configuring Web API with OWIN Self-Hosting</span></span>

<span data-ttu-id="befe6-150">Si estás autohospedaje con OWIN, cree un nuevo **HttpConfiguration** instancia.</span><span class="sxs-lookup"><span data-stu-id="befe6-150">If you are self-hosting with OWIN, create a new **HttpConfiguration** instance.</span></span> <span data-ttu-id="befe6-151">Realizar la configuración de esta instancia y, a continuación, pasar la instancia a la **Owin.UseWebApi** método de extensión.</span><span class="sxs-lookup"><span data-stu-id="befe6-151">Perform any configuration on this instance, and then pass the instance to the **Owin.UseWebApi** extension method.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

<span data-ttu-id="befe6-152">El tutorial [OWIN de uso a Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) muestra todos los pasos.</span><span class="sxs-lookup"><span data-stu-id="befe6-152">The tutorial [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) shows the complete steps.</span></span>

<a id="services"></a>
## <a name="global-web-api-services"></a><span data-ttu-id="befe6-153">Servicios de API Web global</span><span class="sxs-lookup"><span data-stu-id="befe6-153">Global Web API Services</span></span>

<span data-ttu-id="befe6-154">El **HttpConfiguration.Services** colección contiene un conjunto de servicios globales que API Web se utiliza para realizar diversas tareas, como la negociación de selección y el contenido del controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-154">The **HttpConfiguration.Services** collection contains a set of global services that Web API uses to perform various tasks, such as controller selection and content negotiation.</span></span>

> [!NOTE]
> <span data-ttu-id="befe6-155">El **servicios** colección no es un mecanismo de uso general para la inyección de dependencia o detección de servicio.</span><span class="sxs-lookup"><span data-stu-id="befe6-155">The **Services** collection is not a general-purpose mechanism for service discovery or dependency injection.</span></span> <span data-ttu-id="befe6-156">Solo almacena los tipos de servicio que se sabe que el marco Web API.</span><span class="sxs-lookup"><span data-stu-id="befe6-156">It only stores service types that are known to the Web API framework.</span></span>


<span data-ttu-id="befe6-157">El **servicios** colección se inicializa con un conjunto predeterminado de servicios y puede proporcionar sus propias implementaciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="befe6-157">The **Services** collection is initialized with a default set of services, and you can provide your own custom implementations.</span></span> <span data-ttu-id="befe6-158">Algunos servicios admiten varias instancias, mientras que otros usuarios puedan tener sólo una instancia.</span><span class="sxs-lookup"><span data-stu-id="befe6-158">Some services support multiple instances, while others can have only one instance.</span></span> <span data-ttu-id="befe6-159">(Sin embargo, también puede proporcionar servicios en el nivel de controlador; vea [configuración por controlador](#percontrollerconfig).</span><span class="sxs-lookup"><span data-stu-id="befe6-159">(However, you can also provide services at the controller level; see [Per-Controller Configuration](#percontrollerconfig).</span></span>

<span data-ttu-id="befe6-160">Servicios de instancia única</span><span class="sxs-lookup"><span data-stu-id="befe6-160">Single-Instance Services</span></span>


| <span data-ttu-id="befe6-161">Servicio</span><span class="sxs-lookup"><span data-stu-id="befe6-161">Service</span></span> | <span data-ttu-id="befe6-162">Descripción</span><span class="sxs-lookup"><span data-stu-id="befe6-162">Description</span></span> |
| --- | --- |
| <span data-ttu-id="befe6-163">**IActionValueBinder**</span><span class="sxs-lookup"><span data-stu-id="befe6-163">**IActionValueBinder**</span></span> | <span data-ttu-id="befe6-164">Obtiene un enlace para un parámetro.</span><span class="sxs-lookup"><span data-stu-id="befe6-164">Gets a binding for a parameter.</span></span> |
| <span data-ttu-id="befe6-165">**IApiExplorer**</span><span class="sxs-lookup"><span data-stu-id="befe6-165">**IApiExplorer**</span></span> | <span data-ttu-id="befe6-166">Obtiene las descripciones de las API expuestas por la aplicación.</span><span class="sxs-lookup"><span data-stu-id="befe6-166">Gets descriptions of the APIs exposed by the application.</span></span> <span data-ttu-id="befe6-167">Vea [crear una página de Ayuda de una API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-167">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="befe6-168">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="befe6-168">**IAssembliesResolver**</span></span> | <span data-ttu-id="befe6-169">Obtiene una lista de los ensamblados de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="befe6-169">Gets a list of the assemblies for the application.</span></span> <span data-ttu-id="befe6-170">Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-170">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="befe6-171">**IBodyModelValidator**</span><span class="sxs-lookup"><span data-stu-id="befe6-171">**IBodyModelValidator**</span></span> | <span data-ttu-id="befe6-172">Valida un modelo que se lee desde el cuerpo de solicitud por un formateador de tipo de medio.</span><span class="sxs-lookup"><span data-stu-id="befe6-172">Validates a model that is read from the request body by a media-type formatter.</span></span> |
| <span data-ttu-id="befe6-173">**IContentNegotiator**</span><span class="sxs-lookup"><span data-stu-id="befe6-173">**IContentNegotiator**</span></span> | <span data-ttu-id="befe6-174">Realiza la negociación de contenido.</span><span class="sxs-lookup"><span data-stu-id="befe6-174">Performs content negotiation.</span></span> |
| <span data-ttu-id="befe6-175">**IDocumentationProvider**</span><span class="sxs-lookup"><span data-stu-id="befe6-175">**IDocumentationProvider**</span></span> | <span data-ttu-id="befe6-176">Proporciona documentación de API.</span><span class="sxs-lookup"><span data-stu-id="befe6-176">Provides documentation for APIs.</span></span> <span data-ttu-id="befe6-177">El valor predeterminado es **null**.</span><span class="sxs-lookup"><span data-stu-id="befe6-177">The default is **null**.</span></span> <span data-ttu-id="befe6-178">Vea [crear una página de Ayuda de una API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-178">See [Creating a Help Page for a Web API](../getting-started-with-aspnet-web-api/creating-api-help-pages.md).</span></span> |
| <span data-ttu-id="befe6-179">**IHostBufferPolicySelector**</span><span class="sxs-lookup"><span data-stu-id="befe6-179">**IHostBufferPolicySelector**</span></span> | <span data-ttu-id="befe6-180">Indica si el host debe almacenar en búfer cuerpos de entidad de mensaje HTTP.</span><span class="sxs-lookup"><span data-stu-id="befe6-180">Indicates whether the host should buffer HTTP message entity bodies.</span></span> |
| <span data-ttu-id="befe6-181">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="befe6-181">**IHttpActionInvoker**</span></span> | <span data-ttu-id="befe6-182">Se invoca una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-182">Invokes a controller action.</span></span> <span data-ttu-id="befe6-183">Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-183">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="befe6-184">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="befe6-184">**IHttpActionSelector**</span></span> | <span data-ttu-id="befe6-185">Selecciona una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-185">Selects a controller action.</span></span> <span data-ttu-id="befe6-186">Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-186">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="befe6-187">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="befe6-187">**IHttpControllerActivator**</span></span> | <span data-ttu-id="befe6-188">Activa un controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-188">Activates a controller.</span></span> <span data-ttu-id="befe6-189">Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-189">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="befe6-190">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="befe6-190">**IHttpControllerSelector**</span></span> | <span data-ttu-id="befe6-191">Selecciona un controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-191">Selects a controller.</span></span> <span data-ttu-id="befe6-192">Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-192">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="befe6-193">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="befe6-193">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="befe6-194">Proporciona una lista de los tipos de controlador de API Web en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="befe6-194">Provides a list of the Web API controller types in the application.</span></span> <span data-ttu-id="befe6-195">Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-195">See [Routing and Action Selection](../web-api-routing-and-actions/routing-and-action-selection.md).</span></span> |
| <span data-ttu-id="befe6-196">**ITraceManager**</span><span class="sxs-lookup"><span data-stu-id="befe6-196">**ITraceManager**</span></span> | <span data-ttu-id="befe6-197">Inicializa el marco de trabajo de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="befe6-197">Initializes the tracing framework.</span></span> <span data-ttu-id="befe6-198">Vea [seguimiento en ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-198">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="befe6-199">**ITraceWriter**</span><span class="sxs-lookup"><span data-stu-id="befe6-199">**ITraceWriter**</span></span> | <span data-ttu-id="befe6-200">Proporciona un escritor de seguimiento.</span><span class="sxs-lookup"><span data-stu-id="befe6-200">Provides a trace writer.</span></span> <span data-ttu-id="befe6-201">El valor predeterminado es un sistema de escritura de seguimiento "no-op".</span><span class="sxs-lookup"><span data-stu-id="befe6-201">The default is a "no-op" trace writer.</span></span> <span data-ttu-id="befe6-202">Vea [seguimiento en ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="befe6-202">See [Tracing in ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md).</span></span> |
| <span data-ttu-id="befe6-203">**IModelValidatorCache**</span><span class="sxs-lookup"><span data-stu-id="befe6-203">**IModelValidatorCache**</span></span> | <span data-ttu-id="befe6-204">Proporciona una caché de validadores de modelo.</span><span class="sxs-lookup"><span data-stu-id="befe6-204">Provides a cache of model validators.</span></span> |

<span data-ttu-id="befe6-205">Servicios de varias instancias</span><span class="sxs-lookup"><span data-stu-id="befe6-205">Multiple-Instance Services</span></span>


| <span data-ttu-id="befe6-206">Servicio</span><span class="sxs-lookup"><span data-stu-id="befe6-206">Service</span></span> | <span data-ttu-id="befe6-207">Descripción</span><span class="sxs-lookup"><span data-stu-id="befe6-207">Description</span></span> |
| --- | --- |
| <span data-ttu-id="befe6-208">**IFilterProvider**</span><span class="sxs-lookup"><span data-stu-id="befe6-208">**IFilterProvider**</span></span> | <span data-ttu-id="befe6-209">Devuelve una lista de filtros para una acción de controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-209">Returns a list of filters for a controller action.</span></span> |
| <span data-ttu-id="befe6-210">**ModelBinderProvider**</span><span class="sxs-lookup"><span data-stu-id="befe6-210">**ModelBinderProvider**</span></span> | <span data-ttu-id="befe6-211">Devuelve un enlazador de modelos para un tipo determinado.</span><span class="sxs-lookup"><span data-stu-id="befe6-211">Returns a model binder for a given type.</span></span> |
| <span data-ttu-id="befe6-212">**ModelMetadataProvider**</span><span class="sxs-lookup"><span data-stu-id="befe6-212">**ModelMetadataProvider**</span></span> | <span data-ttu-id="befe6-213">Proporciona los metadatos para un modelo.</span><span class="sxs-lookup"><span data-stu-id="befe6-213">Provides metadata for a model.</span></span> |
| <span data-ttu-id="befe6-214">**ModelValidatorProvider**</span><span class="sxs-lookup"><span data-stu-id="befe6-214">**ModelValidatorProvider**</span></span> | <span data-ttu-id="befe6-215">Proporciona un validador para un modelo.</span><span class="sxs-lookup"><span data-stu-id="befe6-215">Provides a validator for a model.</span></span> |
| <span data-ttu-id="befe6-216">**ValueProviderFactory**</span><span class="sxs-lookup"><span data-stu-id="befe6-216">**ValueProviderFactory**</span></span> | <span data-ttu-id="befe6-217">Crea un proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="befe6-217">Creates a value provider.</span></span> <span data-ttu-id="befe6-218">Para obtener más información, consulte el blog de Mike Stall [cómo crear un proveedor de valor personalizado en WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span><span class="sxs-lookup"><span data-stu-id="befe6-218">For more information, see Mike Stall's blog post [How to create a custom value provider in WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)</span></span> |<span data-ttu-id="befe6-219">.</span><span class="sxs-lookup"><span data-stu-id="befe6-219">.</span></span>

<span data-ttu-id="befe6-220">Para agregar una implementación personalizada a un servicio de varias instancias, llame **agregar** o **insertar** en el **servicios** colección:</span><span class="sxs-lookup"><span data-stu-id="befe6-220">To add a custom implementation to a multi-instance service, call **Add** or **Insert** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="befe6-221">Para reemplazar un servicio de instancia única con una implementación personalizada, llame **reemplazar** en el **servicios** colección:</span><span class="sxs-lookup"><span data-stu-id="befe6-221">To replace a single-instance service with a custom implementation, call **Replace** on the **Services** collection:</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a><span data-ttu-id="befe6-222">Configuración por controlador</span><span class="sxs-lookup"><span data-stu-id="befe6-222">Per-Controller Configuration</span></span>

<span data-ttu-id="befe6-223">Puede invalidar la configuración siguiente en una base por cada controlador:</span><span class="sxs-lookup"><span data-stu-id="befe6-223">You can override the following settings on a per-controller basis:</span></span>

- <span data-ttu-id="befe6-224">Formateadores de tipo de medio</span><span class="sxs-lookup"><span data-stu-id="befe6-224">Media-type formatters</span></span>
- <span data-ttu-id="befe6-225">Reglas de enlace de parámetro</span><span class="sxs-lookup"><span data-stu-id="befe6-225">Parameter binding rules</span></span>
- <span data-ttu-id="befe6-226">Servicios</span><span class="sxs-lookup"><span data-stu-id="befe6-226">Services</span></span>

<span data-ttu-id="befe6-227">Para ello, defina un atributo personalizado que implementa la **IControllerConfiguration** interfaz.</span><span class="sxs-lookup"><span data-stu-id="befe6-227">To do so, define a custom attribute that implements the **IControllerConfiguration** interface.</span></span> <span data-ttu-id="befe6-228">A continuación, aplique el atributo al controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-228">Then apply the attribute to the controller.</span></span>

<span data-ttu-id="befe6-229">En el ejemplo siguiente se reemplaza a los formateadores de tipo de medio predeterminado con un formateador personalizado.</span><span class="sxs-lookup"><span data-stu-id="befe6-229">The following example replaces the default media-type formatters with a custom formatter.</span></span>

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="befe6-230">El **IControllerConfiguration.Initialize** método toma dos parámetros:</span><span class="sxs-lookup"><span data-stu-id="befe6-230">The **IControllerConfiguration.Initialize** method takes two parameters:</span></span>

- <span data-ttu-id="befe6-231">Un **HttpControllerSettings** objeto</span><span class="sxs-lookup"><span data-stu-id="befe6-231">An **HttpControllerSettings** object</span></span>
- <span data-ttu-id="befe6-232">Un **HttpControllerDescriptor** objeto</span><span class="sxs-lookup"><span data-stu-id="befe6-232">An **HttpControllerDescriptor** object</span></span>

<span data-ttu-id="befe6-233">El **HttpControllerDescriptor** contiene una descripción del controlador, que se puede examinar con carácter informativo (por ejemplo, para distinguir entre los dos controladores).</span><span class="sxs-lookup"><span data-stu-id="befe6-233">The **HttpControllerDescriptor** contains a description of the controller, which you can examine for informational purposes (say, to distinguish between two controllers).</span></span>

<span data-ttu-id="befe6-234">Use la **HttpControllerSettings** objeto que se va a configurar el controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-234">Use the **HttpControllerSettings** object to configure the controller.</span></span> <span data-ttu-id="befe6-235">Este objeto contiene el subconjunto de los parámetros de configuración que se pueden invalidar en una base por cada controlador.</span><span class="sxs-lookup"><span data-stu-id="befe6-235">This object contains the subset of configuration parameters that you can override on a per-controller basis.</span></span> <span data-ttu-id="befe6-236">Cualquier configuración que no cambie de forma predeterminada global **HttpConfiguration** objeto.</span><span class="sxs-lookup"><span data-stu-id="befe6-236">Any settings that you don't change default to the global **HttpConfiguration** object.</span></span>
