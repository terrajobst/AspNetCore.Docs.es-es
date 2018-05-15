---
title: Migrar de API Web de ASP.NET a ASP.NET Core
author: ardalis
description: Obtenga información acerca de cómo migrar una implementación de la API Web de ASP.NET Web API para MVC de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 8d842877e49e317323d453e71ebb3302245f388d
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2018
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="1e694-103">Migrar de API Web de ASP.NET a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1e694-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="1e694-104">Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="1e694-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="1e694-105">Las API Web son servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="1e694-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="1e694-106">Núcleo de ASP.NET MVC incluye compatibilidad para la creación de las API Web proporcionan una manera única y coherente de la creación de aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="1e694-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="1e694-107">En este artículo, se muestran los pasos necesarios para migrar una implementación de la API Web de ASP.NET Web API para MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e694-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="1e694-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1e694-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="1e694-109">Proyecto de revisión ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1e694-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="1e694-110">Este artículo utiliza el proyecto de ejemplo, *ProductsApp*, creado en el artículo [Introducción a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) como punto de partida.</span><span class="sxs-lookup"><span data-stu-id="1e694-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="1e694-111">En el proyecto, un proyecto de ASP.NET Web API simple se configura como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="1e694-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="1e694-112">En *Global.asax.cs*, se realiza una llamada a `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="1e694-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="1e694-113">`WebApiConfig` se define en *App_Start*, y tiene solo un estático `Register` método:</span><span class="sxs-lookup"><span data-stu-id="1e694-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="1e694-114">Esta clase configura [atributo enrutamiento](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aunque realmente no se usa en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e694-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="1e694-115">También configura la tabla de enrutamiento, que se usa por la API Web de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1e694-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="1e694-116">En este caso, ASP.NET Web API esperará direcciones URL para que coincida con el formato */api/ {controller} / {id}*, con *{id}* que es opcional.</span><span class="sxs-lookup"><span data-stu-id="1e694-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="1e694-117">El *ProductsApp* proyecto incluye un solo controlador simple, que hereda de `ApiController` y expone dos métodos:</span><span class="sxs-lookup"><span data-stu-id="1e694-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="1e694-118">Por último, el modelo, *producto*, utilizada por el *ProductsApp*, es una clase simple:</span><span class="sxs-lookup"><span data-stu-id="1e694-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="1e694-119">Ahora que tenemos un proyecto simple desde el que se va a iniciar, podemos demostrar cómo migrar este proyecto de API Web MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e694-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="1e694-120">Crear el proyecto de destino</span><span class="sxs-lookup"><span data-stu-id="1e694-120">Create the Destination Project</span></span>

<span data-ttu-id="1e694-121">Con Visual Studio, cree una solución nueva y vacía y asígnele el nombre *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="1e694-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="1e694-122">Agregar existente *ProductsApp* proyecto al, a continuación, agregue un nuevo proyecto de aplicación de ASP.NET Core Web a la solución.</span><span class="sxs-lookup"><span data-stu-id="1e694-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="1e694-123">Asigne al nuevo proyecto *ProductsCore*.</span><span class="sxs-lookup"><span data-stu-id="1e694-123">Name the new project *ProductsCore*.</span></span>

![Abrir el cuadro de diálogo nuevo proyecto en las plantillas Web](webapi/_static/add-web-project.png)

<span data-ttu-id="1e694-125">A continuación, elija la plantilla de proyecto de API Web.</span><span class="sxs-lookup"><span data-stu-id="1e694-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="1e694-126">Se migrará el *ProductsApp* contenido para este nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e694-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![Cuadro de diálogo nueva aplicación Web con la plantilla de proyecto de API Web seleccionado en la lista de plantillas de ASP.NET Core](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="1e694-128">Eliminar el `Project_Readme.html` archivo desde el nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e694-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="1e694-129">La solución debe tener el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="1e694-129">Your solution should now look like this:</span></span>

![Solución de aplicación abierta en el Explorador de soluciones con archivos y carpetas de los proyectos ProductsApp y ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="1e694-131">Migrar la configuración</span><span class="sxs-lookup"><span data-stu-id="1e694-131">Migrate Configuration</span></span>

<span data-ttu-id="1e694-132">Ya no usa ASP.NET Core *Global.asax*, *web.config*, o *App_Start* carpetas.</span><span class="sxs-lookup"><span data-stu-id="1e694-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="1e694-133">En su lugar, se realizan todas las tareas de inicio en *Startup.cs* en la raíz del proyecto (vea [inicio de la aplicación](../fundamentals/startup.md)).</span><span class="sxs-lookup"><span data-stu-id="1e694-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="1e694-134">En ASP.NET MVC de núcleo, el enrutamiento basado en atributos ahora se incluye de forma predeterminada cuando `UseMvc()` se denomina; y, es el enfoque recomendado para configurar las rutas de Web API (y es la forma en que el proyecto de inicio de la API Web controla el enrutamiento).</span><span class="sxs-lookup"><span data-stu-id="1e694-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="1e694-135">Suponiendo que desea utilizar el enrutamiento de atributo en el proyecto en el futuro, se necesita ninguna configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="1e694-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="1e694-136">Basta con aplicar los atributos según sea necesario para los controladores y acciones, como se hace en el ejemplo `ValuesController` clase que se incluye en el proyecto de inicio de la API Web:</span><span class="sxs-lookup"><span data-stu-id="1e694-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="1e694-137">Tenga en cuenta la presencia de *[controlador]* en la línea 8.</span><span class="sxs-lookup"><span data-stu-id="1e694-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="1e694-138">Enrutamiento basado en atributos ahora es compatible con determinados símbolos (tokens), como *[controlador]* y *[acción]*.</span><span class="sxs-lookup"><span data-stu-id="1e694-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="1e694-139">Estos tokens se reemplazan en tiempo de ejecución con el nombre del controlador o acción, respectivamente, al que se ha aplicado el atributo.</span><span class="sxs-lookup"><span data-stu-id="1e694-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="1e694-140">Esto sirve para reducir el número de cadenas mágicos en el proyecto y garantiza que las rutas se mantendrá sincronizadas con sus controladores correspondientes y las acciones cuando se aplican refactorizaciones rename automática.</span><span class="sxs-lookup"><span data-stu-id="1e694-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="1e694-141">Para migrar el controlador de API de productos, debemos primero copiaremos *ProductsController* al nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="1e694-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="1e694-142">A continuación, basta con incluir el atributo de ruta en el controlador:</span><span class="sxs-lookup"><span data-stu-id="1e694-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="1e694-143">También debe agregar el `[HttpGet]` atribuir a los dos métodos, ya que se deberían llamar a través de HTTP Get.</span><span class="sxs-lookup"><span data-stu-id="1e694-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="1e694-144">Incluir la expectativa de un parámetro "id" en el atributo para `GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="1e694-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="1e694-145">En este momento, el enrutamiento está configurado correctamente; Sin embargo, aún no podemos probarlo.</span><span class="sxs-lookup"><span data-stu-id="1e694-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="1e694-146">Se deben realizar cambios adicionales antes de *ProductsController* se compilará.</span><span class="sxs-lookup"><span data-stu-id="1e694-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="1e694-147">Migrar los modelos y controladores</span><span class="sxs-lookup"><span data-stu-id="1e694-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="1e694-148">El último paso del proceso de migración para este proyecto de API Web simple es copiar a través de los controladores y los modelos que se usan.</span><span class="sxs-lookup"><span data-stu-id="1e694-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="1e694-149">En este caso, basta con copiar *Controllers/ProductsController.cs* desde el proyecto original al nuevo.</span><span class="sxs-lookup"><span data-stu-id="1e694-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="1e694-150">A continuación, copie toda la carpeta modelos desde el proyecto original al nuevo.</span><span class="sxs-lookup"><span data-stu-id="1e694-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="1e694-151">Ajustar los espacios de nombres para que coincida con el nuevo nombre de proyecto (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="1e694-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="1e694-152">En este momento, puede compilar la aplicación y encontrará un número de errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="1e694-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="1e694-153">Por lo general, estos deben estar en las siguientes categorías:</span><span class="sxs-lookup"><span data-stu-id="1e694-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="1e694-154">*ApiController* no existe</span><span class="sxs-lookup"><span data-stu-id="1e694-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="1e694-155">*System.Web.Http* no existe el espacio de nombres</span><span class="sxs-lookup"><span data-stu-id="1e694-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="1e694-156">*IHttpActionResult* no existe</span><span class="sxs-lookup"><span data-stu-id="1e694-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="1e694-157">Afortunadamente, estos son muy fáciles corregir:</span><span class="sxs-lookup"><span data-stu-id="1e694-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="1e694-158">Cambio *ApiController* a *controlador* (puede que necesite agregar *con Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="1e694-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="1e694-159">Elimine cualquiera con la instrucción que hace referencia a *System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="1e694-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="1e694-160">Cambiar cualquier método devolver *IHttpActionResult* para devolver un *IActionResult*</span><span class="sxs-lookup"><span data-stu-id="1e694-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="1e694-161">Una vez que estos cambios se han realizado y que no use instrucciones using quita, el migrados *ProductsController* clase tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="1e694-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="1e694-162">Ahora puede ejecutar el proyecto migrado y vaya a */api/productos*; y, debería ver la lista completa de los 3 productos.</span><span class="sxs-lookup"><span data-stu-id="1e694-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="1e694-163">Vaya a */api/products/1* y debería ver la primera el producto.</span><span class="sxs-lookup"><span data-stu-id="1e694-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a><span data-ttu-id="1e694-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="1e694-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span></span>

<span data-ttu-id="1e694-165">Es una herramienta muy útil cuando ASP.NET Web API migrar proyectos a ASP.NET Core el [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca.</span><span class="sxs-lookup"><span data-stu-id="1e694-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="1e694-166">La corrección de compatibilidad extiende ASP.NET Core para permitir a un número de distintas convenciones de Web API 2 para usarse.</span><span class="sxs-lookup"><span data-stu-id="1e694-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="1e694-167">El ejemplo portar anteriormente en este documento es bastante básico, por lo que la corrección de compatibilidad no era necesaria.</span><span class="sxs-lookup"><span data-stu-id="1e694-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="1e694-168">Para proyectos grandes, usando la corrección de compatibilidad puede ser útil para temporalmente tender API entre ASP.NET Core y ASP.NET Web API 2.</span><span class="sxs-lookup"><span data-stu-id="1e694-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="1e694-169">La corrección de compatibilidad de API Web está pensada para usarse como una medida temporal para facilitar la migración de grandes proyectos API Web a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e694-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="1e694-170">Con el tiempo, los proyectos deben actualizarse para usar patrones principales de ASP.NET en lugar de depender de la corrección de compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="1e694-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span> 

<span data-ttu-id="1e694-171">Entre las características de compatibilidad que se incluyen en Microsoft.AspNetCore.Mvc.WebApiCompatShim se incluyen:</span><span class="sxs-lookup"><span data-stu-id="1e694-171">Compatibility features included in Microsoft.AspNetCore.Mvc.WebApiCompatShim include:</span></span>

* <span data-ttu-id="1e694-172">Agrega un `ApiController` tipo para que los tipos de base de controladores no tienen que actualizarse.</span><span class="sxs-lookup"><span data-stu-id="1e694-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="1e694-173">Habilita el enlace de modelo de estilo Web API.</span><span class="sxs-lookup"><span data-stu-id="1e694-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="1e694-174">Funciones de enlace del modelo MVC ASP.NET Core de forma similar a MVC 5, de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="1e694-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="1e694-175">Los cambios de la corrección de compatibilidad del modelo enlace para que sea más similar a las convenciones de enlace de API Web 2 modelo.</span><span class="sxs-lookup"><span data-stu-id="1e694-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="1e694-176">Por ejemplo, los tipos complejos se enlazan automáticamente desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="1e694-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="1e694-177">Extiende el enlace de modelos para que las acciones de controlador pueden tomar parámetros de tipo `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="1e694-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="1e694-178">Agrega los formateadores de mensajes que permite a las acciones para devolver los resultados de tipo `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="1e694-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="1e694-179">Agrega los métodos de respuesta adicionales que pueden usado para atender las respuestas Web API 2 acciones:</span><span class="sxs-lookup"><span data-stu-id="1e694-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
    * <span data-ttu-id="1e694-180">Generadores de HttpResponseMessage:</span><span class="sxs-lookup"><span data-stu-id="1e694-180">HttpResponseMessage generators:</span></span>
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * <span data-ttu-id="1e694-181">Métodos de resultado de acción:</span><span class="sxs-lookup"><span data-stu-id="1e694-181">Action result methods:</span></span>
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* <span data-ttu-id="1e694-182">Agrega una instancia de `IContentNegotiator` al contenedor de la aplicación DI y hace contenido tipos relacionados con la negociación de [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) disponibles.</span><span class="sxs-lookup"><span data-stu-id="1e694-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="1e694-183">Esto incluye tipos como `DefaultContentNegotiator`, `MediaTypeFormatter`, etcetera.</span><span class="sxs-lookup"><span data-stu-id="1e694-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="1e694-184">Para usar la corrección de compatibilidad, debe:</span><span class="sxs-lookup"><span data-stu-id="1e694-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="1e694-185">Referencia de la [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="1e694-185">Reference the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="1e694-186">Registrar los servicios de la corrección de compatibilidad con el contenedor de la aplicación DI mediante una llamada a `services.AddWebApiConventions()` en la aplicación `Startup.ConfigureServices` método.</span><span class="sxs-lookup"><span data-stu-id="1e694-186">Register the compatibility shim's services with the app's DI container by calling `services.AddWebApiConventions()` in the application's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="1e694-187">Definir las rutas de Web API específica mediante `MapWebApiRoute` en el `IRouteBuilder` en la aplicación `IApplicationBuilder.UseMvc` llamar.</span><span class="sxs-lookup"><span data-stu-id="1e694-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the application's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="1e694-188">Resumen</span><span class="sxs-lookup"><span data-stu-id="1e694-188">Summary</span></span>

<span data-ttu-id="1e694-189">Migrar un proyecto de ASP.NET Web API simple para MVC de ASP.NET Core es bastante sencilla, gracias a la compatibilidad integrada con las API Web de MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1e694-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="1e694-190">La principal que deben migrar todos los proyectos de ASP.NET Web API son rutas, los controladores y los modelos, junto con las actualizaciones de los tipos utilizados por los controladores y acciones.</span><span class="sxs-lookup"><span data-stu-id="1e694-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
