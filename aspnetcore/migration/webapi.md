---
title: Migrar desde ASP.NET Web API a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar una implementación de API web de ASP.NET 4.x Web API a ASP.NET Core MVC.
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: 3c4ded874de2700e1290022a535c08f30dce9490
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47861023"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="d4afe-103">Migrar desde ASP.NET Web API a ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4afe-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="d4afe-104">Por [Scott Addie](https://twitter.com/scott_addie) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d4afe-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d4afe-105">ASP.NET 4.x Web API es un servicio HTTP que llegue a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.</span><span class="sxs-lookup"><span data-stu-id="d4afe-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="d4afe-106">Modelos de aplicación de API Web en un modelo de programación más sencillo, conocido como ASP.NET Core MVC y unifica de ASP.NET Core MVC de ASP.NET de 4.x.</span><span class="sxs-lookup"><span data-stu-id="d4afe-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="d4afe-107">En este artículo muestra los pasos necesarios para migrar desde ASP.NET 4.x Web API a ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d4afe-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="d4afe-108">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d4afe-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4afe-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="d4afe-109">Prerequisites</span></span>

* [<span data-ttu-id="d4afe-110">SDK de .NET Core 2.1 o versiones posteriores</span><span class="sxs-lookup"><span data-stu-id="d4afe-110">.NET Core 2.1 SDK or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="d4afe-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) versión 15.7.3 o posterior con la carga de trabajo **ASP.NET y desarrollo web**</span><span class="sxs-lookup"><span data-stu-id="d4afe-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="d4afe-112">Revise el proyecto ASP.NET 4.x Web API</span><span class="sxs-lookup"><span data-stu-id="d4afe-112">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="d4afe-113">Como punto de partida, este artículo se usa el *ProductsApp* proyecto creado en [Introducción a ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span><span class="sxs-lookup"><span data-stu-id="d4afe-113">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="d4afe-114">En ese proyecto, un proyecto simple ASP.NET 4.x Web API se configura como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="d4afe-114">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="d4afe-115">En *Global.asax.cs*, se realiza una llamada a `WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="d4afe-115">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="d4afe-116">`WebApiConfig` se define en el *App_Start* carpeta.</span><span class="sxs-lookup"><span data-stu-id="d4afe-116">`WebApiConfig` is defined in the *App_Start* folder.</span></span> <span data-ttu-id="d4afe-117">Tiene solo uno estático `Register` método:</span><span class="sxs-lookup"><span data-stu-id="d4afe-117">It has just one static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

<span data-ttu-id="d4afe-118">Esta clase configura [enrutamiento mediante atributos](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aunque realmente no se usa en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d4afe-118">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="d4afe-119">También configura la tabla de enrutamiento, que es utilizada por ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="d4afe-119">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="d4afe-120">En este caso, ASP.NET 4.x Web API espera que las direcciones URL para que coincida con el formato `/api/{controller}/{id}`, con `{id}` que es opcional.</span><span class="sxs-lookup"><span data-stu-id="d4afe-120">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="d4afe-121">El *ProductsApp* proyecto incluye un controlador.</span><span class="sxs-lookup"><span data-stu-id="d4afe-121">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="d4afe-122">El controlador hereda de `ApiController` y expone dos métodos:</span><span class="sxs-lookup"><span data-stu-id="d4afe-122">The controller inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="d4afe-123">El `Product` modelo usado por `ProductsController` es una clase sencilla:</span><span class="sxs-lookup"><span data-stu-id="d4afe-123">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="d4afe-124">Las siguientes secciones muestran la migración del proyecto Web API a ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="d4afe-124">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="d4afe-125">Crear proyecto de destino</span><span class="sxs-lookup"><span data-stu-id="d4afe-125">Create destination project</span></span>

<span data-ttu-id="d4afe-126">Complete los pasos siguientes en Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d4afe-126">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="d4afe-127">Vaya a **archivo** > **nueva** > **proyecto** > **otros tipos de proyecto**  >  **Soluciones de visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="d4afe-127">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="d4afe-128">Seleccione **solución en blanco**y el nombre de la solución *WebAPIMigration*.</span><span class="sxs-lookup"><span data-stu-id="d4afe-128">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="d4afe-129">Haga clic en el **Aceptar** botón.</span><span class="sxs-lookup"><span data-stu-id="d4afe-129">Click the **OK** button.</span></span>
* <span data-ttu-id="d4afe-130">Agregar existente *ProductsApp* proyecto a la solución.</span><span class="sxs-lookup"><span data-stu-id="d4afe-130">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="d4afe-131">Agregue un nuevo **aplicación Web ASP.NET Core** proyecto a la solución.</span><span class="sxs-lookup"><span data-stu-id="d4afe-131">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="d4afe-132">Seleccione el **.NET Core** destino framework en la lista desplegable y seleccione el **API** plantilla de proyecto.</span><span class="sxs-lookup"><span data-stu-id="d4afe-132">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="d4afe-133">Denomine el proyecto *ProductsCore*y haga clic en el **Aceptar** botón.</span><span class="sxs-lookup"><span data-stu-id="d4afe-133">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="d4afe-134">La solución contiene ahora dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="d4afe-134">The solution now contains two projects.</span></span> <span data-ttu-id="d4afe-135">Las siguientes secciones explican migrar el *ProductsApp* contenido del proyecto para el *ProductsCore* proyecto.</span><span class="sxs-lookup"><span data-stu-id="d4afe-135">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="d4afe-136">Migración de la configuración</span><span class="sxs-lookup"><span data-stu-id="d4afe-136">Migrate configuration</span></span>

<span data-ttu-id="d4afe-137">ASP.NET Core no usa el *App_Start* carpeta o el *Global.asax* archivo y el *web.config* se agrega al archivo en el momento de la publicación.</span><span class="sxs-lookup"><span data-stu-id="d4afe-137">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="d4afe-138">*Startup.cs* es el reemplazo de *Global.asax* y se encuentra en la raíz del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d4afe-138">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="d4afe-139">La `Startup` clase controla todas las tareas de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d4afe-139">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="d4afe-140">Para obtener más información, consulta <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="d4afe-140">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="d4afe-141">En ASP.NET Core MVC, el enrutamiento mediante atributos se incluye de forma predeterminada cuando <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> se denomina en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-141">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="d4afe-142">La siguiente `UseMvc` llamar reemplaza el *ProductsApp* del proyecto *app_start/webapiconfig.cs* archivo:</span><span class="sxs-lookup"><span data-stu-id="d4afe-142">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="d4afe-143">Migrar los modelos y controladores</span><span class="sxs-lookup"><span data-stu-id="d4afe-143">Migrate models and controllers</span></span>

<span data-ttu-id="d4afe-144">Copiar a través de la *ProductApp* controlador del proyecto y el modelo que usa.</span><span class="sxs-lookup"><span data-stu-id="d4afe-144">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="d4afe-145">Siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="d4afe-145">Follow these steps:</span></span>

1. <span data-ttu-id="d4afe-146">Copia *Controllers/ProductsController.cs* desde el proyecto original a una nueva.</span><span class="sxs-lookup"><span data-stu-id="d4afe-146">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="d4afe-147">Copiar todo el *modelos* carpeta del proyecto original al nuevo.</span><span class="sxs-lookup"><span data-stu-id="d4afe-147">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="d4afe-148">Cambiar los espacios de nombres de los archivos copiados para que coincida con el nuevo nombre del proyecto (*ProductsCore*).</span><span class="sxs-lookup"><span data-stu-id="d4afe-148">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="d4afe-149">Ajustar el `using ProductsApp.Models;` instrucción *ProductsController.cs* demasiado.</span><span class="sxs-lookup"><span data-stu-id="d4afe-149">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="d4afe-150">En este momento, compilar los resultados de la aplicación en un número de errores de compilación.</span><span class="sxs-lookup"><span data-stu-id="d4afe-150">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="d4afe-151">Los errores se producen porque no existen los siguientes componentes en ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="d4afe-151">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="d4afe-152">Clase `ApiController`</span><span class="sxs-lookup"><span data-stu-id="d4afe-152">`ApiController` class</span></span>
* <span data-ttu-id="d4afe-153">Espacio de nombres `System.Web.Http`</span><span class="sxs-lookup"><span data-stu-id="d4afe-153">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="d4afe-154">`IHttpActionResult` (interfaz)</span><span class="sxs-lookup"><span data-stu-id="d4afe-154">`IHttpActionResult` interface</span></span>

<span data-ttu-id="d4afe-155">Corrija los errores como sigue:</span><span class="sxs-lookup"><span data-stu-id="d4afe-155">Fix the errors as follows:</span></span>

1. <span data-ttu-id="d4afe-156">Cambio `ApiController` a <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span><span class="sxs-lookup"><span data-stu-id="d4afe-156">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="d4afe-157">Agregar `using Microsoft.AspNetCore.Mvc;` para resolver el `ControllerBase` referencia.</span><span class="sxs-lookup"><span data-stu-id="d4afe-157">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="d4afe-158">Elimine `using System.Web.Http;`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-158">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="d4afe-159">Cambiar el `GetProduct` tipo de valor devuelto de la acción de `IHttpActionResult` a `ActionResult<Product>`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-159">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

## <a name="configure-routing"></a><span data-ttu-id="d4afe-160">Configurar el enrutamiento</span><span class="sxs-lookup"><span data-stu-id="d4afe-160">Configure routing</span></span>

<span data-ttu-id="d4afe-161">Configurar el enrutamiento como sigue:</span><span class="sxs-lookup"><span data-stu-id="d4afe-161">Configure routing as follows:</span></span>

1. <span data-ttu-id="d4afe-162">Decorar el `ProductsController` clase con los siguientes atributos:</span><span class="sxs-lookup"><span data-stu-id="d4afe-162">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="d4afe-163">Anterior [[ruta]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) atributo configura el modelo de enrutamiento de atributos del controlador.</span><span class="sxs-lookup"><span data-stu-id="d4afe-163">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="d4afe-164">El [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) atributo hace que sea un requisito para todas las acciones de enrutamiento en este controlador de atributos.</span><span class="sxs-lookup"><span data-stu-id="d4afe-164">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="d4afe-165">Enrutamiento mediante atributos admite tokens, como `[controller]` y `[action]`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-165">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="d4afe-166">En tiempo de ejecución, cada token se reemplaza por el nombre del controlador o acción, respectivamente, al que se ha aplicado el atributo.</span><span class="sxs-lookup"><span data-stu-id="d4afe-166">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="d4afe-167">Los tokens de reducen el número de cadenas mágicas en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="d4afe-167">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="d4afe-168">Los tokens también Asegúrese de rutas sigan estando sincronizadas con los controladores correspondientes y se aplican acciones al automático cambiar el nombre de refactorizaciones.</span><span class="sxs-lookup"><span data-stu-id="d4afe-168">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="d4afe-169">Habilitar las solicitudes HTTP Get a la `ProductController` acciones:</span><span class="sxs-lookup"><span data-stu-id="d4afe-169">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="d4afe-170">Aplicar el [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) atributo a la `GetAllProducts` acción.</span><span class="sxs-lookup"><span data-stu-id="d4afe-170">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="d4afe-171">Aplicar el `[HttpGet("{id}")]` atributo a la `GetProduct` acción.</span><span class="sxs-lookup"><span data-stu-id="d4afe-171">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="d4afe-172">Después de estos cambios y la eliminación de sin usar `using` instrucciones, *ProductsController.cs* archivo tiene este aspecto:</span><span class="sxs-lookup"><span data-stu-id="d4afe-172">After these changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="d4afe-173">Ejecute el proyecto migrado y vaya a `/api/products`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-173">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="d4afe-174">Aparece una lista completa de los tres productos.</span><span class="sxs-lookup"><span data-stu-id="d4afe-174">A full list of three products appears.</span></span> <span data-ttu-id="d4afe-175">Vaya a `/api/products/1`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-175">Browse to `/api/products/1`.</span></span> <span data-ttu-id="d4afe-176">El primer producto aparece.</span><span class="sxs-lookup"><span data-stu-id="d4afe-176">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="d4afe-177">Corrección de compatibilidad</span><span class="sxs-lookup"><span data-stu-id="d4afe-177">Compatibility shim</span></span>

<span data-ttu-id="d4afe-178">El [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) biblioteca proporciona una corrección de compatibilidad para mover proyectos de ASP.NET 4.x Web API a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4afe-178">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="d4afe-179">La corrección de compatibilidad amplía ASP.NET Core para admitir una serie de convenciones de ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="d4afe-179">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="d4afe-180">El ejemplo portado anteriormente en este documento es bastante básico para la corrección de compatibilidad no era necesario.</span><span class="sxs-lookup"><span data-stu-id="d4afe-180">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="d4afe-181">Para proyectos grandes, el uso de la corrección de compatibilidad puede ser útil para temporalmente reduciendo la brecha de API entre ASP.NET Core y ASP.NET 4.x Web API 2.</span><span class="sxs-lookup"><span data-stu-id="d4afe-181">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="d4afe-182">La corrección de compatibilidad de API Web está pensada para usarse como una medida temporal para admitir migración grandes proyectos ASP.NET 4.x Web API a ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4afe-182">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="d4afe-183">Con el tiempo, los proyectos deben actualizarse para utilizar patrones de ASP.NET Core en lugar de depender de la corrección de compatibilidad.</span><span class="sxs-lookup"><span data-stu-id="d4afe-183">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="d4afe-184">Características de compatibilidad que se incluyen en `Microsoft.AspNetCore.Mvc.WebApiCompatShim` incluyen:</span><span class="sxs-lookup"><span data-stu-id="d4afe-184">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="d4afe-185">Agrega un `ApiController` del tipo para que los tipos de base de controladores no deben actualizarse.</span><span class="sxs-lookup"><span data-stu-id="d4afe-185">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="d4afe-186">Habilita el enlace de modelo de estilo Web API.</span><span class="sxs-lookup"><span data-stu-id="d4afe-186">Enables Web API-style model binding.</span></span> <span data-ttu-id="d4afe-187">ASP.NET Core MVC modelar las funciones de enlace de forma similar al de ASP.NET 4.x MVC 5, de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d4afe-187">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="d4afe-188">Los cambios de correcciones de compatibilidad de enlace para que sea más similar a las convenciones de enlace de modelo ASP.NET 4.x Web API 2 de modelos.</span><span class="sxs-lookup"><span data-stu-id="d4afe-188">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="d4afe-189">Por ejemplo, los tipos complejos se enlazan automáticamente desde el cuerpo de solicitud.</span><span class="sxs-lookup"><span data-stu-id="d4afe-189">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="d4afe-190">Amplía el enlace de modelos para que las acciones de controlador pueden tomar parámetros de tipo `HttpRequestMessage`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-190">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="d4afe-191">Agrega los formateadores de mensajes que permite las acciones para devolver los resultados de tipo `HttpResponseMessage`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-191">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="d4afe-192">Agrega métodos de respuesta adicionales que Web API 2 acciones que haya utilizado para atender respuestas:</span><span class="sxs-lookup"><span data-stu-id="d4afe-192">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="d4afe-193">`HttpResponseMessage` generadores de:</span><span class="sxs-lookup"><span data-stu-id="d4afe-193">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="d4afe-194">Métodos de resultados de acción:</span><span class="sxs-lookup"><span data-stu-id="d4afe-194">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="d4afe-195">Agrega una instancia de `IContentNegotiator` a la aplicación del contenedor de dependencias (DI) de la inyección de código y hace que estén disponibles los tipos relacionados con la negociación de contenido desde [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span><span class="sxs-lookup"><span data-stu-id="d4afe-195">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="d4afe-196">Ejemplos de tales tipos `DefaultContentNegotiator` y `MediaTypeFormatter`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-196">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="d4afe-197">Para usar la corrección de compatibilidad:</span><span class="sxs-lookup"><span data-stu-id="d4afe-197">To use the compatibility shim:</span></span>

1. <span data-ttu-id="d4afe-198">Instalar el [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="d4afe-198">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="d4afe-199">Registrar servicios de corrección de compatibilidad con el contenedor de DI de la aplicación mediante una llamada a `services.AddMvc().AddWebApiConventions()` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d4afe-199">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="d4afe-200">Definir web específicos de la API se enruta mediante `MapWebApiRoute` en el `IRouteBuilder` en la aplicación `IApplicationBuilder.UseMvc` llamar.</span><span class="sxs-lookup"><span data-stu-id="d4afe-200">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4afe-201">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d4afe-201">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
