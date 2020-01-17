---
title: Filtros en ASP.NET Core
author: Rick-Anderson
description: Obtenga información sobre cómo funcionan los filtros y cómo se pueden usar en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 1/1/2020
uid: mvc/controllers/filters
ms.openlocfilehash: 759c150e7f35f3f6a52947edc5ef41448dc227fe
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828976"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="05e04-103">Filtros en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05e04-103">Filters in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="05e04-104">Por [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="05e04-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="05e04-105">Los *filtros* en ASP.NET Core permiten que se ejecute el código antes o después de determinadas fases de la canalización del procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="05e04-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="05e04-106">Los filtros integrados se encargan de tareas como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="05e04-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="05e04-107">Autorización (impedir el acceso a los recursos a un usuario que no está autorizado).</span><span class="sxs-lookup"><span data-stu-id="05e04-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="05e04-108">Almacenamiento en caché de respuestas (cortocircuitar la canalización de solicitud para devolver una respuesta almacenada en caché).</span><span class="sxs-lookup"><span data-stu-id="05e04-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="05e04-109">Se pueden crear filtros personalizados que se encarguen de cuestiones transversales.</span><span class="sxs-lookup"><span data-stu-id="05e04-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="05e04-110">Entre los ejemplos de cuestiones transversales se incluyen el control de errores, el almacenamiento en caché, la configuración, la autorización y el registro.</span><span class="sxs-lookup"><span data-stu-id="05e04-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="05e04-111">Los filtros evitan la duplicación de código.</span><span class="sxs-lookup"><span data-stu-id="05e04-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="05e04-112">Así, por ejemplo, un filtro de excepción de control de errores puede consolidar el control de errores.</span><span class="sxs-lookup"><span data-stu-id="05e04-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="05e04-113">Este documento se aplica a Razor Pages, a los controladores de API y a los controladores con vistas.</span><span class="sxs-lookup"><span data-stu-id="05e04-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="05e04-114">[Vea o descargue el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="05e04-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="05e04-115">Funcionamiento de los filtros</span><span class="sxs-lookup"><span data-stu-id="05e04-115">How filters work</span></span>

<span data-ttu-id="05e04-116">Los filtros se ejecutan dentro de la *canalización de invocación de acciones de ASP.NET Core*, a veces denominada *canalización de filtro*.</span><span class="sxs-lookup"><span data-stu-id="05e04-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="05e04-117">La canalización de filtro se ejecuta después de que ASP.NET Core seleccione la acción que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="05e04-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La solicitud se procesa a través de las fases Otro middleware, Middleware de enrutamiento, Selección de acción y Canalización de invocación de acción.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="05e04-120">Tipos de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-120">Filter types</span></span>

<span data-ttu-id="05e04-121">Cada tipo de filtro se ejecuta en una fase diferente dentro de la canalización de filtro:</span><span class="sxs-lookup"><span data-stu-id="05e04-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="05e04-122">Los [filtros de autorización](#authorization-filters) se ejecutan en primer lugar y sirven para averiguar si el usuario está autorizado para realizar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="05e04-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="05e04-123">Los filtros de autorización pueden cortocircuitar la canalización si una solicitud no está autorizada.</span><span class="sxs-lookup"><span data-stu-id="05e04-123">Authorization filters short-circuit the pipeline if the request is not authorized.</span></span>

* <span data-ttu-id="05e04-124">[Filtros de recursos](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="05e04-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="05e04-125">Se ejecutan después de la autorización.</span><span class="sxs-lookup"><span data-stu-id="05e04-125">Run after authorization.</span></span>  
  * <span data-ttu-id="05e04-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> ejecuta código antes que el resto de la canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> runs code before the rest of the filter pipeline.</span></span> <span data-ttu-id="05e04-127">Por ejemplo, `OnResourceExecuting` ejecuta código antes que el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="05e04-127">For example, `OnResourceExecuting` runs code before model binding.</span></span>
  * <span data-ttu-id="05e04-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> ejecuta el código una vez que el resto de la canalización se haya completado.</span><span class="sxs-lookup"><span data-stu-id="05e04-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> runs code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="05e04-129">Los [filtros de acciones](#action-filters):</span><span class="sxs-lookup"><span data-stu-id="05e04-129">[Action filters](#action-filters):</span></span>

  * <span data-ttu-id="05e04-130">Ejecutan código inmediatamente antes y después de llamar a un método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-130">Run code immediately before and after an action method is called.</span></span>
  * <span data-ttu-id="05e04-131">Pueden cambiar los argumentos pasados a una acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-131">Can change the arguments passed into an action.</span></span>
  * <span data-ttu-id="05e04-132">Pueden cambiar el resultado devuelto de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-132">Can change the result returned from the action.</span></span>
  * <span data-ttu-id="05e04-133">**No** se admiten en Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-133">Are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="05e04-134">Los [filtros de excepciones](#exception-filters) aplican directivas globales a las excepciones no controladas que se producen antes de que se escriba el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="05e04-134">[Exception filters](#exception-filters) apply global policies to unhandled exceptions that occur before the response body has been written to.</span></span>

* <span data-ttu-id="05e04-135">Los [filtros de resultados](#result-filters) ejecutan código inmediatamente antes y después de la ejecución de resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-135">[Result filters](#result-filters) run code immediately before and after the execution of action results.</span></span> <span data-ttu-id="05e04-136">Se ejecutan solo cuando el método de acción se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="05e04-136">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="05e04-137">Son útiles para la lógica que debe regir la ejecución de la vista o el formateador.</span><span class="sxs-lookup"><span data-stu-id="05e04-137">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="05e04-138">En el siguiente diagrama se muestra cómo interactúan los tipos de filtro en la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-138">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La solicitud se procesa a través de las fases Filtros de autorización, Filtros de recursos, Enlace de modelos, Filtros de acciones, Ejecución de acciones/Conversión del resultado de acción, Filtros de excepción, Filtros de resultados y Ejecución del resultado.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="05e04-141">Implementación</span><span class="sxs-lookup"><span data-stu-id="05e04-141">Implementation</span></span>

<span data-ttu-id="05e04-142">Los filtros admiten implementaciones tanto sincrónicas como asincrónicas a través de diferentes definiciones de interfaz.</span><span class="sxs-lookup"><span data-stu-id="05e04-142">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="05e04-143">Los filtros sincrónicos ejecutan código antes y después de la fase de canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-143">Synchronous filters run code before and after their pipeline stage.</span></span> <span data-ttu-id="05e04-144">Por ejemplo, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> se llama antes de llamar al método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-144">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*> is called before the action method is called.</span></span> <span data-ttu-id="05e04-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> se llama después de devolver el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-145"><xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*> is called after the action method returns.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="05e04-146">Los filtros asincrónicos definen un método `On-Stage-ExecutionAsync`.</span><span class="sxs-lookup"><span data-stu-id="05e04-146">Asynchronous filters define an `On-Stage-ExecutionAsync` method.</span></span> <span data-ttu-id="05e04-147">Por ejemplo, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span><span class="sxs-lookup"><span data-stu-id="05e04-147">For example, <xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*>:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="05e04-148">En el código anterior, `SampleAsyncActionFilter` tiene un delegado <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) que ejecuta el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-148">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) that executes the action method.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="05e04-149">Varias fases de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-149">Multiple filter stages</span></span>

<span data-ttu-id="05e04-150">Se pueden implementar interfaces para varias fases de filtro en una sola clase.</span><span class="sxs-lookup"><span data-stu-id="05e04-150">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="05e04-151">Por ejemplo, la clase <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa:</span><span class="sxs-lookup"><span data-stu-id="05e04-151">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements:</span></span>

* <span data-ttu-id="05e04-152">Sincrónicas: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> y <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span><span class="sxs-lookup"><span data-stu-id="05e04-152">Synchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> and  <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter></span></span>
* <span data-ttu-id="05e04-153">Asincrónicas: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> y <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="05e04-153">Asynchronous: <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
* <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>

<span data-ttu-id="05e04-154">Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero **no** ambas.</span><span class="sxs-lookup"><span data-stu-id="05e04-154">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="05e04-155">El entorno de ejecución comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, llama a la interfaz.</span><span class="sxs-lookup"><span data-stu-id="05e04-155">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="05e04-156">De lo contrario, llamará a métodos de interfaz sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="05e04-156">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="05e04-157">Si se implementan las interfaces asincrónicas y sincrónicas en una clase, solo se llama al método asincrónico.</span><span class="sxs-lookup"><span data-stu-id="05e04-157">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="05e04-158">Cuando se usan clases abstractas como <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, se invalidan solo los métodos sincrónicos o el método asincrónico de cada tipo de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-158">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, override only the synchronous methods or the asynchronous method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="05e04-159">Atributos de filtros integrados</span><span class="sxs-lookup"><span data-stu-id="05e04-159">Built-in filter attributes</span></span>

<span data-ttu-id="05e04-160">ASP.NET Core incluye filtros integrados basados en atributos que se pueden personalizar y a partir de los cuales crear subclases.</span><span class="sxs-lookup"><span data-stu-id="05e04-160">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="05e04-161">Por ejemplo, el siguiente filtro de resultados agrega un encabezado a la respuesta:</span><span class="sxs-lookup"><span data-stu-id="05e04-161">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="05e04-162">Los atributos permiten a los filtros aceptar argumentos, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="05e04-162">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="05e04-163">Aplique el `AddHeaderAttribute` a un método de acción o controlador y especifique el nombre y el valor del encabezado HTTP:</span><span class="sxs-lookup"><span data-stu-id="05e04-163">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<span data-ttu-id="05e04-164">Use una herramienta como las [herramientas de desarrollo del explorador](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) para examinar los encabezados.</span><span class="sxs-lookup"><span data-stu-id="05e04-164">Use a tool such as the [browser developer tools](https://developer.mozilla.org/docs/Learn/Common_questions/What_are_browser_developer_tools) to examine the headers.</span></span> <span data-ttu-id="05e04-165">En **Encabezados de respuesta**, se muestra `author: Rick Anderson`.</span><span class="sxs-lookup"><span data-stu-id="05e04-165">Under **Response Headers**, `author: Rick Anderson` is displayed.</span></span>

<span data-ttu-id="05e04-166">El código siguiente implementa un atributo `ActionFilterAttribute` que:</span><span class="sxs-lookup"><span data-stu-id="05e04-166">The following code implements an `ActionFilterAttribute` that:</span></span>

* <span data-ttu-id="05e04-167">Lee el título y el nombre del sistema de configuración.</span><span class="sxs-lookup"><span data-stu-id="05e04-167">Reads the title and name from the configuration system.</span></span> <span data-ttu-id="05e04-168">A diferencia del ejemplo anterior, el siguiente código no requiere que se agreguen parámetros de filtro al código.</span><span class="sxs-lookup"><span data-stu-id="05e04-168">Unlike the previous sample, the following code doesn't require filter parameters to be added to the code.</span></span>
* <span data-ttu-id="05e04-169">Agrega el título y el nombre al encabezado de respuesta.</span><span class="sxs-lookup"><span data-stu-id="05e04-169">Adds the title and name to the response header.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyActionFilterAttribute.cs?name=snippet)]

<span data-ttu-id="05e04-170">Las opciones de configuración las proporciona el [sistema de configuración](xref:fundamentals/configuration/index) mediante el [patrón de opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="05e04-170">The configuration options are provided from the [configuration system](xref:fundamentals/configuration/index) using the [options pattern](xref:fundamentals/configuration/options).</span></span> <span data-ttu-id="05e04-171">Por ejemplo, en el archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="05e04-171">For example, from the *appsettings.json* file:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/appsettings.json)]

<span data-ttu-id="05e04-172">En `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05e04-172">In the `StartUp.ConfigureServices`:</span></span>

* <span data-ttu-id="05e04-173">La clase `PositionOptions` se agrega al contenedor de servicios con el área de configuración `"Position"`.</span><span class="sxs-lookup"><span data-stu-id="05e04-173">The `PositionOptions` class is added to the service container with the `"Position"` configuration area.</span></span>
* <span data-ttu-id="05e04-174">`MyActionFilterAttribute` se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="05e04-174">The `MyActionFilterAttribute` is added to the service container.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupAF.cs?name=snippet)]

<span data-ttu-id="05e04-175">En el código siguiente se muestra la clase `PositionOptions` :</span><span class="sxs-lookup"><span data-stu-id="05e04-175">The following code shows the `PositionOptions` class:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Helper/PositionOptions.cs?name=snippet)]

<span data-ttu-id="05e04-176">El siguiente código aplica el atributo `MyActionFilterAttribute` al método `Index2`:</span><span class="sxs-lookup"><span data-stu-id="05e04-176">The following code applies the `MyActionFilterAttribute` to the `Index2` method:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet2&highlight=9)]

<span data-ttu-id="05e04-177">En **Encabezados de respuesta**, `author: Rick Anderson` y `Editor: Joe Smith` se muestran cuando se llama al punto de conexión `Sample/Index2`.</span><span class="sxs-lookup"><span data-stu-id="05e04-177">Under **Response Headers**, `author: Rick Anderson`, and `Editor: Joe Smith` is displayed when the `Sample/Index2` endpoint is called.</span></span>

<span data-ttu-id="05e04-178">El siguiente código aplica los atributos `MyActionFilterAttribute` y `AddHeaderAttribute` a la página de Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="05e04-178">The following code applies the `MyActionFilterAttribute` and the `AddHeaderAttribute` to the Razor Page:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Pages/Movies/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="05e04-179">Los filtros no se pueden usar con métodos de controlador de páginas de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-179">Filters cannot be applied to Razor Page handler methods.</span></span> <span data-ttu-id="05e04-180">Se pueden usar con el modelo de página de Razor Pages o globalmente.</span><span class="sxs-lookup"><span data-stu-id="05e04-180">They can be applied either to the Razor Page model or globally.</span></span>

<span data-ttu-id="05e04-181">Algunas de las interfaces de filtro tienen atributos correspondientes que se pueden usar como clases base en las implementaciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="05e04-181">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="05e04-182">Atributos de filtro:</span><span class="sxs-lookup"><span data-stu-id="05e04-182">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="05e04-183">Ámbitos del filtro y orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="05e04-183">Filter scopes and order of execution</span></span>

<span data-ttu-id="05e04-184">Un filtro se puede agregar a la canalización en uno de tres *ámbitos* posibles:</span><span class="sxs-lookup"><span data-stu-id="05e04-184">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="05e04-185">Mediante un atributo en una acción del controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-185">Using an attribute on a controller action.</span></span> <span data-ttu-id="05e04-186">Los atributos de filtro no se pueden usar con métodos de controlador de páginas de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-186">Filter attributes cannot be applied to Razor Pages handler methods.</span></span>
* <span data-ttu-id="05e04-187">Mediante un atributo en un controlador o una página de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-187">Using an attribute on a controller or Razor Page.</span></span>
* <span data-ttu-id="05e04-188">Globalmente para todos los controladores, las acciones y las páginas de Razor Pages tal como se muestra en el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="05e04-188">Globally for all controllers, actions, and Razor Pages as shown in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

### <a name="default-order-of-execution"></a><span data-ttu-id="05e04-189">Orden de ejecución predeterminado</span><span class="sxs-lookup"><span data-stu-id="05e04-189">Default order of execution</span></span>

<span data-ttu-id="05e04-190">Cuando hay varios filtros en una determinada fase de la canalización, el ámbito determina el orden predeterminado en el que esos filtros se van a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="05e04-190">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="05e04-191">Los filtros globales abarcan a los filtros de clase, que a su vez engloban a los filtros de método.</span><span class="sxs-lookup"><span data-stu-id="05e04-191">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="05e04-192">Como resultado de este anidamiento de filtros, el código de filtros *posterior* se ejecuta en el orden inverso al código *anterior*.</span><span class="sxs-lookup"><span data-stu-id="05e04-192">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="05e04-193">La secuencia de filtro:</span><span class="sxs-lookup"><span data-stu-id="05e04-193">The filter sequence:</span></span>

* <span data-ttu-id="05e04-194">El código *anterior* de los filtros globales.</span><span class="sxs-lookup"><span data-stu-id="05e04-194">The *before* code of global filters.</span></span>
  * <span data-ttu-id="05e04-195">El código *anterior* de los filtros de controlador y página de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-195">The *before* code of controller and Razor Page filters.</span></span>
    * <span data-ttu-id="05e04-196">El código *anterior* de los filtros de métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-196">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="05e04-197">El código *posterior* de los filtros de métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-197">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="05e04-198">El código *posterior* de los filtros de controlador y página de Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-198">The *after* code of controller and Razor Page filters.</span></span>
* <span data-ttu-id="05e04-199">El código *posterior* de los filtros globales.</span><span class="sxs-lookup"><span data-stu-id="05e04-199">The *after* code of global filters.</span></span>
  
<span data-ttu-id="05e04-200">El ejemplo siguiente que ilustra el orden en el que se llama a los métodos de filtro relativos a filtros de acciones sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="05e04-200">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="05e04-201">Secuencia</span><span class="sxs-lookup"><span data-stu-id="05e04-201">Sequence</span></span> | <span data-ttu-id="05e04-202">Ámbito del filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-202">Filter scope</span></span> | <span data-ttu-id="05e04-203">Método de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-203">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="05e04-204">1</span><span class="sxs-lookup"><span data-stu-id="05e04-204">1</span></span> | <span data-ttu-id="05e04-205">Global</span><span class="sxs-lookup"><span data-stu-id="05e04-205">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="05e04-206">2</span><span class="sxs-lookup"><span data-stu-id="05e04-206">2</span></span> | <span data-ttu-id="05e04-207">Controlador o página de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="05e04-207">Controller or Razor Page</span></span>| `OnActionExecuting` |
| <span data-ttu-id="05e04-208">3</span><span class="sxs-lookup"><span data-stu-id="05e04-208">3</span></span> | <span data-ttu-id="05e04-209">Método</span><span class="sxs-lookup"><span data-stu-id="05e04-209">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="05e04-210">4</span><span class="sxs-lookup"><span data-stu-id="05e04-210">4</span></span> | <span data-ttu-id="05e04-211">Método</span><span class="sxs-lookup"><span data-stu-id="05e04-211">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="05e04-212">5</span><span class="sxs-lookup"><span data-stu-id="05e04-212">5</span></span> | <span data-ttu-id="05e04-213">Controlador o página de Razor Pages</span><span class="sxs-lookup"><span data-stu-id="05e04-213">Controller or Razor Page</span></span> | `OnActionExecuted` |
| <span data-ttu-id="05e04-214">6</span><span class="sxs-lookup"><span data-stu-id="05e04-214">6</span></span> | <span data-ttu-id="05e04-215">Global</span><span class="sxs-lookup"><span data-stu-id="05e04-215">Global</span></span> | `OnActionExecuted` |

### <a name="controller-level-filters"></a><span data-ttu-id="05e04-216">Filtros de nivel de controlador</span><span class="sxs-lookup"><span data-stu-id="05e04-216">Controller level filters</span></span>

<span data-ttu-id="05e04-217">Cada controlador que hereda de la clase base <xref:Microsoft.AspNetCore.Mvc.Controller> incluye los métodos [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) y [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="05e04-217">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="05e04-218">Estos métodos:</span><span class="sxs-lookup"><span data-stu-id="05e04-218">These methods:</span></span>

* <span data-ttu-id="05e04-219">Encapsulan los filtros que se ejecutan para una acción determinada.</span><span class="sxs-lookup"><span data-stu-id="05e04-219">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="05e04-220">`OnActionExecuting` se llama antes de cualquiera de los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-220">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="05e04-221">`OnActionExecuted` se llama después de todos los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-221">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="05e04-222">`OnActionExecutionAsync` se llama antes de cualquiera de los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-222">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="05e04-223">El código del filtro después de `next` se ejecuta después del método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-223">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="05e04-224">Por ejemplo, en el ejemplo de descarga, se aplica `MySampleActionFilter` globalmente al inicio.</span><span class="sxs-lookup"><span data-stu-id="05e04-224">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="05e04-225">El `TestController`:</span><span class="sxs-lookup"><span data-stu-id="05e04-225">The `TestController`:</span></span>

* <span data-ttu-id="05e04-226">Aplica `SampleActionFilterAttribute` (`[SampleActionFilter]`) a la acción `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="05e04-226">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="05e04-227">Invalida `OnActionExecuting` y `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="05e04-227">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<!-- test via  webBuilder.UseStartup<Startup>(); -->

<span data-ttu-id="05e04-228">Si se dirige a `https://localhost:5001/Test2/FilterTest2`, se ejecuta el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="05e04-228">Navigating to `https://localhost:5001/Test2/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="05e04-229">Los filtros de nivel de controlador establecen la propiedad [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) en `int.MinValue`.</span><span class="sxs-lookup"><span data-stu-id="05e04-229">Controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue`.</span></span> <span data-ttu-id="05e04-230">Los filtros de nivel de controlador **no** pueden establecerse para ejecutarse tras aplicarse los filtros a los métodos.</span><span class="sxs-lookup"><span data-stu-id="05e04-230">Controller level filters can **not** be set to run after filters applied to methods.</span></span> <span data-ttu-id="05e04-231">Order se explica en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="05e04-231">Order is explained in the next section.</span></span>

<span data-ttu-id="05e04-232">Para Razor Pages, consulte [Implementar filtros de páginas de Razor globalmente](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="05e04-232">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="05e04-233">Invalidación del orden predeterminado</span><span class="sxs-lookup"><span data-stu-id="05e04-233">Overriding the default order</span></span>

<span data-ttu-id="05e04-234">La secuencia de ejecución predeterminada se puede invalidar con la implementación de <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-234">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="05e04-235"><xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> expone la propiedad `IOrderedFilter` que tiene prioridad sobre el ámbito a la hora de determinar el orden de ejecución.</span><span class="sxs-lookup"><span data-stu-id="05e04-235">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="05e04-236">Un filtro con un valor `Order` menor:</span><span class="sxs-lookup"><span data-stu-id="05e04-236">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="05e04-237">Ejecuta el código *anterior* antes que el de un filtro con un valor mayor de `Order`.</span><span class="sxs-lookup"><span data-stu-id="05e04-237">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="05e04-238">Ejecuta el código *posterior* después que el de un filtro con un valor mayor de `Order`.</span><span class="sxs-lookup"><span data-stu-id="05e04-238">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="05e04-239">La propiedad `Order` se establece con un parámetro de constructor:</span><span class="sxs-lookup"><span data-stu-id="05e04-239">The `Order` property is set with a constructor parameter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test3Controller.cs?name=snippet)]

<span data-ttu-id="05e04-240">Tenga en cuenta los dos filtros de acción en el siguiente controlador:</span><span class="sxs-lookup"><span data-stu-id="05e04-240">Consider the two action filters in the following controller:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="05e04-241">Un filtro global se agrega en `StartUp.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="05e04-241">A global filter is added in `StartUp.ConfigureServices`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder.cs?name=snippet)]

<span data-ttu-id="05e04-242">Los tres filtros se ejecutan en el siguiente orden:</span><span class="sxs-lookup"><span data-stu-id="05e04-242">The 3 filters run in the following order:</span></span>

* `Test2Controller.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `MyAction2FilterAttribute.OnActionExecuting`
      * `Test2Controller.FilterTest2`
    * `MySampleActionFilter.OnActionExecuted`
  * `MyAction2FilterAttribute.OnResultExecuting`
* `Test2Controller.OnActionExecuted`

<span data-ttu-id="05e04-243">La propiedad `Order` invalida el ámbito al determinar el orden en el que se ejecutarán los filtros.</span><span class="sxs-lookup"><span data-stu-id="05e04-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="05e04-244">Los filtros se clasifican por orden en primer lugar y, después, se usa el ámbito para priorizar en caso de igualdad.</span><span class="sxs-lookup"><span data-stu-id="05e04-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="05e04-245">Todos los filtros integrados implementan `IOrderedFilter` y establecen el valor predeterminado de `Order` en 0.</span><span class="sxs-lookup"><span data-stu-id="05e04-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="05e04-246">Como se mencionó anteriormente, los filtros de nivel de controlador establecen la propiedad [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) en `int.MinValue`. En el caso de los filtros integrados, el ámbito determina el orden a menos que se establezca `Order` en un valor distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="05e04-246">As mentioned previously, controller level filters set the [Order](https://github.com/dotnet/AspNetCore/blob/master/src/Mvc/Mvc.Core/src/Filters/ControllerActionFilter.cs#L15-L17) property to `int.MinValue` For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

<span data-ttu-id="05e04-247">En el código anterior, `MySampleActionFilter` tiene ámbito global, por lo que se ejecuta antes de `MyAction2FilterAttribute`, que tiene ámbito de controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-247">In the preceding code, `MySampleActionFilter` has global scope so it runs before `MyAction2FilterAttribute`, which has controller scope.</span></span> <span data-ttu-id="05e04-248">Para que `MyAction2FilterAttribute` se ejecute en primer lugar, establezca el orden en `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="05e04-248">To make `MyAction2FilterAttribute` run first, set the order to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/Test2Controller.cs?name=snippet2)]

<span data-ttu-id="05e04-249">Para que el filtro global `MySampleActionFilter` se ejecute en primer lugar, establezca `Order` en `int.MinValue`:</span><span class="sxs-lookup"><span data-stu-id="05e04-249">To make the global filter `MySampleActionFilter` run first, set `Order` to `int.MinValue`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/StartupOrder2.cs?name=snippet&highlight=6)]

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="05e04-250">Cancelación y cortocircuito</span><span class="sxs-lookup"><span data-stu-id="05e04-250">Cancellation and short-circuiting</span></span>

<span data-ttu-id="05e04-251">La canalización de filtro se puede cortocircuitar en cualquier momento mediante el establecimiento de la propiedad <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> en el parámetro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> que se ha proporcionado al método de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-251">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="05e04-252">Por ejemplo, el siguiente filtro de recursos impide que el resto de la canalización se ejecute:</span><span class="sxs-lookup"><span data-stu-id="05e04-252">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="05e04-253">En el siguiente código, tanto el filtro `ShortCircuitingResourceFilter` como el filtro `AddHeader` tienen como destino el método de acción `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="05e04-253">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="05e04-254">El `ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="05e04-254">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="05e04-255">Se ejecuta en primer lugar, porque es un filtro de recursos y `AddHeader` es un filtro de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-255">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="05e04-256">Cortocircuita el resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-256">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="05e04-257">Por tanto, el filtro `AddHeader` nunca se ejecuta en relación con la acción `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="05e04-257">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="05e04-258">Este comportamiento sería el mismo si ambos filtros se aplicaran en el nivel de método de acción, siempre y cuando `ShortCircuitingResourceFilter` se haya ejecutado primero.</span><span class="sxs-lookup"><span data-stu-id="05e04-258">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="05e04-259">`ShortCircuitingResourceFilter` se ejecuta primero debido a su tipo de filtro o al uso explícito de la propiedad `Order`.</span><span class="sxs-lookup"><span data-stu-id="05e04-259">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

## <a name="dependency-injection"></a><span data-ttu-id="05e04-260">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="05e04-260">Dependency injection</span></span>

<span data-ttu-id="05e04-261">Los filtros se pueden agregar por tipo o por instancia.</span><span class="sxs-lookup"><span data-stu-id="05e04-261">Filters can be added by type or by instance.</span></span> <span data-ttu-id="05e04-262">Si se agrega una instancia, esta se utiliza para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="05e04-262">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="05e04-263">Si se agrega un tipo, se activa por tipo.</span><span class="sxs-lookup"><span data-stu-id="05e04-263">If a type is added, it's type-activated.</span></span> <span data-ttu-id="05e04-264">Un filtro activado por tipo significa:</span><span class="sxs-lookup"><span data-stu-id="05e04-264">A type-activated filter means:</span></span>

* <span data-ttu-id="05e04-265">Se crea una instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="05e04-265">An instance is created for each request.</span></span>
* <span data-ttu-id="05e04-266">Las dependencias de constructor se rellenan mediante la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05e04-266">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="05e04-267">Los filtros que se implementan como atributos y se agregan directamente a las clases de controlador o a los métodos de acción no pueden tener dependencias de constructor proporcionadas por la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05e04-267">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="05e04-268">Las dependencias de constructor no pueden proporcionarse mediante la inserción de dependencias porque:</span><span class="sxs-lookup"><span data-stu-id="05e04-268">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="05e04-269">Los atributos deben tener los parámetros de constructor proporcionados allá donde se apliquen.</span><span class="sxs-lookup"><span data-stu-id="05e04-269">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="05e04-270">Se trata de una limitación de cómo funcionan los atributos.</span><span class="sxs-lookup"><span data-stu-id="05e04-270">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="05e04-271">Los filtros siguientes admiten dependencias de constructor proporcionadas en la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-271">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="05e04-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> se implementa en el atributo.</span><span class="sxs-lookup"><span data-stu-id="05e04-272"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="05e04-273">Los filtros anteriores se pueden aplicar a un método de controlador o de acción:</span><span class="sxs-lookup"><span data-stu-id="05e04-273">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="05e04-274">Los registradores están disponibles en la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-274">Loggers are available from DI.</span></span> <span data-ttu-id="05e04-275">Sin embargo, evite crear y utilizar filtros únicamente con fines de registro.</span><span class="sxs-lookup"><span data-stu-id="05e04-275">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="05e04-276">El [registro del marco integrado](xref:fundamentals/logging/index) proporciona normalmente lo que se necesita para el registro.</span><span class="sxs-lookup"><span data-stu-id="05e04-276">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="05e04-277">Registro agregado a los filtros:</span><span class="sxs-lookup"><span data-stu-id="05e04-277">Logging added to filters:</span></span>

* <span data-ttu-id="05e04-278">Debe centrarse en cuestiones de dominio empresarial o en el comportamiento específico del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-278">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="05e04-279">**No** debe registrar acciones u otros eventos del marco.</span><span class="sxs-lookup"><span data-stu-id="05e04-279">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="05e04-280">Los filtros integrados registran acciones y eventos del marco.</span><span class="sxs-lookup"><span data-stu-id="05e04-280">The built-in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="05e04-281">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="05e04-281">ServiceFilterAttribute</span></span>

<span data-ttu-id="05e04-282">Los tipos de implementación de filtro de servicio se registran en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="05e04-282">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="05e04-283"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera una instancia del filtro de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-283">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="05e04-284">El código siguiente muestra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="05e04-284">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="05e04-285">En el código siguiente, `AddHeaderResultServiceFilter` se agrega al contenedor de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-285">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Startup.cs?name=snippet&highlight=4)]

<span data-ttu-id="05e04-286">En el código siguiente, el atributo `ServiceFilter` recupera una instancia del filtro `AddHeaderResultServiceFilter` desde la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-286">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="05e04-287">Al usar `ServiceFilterAttribute`, si se establece [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="05e04-287">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="05e04-288">Proporciona una sugerencia que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó.</span><span class="sxs-lookup"><span data-stu-id="05e04-288">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="05e04-289">El entorno de ejecución de ASP.NET Core no garantiza:</span><span class="sxs-lookup"><span data-stu-id="05e04-289">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="05e04-290">Que se creará una única instancia del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-290">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="05e04-291">El filtro no volverá a solicitarse desde el contenedor de inserción de dependencias en algún momento posterior.</span><span class="sxs-lookup"><span data-stu-id="05e04-291">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="05e04-292">No debe usarse con un filtro que depende de servicios con una duración distinta de singleton.</span><span class="sxs-lookup"><span data-stu-id="05e04-292">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="05e04-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="05e04-293"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="05e04-294">`IFilterFactory` expone el método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear una instancia de <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="05e04-294">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="05e04-295">`CreateInstance` carga el tipo especificado desde la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-295">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="05e04-296">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="05e04-296">TypeFilterAttribute</span></span>

<span data-ttu-id="05e04-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> es similar a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, pero su tipo no se resuelve directamente desde el contenedor de inserción de dependencias,</span><span class="sxs-lookup"><span data-stu-id="05e04-297"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="05e04-298">sino que crea una instancia del tipo usando el elemento <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="05e04-298">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="05e04-299">Dado que los tipos `TypeFilterAttribute` no se resuelven directamente desde el contenedor de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-299">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="05e04-300">Los tipos a los que se hace referencia con `TypeFilterAttribute` no tienen que estar registrados con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-300">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="05e04-301">Sus dependencias se completan a través del contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-301">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="05e04-302">`TypeFilterAttribute` puede aceptar opcionalmente argumentos de constructor del tipo en cuestión.</span><span class="sxs-lookup"><span data-stu-id="05e04-302">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="05e04-303">Al usar `TypeFilterAttribute`, si se establece [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="05e04-303">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="05e04-304">Proporciona una sugerencia que indica que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó.</span><span class="sxs-lookup"><span data-stu-id="05e04-304">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="05e04-305">El entorno de ejecución de ASP.NET Core no ofrece ninguna garantía de que se vaya a crear una única instancia del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-305">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="05e04-306">No debe usarse con un filtro que depende de servicios con una duración distinta de singleton.</span><span class="sxs-lookup"><span data-stu-id="05e04-306">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="05e04-307">En el siguiente ejemplo se muestra cómo pasar argumentos a un tipo mediante `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="05e04-307">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="05e04-308">Filtros de autorización</span><span class="sxs-lookup"><span data-stu-id="05e04-308">Authorization filters</span></span>

<span data-ttu-id="05e04-309">Filtros de autorización:</span><span class="sxs-lookup"><span data-stu-id="05e04-309">Authorization filters:</span></span>

* <span data-ttu-id="05e04-310">Son los primeros filtros que se ejecutan en la canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-310">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="05e04-311">Controlan el acceso a los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-311">Control access to action methods.</span></span>
* <span data-ttu-id="05e04-312">Tienen un método anterior, pero no uno posterior.</span><span class="sxs-lookup"><span data-stu-id="05e04-312">Have a before method, but no after method.</span></span>

<span data-ttu-id="05e04-313">Los filtros de autorización personalizados requieren un marco de autorización personalizado.</span><span class="sxs-lookup"><span data-stu-id="05e04-313">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="05e04-314">Es preferible configurar directivas de autorización o escribir una directiva de autorización personalizada a escribir un filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="05e04-314">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="05e04-315">El filtro de autorización integrado:</span><span class="sxs-lookup"><span data-stu-id="05e04-315">The built-in authorization filter:</span></span>

* <span data-ttu-id="05e04-316">Llama a la autorización del sistema.</span><span class="sxs-lookup"><span data-stu-id="05e04-316">Calls the authorization system.</span></span>
* <span data-ttu-id="05e04-317">No autoriza las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="05e04-317">Does not authorize requests.</span></span>

<span data-ttu-id="05e04-318">**No** inicie excepciones dentro de los filtros de autorización:</span><span class="sxs-lookup"><span data-stu-id="05e04-318">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="05e04-319">La excepción no se controlará.</span><span class="sxs-lookup"><span data-stu-id="05e04-319">The exception will not be handled.</span></span>
* <span data-ttu-id="05e04-320">Los filtros de excepciones no controlarán la excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-320">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="05e04-321">Considere la posibilidad de emitir un desafío cuando se produzca una excepción en un filtro de autorizaciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-321">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="05e04-322">[Aquí](xref:security/authorization/introduction) encontrará más información sobre la autorización.</span><span class="sxs-lookup"><span data-stu-id="05e04-322">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="05e04-323">Filtros de recursos</span><span class="sxs-lookup"><span data-stu-id="05e04-323">Resource filters</span></span>

<span data-ttu-id="05e04-324">Filtros de recursos:</span><span class="sxs-lookup"><span data-stu-id="05e04-324">Resource filters:</span></span>

* <span data-ttu-id="05e04-325">Implementan la interfaz <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-325">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="05e04-326">La ejecución encapsula la mayor parte de la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-326">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="05e04-327">Los [filtros de autorizaciones](#authorization-filters) son los únicos que se ejecutan antes que los filtros de recursos.</span><span class="sxs-lookup"><span data-stu-id="05e04-327">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="05e04-328">Los filtros de recursos son útiles para cortocircuitar la mayor parte de la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-328">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="05e04-329">Por ejemplo, un filtro de almacenamiento en caché puede evitar que se ejecute el resto de la canalización en un acierto de caché.</span><span class="sxs-lookup"><span data-stu-id="05e04-329">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="05e04-330">Ejemplos de filtros de recursos:</span><span class="sxs-lookup"><span data-stu-id="05e04-330">Resource filter examples:</span></span>

* <span data-ttu-id="05e04-331">[El filtro de recursos de cortocircuito](#short-circuiting-resource-filter) mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="05e04-331">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="05e04-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="05e04-332">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="05e04-333">Evita que el enlace de modelos tenga acceso a los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="05e04-333">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="05e04-334">Se utiliza cuando hay cargas de archivos muy voluminosos para impedir que los datos del formulario se lean en la memoria.</span><span class="sxs-lookup"><span data-stu-id="05e04-334">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="05e04-335">Filtros de acciones</span><span class="sxs-lookup"><span data-stu-id="05e04-335">Action filters</span></span>

<span data-ttu-id="05e04-336">Los filtros de acción **no** se aplican a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-336">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="05e04-337">Razor Pages admite <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> y <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-337">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="05e04-338">Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).</span><span class="sxs-lookup"><span data-stu-id="05e04-338">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="05e04-339">Filtros de acciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-339">Action filters:</span></span>

* <span data-ttu-id="05e04-340">Implementan la interfaz <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-340">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="05e04-341">Su ejecución rodea la ejecución de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-341">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="05e04-342">El código siguiente muestra un ejemplo de filtro de acciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-342">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="05e04-343"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> ofrece las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="05e04-343">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="05e04-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>: permite leer las entradas de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-344"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables reading the inputs to an action method.</span></span>
* <span data-ttu-id="05e04-345"><xref:Microsoft.AspNetCore.Mvc.Controller>: permite manipular la instancia del controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-345"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="05e04-346"><xref:System.Web.Mvc.ActionExecutingContext.Result>: si se establece `Result`, se cortocircuita la ejecución del método de acción y de los filtros de acciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="05e04-346"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="05e04-347">Inicio de una excepción en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="05e04-347">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="05e04-348">Impide la ejecución de los filtros subsiguientes.</span><span class="sxs-lookup"><span data-stu-id="05e04-348">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="05e04-349">A diferencia del establecimiento de `Result`, se trata como un error en lugar de como un resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="05e04-349">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="05e04-350"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> proporciona `Controller` y `Result`, además de las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="05e04-350">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="05e04-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: es true si otro filtro ha cortocircuitado la ejecución de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-351"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="05e04-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: es un valor distinto de NULL si la acción o un filtro de acción de ejecución anterior han producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-352"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="05e04-353">Si se establece esta propiedad en un valor NULL:</span><span class="sxs-lookup"><span data-stu-id="05e04-353">Setting this property to null:</span></span>

  * <span data-ttu-id="05e04-354">Controla la excepción eficazmente.</span><span class="sxs-lookup"><span data-stu-id="05e04-354">Effectively handles the exception.</span></span>
  * <span data-ttu-id="05e04-355">`Result` se ejecuta como si se devolviera desde el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-355">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="05e04-356">En un `IAsyncActionFilter`, una llamada a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="05e04-356">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="05e04-357">Ejecuta cualquier filtro de acciones posterior y el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-357">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="05e04-358">Devuelve `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="05e04-358">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="05e04-359">Para cortocircuitar esto, asigne <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a una instancia de resultado y no llame a `next` (la clase `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="05e04-359">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="05e04-360">El marco proporciona una clase <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> abstracta de la que se pueden crear subclases.</span><span class="sxs-lookup"><span data-stu-id="05e04-360">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="05e04-361">Se puede usar el filtro de acción `OnActionExecuting` para:</span><span class="sxs-lookup"><span data-stu-id="05e04-361">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="05e04-362">Validar el estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="05e04-362">Validate model state.</span></span>
* <span data-ttu-id="05e04-363">Devolver un error si el estado no es válido.</span><span class="sxs-lookup"><span data-stu-id="05e04-363">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="05e04-364">El método `OnActionExecuted` se ejecuta después del método de acción:</span><span class="sxs-lookup"><span data-stu-id="05e04-364">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="05e04-365">Y puede ver y manipular los resultados de la acción a través de la propiedad <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="05e04-365">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="05e04-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> se establece en true si otro filtro ha cortocircuitado la ejecución de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-366"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="05e04-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> se establece en un valor distinto de NULL si la acción o un filtro de acción posterior han producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-367"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="05e04-368">Si `Exception` se establece como nulo:</span><span class="sxs-lookup"><span data-stu-id="05e04-368">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="05e04-369">Controla una excepción eficazmente.</span><span class="sxs-lookup"><span data-stu-id="05e04-369">Effectively handles an exception.</span></span>
  * <span data-ttu-id="05e04-370">`ActionExecutedContext.Result` se ejecuta como si se devolviera con normalidad desde el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-370">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="05e04-371">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="05e04-371">Exception filters</span></span>

<span data-ttu-id="05e04-372">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-372">Exception filters:</span></span>

* <span data-ttu-id="05e04-373">Implementan <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-373">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span>
* <span data-ttu-id="05e04-374">Se pueden usar para implementar directivas de control de errores comunes.</span><span class="sxs-lookup"><span data-stu-id="05e04-374">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="05e04-375">En el siguiente filtro de excepciones de ejemplo se usa una vista de error personalizada para mostrar los detalles sobre las excepciones que se producen cuando la aplicación está en fase de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="05e04-375">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="05e04-376">El código siguiente prueba el filtro de excepciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-376">The following code tests the exception filter:</span></span>

[!code-csharp[](filters/3.1sample/FiltersSample/Controllers/FailingController.cs?name=snippet)]

<span data-ttu-id="05e04-377">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-377">Exception filters:</span></span>

* <span data-ttu-id="05e04-378">No tienen eventos anteriores ni posteriores.</span><span class="sxs-lookup"><span data-stu-id="05e04-378">Don't have before and after events.</span></span>
* <span data-ttu-id="05e04-379">Implementan <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="05e04-379">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="05e04-380">Controlan las excepciones sin controlar que se producen en Razor Pages, al crear controladores, en el [enlace de modelos](xref:mvc/models/model-binding), en los filtros de acciones o en los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-380">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="05e04-381">**No** detectan aquellas excepciones que se produzcan en los filtros de recursos, en los filtros de resultados o en la ejecución de resultados de MVC.</span><span class="sxs-lookup"><span data-stu-id="05e04-381">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="05e04-382">Para controlar una excepción, establezca la propiedad <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> en `true` o escriba una respuesta.</span><span class="sxs-lookup"><span data-stu-id="05e04-382">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="05e04-383">Esto detiene la propagación de la excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-383">This stops propagation of the exception.</span></span> <span data-ttu-id="05e04-384">Un filtro de excepciones no tiene capacidad para convertir una excepción en un proceso "correcto".</span><span class="sxs-lookup"><span data-stu-id="05e04-384">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="05e04-385">Eso solo lo pueden hacer los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-385">Only an action filter can do that.</span></span>

<span data-ttu-id="05e04-386">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-386">Exception filters:</span></span>

* <span data-ttu-id="05e04-387">Son adecuados para interceptar las excepciones que se producen en las acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-387">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="05e04-388">No son tan flexibles como el middleware de control de errores.</span><span class="sxs-lookup"><span data-stu-id="05e04-388">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="05e04-389">Es preferible usar middleware de control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-389">Prefer middleware for exception handling.</span></span> <span data-ttu-id="05e04-390">Utilice los filtros de excepciones solo cuando el control de errores *es diferente* en función del método de acción que se llama.</span><span class="sxs-lookup"><span data-stu-id="05e04-390">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="05e04-391">Por ejemplo, puede que una aplicación tenga métodos de acción tanto para los puntos de conexión de API como para las vistas/HTML.</span><span class="sxs-lookup"><span data-stu-id="05e04-391">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="05e04-392">Los puntos de conexión de API podrían devolver información sobre errores como JSON, mientras que las acciones basadas en vistas podrían devolver una página de error como HTML.</span><span class="sxs-lookup"><span data-stu-id="05e04-392">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="05e04-393">Filtros de resultados</span><span class="sxs-lookup"><span data-stu-id="05e04-393">Result filters</span></span>

<span data-ttu-id="05e04-394">Filtros de resultados:</span><span class="sxs-lookup"><span data-stu-id="05e04-394">Result filters:</span></span>

* <span data-ttu-id="05e04-395">Implementar una interfaz:</span><span class="sxs-lookup"><span data-stu-id="05e04-395">Implement an interface:</span></span>
  * <span data-ttu-id="05e04-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="05e04-396"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="05e04-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="05e04-397"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="05e04-398">Su ejecución rodea la ejecución de los resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-398">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="05e04-399">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="05e04-399">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="05e04-400">El código siguiente muestra un filtro de resultados que agrega un encabezado HTTP:</span><span class="sxs-lookup"><span data-stu-id="05e04-400">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="05e04-401">El tipo de resultado que se ejecute dependerá de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-401">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="05e04-402">Una acción que devuelve una vista incluye todo el procesamiento de Razor como parte del elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="05e04-402">An action returning a view includes all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="05e04-403">Un método API puede llevar a cabo algunas funciones de serialización como parte de la ejecución del resultado.</span><span class="sxs-lookup"><span data-stu-id="05e04-403">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="05e04-404">[Aquí](xref:mvc/controllers/actions) encontrará más información sobre los resultados de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-404">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="05e04-405">Los filtros de resultados solo se ejecutan cuando una acción o un filtro de acción genera un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-405">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="05e04-406">Los filtros de resultados no se ejecutan cuando:</span><span class="sxs-lookup"><span data-stu-id="05e04-406">Result filters are not executed when:</span></span>

* <span data-ttu-id="05e04-407">Un filtro de autorización o un filtro de recursos genera un cortocircuito en la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-407">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="05e04-408">Un filtro de excepciones controla una excepción al producir un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-408">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="05e04-409">El método <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> puede cortocircuitar la ejecución del resultado de la acción y de los filtros de resultados posteriores mediante el establecimiento de <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> en `true`.</span><span class="sxs-lookup"><span data-stu-id="05e04-409">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="05e04-410">Escriba en el objeto de respuesta cuando el proceso se cortocircuite, ya que así evitará que se genere una respuesta vacía.</span><span class="sxs-lookup"><span data-stu-id="05e04-410">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="05e04-411">Generación de una excepción en `IResultFilter.OnResultExecuting`:</span><span class="sxs-lookup"><span data-stu-id="05e04-411">Throwing an exception in `IResultFilter.OnResultExecuting`:</span></span>

* <span data-ttu-id="05e04-412">Impide la ejecución del resultado de la acción y de los filtros subsiguientes.</span><span class="sxs-lookup"><span data-stu-id="05e04-412">Prevents execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="05e04-413">Se trata como un error en lugar de como un resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="05e04-413">Is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="05e04-414">Cuando se ejecuta el método <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>, es probable que la respuesta ya se haya enviado al cliente.</span><span class="sxs-lookup"><span data-stu-id="05e04-414">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has probably already been sent to the client.</span></span> <span data-ttu-id="05e04-415">En este caso, no se puede cambiar.</span><span class="sxs-lookup"><span data-stu-id="05e04-415">If the response has already been sent to the client, it cannot be changed.</span></span>

<span data-ttu-id="05e04-416">`ResultExecutedContext.Canceled` se establece en `true` si otro filtro ha cortocircuitado la ejecución del resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-416">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="05e04-417">`ResultExecutedContext.Exception` se establece en un valor distinto de NULL si el resultado de la acción o un filtro de resultado posterior ha producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-417">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="05e04-418">Establecer `Exception` en un valor null hace que una excepción se controle de forma eficaz y evita que se vuelva a producir dicha excepción más adelante en la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-418">Setting `Exception` to null effectively handles an exception and prevents the exception from being thrown again later in the pipeline.</span></span> <span data-ttu-id="05e04-419">No hay una forma confiable de escribir datos en una respuesta cuando se controla una excepción en un filtro de resultados.</span><span class="sxs-lookup"><span data-stu-id="05e04-419">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="05e04-420">Si los encabezados ya se han vaciado en el cliente si el resultado de una acción inicia una excepción, no hay ningún mecanismo confiable que permita enviar un código de error.</span><span class="sxs-lookup"><span data-stu-id="05e04-420">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="05e04-421">En un elemento <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una llamada a `await next` en <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> ejecuta cualquier filtro de resultados posterior y el resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-421">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="05e04-422">Para cortocircuitarlo, establezca [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) en `true` y no llame a `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="05e04-422">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="05e04-423">El marco proporciona una clase abstracta `ResultFilterAttribute` de la que se pueden crear subclases.</span><span class="sxs-lookup"><span data-stu-id="05e04-423">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="05e04-424">La clase [AddHeaderAttribute](#add-header-attribute) mostrada anteriormente es un ejemplo de un atributo de filtro de resultados.</span><span class="sxs-lookup"><span data-stu-id="05e04-424">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="05e04-425">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="05e04-425">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="05e04-426">Las interfaces <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> declaran una implementación <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> que se ejecuta para obtener todos los resultados de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-426">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="05e04-427">Esto incluye los resultados de la acción generados por:</span><span class="sxs-lookup"><span data-stu-id="05e04-427">This includes action results produced by:</span></span>

* <span data-ttu-id="05e04-428">Filtros de autorización y filtros de recursos que generan un cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="05e04-428">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="05e04-429">Filtros de excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-429">Exception filters.</span></span>

<span data-ttu-id="05e04-430">Por ejemplo, el siguiente filtro siempre ejecuta y establece un resultado de la acción (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un código de estado *422 - Entidad no procesable* cuando se produce un error en la negociación de contenido:</span><span class="sxs-lookup"><span data-stu-id="05e04-430">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="05e04-431">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="05e04-431">IFilterFactory</span></span>

<span data-ttu-id="05e04-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="05e04-432"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="05e04-433">Por tanto, una instancia de `IFilterFactory` se puede usar como una instancia de `IFilterMetadata` en cualquier parte de la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-433">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="05e04-434">Cuando el entorno de ejecución se prepara para invocar el filtro, intenta convertirlo a un `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="05e04-434">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="05e04-435">Si esa conversión se realiza correctamente, se llama al método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear la instancia `IFilterMetadata` que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="05e04-435">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="05e04-436">Esto proporciona un diseño flexible, dado que no hay que establecer la canalización de filtro exacta de forma explícita cuando la aplicación se inicia.</span><span class="sxs-lookup"><span data-stu-id="05e04-436">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="05e04-437">Puede implementar `IFilterFactory` con las implementaciones de atributos personalizados como método alternativo para crear filtros:</span><span class="sxs-lookup"><span data-stu-id="05e04-437">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="05e04-438">El filtro se aplica en el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="05e04-438">The filter is applied in the following code:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/SampleController.cs?name=snippet3&highlight=21)]

<span data-ttu-id="05e04-439">Pruebe el código anterior mediante la ejecución del [ejemplo de descarga](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span><span class="sxs-lookup"><span data-stu-id="05e04-439">Test the preceding code by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample):</span></span>

* <span data-ttu-id="05e04-440">Invoque las herramientas de desarrollador de F12.</span><span class="sxs-lookup"><span data-stu-id="05e04-440">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="05e04-441">Navegue a `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="05e04-441">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="05e04-442">Las herramientas de desarrollador F12 muestran los siguientes encabezados de respuesta agregados por el código de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="05e04-442">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="05e04-443">**author:** `Rick Anderson`</span><span class="sxs-lookup"><span data-stu-id="05e04-443">**author:** `Rick Anderson`</span></span>
* <span data-ttu-id="05e04-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="05e04-444">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="05e04-445">**internal:** `My header`</span><span class="sxs-lookup"><span data-stu-id="05e04-445">**internal:** `My header`</span></span>

<span data-ttu-id="05e04-446">El código anterior crea el encabezado de respuesta **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="05e04-446">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="05e04-447">IFilterFactory implementado en un atributo</span><span class="sxs-lookup"><span data-stu-id="05e04-447">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="05e04-448">Los filtros que implementan `IFilterFactory` son útiles para los filtros que:</span><span class="sxs-lookup"><span data-stu-id="05e04-448">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="05e04-449">No requieren pasar parámetros.</span><span class="sxs-lookup"><span data-stu-id="05e04-449">Don't require passing parameters.</span></span>
* <span data-ttu-id="05e04-450">Tienen dependencias de constructor que deben completarse por medio de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-450">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="05e04-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="05e04-451"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="05e04-452">`IFilterFactory` expone el método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear una instancia de <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="05e04-452">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="05e04-453">`CreateInstance` carga el tipo especificado desde el contenedor de servicios (inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="05e04-453">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="05e04-454">El código siguiente muestra tres métodos para aplicar `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="05e04-454">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="05e04-455">En el código anterior, decorar el método con `[SampleActionFilter]` es el enfoque preferido para aplicar `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="05e04-455">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="05e04-456">Uso de middleware en la canalización de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-456">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="05e04-457">Los filtros de recursos funcionan como el [middleware](xref:fundamentals/middleware/index), en el sentido de que se encargan de la ejecución de todo lo que viene después en la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-457">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="05e04-458">Pero los filtros se diferencian del middleware en que forman parte del runtime, lo que significa que tienen acceso al contexto y las construcciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-458">But filters differ from middleware in that they're part of the runtime, which means that they have access to context and constructs.</span></span>

<span data-ttu-id="05e04-459">Para usar middleware como un filtro, cree un tipo con un método `Configure` en el que se especifique el middleware para insertar en la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-459">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="05e04-460">El ejemplo siguiente usa el middleware de localización para establecer la referencia cultural actual de una solicitud:</span><span class="sxs-lookup"><span data-stu-id="05e04-460">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="05e04-461">Use <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> para ejecutar el middleware:</span><span class="sxs-lookup"><span data-stu-id="05e04-461">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/3.1sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="05e04-462">Los filtros de middleware se ejecutan en la misma fase de la canalización de filtro que los filtros de recursos, antes del enlace de modelos y después del resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-462">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="05e04-463">Siguientes acciones</span><span class="sxs-lookup"><span data-stu-id="05e04-463">Next actions</span></span>

* <span data-ttu-id="05e04-464">Consulte [Métodos de filtrado para páginas de Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="05e04-464">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="05e04-465">Para experimentar con los filtros, [descargue, pruebe y modifique el ejemplo de GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span><span class="sxs-lookup"><span data-stu-id="05e04-465">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/3.1sample).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="05e04-466">Por [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="05e04-466">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="05e04-467">Los *filtros* en ASP.NET Core permiten que se ejecute el código antes o después de determinadas fases de la canalización del procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="05e04-467">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="05e04-468">Los filtros integrados se encargan de tareas como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="05e04-468">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="05e04-469">Autorización (impedir el acceso a los recursos a un usuario que no está autorizado).</span><span class="sxs-lookup"><span data-stu-id="05e04-469">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="05e04-470">Almacenamiento en caché de respuestas (cortocircuitar la canalización de solicitud para devolver una respuesta almacenada en caché).</span><span class="sxs-lookup"><span data-stu-id="05e04-470">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="05e04-471">Se pueden crear filtros personalizados que se encarguen de cuestiones transversales.</span><span class="sxs-lookup"><span data-stu-id="05e04-471">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="05e04-472">Entre los ejemplos de cuestiones transversales se incluyen el control de errores, el almacenamiento en caché, la configuración, la autorización y el registro.</span><span class="sxs-lookup"><span data-stu-id="05e04-472">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="05e04-473">Los filtros evitan la duplicación de código.</span><span class="sxs-lookup"><span data-stu-id="05e04-473">Filters avoid duplicating code.</span></span> <span data-ttu-id="05e04-474">Así, por ejemplo, un filtro de excepción de control de errores puede consolidar el control de errores.</span><span class="sxs-lookup"><span data-stu-id="05e04-474">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="05e04-475">Este documento se aplica a Razor Pages, a los controladores de API y a los controladores con vistas.</span><span class="sxs-lookup"><span data-stu-id="05e04-475">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="05e04-476">[Vea o descargue el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="05e04-476">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="05e04-477">Funcionamiento de los filtros</span><span class="sxs-lookup"><span data-stu-id="05e04-477">How filters work</span></span>

<span data-ttu-id="05e04-478">Los filtros se ejecutan dentro de la *canalización de invocación de acciones de ASP.NET Core*, a veces denominada *canalización de filtro*.</span><span class="sxs-lookup"><span data-stu-id="05e04-478">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="05e04-479">La canalización de filtro se ejecuta después de que ASP.NET Core seleccione la acción que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="05e04-479">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La solicitud se procesa a través de las fases de otro middleware, del middleware de enrutamiento, de la selección de acción y de la canalización de invocación de acciones de ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="05e04-482">Tipos de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-482">Filter types</span></span>

<span data-ttu-id="05e04-483">Cada tipo de filtro se ejecuta en una fase diferente dentro de la canalización de filtro:</span><span class="sxs-lookup"><span data-stu-id="05e04-483">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="05e04-484">Los [filtros de autorización](#authorization-filters) se ejecutan en primer lugar y sirven para averiguar si el usuario está autorizado para realizar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="05e04-484">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="05e04-485">Los filtros de autorización pueden cortocircuitar la canalización si una solicitud no está autorizada.</span><span class="sxs-lookup"><span data-stu-id="05e04-485">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="05e04-486">[Filtros de recursos](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="05e04-486">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="05e04-487">Se ejecutan después de la autorización.</span><span class="sxs-lookup"><span data-stu-id="05e04-487">Run after authorization.</span></span>  
  * <span data-ttu-id="05e04-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> puede ejecutar código antes que el resto de la canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-488"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="05e04-489">Por ejemplo, `OnResourceExecuting` puede ejecutar código antes que el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="05e04-489">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="05e04-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> puede ejecutar el código una vez que el resto de la canalización se haya completado.</span><span class="sxs-lookup"><span data-stu-id="05e04-490"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="05e04-491">Los [filtros de acciones](#action-filters) pueden ejecutar código inmediatamente antes y después de llamar a un método de acción individual.</span><span class="sxs-lookup"><span data-stu-id="05e04-491">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="05e04-492">Se pueden usar para manipular los argumentos pasados a una acción y el resultado obtenido de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-492">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="05e04-493">Los filtros de acciones **no** se admiten en Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-493">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="05e04-494">Los [filtros de excepciones](#exception-filters) sirven para aplicar directivas globales a las excepciones no controladas que se producen antes de que se escriba algo en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="05e04-494">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="05e04-495">Los [filtros de resultados](#result-filters) pueden ejecutar código inmediatamente antes y después de la ejecución de resultados de acción individuales.</span><span class="sxs-lookup"><span data-stu-id="05e04-495">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="05e04-496">Se ejecutan solo cuando el método de acción se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="05e04-496">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="05e04-497">Son útiles para la lógica que debe regir la ejecución de la vista o el formateador.</span><span class="sxs-lookup"><span data-stu-id="05e04-497">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="05e04-498">En el siguiente diagrama se muestra cómo interactúan los tipos de filtro en la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-498">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La solicitud se procesa a través de las fases Filtros de autorización, Filtros de recursos, Enlace de modelos, Filtros de acciones, Ejecución de acciones/Conversión del resultado de acción, Filtros de excepción, Filtros de resultados y Ejecución del resultado.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="05e04-501">Implementación</span><span class="sxs-lookup"><span data-stu-id="05e04-501">Implementation</span></span>

<span data-ttu-id="05e04-502">Los filtros admiten implementaciones tanto sincrónicas como asincrónicas a través de diferentes definiciones de interfaz.</span><span class="sxs-lookup"><span data-stu-id="05e04-502">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="05e04-503">Los filtros sincrónicos pueden ejecutar código antes (`On-Stage-Executing`) y después (`On-Stage-Executed`) de la fase de canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-503">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="05e04-504">Por ejemplo, `OnActionExecuting` se llama antes de llamar al método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-504">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="05e04-505">`OnActionExecuted` se llama después de devolver el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-505">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="05e04-506">Los filtros asincrónicos definen un método `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="05e04-506">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="05e04-507">En el código anterior, `SampleAsyncActionFilter` tiene un delegado <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) que ejecuta el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-507">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="05e04-508">Cada uno de los métodos `On-Stage-ExecutionAsync` toman un `FilterType-ExecutionDelegate` que ejecuta la fase de canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-508">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="05e04-509">Varias fases de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-509">Multiple filter stages</span></span>

<span data-ttu-id="05e04-510">Se pueden implementar interfaces para varias fases de filtro en una sola clase.</span><span class="sxs-lookup"><span data-stu-id="05e04-510">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="05e04-511">Por ejemplo, la clase <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa `IActionFilter`, `IResultFilter` y sus equivalentes asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="05e04-511">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="05e04-512">Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero **no** ambas.</span><span class="sxs-lookup"><span data-stu-id="05e04-512">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="05e04-513">El entorno de ejecución comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, llama a la interfaz.</span><span class="sxs-lookup"><span data-stu-id="05e04-513">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="05e04-514">De lo contrario, llamará a métodos de interfaz sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="05e04-514">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="05e04-515">Si se implementan las interfaces asincrónicas y sincrónicas en una clase, solo se llama al método asincrónico.</span><span class="sxs-lookup"><span data-stu-id="05e04-515">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="05e04-516">Cuando se usan clases abstractas como <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, se invalidan solo los métodos sincrónicos o el método asincrónico de cada tipo de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-516">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="05e04-517">Atributos de filtros integrados</span><span class="sxs-lookup"><span data-stu-id="05e04-517">Built-in filter attributes</span></span>

<span data-ttu-id="05e04-518">ASP.NET Core incluye filtros integrados basados en atributos que se pueden personalizar y a partir de los cuales crear subclases.</span><span class="sxs-lookup"><span data-stu-id="05e04-518">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="05e04-519">Por ejemplo, el siguiente filtro de resultados agrega un encabezado a la respuesta:</span><span class="sxs-lookup"><span data-stu-id="05e04-519">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="05e04-520">Los atributos permiten a los filtros aceptar argumentos, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="05e04-520">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="05e04-521">Aplique el `AddHeaderAttribute` a un método de acción o controlador y especifique el nombre y el valor del encabezado HTTP:</span><span class="sxs-lookup"><span data-stu-id="05e04-521">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="05e04-522">Algunas de las interfaces de filtro tienen atributos correspondientes que se pueden usar como clases base en las implementaciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="05e04-522">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="05e04-523">Atributos de filtro:</span><span class="sxs-lookup"><span data-stu-id="05e04-523">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="05e04-524">Ámbitos del filtro y orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="05e04-524">Filter scopes and order of execution</span></span>

<span data-ttu-id="05e04-525">Un filtro se puede agregar a la canalización en uno de tres *ámbitos* posibles:</span><span class="sxs-lookup"><span data-stu-id="05e04-525">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="05e04-526">Mediante un atributo en una acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-526">Using an attribute on an action.</span></span>
* <span data-ttu-id="05e04-527">Mediante un atributo en un controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-527">Using an attribute on a controller.</span></span>
* <span data-ttu-id="05e04-528">Globalmente para todos los controladores y las acciones, tal como se muestra en el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="05e04-528">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="05e04-529">El código anterior agrega tres filtros globalmente mediante la colección [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="05e04-529">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="05e04-530">Orden de ejecución predeterminado</span><span class="sxs-lookup"><span data-stu-id="05e04-530">Default order of execution</span></span>

<span data-ttu-id="05e04-531">Cuando hay varios filtros *del mismo tipo*, el ámbito determina el orden predeterminado en el que esos filtros se van a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="05e04-531">When there are multiple filters *of the same type*, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="05e04-532">Los filtros globales delimitan los filtros de clase.</span><span class="sxs-lookup"><span data-stu-id="05e04-532">Global filters surround class filters.</span></span> <span data-ttu-id="05e04-533">Los filtros de clase delimitan los filtros de método.</span><span class="sxs-lookup"><span data-stu-id="05e04-533">Class filters surround method filters.</span></span>

<span data-ttu-id="05e04-534">Como resultado de este anidamiento de filtros, el código de filtros *posterior* se ejecuta en el orden inverso al código *anterior*.</span><span class="sxs-lookup"><span data-stu-id="05e04-534">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="05e04-535">La secuencia de filtro:</span><span class="sxs-lookup"><span data-stu-id="05e04-535">The filter sequence:</span></span>

* <span data-ttu-id="05e04-536">El código *anterior* de los filtros globales.</span><span class="sxs-lookup"><span data-stu-id="05e04-536">The *before* code of global filters.</span></span>
  * <span data-ttu-id="05e04-537">El código *anterior* de los filtros de controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-537">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="05e04-538">El código *anterior* de los filtros de métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-538">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="05e04-539">El código *posterior* de los filtros de métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-539">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="05e04-540">El código *posterior* de los filtros de controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-540">The *after* code of controller filters.</span></span>
* <span data-ttu-id="05e04-541">El código *posterior* de los filtros globales.</span><span class="sxs-lookup"><span data-stu-id="05e04-541">The *after* code of global filters.</span></span>
  
<span data-ttu-id="05e04-542">El ejemplo siguiente que ilustra el orden en el que se llama a los métodos de filtro relativos a filtros de acciones sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="05e04-542">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="05e04-543">Secuencia</span><span class="sxs-lookup"><span data-stu-id="05e04-543">Sequence</span></span> | <span data-ttu-id="05e04-544">Ámbito del filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-544">Filter scope</span></span> | <span data-ttu-id="05e04-545">Método de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-545">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="05e04-546">1</span><span class="sxs-lookup"><span data-stu-id="05e04-546">1</span></span> | <span data-ttu-id="05e04-547">Global</span><span class="sxs-lookup"><span data-stu-id="05e04-547">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="05e04-548">2</span><span class="sxs-lookup"><span data-stu-id="05e04-548">2</span></span> | <span data-ttu-id="05e04-549">Controlador</span><span class="sxs-lookup"><span data-stu-id="05e04-549">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="05e04-550">3</span><span class="sxs-lookup"><span data-stu-id="05e04-550">3</span></span> | <span data-ttu-id="05e04-551">Método</span><span class="sxs-lookup"><span data-stu-id="05e04-551">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="05e04-552">4</span><span class="sxs-lookup"><span data-stu-id="05e04-552">4</span></span> | <span data-ttu-id="05e04-553">Método</span><span class="sxs-lookup"><span data-stu-id="05e04-553">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="05e04-554">5</span><span class="sxs-lookup"><span data-stu-id="05e04-554">5</span></span> | <span data-ttu-id="05e04-555">Controlador</span><span class="sxs-lookup"><span data-stu-id="05e04-555">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="05e04-556">6</span><span class="sxs-lookup"><span data-stu-id="05e04-556">6</span></span> | <span data-ttu-id="05e04-557">Global</span><span class="sxs-lookup"><span data-stu-id="05e04-557">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="05e04-558">Esta secuencia pone de manifiesto lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="05e04-558">This sequence shows:</span></span>

* <span data-ttu-id="05e04-559">El filtro de método está anidado en el filtro de controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-559">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="05e04-560">El filtro de controlador está anidado en el filtro global.</span><span class="sxs-lookup"><span data-stu-id="05e04-560">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="05e04-561">Filtros de nivel de controlador y de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="05e04-561">Controller and Razor Page level filters</span></span>

<span data-ttu-id="05e04-562">Cada controlador que hereda de la clase base <xref:Microsoft.AspNetCore.Mvc.Controller> incluye los métodos [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) y [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="05e04-562">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="05e04-563">Estos métodos:</span><span class="sxs-lookup"><span data-stu-id="05e04-563">These methods:</span></span>

* <span data-ttu-id="05e04-564">Encapsulan los filtros que se ejecutan para una acción determinada.</span><span class="sxs-lookup"><span data-stu-id="05e04-564">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="05e04-565">`OnActionExecuting` se llama antes de cualquiera de los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-565">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="05e04-566">`OnActionExecuted` se llama después de todos los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-566">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="05e04-567">`OnActionExecutionAsync` se llama antes de cualquiera de los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-567">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="05e04-568">El código del filtro después de `next` se ejecuta después del método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-568">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="05e04-569">Por ejemplo, en el ejemplo de descarga, se aplica `MySampleActionFilter` globalmente al inicio.</span><span class="sxs-lookup"><span data-stu-id="05e04-569">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="05e04-570">El `TestController`:</span><span class="sxs-lookup"><span data-stu-id="05e04-570">The `TestController`:</span></span>

* <span data-ttu-id="05e04-571">Aplica `SampleActionFilterAttribute` (`[SampleActionFilter]`) a la acción `FilterTest2`.</span><span class="sxs-lookup"><span data-stu-id="05e04-571">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action.</span></span>
* <span data-ttu-id="05e04-572">Invalida `OnActionExecuting` y `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="05e04-572">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="05e04-573">Si se dirige a `https://localhost:5001/Test/FilterTest2`, se ejecuta el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="05e04-573">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="05e04-574">Para Razor Pages, consulte [Implementar filtros de páginas de Razor globalmente](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="05e04-574">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="05e04-575">Invalidación del orden predeterminado</span><span class="sxs-lookup"><span data-stu-id="05e04-575">Overriding the default order</span></span>

<span data-ttu-id="05e04-576">La secuencia de ejecución predeterminada se puede invalidar con la implementación de <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-576">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="05e04-577"><xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> expone la propiedad `IOrderedFilter` que tiene prioridad sobre el ámbito a la hora de determinar el orden de ejecución.</span><span class="sxs-lookup"><span data-stu-id="05e04-577">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="05e04-578">Un filtro con un valor `Order` menor:</span><span class="sxs-lookup"><span data-stu-id="05e04-578">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="05e04-579">Ejecuta el código *anterior* antes que el de un filtro con un valor mayor de `Order`.</span><span class="sxs-lookup"><span data-stu-id="05e04-579">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="05e04-580">Ejecuta el código *posterior* después que el de un filtro con un valor mayor de `Order`.</span><span class="sxs-lookup"><span data-stu-id="05e04-580">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="05e04-581">La propiedad `Order` se puede establecer con un parámetro de constructor:</span><span class="sxs-lookup"><span data-stu-id="05e04-581">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="05e04-582">Considere los mismos tres filtros de acción que se muestran en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="05e04-582">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="05e04-583">Si la propiedad `Order` del controlador y de los filtros globales está establecida en 1 y 2 respectivamente, el orden de ejecución se invierte.</span><span class="sxs-lookup"><span data-stu-id="05e04-583">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="05e04-584">Secuencia</span><span class="sxs-lookup"><span data-stu-id="05e04-584">Sequence</span></span> | <span data-ttu-id="05e04-585">Ámbito del filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-585">Filter scope</span></span> | <span data-ttu-id="05e04-586">Propiedad`Order`</span><span class="sxs-lookup"><span data-stu-id="05e04-586">`Order` property</span></span> | <span data-ttu-id="05e04-587">Método de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-587">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="05e04-588">1</span><span class="sxs-lookup"><span data-stu-id="05e04-588">1</span></span> | <span data-ttu-id="05e04-589">Método</span><span class="sxs-lookup"><span data-stu-id="05e04-589">Method</span></span> | <span data-ttu-id="05e04-590">0</span><span class="sxs-lookup"><span data-stu-id="05e04-590">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="05e04-591">2</span><span class="sxs-lookup"><span data-stu-id="05e04-591">2</span></span> | <span data-ttu-id="05e04-592">Controlador</span><span class="sxs-lookup"><span data-stu-id="05e04-592">Controller</span></span> | <span data-ttu-id="05e04-593">1</span><span class="sxs-lookup"><span data-stu-id="05e04-593">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="05e04-594">3</span><span class="sxs-lookup"><span data-stu-id="05e04-594">3</span></span> | <span data-ttu-id="05e04-595">Global</span><span class="sxs-lookup"><span data-stu-id="05e04-595">Global</span></span> | <span data-ttu-id="05e04-596">2</span><span class="sxs-lookup"><span data-stu-id="05e04-596">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="05e04-597">4</span><span class="sxs-lookup"><span data-stu-id="05e04-597">4</span></span> | <span data-ttu-id="05e04-598">Global</span><span class="sxs-lookup"><span data-stu-id="05e04-598">Global</span></span> | <span data-ttu-id="05e04-599">2</span><span class="sxs-lookup"><span data-stu-id="05e04-599">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="05e04-600">5</span><span class="sxs-lookup"><span data-stu-id="05e04-600">5</span></span> | <span data-ttu-id="05e04-601">Controlador</span><span class="sxs-lookup"><span data-stu-id="05e04-601">Controller</span></span> | <span data-ttu-id="05e04-602">1</span><span class="sxs-lookup"><span data-stu-id="05e04-602">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="05e04-603">6</span><span class="sxs-lookup"><span data-stu-id="05e04-603">6</span></span> | <span data-ttu-id="05e04-604">Método</span><span class="sxs-lookup"><span data-stu-id="05e04-604">Method</span></span> | <span data-ttu-id="05e04-605">0</span><span class="sxs-lookup"><span data-stu-id="05e04-605">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="05e04-606">La propiedad `Order` invalida el ámbito al determinar el orden en el que se ejecutarán los filtros.</span><span class="sxs-lookup"><span data-stu-id="05e04-606">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="05e04-607">Los filtros se clasifican por orden en primer lugar y, después, se usa el ámbito para priorizar en caso de igualdad.</span><span class="sxs-lookup"><span data-stu-id="05e04-607">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="05e04-608">Todos los filtros integrados implementan `IOrderedFilter` y establecen el valor predeterminado de `Order` en 0.</span><span class="sxs-lookup"><span data-stu-id="05e04-608">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="05e04-609">En los filtros integrados, el ámbito determina el orden, a menos que `Order` se establezca en un valor distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="05e04-609">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="05e04-610">Cancelación y cortocircuito</span><span class="sxs-lookup"><span data-stu-id="05e04-610">Cancellation and short-circuiting</span></span>

<span data-ttu-id="05e04-611">La canalización de filtro se puede cortocircuitar en cualquier momento mediante el establecimiento de la propiedad <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> en el parámetro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> que se ha proporcionado al método de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-611">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="05e04-612">Por ejemplo, el siguiente filtro de recursos impide que el resto de la canalización se ejecute:</span><span class="sxs-lookup"><span data-stu-id="05e04-612">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="05e04-613">En el siguiente código, tanto el filtro `ShortCircuitingResourceFilter` como el filtro `AddHeader` tienen como destino el método de acción `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="05e04-613">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="05e04-614">El `ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="05e04-614">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="05e04-615">Se ejecuta en primer lugar, porque es un filtro de recursos y `AddHeader` es un filtro de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-615">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="05e04-616">Cortocircuita el resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-616">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="05e04-617">Por tanto, el filtro `AddHeader` nunca se ejecuta en relación con la acción `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="05e04-617">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="05e04-618">Este comportamiento sería el mismo si ambos filtros se aplicaran en el nivel de método de acción, siempre y cuando `ShortCircuitingResourceFilter` se haya ejecutado primero.</span><span class="sxs-lookup"><span data-stu-id="05e04-618">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="05e04-619">`ShortCircuitingResourceFilter` se ejecuta primero debido a su tipo de filtro o al uso explícito de la propiedad `Order`.</span><span class="sxs-lookup"><span data-stu-id="05e04-619">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="05e04-620">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="05e04-620">Dependency injection</span></span>

<span data-ttu-id="05e04-621">Los filtros se pueden agregar por tipo o por instancia.</span><span class="sxs-lookup"><span data-stu-id="05e04-621">Filters can be added by type or by instance.</span></span> <span data-ttu-id="05e04-622">Si se agrega una instancia, esta se utiliza para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="05e04-622">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="05e04-623">Si se agrega un tipo, se activa por tipo.</span><span class="sxs-lookup"><span data-stu-id="05e04-623">If a type is added, it's type-activated.</span></span> <span data-ttu-id="05e04-624">Un filtro activado por tipo significa:</span><span class="sxs-lookup"><span data-stu-id="05e04-624">A type-activated filter means:</span></span>

* <span data-ttu-id="05e04-625">Se crea una instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="05e04-625">An instance is created for each request.</span></span>
* <span data-ttu-id="05e04-626">Las dependencias de constructor se rellenan mediante la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05e04-626">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="05e04-627">Los filtros que se implementan como atributos y se agregan directamente a las clases de controlador o a los métodos de acción no pueden tener dependencias de constructor proporcionadas por la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="05e04-627">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="05e04-628">Las dependencias de constructor no pueden proporcionarse mediante la inserción de dependencias porque:</span><span class="sxs-lookup"><span data-stu-id="05e04-628">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="05e04-629">Los atributos deben tener los parámetros de constructor proporcionados allá donde se apliquen.</span><span class="sxs-lookup"><span data-stu-id="05e04-629">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="05e04-630">Se trata de una limitación de cómo funcionan los atributos.</span><span class="sxs-lookup"><span data-stu-id="05e04-630">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="05e04-631">Los filtros siguientes admiten dependencias de constructor proporcionadas en la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-631">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="05e04-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> se implementa en el atributo.</span><span class="sxs-lookup"><span data-stu-id="05e04-632"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="05e04-633">Los filtros anteriores se pueden aplicar a un método de controlador o de acción:</span><span class="sxs-lookup"><span data-stu-id="05e04-633">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="05e04-634">Los registradores están disponibles en la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-634">Loggers are available from DI.</span></span> <span data-ttu-id="05e04-635">Sin embargo, evite crear y utilizar filtros únicamente con fines de registro.</span><span class="sxs-lookup"><span data-stu-id="05e04-635">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="05e04-636">El [registro del marco integrado](xref:fundamentals/logging/index) proporciona normalmente lo que se necesita para el registro.</span><span class="sxs-lookup"><span data-stu-id="05e04-636">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="05e04-637">Registro agregado a los filtros:</span><span class="sxs-lookup"><span data-stu-id="05e04-637">Logging added to filters:</span></span>

* <span data-ttu-id="05e04-638">Debe centrarse en cuestiones de dominio empresarial o en el comportamiento específico del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-638">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="05e04-639">**No** debe registrar acciones u otros eventos del marco.</span><span class="sxs-lookup"><span data-stu-id="05e04-639">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="05e04-640">Los filtros integrados registran acciones y eventos del marco.</span><span class="sxs-lookup"><span data-stu-id="05e04-640">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="05e04-641">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="05e04-641">ServiceFilterAttribute</span></span>

<span data-ttu-id="05e04-642">Los tipos de implementación de filtro de servicio se registran en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="05e04-642">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="05e04-643"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera una instancia del filtro de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-643">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="05e04-644">El código siguiente muestra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="05e04-644">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="05e04-645">En el código siguiente, `AddHeaderResultServiceFilter` se agrega al contenedor de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-645">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="05e04-646">En el código siguiente, el atributo `ServiceFilter` recupera una instancia del filtro `AddHeaderResultServiceFilter` desde la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-646">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="05e04-647">Al usar `ServiceFilterAttribute`, si se establece [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="05e04-647">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="05e04-648">Proporciona una sugerencia que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó.</span><span class="sxs-lookup"><span data-stu-id="05e04-648">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="05e04-649">El entorno de ejecución de ASP.NET Core no garantiza:</span><span class="sxs-lookup"><span data-stu-id="05e04-649">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="05e04-650">Que se creará una única instancia del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-650">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="05e04-651">El filtro no volverá a solicitarse desde el contenedor de inserción de dependencias en algún momento posterior.</span><span class="sxs-lookup"><span data-stu-id="05e04-651">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="05e04-652">No debe usarse con un filtro que depende de servicios con una duración distinta de singleton.</span><span class="sxs-lookup"><span data-stu-id="05e04-652">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="05e04-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="05e04-653"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="05e04-654">`IFilterFactory` expone el método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear una instancia de <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="05e04-654">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="05e04-655">`CreateInstance` carga el tipo especificado desde la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-655">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="05e04-656">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="05e04-656">TypeFilterAttribute</span></span>

<span data-ttu-id="05e04-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> es similar a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, pero su tipo no se resuelve directamente desde el contenedor de inserción de dependencias,</span><span class="sxs-lookup"><span data-stu-id="05e04-657"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="05e04-658">sino que crea una instancia del tipo usando el elemento <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="05e04-658">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="05e04-659">Dado que los tipos `TypeFilterAttribute` no se resuelven directamente desde el contenedor de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="05e04-659">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="05e04-660">Los tipos a los que se hace referencia con `TypeFilterAttribute` no tienen que estar registrados con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-660">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="05e04-661">Sus dependencias se completan a través del contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-661">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="05e04-662">`TypeFilterAttribute` puede aceptar opcionalmente argumentos de constructor del tipo en cuestión.</span><span class="sxs-lookup"><span data-stu-id="05e04-662">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="05e04-663">Al usar `TypeFilterAttribute`, si se establece [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="05e04-663">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="05e04-664">Proporciona una sugerencia que indica que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó.</span><span class="sxs-lookup"><span data-stu-id="05e04-664">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="05e04-665">El entorno de ejecución de ASP.NET Core no ofrece ninguna garantía de que se vaya a crear una única instancia del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-665">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="05e04-666">No debe usarse con un filtro que depende de servicios con una duración distinta de singleton.</span><span class="sxs-lookup"><span data-stu-id="05e04-666">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="05e04-667">En el siguiente ejemplo se muestra cómo pasar argumentos a un tipo mediante `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="05e04-667">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="05e04-668">Filtros de autorización</span><span class="sxs-lookup"><span data-stu-id="05e04-668">Authorization filters</span></span>

<span data-ttu-id="05e04-669">Filtros de autorización:</span><span class="sxs-lookup"><span data-stu-id="05e04-669">Authorization filters:</span></span>

* <span data-ttu-id="05e04-670">Son los primeros filtros que se ejecutan en la canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-670">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="05e04-671">Controlan el acceso a los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-671">Control access to action methods.</span></span>
* <span data-ttu-id="05e04-672">Tienen un método anterior, pero no uno posterior.</span><span class="sxs-lookup"><span data-stu-id="05e04-672">Have a before method, but no after method.</span></span>

<span data-ttu-id="05e04-673">Los filtros de autorización personalizados requieren un marco de autorización personalizado.</span><span class="sxs-lookup"><span data-stu-id="05e04-673">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="05e04-674">Es preferible configurar directivas de autorización o escribir una directiva de autorización personalizada a escribir un filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="05e04-674">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="05e04-675">El filtro de autorización integrado:</span><span class="sxs-lookup"><span data-stu-id="05e04-675">The built-in authorization filter:</span></span>

* <span data-ttu-id="05e04-676">Llama a la autorización del sistema.</span><span class="sxs-lookup"><span data-stu-id="05e04-676">Calls the authorization system.</span></span>
* <span data-ttu-id="05e04-677">No autoriza las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="05e04-677">Does not authorize requests.</span></span>

<span data-ttu-id="05e04-678">**No** inicie excepciones dentro de los filtros de autorización:</span><span class="sxs-lookup"><span data-stu-id="05e04-678">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="05e04-679">La excepción no se controlará.</span><span class="sxs-lookup"><span data-stu-id="05e04-679">The exception will not be handled.</span></span>
* <span data-ttu-id="05e04-680">Los filtros de excepciones no controlarán la excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-680">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="05e04-681">Considere la posibilidad de emitir un desafío cuando se produzca una excepción en un filtro de autorizaciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-681">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="05e04-682">[Aquí](xref:security/authorization/introduction) encontrará más información sobre la autorización.</span><span class="sxs-lookup"><span data-stu-id="05e04-682">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="05e04-683">Filtros de recursos</span><span class="sxs-lookup"><span data-stu-id="05e04-683">Resource filters</span></span>

<span data-ttu-id="05e04-684">Filtros de recursos:</span><span class="sxs-lookup"><span data-stu-id="05e04-684">Resource filters:</span></span>

* <span data-ttu-id="05e04-685">Implementan la interfaz <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-685">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="05e04-686">La ejecución encapsula la mayor parte de la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-686">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="05e04-687">Los [filtros de autorizaciones](#authorization-filters) son los únicos que se ejecutan antes que los filtros de recursos.</span><span class="sxs-lookup"><span data-stu-id="05e04-687">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="05e04-688">Los filtros de recursos son útiles para cortocircuitar la mayor parte de la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-688">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="05e04-689">Por ejemplo, un filtro de almacenamiento en caché puede evitar que se ejecute el resto de la canalización en un acierto de caché.</span><span class="sxs-lookup"><span data-stu-id="05e04-689">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="05e04-690">Ejemplos de filtros de recursos:</span><span class="sxs-lookup"><span data-stu-id="05e04-690">Resource filter examples:</span></span>

* <span data-ttu-id="05e04-691">[El filtro de recursos de cortocircuito](#short-circuiting-resource-filter) mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="05e04-691">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="05e04-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="05e04-692">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="05e04-693">Evita que el enlace de modelos tenga acceso a los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="05e04-693">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="05e04-694">Se utiliza cuando hay cargas de archivos muy voluminosos para impedir que los datos del formulario se lean en la memoria.</span><span class="sxs-lookup"><span data-stu-id="05e04-694">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="05e04-695">Filtros de acciones</span><span class="sxs-lookup"><span data-stu-id="05e04-695">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05e04-696">Los filtros de acción **no** se aplican a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="05e04-696">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="05e04-697">Razor Pages admite <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> y <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-697">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="05e04-698">Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).</span><span class="sxs-lookup"><span data-stu-id="05e04-698">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="05e04-699">Filtros de acciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-699">Action filters:</span></span>

* <span data-ttu-id="05e04-700">Implementan la interfaz <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-700">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="05e04-701">Su ejecución rodea la ejecución de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-701">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="05e04-702">El código siguiente muestra un ejemplo de filtro de acciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-702">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="05e04-703"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> ofrece las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="05e04-703">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="05e04-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>: permite leer las entradas de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-704"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="05e04-705"><xref:Microsoft.AspNetCore.Mvc.Controller>: permite manipular la instancia del controlador.</span><span class="sxs-lookup"><span data-stu-id="05e04-705"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="05e04-706"><xref:System.Web.Mvc.ActionExecutingContext.Result>: si se establece `Result`, se cortocircuita la ejecución del método de acción y de los filtros de acciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="05e04-706"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="05e04-707">Inicio de una excepción en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="05e04-707">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="05e04-708">Impide la ejecución de los filtros subsiguientes.</span><span class="sxs-lookup"><span data-stu-id="05e04-708">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="05e04-709">A diferencia del establecimiento de `Result`, se trata como un error en lugar de como un resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="05e04-709">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="05e04-710"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> proporciona `Controller` y `Result`, además de las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="05e04-710">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="05e04-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: es true si otro filtro ha cortocircuitado la ejecución de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-711"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="05e04-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: es un valor distinto de NULL si la acción o un filtro de acción de ejecución anterior han producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-712"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="05e04-713">Si se establece esta propiedad en un valor NULL:</span><span class="sxs-lookup"><span data-stu-id="05e04-713">Setting this property to null:</span></span>

  * <span data-ttu-id="05e04-714">Controla la excepción eficazmente.</span><span class="sxs-lookup"><span data-stu-id="05e04-714">Effectively handles the exception.</span></span>
  * <span data-ttu-id="05e04-715">`Result` se ejecuta como si se devolviera desde el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-715">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="05e04-716">En un `IAsyncActionFilter`, una llamada a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="05e04-716">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="05e04-717">Ejecuta cualquier filtro de acciones posterior y el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-717">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="05e04-718">Devuelve `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="05e04-718">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="05e04-719">Para cortocircuitar esto, asigne <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a una instancia de resultado y no llame a `next` (la clase `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="05e04-719">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="05e04-720">El marco proporciona una clase <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> abstracta de la que se pueden crear subclases.</span><span class="sxs-lookup"><span data-stu-id="05e04-720">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="05e04-721">Se puede usar el filtro de acción `OnActionExecuting` para:</span><span class="sxs-lookup"><span data-stu-id="05e04-721">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="05e04-722">Validar el estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="05e04-722">Validate model state.</span></span>
* <span data-ttu-id="05e04-723">Devolver un error si el estado no es válido.</span><span class="sxs-lookup"><span data-stu-id="05e04-723">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="05e04-724">El método `OnActionExecuted` se ejecuta después del método de acción:</span><span class="sxs-lookup"><span data-stu-id="05e04-724">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="05e04-725">Y puede ver y manipular los resultados de la acción a través de la propiedad <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="05e04-725">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="05e04-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> se establece en true si otro filtro ha cortocircuitado la ejecución de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-726"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="05e04-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> se establece en un valor distinto de NULL si la acción o un filtro de acción posterior han producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-727"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="05e04-728">Si `Exception` se establece como nulo:</span><span class="sxs-lookup"><span data-stu-id="05e04-728">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="05e04-729">Controla una excepción eficazmente.</span><span class="sxs-lookup"><span data-stu-id="05e04-729">Effectively handles an exception.</span></span>
  * <span data-ttu-id="05e04-730">`ActionExecutedContext.Result` se ejecuta como si se devolviera con normalidad desde el método de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-730">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="05e04-731">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="05e04-731">Exception filters</span></span>

<span data-ttu-id="05e04-732">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-732">Exception filters:</span></span>

* <span data-ttu-id="05e04-733">Implementan <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="05e04-733">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="05e04-734">Se pueden usar para implementar directivas de control de errores comunes.</span><span class="sxs-lookup"><span data-stu-id="05e04-734">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="05e04-735">En el siguiente filtro de excepciones de ejemplo se usa una vista de error personalizada para mostrar los detalles sobre las excepciones que se producen cuando la aplicación está en fase de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="05e04-735">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilter.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="05e04-736">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-736">Exception filters:</span></span>

* <span data-ttu-id="05e04-737">No tienen eventos anteriores ni posteriores.</span><span class="sxs-lookup"><span data-stu-id="05e04-737">Don't have before and after events.</span></span>
* <span data-ttu-id="05e04-738">Implementan <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="05e04-738">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="05e04-739">Controlan las excepciones sin controlar que se producen en Razor Pages, al crear controladores, en el [enlace de modelos](xref:mvc/models/model-binding), en los filtros de acciones o en los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-739">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="05e04-740">**No** detectan aquellas excepciones que se produzcan en los filtros de recursos, en los filtros de resultados o en la ejecución de resultados de MVC.</span><span class="sxs-lookup"><span data-stu-id="05e04-740">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="05e04-741">Para controlar una excepción, establezca la propiedad <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> en `true` o escriba una respuesta.</span><span class="sxs-lookup"><span data-stu-id="05e04-741">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="05e04-742">Esto detiene la propagación de la excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-742">This stops propagation of the exception.</span></span> <span data-ttu-id="05e04-743">Un filtro de excepciones no tiene capacidad para convertir una excepción en un proceso "correcto".</span><span class="sxs-lookup"><span data-stu-id="05e04-743">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="05e04-744">Eso solo lo pueden hacer los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-744">Only an action filter can do that.</span></span>

<span data-ttu-id="05e04-745">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="05e04-745">Exception filters:</span></span>

* <span data-ttu-id="05e04-746">Son adecuados para interceptar las excepciones que se producen en las acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-746">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="05e04-747">No son tan flexibles como el middleware de control de errores.</span><span class="sxs-lookup"><span data-stu-id="05e04-747">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="05e04-748">Es preferible usar middleware de control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-748">Prefer middleware for exception handling.</span></span> <span data-ttu-id="05e04-749">Utilice los filtros de excepciones solo cuando el control de errores *es diferente* en función del método de acción que se llama.</span><span class="sxs-lookup"><span data-stu-id="05e04-749">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="05e04-750">Por ejemplo, puede que una aplicación tenga métodos de acción tanto para los puntos de conexión de API como para las vistas/HTML.</span><span class="sxs-lookup"><span data-stu-id="05e04-750">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="05e04-751">Los puntos de conexión de API podrían devolver información sobre errores como JSON, mientras que las acciones basadas en vistas podrían devolver una página de error como HTML.</span><span class="sxs-lookup"><span data-stu-id="05e04-751">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="05e04-752">Filtros de resultados</span><span class="sxs-lookup"><span data-stu-id="05e04-752">Result filters</span></span>

<span data-ttu-id="05e04-753">Filtros de resultados:</span><span class="sxs-lookup"><span data-stu-id="05e04-753">Result filters:</span></span>

* <span data-ttu-id="05e04-754">Implementar una interfaz:</span><span class="sxs-lookup"><span data-stu-id="05e04-754">Implement an interface:</span></span>
  * <span data-ttu-id="05e04-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="05e04-755"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="05e04-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="05e04-756"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="05e04-757">Su ejecución rodea la ejecución de los resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-757">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="05e04-758">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="05e04-758">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="05e04-759">El código siguiente muestra un filtro de resultados que agrega un encabezado HTTP:</span><span class="sxs-lookup"><span data-stu-id="05e04-759">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="05e04-760">El tipo de resultado que se ejecute dependerá de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-760">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="05e04-761">Una acción que devuelve una vista incluye todo el procesamiento de Razor como parte del elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="05e04-761">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="05e04-762">Un método API puede llevar a cabo algunas funciones de serialización como parte de la ejecución del resultado.</span><span class="sxs-lookup"><span data-stu-id="05e04-762">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="05e04-763">[Aquí](xref:mvc/controllers/actions) encontrará más información sobre los resultados de acciones.</span><span class="sxs-lookup"><span data-stu-id="05e04-763">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="05e04-764">Los filtros de resultados solo se ejecutan cuando una acción o un filtro de acción genera un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-764">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="05e04-765">Los filtros de resultados no se ejecutan cuando:</span><span class="sxs-lookup"><span data-stu-id="05e04-765">Result filters are not executed when:</span></span>

* <span data-ttu-id="05e04-766">Un filtro de autorización o un filtro de recursos genera un cortocircuito en la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-766">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="05e04-767">Un filtro de excepciones controla una excepción al producir un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-767">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="05e04-768">El método <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> puede cortocircuitar la ejecución del resultado de la acción y de los filtros de resultados posteriores mediante el establecimiento de <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> en `true`.</span><span class="sxs-lookup"><span data-stu-id="05e04-768">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="05e04-769">Escriba en el objeto de respuesta cuando el proceso se cortocircuite, ya que así evitará que se genere una respuesta vacía.</span><span class="sxs-lookup"><span data-stu-id="05e04-769">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="05e04-770">Si se inicia una excepción en `IResultFilter.OnResultExecuting`, sucederá lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="05e04-770">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="05e04-771">Se impedirá la ejecución del resultado de la acción y de los filtros subsiguientes.</span><span class="sxs-lookup"><span data-stu-id="05e04-771">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="05e04-772">Se tratará como un error en lugar de como un resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="05e04-772">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="05e04-773">Cuando se ejecuta el método <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>, es probable que la respuesta ya se haya enviado al cliente.</span><span class="sxs-lookup"><span data-stu-id="05e04-773">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs, the response has likely already been sent to the client.</span></span> <span data-ttu-id="05e04-774">En este caso, no se puede cambiar más.</span><span class="sxs-lookup"><span data-stu-id="05e04-774">If the response has already been sent to the client, it cannot be changed further.</span></span>

<span data-ttu-id="05e04-775">`ResultExecutedContext.Canceled` se establece en `true` si otro filtro ha cortocircuitado la ejecución del resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-775">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="05e04-776">`ResultExecutedContext.Exception` se establece en un valor distinto de NULL si el resultado de la acción o un filtro de resultado posterior ha producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-776">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="05e04-777">Establecer `Exception` en un valor NULL hace que una excepción se "controle" de forma eficaz y evita que ASP.NET Core vuelva a producir dicha excepción más adelante en la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-777">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="05e04-778">No hay una forma confiable de escribir datos en una respuesta cuando se controla una excepción en un filtro de resultados.</span><span class="sxs-lookup"><span data-stu-id="05e04-778">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="05e04-779">Si los encabezados ya se han vaciado en el cliente si el resultado de una acción inicia una excepción, no hay ningún mecanismo confiable que permita enviar un código de error.</span><span class="sxs-lookup"><span data-stu-id="05e04-779">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="05e04-780">En un elemento <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una llamada a `await next` en <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> ejecuta cualquier filtro de resultados posterior y el resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-780">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="05e04-781">Para cortocircuitarlo, establezca [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) en `true` y no llame a `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="05e04-781">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="05e04-782">El marco proporciona una clase abstracta `ResultFilterAttribute` de la que se pueden crear subclases.</span><span class="sxs-lookup"><span data-stu-id="05e04-782">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="05e04-783">La clase [AddHeaderAttribute](#add-header-attribute) mostrada anteriormente es un ejemplo de un atributo de filtro de resultados.</span><span class="sxs-lookup"><span data-stu-id="05e04-783">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="05e04-784">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="05e04-784">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="05e04-785">Las interfaces <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> declaran una implementación <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> que se ejecuta para obtener todos los resultados de la acción.</span><span class="sxs-lookup"><span data-stu-id="05e04-785">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="05e04-786">Esto incluye los resultados de la acción generados por:</span><span class="sxs-lookup"><span data-stu-id="05e04-786">This includes action results produced by:</span></span>

* <span data-ttu-id="05e04-787">Filtros de autorización y filtros de recursos que generan un cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="05e04-787">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="05e04-788">Filtros de excepción.</span><span class="sxs-lookup"><span data-stu-id="05e04-788">Exception filters.</span></span>

<span data-ttu-id="05e04-789">Por ejemplo, el siguiente filtro siempre ejecuta y establece un resultado de la acción (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un código de estado *422 - Entidad no procesable* cuando se produce un error en la negociación de contenido:</span><span class="sxs-lookup"><span data-stu-id="05e04-789">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="05e04-790">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="05e04-790">IFilterFactory</span></span>

<span data-ttu-id="05e04-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="05e04-791"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="05e04-792">Por tanto, una instancia de `IFilterFactory` se puede usar como una instancia de `IFilterMetadata` en cualquier parte de la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-792">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="05e04-793">Cuando el entorno de ejecución se prepara para invocar el filtro, intenta convertirlo a un `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="05e04-793">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="05e04-794">Si esa conversión se realiza correctamente, se llama al método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear la instancia `IFilterMetadata` que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="05e04-794">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="05e04-795">Esto proporciona un diseño flexible, dado que no hay que establecer la canalización de filtro exacta de forma explícita cuando la aplicación se inicia.</span><span class="sxs-lookup"><span data-stu-id="05e04-795">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="05e04-796">Puede implementar `IFilterFactory` con las implementaciones de atributos personalizados como método alternativo para crear filtros:</span><span class="sxs-lookup"><span data-stu-id="05e04-796">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="05e04-797">El código anterior se puede probar mediante la ejecución del [ejemplo de descargar](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="05e04-797">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="05e04-798">Invoque las herramientas de desarrollador de F12.</span><span class="sxs-lookup"><span data-stu-id="05e04-798">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="05e04-799">Navegue a `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="05e04-799">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`.</span></span>

<span data-ttu-id="05e04-800">Las herramientas de desarrollador F12 muestran los siguientes encabezados de respuesta agregados por el código de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="05e04-800">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="05e04-801">**author:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="05e04-801">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="05e04-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="05e04-802">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="05e04-803">**internal:** `My header`</span><span class="sxs-lookup"><span data-stu-id="05e04-803">**internal:** `My header`</span></span>

<span data-ttu-id="05e04-804">El código anterior crea el encabezado de respuesta **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="05e04-804">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="05e04-805">IFilterFactory implementado en un atributo</span><span class="sxs-lookup"><span data-stu-id="05e04-805">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="05e04-806">Los filtros que implementan `IFilterFactory` son útiles para los filtros que:</span><span class="sxs-lookup"><span data-stu-id="05e04-806">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="05e04-807">No requieren pasar parámetros.</span><span class="sxs-lookup"><span data-stu-id="05e04-807">Don't require passing parameters.</span></span>
* <span data-ttu-id="05e04-808">Tienen dependencias de constructor que deben completarse por medio de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="05e04-808">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="05e04-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="05e04-809"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="05e04-810">`IFilterFactory` expone el método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear una instancia de <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="05e04-810">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="05e04-811">`CreateInstance` carga el tipo especificado desde el contenedor de servicios (inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="05e04-811">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="05e04-812">El código siguiente muestra tres métodos para aplicar `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="05e04-812">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="05e04-813">En el código anterior, decorar el método con `[SampleActionFilter]` es el enfoque preferido para aplicar `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="05e04-813">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="05e04-814">Uso de middleware en la canalización de filtro</span><span class="sxs-lookup"><span data-stu-id="05e04-814">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="05e04-815">Los filtros de recursos funcionan como el [middleware](xref:fundamentals/middleware/index), en el sentido de que se encargan de la ejecución de todo lo que viene después en la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-815">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="05e04-816">Pero los filtros se diferencian del middleware en que forman parte del entorno de ejecución de ASP.NET Core, lo que significa que tienen acceso al contexto y las construcciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="05e04-816">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="05e04-817">Para usar middleware como un filtro, cree un tipo con un método `Configure` en el que se especifique el middleware para insertar en la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="05e04-817">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="05e04-818">El ejemplo siguiente usa el middleware de localización para establecer la referencia cultural actual de una solicitud:</span><span class="sxs-lookup"><span data-stu-id="05e04-818">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,22)]

<span data-ttu-id="05e04-819">Use <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> para ejecutar el middleware:</span><span class="sxs-lookup"><span data-stu-id="05e04-819">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="05e04-820">Los filtros de middleware se ejecutan en la misma fase de la canalización de filtro que los filtros de recursos, antes del enlace de modelos y después del resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="05e04-820">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="05e04-821">Siguientes acciones</span><span class="sxs-lookup"><span data-stu-id="05e04-821">Next actions</span></span>

* <span data-ttu-id="05e04-822">Consulte [Métodos de filtrado para páginas de Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="05e04-822">See [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>
* <span data-ttu-id="05e04-823">Para experimentar con los filtros, [descargue, pruebe y modifique el ejemplo de GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="05e04-823">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>

::: moniker-end
