---
title: Migrar de ASP.NET Web API
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4f0564b4-ed4e-4e1e-9755-c1144d21a0ef
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 55125e711a8b04f5a363ba965ab2223da02aab78
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="migrating-from-aspnet-web-api"></a><span data-ttu-id="96719-103">Migrar de ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="96719-103">Migrating from ASP.NET Web API</span></span>

<span data-ttu-id="96719-104">Por [Steve Smith](http://ardalis.com) y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="96719-104">By [Steve Smith](http://ardalis.com) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="96719-105">Las API Web son servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="96719-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="96719-106">Núcleo de ASP.NET MVC incluye compatibilidad para la creación de las API Web proporcionan una manera única y coherente de la creación de aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="96719-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="96719-107">En este artículo, se muestran los pasos necesarios para migrar una implementación de la API Web de ASP.NET Web API para MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96719-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

[<span data-ttu-id="96719-108">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="96719-108">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="96719-109">Proyecto de revisión ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="96719-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="96719-110">Este artículo utiliza el proyecto de ejemplo, *ProductsApp*, creado en el artículo [Introducción a ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como punto de partida.</span><span class="sxs-lookup"><span data-stu-id="96719-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="96719-111">En el proyecto, un proyecto de ASP.NET Web API simple se configura como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="96719-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="96719-112">En *Global.asax.cs*, se realiza una llamada a `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="96719-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

<span data-ttu-id="96719-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="96719-113">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]</span></span>

<span data-ttu-id="96719-114">`WebApiConfig`se define en *App_Start*, y tiene solo un estático `Register` método:</span><span class="sxs-lookup"><span data-stu-id="96719-114">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

<span data-ttu-id="96719-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span><span class="sxs-lookup"><span data-stu-id="96719-115">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]</span></span>


<span data-ttu-id="96719-116">Esta clase configura [atributo enrutamiento](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aunque realmente no se usa en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="96719-116">This class configures [attribute routing](http://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="96719-117">También configura la tabla de enrutamiento que se usa por la API Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="96719-117">It also configures the routing table which is used by ASP.NET Web API.</span></span> <span data-ttu-id="96719-118">En este caso, ASP.NET Web API esperará direcciones URL para que coincida con el formato */api/ {controller} / {id}*, con *{id}* que es opcional.</span><span class="sxs-lookup"><span data-stu-id="96719-118">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="96719-119">El *ProductsApp* proyecto incluye un solo controlador simple, que hereda de `ApiController` y expone dos métodos:</span><span class="sxs-lookup"><span data-stu-id="96719-119">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

<span data-ttu-id="96719-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span><span class="sxs-lookup"><span data-stu-id="96719-120">[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]</span></span>

<span data-ttu-id="96719-121">Por último, el modelo, *producto*, utilizada por el *ProductsApp*, es una clase simple:</span><span class="sxs-lookup"><span data-stu-id="96719-121">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

<span data-ttu-id="96719-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span><span class="sxs-lookup"><span data-stu-id="96719-122">[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]</span></span>

<span data-ttu-id="96719-123">Ahora que tenemos un proyecto simple desde el que se va a iniciar, podemos demostrar cómo migrar este proyecto de API Web MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96719-123">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="96719-124">Crear el proyecto de destino</span><span class="sxs-lookup"><span data-stu-id="96719-124">Create the Destination Project</span></span>

<span data-ttu-id="96719-125">Con Visual Studio, cree una solución nueva y vacía y asígnele el nombre *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="96719-125">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="96719-126">Agregar existente *ProductsApp* proyecto al, a continuación, agregue un nuevo proyecto de aplicación de ASP.NET Core Web a la solución.</span><span class="sxs-lookup"><span data-stu-id="96719-126">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="96719-127">Asigne al nuevo proyecto *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="96719-127">Name the new project *ProductsCore*.</span></span>

![Abrir el cuadro de diálogo nuevo proyecto en las plantillas Web](webapi/_static/add-web-project.png)

<span data-ttu-id="96719-129">A continuación, elija la plantilla de proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="96719-129">Next, choose the Web API project template.</span></span> <span data-ttu-id="96719-130">Se migrará el *ProductsApp* contenido para este nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="96719-130">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Cuadro de diálogo nueva aplicación Web con la plantilla de proyecto de API Web seleccionado en la lista de plantillas de ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="96719-132">Eliminar el `Project_Readme.html` archivo desde el nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="96719-132">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="96719-133">La solución debe tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="96719-133">Your solution should now look like this:</span></span>

![Solución de aplicación abierta en el Explorador de soluciones con archivos y carpetas de los proyectos ProductsApp y ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="96719-135">Migrar la configuración</span><span class="sxs-lookup"><span data-stu-id="96719-135">Migrate Configuration</span></span>

<span data-ttu-id="96719-136">Ya no usa ASP.NET Core *Global.asax*, *web.config*, o *App_Start* carpetas.</span><span class="sxs-lookup"><span data-stu-id="96719-136">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="96719-137">En su lugar, se realizan todas las tareas de inicio en *Startup.cs* en la raíz del proyecto (vea [inicio de la aplicación](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="96719-137">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="96719-138">En ASP.NET MVC de núcleo, el enrutamiento basado en atributos ahora se incluye de forma predeterminada cuando `UseMvc()` se denomina; y, es el enfoque recomendado para configurar las rutas de Web API (y es la forma en que el proyecto de inicio de la API Web controla el enrutamiento).</span><span class="sxs-lookup"><span data-stu-id="96719-138">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

<span data-ttu-id="96719-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span><span class="sxs-lookup"><span data-stu-id="96719-139">[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]</span></span>

<span data-ttu-id="96719-140">Suponiendo que desea utilizar el enrutamiento de atributo en el proyecto en el futuro, se necesita ninguna configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="96719-140">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="96719-141">Basta con aplicar los atributos según sea necesario para los controladores y acciones, como se hace en el ejemplo `ValuesController` clase que se incluye en el proyecto de inicio de la API Web:</span><span class="sxs-lookup"><span data-stu-id="96719-141">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that is included in the Web API starter project:</span></span>

<span data-ttu-id="96719-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span><span class="sxs-lookup"><span data-stu-id="96719-142">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]</span></span>

<span data-ttu-id="96719-143">Tenga en cuenta la presencia de *[controlador]* en la línea 8.</span><span class="sxs-lookup"><span data-stu-id="96719-143">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="96719-144">Enrutamiento basado en atributos ahora es compatible con determinados símbolos (tokens), como *[controlador]* y *[acción]*.</span><span class="sxs-lookup"><span data-stu-id="96719-144">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="96719-145">Estos tokens se reemplazan en tiempo de ejecución con el nombre del controlador o acción, respectivamente, al que se ha aplicado el atributo.</span><span class="sxs-lookup"><span data-stu-id="96719-145">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="96719-146">Esto sirve para reducir el número de cadenas mágicos en el proyecto y garantiza que las rutas se mantendrá sincronizadas con sus controladores correspondientes y las acciones cuando se aplican refactorizaciones rename automática.</span><span class="sxs-lookup"><span data-stu-id="96719-146">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="96719-147">Para migrar el controlador de API de productos, debemos primero copiaremos *ProductsController* al nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="96719-147">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="96719-148">A continuación, basta con incluir el atributo de ruta en el controlador:</span><span class="sxs-lookup"><span data-stu-id="96719-148">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="96719-149">También debe agregar el `[HttpGet]` atribuir a los dos métodos, ya que se deberían llamar a través de HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="96719-149">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="96719-150">Incluir la expectativa de un parámetro "id" en el atributo para `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="96719-150">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="96719-151">En este momento, el enrutamiento está configurado correctamente; Sin embargo, aún no podemos probarlo.</span><span class="sxs-lookup"><span data-stu-id="96719-151">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="96719-152">Se deben realizar cambios adicionales antes de *ProductsController* se compilará.</span><span class="sxs-lookup"><span data-stu-id="96719-152">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="96719-153">Migrar los modelos y controladores</span><span class="sxs-lookup"><span data-stu-id="96719-153">Migrate Models and Controllers</span></span>

<span data-ttu-id="96719-154">El último paso del proceso de migración para este proyecto de API Web simple es copiar a través de los controladores y los modelos que se usan.</span><span class="sxs-lookup"><span data-stu-id="96719-154">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="96719-155">En este caso, basta con copiar *Controllers/ProductsController.cs* desde el proyecto original al nuevo.</span><span class="sxs-lookup"><span data-stu-id="96719-155">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="96719-156">A continuación, copie toda la carpeta modelos desde el proyecto original al nuevo.</span><span class="sxs-lookup"><span data-stu-id="96719-156">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="96719-157">Ajustar los espacios de nombres para que coincida con el nuevo nombre de proyecto (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="96719-157">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="96719-158">En este momento, puede compilar la aplicación y encontrará un número de errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="96719-158">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="96719-159">Por lo general, estos deben estar en las siguientes categorías:</span><span class="sxs-lookup"><span data-stu-id="96719-159">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="96719-160">*ApiController* no existe</span><span class="sxs-lookup"><span data-stu-id="96719-160">*ApiController* does not exist</span></span>

* <span data-ttu-id="96719-161">*System.Web.Http* no existe el espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="96719-161">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="96719-162">*IHttpActionResult* no existe</span><span class="sxs-lookup"><span data-stu-id="96719-162">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="96719-163">Afortunadamente, estos son muy fáciles corregir:</span><span class="sxs-lookup"><span data-stu-id="96719-163">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="96719-164">Cambio *ApiController* a *controlador* (puede que necesite agregar *con Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="96719-164">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="96719-165">Elimine cualquiera con la instrucción que hace referencia a *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="96719-165">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="96719-166">Cambiar cualquier método devolver *IHttpActionResult* para devolver un *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="96719-166">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="96719-167">Una vez que estos cambios se han realizado y que no use instrucciones using quita, el migrados *ProductsController* clase tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="96719-167">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

<span data-ttu-id="96719-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span><span class="sxs-lookup"><span data-stu-id="96719-168">[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]</span></span>

<span data-ttu-id="96719-169">Ahora puede ejecutar el proyecto migrado y vaya a */api/productos*; y, debería ver la lista completa de los 3 productos.</span><span class="sxs-lookup"><span data-stu-id="96719-169">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="96719-170">Vaya a */api/products/1* y debería ver la primera el producto.</span><span class="sxs-lookup"><span data-stu-id="96719-170">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="summary"></a><span data-ttu-id="96719-171">Resumen</span><span class="sxs-lookup"><span data-stu-id="96719-171">Summary</span></span>

<span data-ttu-id="96719-172">Migrar un proyecto de ASP.NET Web API simple para MVC de ASP.NET Core es bastante sencilla, gracias a la compatibilidad integrada con las API Web de MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96719-172">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="96719-173">La principal que deben migrar todos los proyectos de ASP.NET Web API son rutas, los controladores y los modelos, junto con las actualizaciones de los tipos utilizados por los controladores y acciones.</span><span class="sxs-lookup"><span data-stu-id="96719-173">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
