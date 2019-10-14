---
title: Filtros en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo funcionan los filtros y cómo se pueden usar en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: mvc/controllers/filters
ms.openlocfilehash: ed48c2074360768b8d8c5af7057b353b00592394
ms.sourcegitcommit: 73a451e9a58ac7102f90b608d661d8c23dd9bbaf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72037694"
---
# <a name="filters-in-aspnet-core"></a><span data-ttu-id="6d3c5-103">Filtros en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6d3c5-103">Filters in ASP.NET Core</span></span>

<span data-ttu-id="6d3c5-104">Por [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/) y [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="6d3c5-104">By [Kirk Larkin](https://github.com/serpent5), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra/), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="6d3c5-105">Los *filtros* en ASP.NET Core permiten que se ejecute el código antes o después de determinadas fases de la canalización del procesamiento de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-105">*Filters* in ASP.NET Core allow code to be run before or after specific stages in the request processing pipeline.</span></span>

<span data-ttu-id="6d3c5-106">Los filtros integrados se encargan de tareas como las siguientes:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-106">Built-in filters handle tasks such as:</span></span>

* <span data-ttu-id="6d3c5-107">Autorización (impedir el acceso a los recursos a un usuario que no está autorizado).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-107">Authorization (preventing access to resources a user isn't authorized for).</span></span>
* <span data-ttu-id="6d3c5-108">Almacenamiento en caché de respuestas (cortocircuitar la canalización de solicitud para devolver una respuesta almacenada en caché).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-108">Response caching (short-circuiting the request pipeline to return a cached response).</span></span>

<span data-ttu-id="6d3c5-109">Se pueden crear filtros personalizados que se encarguen de cuestiones transversales.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-109">Custom filters can be created to handle cross-cutting concerns.</span></span> <span data-ttu-id="6d3c5-110">Entre los ejemplos de cuestiones transversales se incluyen el control de errores, el almacenamiento en caché, la configuración, la autorización y el registro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-110">Examples of cross-cutting concerns include error handling, caching, configuration, authorization, and logging.</span></span>  <span data-ttu-id="6d3c5-111">Los filtros evitan la duplicación de código.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-111">Filters avoid duplicating code.</span></span> <span data-ttu-id="6d3c5-112">Así, por ejemplo, un filtro de excepción de control de errores puede consolidar el control de errores.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-112">For example, an error handling exception filter could consolidate error handling.</span></span>

<span data-ttu-id="6d3c5-113">Este documento se aplica a Razor Pages, a los controladores de API y a los controladores con vistas.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-113">This document applies to Razor Pages, API controllers, and controllers with views.</span></span>

<span data-ttu-id="6d3c5-114">[Vea o descargue el ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-114">[View or download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="how-filters-work"></a><span data-ttu-id="6d3c5-115">Funcionamiento de los filtros</span><span class="sxs-lookup"><span data-stu-id="6d3c5-115">How filters work</span></span>

<span data-ttu-id="6d3c5-116">Los filtros se ejecutan dentro de la *canalización de invocación de acciones de ASP.NET Core*, a veces denominada *canalización de filtro*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-116">Filters run within the *ASP.NET Core action invocation pipeline*, sometimes referred to as the *filter pipeline*.</span></span>  <span data-ttu-id="6d3c5-117">La canalización de filtro se ejecuta después de que ASP.NET Core seleccione la acción que se va a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-117">The filter pipeline runs after ASP.NET Core selects the action to execute.</span></span>

![La solicitud se procesa a través de las fases de otro middleware, del middleware de enrutamiento, de la selección de acción y de la canalización de invocación de acciones de ASP.NET Core.](filters/_static/filter-pipeline-1.png)

### <a name="filter-types"></a><span data-ttu-id="6d3c5-120">Tipos de filtro</span><span class="sxs-lookup"><span data-stu-id="6d3c5-120">Filter types</span></span>

<span data-ttu-id="6d3c5-121">Cada tipo de filtro se ejecuta en una fase diferente dentro de la canalización de filtro:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-121">Each filter type is executed at a different stage in the filter pipeline:</span></span>

* <span data-ttu-id="6d3c5-122">Los [filtros de autorización](#authorization-filters) se ejecutan en primer lugar y sirven para averiguar si el usuario está autorizado para realizar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-122">[Authorization filters](#authorization-filters) run first and are used to determine whether the user is authorized for the request.</span></span> <span data-ttu-id="6d3c5-123">Los filtros de autorización pueden cortocircuitar la canalización si una solicitud no está autorizada.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-123">Authorization filters short-circuit the pipeline if the request is unauthorized.</span></span>

* <span data-ttu-id="6d3c5-124">[Filtros de recursos](#resource-filters):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-124">[Resource filters](#resource-filters):</span></span>

  * <span data-ttu-id="6d3c5-125">Se ejecutan después de la autorización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-125">Run after authorization.</span></span>  
  * <span data-ttu-id="6d3c5-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> puede ejecutar código antes que el resto de la canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-126"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuting*> can run code before the rest of the filter pipeline.</span></span> <span data-ttu-id="6d3c5-127">Por ejemplo, `OnResourceExecuting` puede ejecutar código antes que el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-127">For example, `OnResourceExecuting` can run code before model binding.</span></span>
  * <span data-ttu-id="6d3c5-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> puede ejecutar el código una vez que el resto de la canalización se haya completado.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-128"><xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter.OnResourceExecuted*> can run code after the rest of the pipeline has completed.</span></span>

* <span data-ttu-id="6d3c5-129">Los [filtros de acciones](#action-filters) pueden ejecutar código inmediatamente antes y después de llamar a un método de acción individual.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-129">[Action filters](#action-filters) can run code immediately before and after an individual action method is called.</span></span> <span data-ttu-id="6d3c5-130">Se pueden usar para manipular los argumentos pasados a una acción y el resultado obtenido de la acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-130">They can be used to manipulate the arguments passed into an action and the result returned from the action.</span></span> <span data-ttu-id="6d3c5-131">Los filtros de acciones **no** se admiten en Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-131">Action filters are **not** supported in Razor Pages.</span></span>

* <span data-ttu-id="6d3c5-132">Los [filtros de excepciones](#exception-filters) sirven para aplicar directivas globales a las excepciones no controladas que se producen antes de que se escriba algo en el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-132">[Exception filters](#exception-filters) are used to apply global policies to unhandled exceptions that occur before anything has been written to the response body.</span></span>

* <span data-ttu-id="6d3c5-133">Los [filtros de resultados](#result-filters) pueden ejecutar código inmediatamente antes y después de la ejecución de resultados de acción individuales.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-133">[Result filters](#result-filters) can run code immediately before and after the execution of individual action results.</span></span> <span data-ttu-id="6d3c5-134">Se ejecutan solo cuando el método de acción se ha ejecutado correctamente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-134">They run only when the action method has executed successfully.</span></span> <span data-ttu-id="6d3c5-135">Son útiles para la lógica que debe regir la ejecución de la vista o el formateador.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-135">They are useful for logic that must surround view or formatter execution.</span></span>

<span data-ttu-id="6d3c5-136">En el siguiente diagrama se muestra cómo interactúan los tipos de filtro en la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-136">The following diagram shows how filter types interact in the filter pipeline.</span></span>

![La solicitud se procesa a través de las fases Filtros de autorización, Filtros de recursos, Enlace de modelos, Filtros de acciones, Ejecución de acciones/Conversión del resultado de acción, Filtros de excepción, Filtros de resultados y Ejecución del resultado.](filters/_static/filter-pipeline-2.png)

## <a name="implementation"></a><span data-ttu-id="6d3c5-139">Implementación</span><span class="sxs-lookup"><span data-stu-id="6d3c5-139">Implementation</span></span>

<span data-ttu-id="6d3c5-140">Los filtros admiten implementaciones tanto sincrónicas como asincrónicas a través de diferentes definiciones de interfaz.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-140">Filters support both synchronous and asynchronous implementations through different interface definitions.</span></span>

<span data-ttu-id="6d3c5-141">Los filtros sincrónicos pueden ejecutar código antes (`On-Stage-Executing`) y después (`On-Stage-Executed`) de la fase de canalización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-141">Synchronous filters can run code before (`On-Stage-Executing`) and after (`On-Stage-Executed`) their pipeline stage.</span></span> <span data-ttu-id="6d3c5-142">Por ejemplo, `OnActionExecuting` se llama antes de llamar al método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-142">For example, `OnActionExecuting` is called before the action method is called.</span></span> <span data-ttu-id="6d3c5-143">`OnActionExecuted` se llama después de devolver el método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-143">`OnActionExecuted` is called after the action method returns.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="6d3c5-144">Los filtros asincrónicos definen un método `On-Stage-ExecutionAsync`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-144">Asynchronous filters define an `On-Stage-ExecutionAsync` method:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleAsyncActionFilter.cs?name=snippet)]

<span data-ttu-id="6d3c5-145">En el código anterior, `SampleAsyncActionFilter` tiene un delegado <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`) que ejecuta el método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-145">In the preceding code, the `SampleAsyncActionFilter` has an <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate> (`next`)  that executes the action method.</span></span>  <span data-ttu-id="6d3c5-146">Cada uno de los métodos `On-Stage-ExecutionAsync` toman un `FilterType-ExecutionDelegate` que ejecuta la fase de canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-146">Each of the `On-Stage-ExecutionAsync` methods take a `FilterType-ExecutionDelegate` that executes the filter's pipeline stage.</span></span>

### <a name="multiple-filter-stages"></a><span data-ttu-id="6d3c5-147">Varias fases de filtro</span><span class="sxs-lookup"><span data-stu-id="6d3c5-147">Multiple filter stages</span></span>

<span data-ttu-id="6d3c5-148">Se pueden implementar interfaces para varias fases de filtro en una sola clase.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-148">Interfaces for multiple filter stages can be implemented in a single class.</span></span> <span data-ttu-id="6d3c5-149">Por ejemplo, la clase <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> implementa `IActionFilter`, `IResultFilter` y sus equivalentes asincrónicos.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-149">For example, the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> class implements `IActionFilter`, `IResultFilter`, and their async equivalents.</span></span>

<span data-ttu-id="6d3c5-150">Implemente la versión sincrónica **o** la versión asincrónica de una interfaz de filtro, pero **no** ambas.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-150">Implement **either** the synchronous or the async version of a filter interface, **not** both.</span></span> <span data-ttu-id="6d3c5-151">El entorno de ejecución comprueba primero si el filtro implementa la interfaz asincrónica y, si es así, llama a la interfaz.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-151">The runtime checks first to see if the filter implements the async interface, and if so, it calls that.</span></span> <span data-ttu-id="6d3c5-152">De lo contrario, llamará a métodos de interfaz sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-152">If not, it calls the synchronous interface's method(s).</span></span> <span data-ttu-id="6d3c5-153">Si se implementan las interfaces asincrónicas y sincrónicas en una clase, solo se llama al método asincrónico.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-153">If both asynchronous and synchronous interfaces are implemented in one class, only the async method is called.</span></span> <span data-ttu-id="6d3c5-154">Cuando se usan clases abstractas como <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>, se invalidan solo los métodos sincrónicos o el método asincrónico de cada tipo de filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-154">When using abstract classes like <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> override only the synchronous methods or the async method for each filter type.</span></span>

### <a name="built-in-filter-attributes"></a><span data-ttu-id="6d3c5-155">Atributos de filtros integrados</span><span class="sxs-lookup"><span data-stu-id="6d3c5-155">Built-in filter attributes</span></span>

<span data-ttu-id="6d3c5-156">ASP.NET Core incluye filtros integrados basados en atributos que se pueden personalizar y a partir de los cuales crear subclases.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-156">ASP.NET Core includes built-in attribute-based filters that can be subclassed and customized.</span></span> <span data-ttu-id="6d3c5-157">Por ejemplo, el siguiente filtro de resultados agrega un encabezado a la respuesta:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-157">For example, the following result filter adds a header to the response:</span></span>

<a name="add-header-attribute"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderAttribute.cs?name=snippet)]

<span data-ttu-id="6d3c5-158">Los atributos permiten a los filtros aceptar argumentos, como se muestra en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-158">Attributes allow filters to accept arguments, as shown in the preceding example.</span></span> <span data-ttu-id="6d3c5-159">Aplique el `AddHeaderAttribute` a un método de acción o controlador y especifique el nombre y el valor del encabezado HTTP:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-159">Apply the `AddHeaderAttribute` to a controller or action method and specify the name and value of the HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1)]

<!-- `https://localhost:5001/Sample` -->

<span data-ttu-id="6d3c5-160">Algunas de las interfaces de filtro tienen atributos correspondientes que se pueden usar como clases base en las implementaciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-160">Several of the filter interfaces have corresponding attributes that can be used as base classes for custom implementations.</span></span>

<span data-ttu-id="6d3c5-161">Atributos de filtro:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-161">Filter attributes:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ExceptionFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.Filters.ResultFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>

## <a name="filter-scopes-and-order-of-execution"></a><span data-ttu-id="6d3c5-162">Ámbitos del filtro y orden de ejecución</span><span class="sxs-lookup"><span data-stu-id="6d3c5-162">Filter scopes and order of execution</span></span>

<span data-ttu-id="6d3c5-163">Un filtro se puede agregar a la canalización en uno de tres *ámbitos* posibles:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-163">A filter can be added to the pipeline at one of three *scopes*:</span></span>

* <span data-ttu-id="6d3c5-164">Mediante un atributo en una acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-164">Using an attribute on an action.</span></span>
* <span data-ttu-id="6d3c5-165">Mediante un atributo en un controlador.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-165">Using an attribute on a controller.</span></span>
* <span data-ttu-id="6d3c5-166">Globalmente para todos los controladores y las acciones, tal como se muestra en el siguiente código:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-166">Globally for all controllers and actions as shown in the following code:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/StartupGF.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="6d3c5-167">El código anterior agrega tres filtros globalmente mediante la colección [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-167">The preceding code adds three filters globally using the [MvcOptions.Filters](xref:Microsoft.AspNetCore.Mvc.MvcOptions.Filters) collection.</span></span>

### <a name="default-order-of-execution"></a><span data-ttu-id="6d3c5-168">Orden de ejecución predeterminado</span><span class="sxs-lookup"><span data-stu-id="6d3c5-168">Default order of execution</span></span>

<span data-ttu-id="6d3c5-169">Cuando hay varios filtros en una determinada fase de la canalización, el ámbito determina el orden predeterminado en el que esos filtros se van a ejecutar.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-169">When there are multiple filters for a particular stage of the pipeline, scope determines the default order of filter execution.</span></span>  <span data-ttu-id="6d3c5-170">Los filtros globales abarcan a los filtros de clase, que a su vez engloban a los filtros de método.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-170">Global filters surround class filters, which in turn surround method filters.</span></span>

<span data-ttu-id="6d3c5-171">Como resultado de este anidamiento de filtros, el código de filtros *posterior* se ejecuta en el orden inverso al código *anterior*.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-171">As a result of filter nesting, the *after* code of filters runs in the reverse order of the *before* code.</span></span> <span data-ttu-id="6d3c5-172">La secuencia de filtro:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-172">The filter sequence:</span></span>

* <span data-ttu-id="6d3c5-173">El código *anterior* de los filtros globales.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-173">The *before* code of global filters.</span></span>
  * <span data-ttu-id="6d3c5-174">El código *anterior* de los filtros de controlador.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-174">The *before* code of controller filters.</span></span>
    * <span data-ttu-id="6d3c5-175">El código *anterior* de los filtros de métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-175">The *before* code of action method filters.</span></span>
    * <span data-ttu-id="6d3c5-176">El código *posterior* de los filtros de métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-176">The *after* code of action method filters.</span></span>
  * <span data-ttu-id="6d3c5-177">El código *posterior* de los filtros de controlador.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-177">The *after* code of controller filters.</span></span>
* <span data-ttu-id="6d3c5-178">El código *posterior* de los filtros globales.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-178">The *after* code of global filters.</span></span>
  
<span data-ttu-id="6d3c5-179">El ejemplo siguiente que ilustra el orden en el que se llama a los métodos de filtro relativos a filtros de acciones sincrónicos.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-179">The following example that illustrates the order in which filter methods are called for synchronous action filters.</span></span>

| <span data-ttu-id="6d3c5-180">Secuencia</span><span class="sxs-lookup"><span data-stu-id="6d3c5-180">Sequence</span></span> | <span data-ttu-id="6d3c5-181">Ámbito del filtro</span><span class="sxs-lookup"><span data-stu-id="6d3c5-181">Filter scope</span></span> | <span data-ttu-id="6d3c5-182">Método de filtro</span><span class="sxs-lookup"><span data-stu-id="6d3c5-182">Filter method</span></span> |
|:--------:|:------------:|:-------------:|
| <span data-ttu-id="6d3c5-183">1</span><span class="sxs-lookup"><span data-stu-id="6d3c5-183">1</span></span> | <span data-ttu-id="6d3c5-184">Global</span><span class="sxs-lookup"><span data-stu-id="6d3c5-184">Global</span></span> | `OnActionExecuting` |
| <span data-ttu-id="6d3c5-185">2</span><span class="sxs-lookup"><span data-stu-id="6d3c5-185">2</span></span> | <span data-ttu-id="6d3c5-186">Controlador</span><span class="sxs-lookup"><span data-stu-id="6d3c5-186">Controller</span></span> | `OnActionExecuting` |
| <span data-ttu-id="6d3c5-187">3</span><span class="sxs-lookup"><span data-stu-id="6d3c5-187">3</span></span> | <span data-ttu-id="6d3c5-188">Método</span><span class="sxs-lookup"><span data-stu-id="6d3c5-188">Method</span></span> | `OnActionExecuting` |
| <span data-ttu-id="6d3c5-189">4</span><span class="sxs-lookup"><span data-stu-id="6d3c5-189">4</span></span> | <span data-ttu-id="6d3c5-190">Método</span><span class="sxs-lookup"><span data-stu-id="6d3c5-190">Method</span></span> | `OnActionExecuted` |
| <span data-ttu-id="6d3c5-191">5</span><span class="sxs-lookup"><span data-stu-id="6d3c5-191">5</span></span> | <span data-ttu-id="6d3c5-192">Controlador</span><span class="sxs-lookup"><span data-stu-id="6d3c5-192">Controller</span></span> | `OnActionExecuted` |
| <span data-ttu-id="6d3c5-193">6</span><span class="sxs-lookup"><span data-stu-id="6d3c5-193">6</span></span> | <span data-ttu-id="6d3c5-194">Global</span><span class="sxs-lookup"><span data-stu-id="6d3c5-194">Global</span></span> | `OnActionExecuted` |

<span data-ttu-id="6d3c5-195">Esta secuencia pone de manifiesto lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-195">This sequence shows:</span></span>

* <span data-ttu-id="6d3c5-196">El filtro de método está anidado en el filtro de controlador.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-196">The method filter is nested within the controller filter.</span></span>
* <span data-ttu-id="6d3c5-197">El filtro de controlador está anidado en el filtro global.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-197">The controller filter is nested within the global filter.</span></span>

### <a name="controller-and-razor-page-level-filters"></a><span data-ttu-id="6d3c5-198">Filtros de nivel de controlador y de páginas de Razor</span><span class="sxs-lookup"><span data-stu-id="6d3c5-198">Controller and Razor Page level filters</span></span>

<span data-ttu-id="6d3c5-199">Cada controlador que hereda de la clase base <xref:Microsoft.AspNetCore.Mvc.Controller> incluye los métodos [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*), [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*) y [ Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-199">Every controller that inherits from the <xref:Microsoft.AspNetCore.Mvc.Controller> base class includes [Controller.OnActionExecuting](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuting*),  [Controller.OnActionExecutionAsync](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecutionAsync*), and [Controller.OnActionExecuted](xref:Microsoft.AspNetCore.Mvc.Controller.OnActionExecuted*)
`OnActionExecuted` methods.</span></span> <span data-ttu-id="6d3c5-200">Estos métodos:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-200">These methods:</span></span>

* <span data-ttu-id="6d3c5-201">Encapsulan los filtros que se ejecutan para una acción determinada.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-201">Wrap the filters that run for a given action.</span></span>
* <span data-ttu-id="6d3c5-202">`OnActionExecuting` se llama antes de cualquiera de los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-202">`OnActionExecuting` is called before any of the action's filters.</span></span>
* <span data-ttu-id="6d3c5-203">`OnActionExecuted` se llama después de todos los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-203">`OnActionExecuted` is called after all of the action filters.</span></span>
* <span data-ttu-id="6d3c5-204">`OnActionExecutionAsync` se llama antes de cualquiera de los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-204">`OnActionExecutionAsync` is called before any of the action's filters.</span></span> <span data-ttu-id="6d3c5-205">El código del filtro después de `next` se ejecuta después del método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-205">Code in the filter after `next` runs after the action method.</span></span>

<span data-ttu-id="6d3c5-206">Por ejemplo, en el ejemplo de descarga, se aplica `MySampleActionFilter` globalmente al inicio.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-206">For example, in the download sample, `MySampleActionFilter` is applied globally in startup.</span></span>

<span data-ttu-id="6d3c5-207">El `TestController`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-207">The `TestController`:</span></span>

* <span data-ttu-id="6d3c5-208">Aplica `SampleActionFilterAttribute` (`[SampleActionFilter]`) a la acción `FilterTest2`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-208">Applies the `SampleActionFilterAttribute` (`[SampleActionFilter]`) to the `FilterTest2` action:</span></span>
* <span data-ttu-id="6d3c5-209">Invalida `OnActionExecuting` y `OnActionExecuted`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-209">Overrides `OnActionExecuting` and `OnActionExecuted`.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="6d3c5-210">Si se dirige a `https://localhost:5001/Test/FilterTest2`, se ejecuta el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-210">Navigating to `https://localhost:5001/Test/FilterTest2` runs the following code:</span></span>

* `TestController.OnActionExecuting`
  * `MySampleActionFilter.OnActionExecuting`
    * `SampleActionFilterAttribute.OnActionExecuting`
      * `TestController.FilterTest2`
    * `SampleActionFilterAttribute.OnActionExecuted`
  * `MySampleActionFilter.OnActionExecuted`
* `TestController.OnActionExecuted`

<span data-ttu-id="6d3c5-211">Para Razor Pages, consulte [Implementar filtros de páginas de Razor globalmente](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-211">For Razor Pages, see [Implement Razor Page filters by overriding filter methods](xref:razor-pages/filter#implement-razor-page-filters-by-overriding-filter-methods).</span></span>

### <a name="overriding-the-default-order"></a><span data-ttu-id="6d3c5-212">Invalidación del orden predeterminado</span><span class="sxs-lookup"><span data-stu-id="6d3c5-212">Overriding the default order</span></span>

<span data-ttu-id="6d3c5-213">La secuencia de ejecución predeterminada se puede invalidar con la implementación de <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-213">The default sequence of execution can be overridden by implementing <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter>.</span></span> <span data-ttu-id="6d3c5-214"><xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> expone la propiedad `IOrderedFilter` que tiene prioridad sobre el ámbito a la hora de determinar el orden de ejecución.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-214">`IOrderedFilter` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IOrderedFilter.Order> property that takes precedence over scope to determine the order of execution.</span></span> <span data-ttu-id="6d3c5-215">Un filtro con un valor `Order` menor:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-215">A filter with a lower `Order` value:</span></span>

* <span data-ttu-id="6d3c5-216">Ejecuta el código *anterior* antes que el de un filtro con un valor mayor de `Order`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-216">Runs the *before* code before that of a filter with a higher value of `Order`.</span></span>
* <span data-ttu-id="6d3c5-217">Ejecuta el código *posterior* después que el de un filtro con un valor mayor de `Order`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-217">Runs the *after* code after that of a filter with a higher `Order` value.</span></span>

<span data-ttu-id="6d3c5-218">La propiedad `Order` se puede establecer con un parámetro de constructor:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-218">The `Order` property can be set with a constructor parameter:</span></span>

```csharp
[MyFilter(Name = "Controller Level Attribute", Order=1)]
```

<span data-ttu-id="6d3c5-219">Considere los mismos tres filtros de acción que se muestran en el ejemplo anterior.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-219">Consider the same 3 action filters shown in the preceding example.</span></span> <span data-ttu-id="6d3c5-220">Si la propiedad `Order` del controlador y de los filtros globales está establecida en 1 y 2 respectivamente, el orden de ejecución se invierte.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-220">If the `Order` property of the controller and global filters is set to 1 and 2 respectively, the order of execution is reversed.</span></span>

| <span data-ttu-id="6d3c5-221">Secuencia</span><span class="sxs-lookup"><span data-stu-id="6d3c5-221">Sequence</span></span> | <span data-ttu-id="6d3c5-222">Ámbito del filtro</span><span class="sxs-lookup"><span data-stu-id="6d3c5-222">Filter scope</span></span> | <span data-ttu-id="6d3c5-223">Propiedad`Order`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-223">`Order` property</span></span> | <span data-ttu-id="6d3c5-224">Método de filtro</span><span class="sxs-lookup"><span data-stu-id="6d3c5-224">Filter method</span></span> |
|:--------:|:------------:|:-----------------:|:-------------:|
| <span data-ttu-id="6d3c5-225">1</span><span class="sxs-lookup"><span data-stu-id="6d3c5-225">1</span></span> | <span data-ttu-id="6d3c5-226">Método</span><span class="sxs-lookup"><span data-stu-id="6d3c5-226">Method</span></span> | <span data-ttu-id="6d3c5-227">0</span><span class="sxs-lookup"><span data-stu-id="6d3c5-227">0</span></span> | `OnActionExecuting` |
| <span data-ttu-id="6d3c5-228">2</span><span class="sxs-lookup"><span data-stu-id="6d3c5-228">2</span></span> | <span data-ttu-id="6d3c5-229">Controlador</span><span class="sxs-lookup"><span data-stu-id="6d3c5-229">Controller</span></span> | <span data-ttu-id="6d3c5-230">1</span><span class="sxs-lookup"><span data-stu-id="6d3c5-230">1</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="6d3c5-231">3</span><span class="sxs-lookup"><span data-stu-id="6d3c5-231">3</span></span> | <span data-ttu-id="6d3c5-232">Global</span><span class="sxs-lookup"><span data-stu-id="6d3c5-232">Global</span></span> | <span data-ttu-id="6d3c5-233">2</span><span class="sxs-lookup"><span data-stu-id="6d3c5-233">2</span></span>  | `OnActionExecuting` |
| <span data-ttu-id="6d3c5-234">4</span><span class="sxs-lookup"><span data-stu-id="6d3c5-234">4</span></span> | <span data-ttu-id="6d3c5-235">Global</span><span class="sxs-lookup"><span data-stu-id="6d3c5-235">Global</span></span> | <span data-ttu-id="6d3c5-236">2</span><span class="sxs-lookup"><span data-stu-id="6d3c5-236">2</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="6d3c5-237">5</span><span class="sxs-lookup"><span data-stu-id="6d3c5-237">5</span></span> | <span data-ttu-id="6d3c5-238">Controlador</span><span class="sxs-lookup"><span data-stu-id="6d3c5-238">Controller</span></span> | <span data-ttu-id="6d3c5-239">1</span><span class="sxs-lookup"><span data-stu-id="6d3c5-239">1</span></span>  | `OnActionExecuted` |
| <span data-ttu-id="6d3c5-240">6</span><span class="sxs-lookup"><span data-stu-id="6d3c5-240">6</span></span> | <span data-ttu-id="6d3c5-241">Método</span><span class="sxs-lookup"><span data-stu-id="6d3c5-241">Method</span></span> | <span data-ttu-id="6d3c5-242">0</span><span class="sxs-lookup"><span data-stu-id="6d3c5-242">0</span></span>  | `OnActionExecuted` |

<span data-ttu-id="6d3c5-243">La propiedad `Order` invalida el ámbito al determinar el orden en el que se ejecutarán los filtros.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-243">The `Order` property overrides scope when determining the order in which filters run.</span></span> <span data-ttu-id="6d3c5-244">Los filtros se clasifican por orden en primer lugar y, después, se usa el ámbito para priorizar en caso de igualdad.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-244">Filters are sorted first by order, then scope is used to break ties.</span></span> <span data-ttu-id="6d3c5-245">Todos los filtros integrados implementan `IOrderedFilter` y establecen el valor predeterminado de `Order` en 0.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-245">All of the built-in filters implement `IOrderedFilter` and set the default `Order` value to 0.</span></span> <span data-ttu-id="6d3c5-246">En los filtros integrados, el ámbito determina el orden, a menos que `Order` se establezca en un valor distinto de cero.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-246">For built-in filters, scope determines order unless `Order` is set to a non-zero value.</span></span>

## <a name="cancellation-and-short-circuiting"></a><span data-ttu-id="6d3c5-247">Cancelación y cortocircuito</span><span class="sxs-lookup"><span data-stu-id="6d3c5-247">Cancellation and short-circuiting</span></span>

<span data-ttu-id="6d3c5-248">La canalización de filtro se puede cortocircuitar en cualquier momento mediante el establecimiento de la propiedad <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> en el parámetro <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> que se ha proporcionado al método de filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-248">The filter pipeline can be short-circuited by setting the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext.Result> property on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResourceExecutingContext> parameter provided to the filter method.</span></span> <span data-ttu-id="6d3c5-249">Por ejemplo, el siguiente filtro de recursos impide que el resto de la canalización se ejecute:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-249">For instance, the following Resource filter prevents the rest of the pipeline from executing:</span></span>

<a name="short-circuiting-resource-filter"></a>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ShortCircuitingResourceFilterAttribute.cs?name=snippet)]

<span data-ttu-id="6d3c5-250">En el siguiente código, tanto el filtro `ShortCircuitingResourceFilter` como el filtro `AddHeader` tienen como destino el método de acción `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-250">In the following code, both the `ShortCircuitingResourceFilter` and the `AddHeader` filter target the `SomeResource` action method.</span></span> <span data-ttu-id="6d3c5-251">El `ShortCircuitingResourceFilter`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-251">The `ShortCircuitingResourceFilter`:</span></span>

* <span data-ttu-id="6d3c5-252">Se ejecuta en primer lugar, porque es un filtro de recursos y `AddHeader` es un filtro de acciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-252">Runs first, because it's a Resource Filter and `AddHeader` is an Action Filter.</span></span>
* <span data-ttu-id="6d3c5-253">Cortocircuita el resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-253">Short-circuits the rest of the pipeline.</span></span>

<span data-ttu-id="6d3c5-254">Por tanto, el filtro `AddHeader` nunca se ejecuta en relación con la acción `SomeResource`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-254">Therefore the `AddHeader` filter never runs for the `SomeResource` action.</span></span> <span data-ttu-id="6d3c5-255">Este comportamiento sería el mismo si ambos filtros se aplicaran en el nivel de método de acción, siempre y cuando `ShortCircuitingResourceFilter` se haya ejecutado primero.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-255">This behavior would be the same if both filters were applied at the action method level, provided the `ShortCircuitingResourceFilter` ran first.</span></span> <span data-ttu-id="6d3c5-256">`ShortCircuitingResourceFilter` se ejecuta primero debido a su tipo de filtro o al uso explícito de la propiedad `Order`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-256">The `ShortCircuitingResourceFilter` runs first because of its filter type, or by explicit use of `Order` property.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/SampleController.cs?name=snippet_AddHeader&highlight=1,9)]

## <a name="dependency-injection"></a><span data-ttu-id="6d3c5-257">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="6d3c5-257">Dependency injection</span></span>

<span data-ttu-id="6d3c5-258">Los filtros se pueden agregar por tipo o por instancia.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-258">Filters can be added by type or by instance.</span></span> <span data-ttu-id="6d3c5-259">Si se agrega una instancia, esta se utiliza para todas las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-259">If an instance is added, that instance is used for every request.</span></span> <span data-ttu-id="6d3c5-260">Si se agrega un tipo, se activa por tipo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-260">If a type is added, it's type-activated.</span></span> <span data-ttu-id="6d3c5-261">Un filtro activado por tipo significa:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-261">A type-activated filter means:</span></span>

* <span data-ttu-id="6d3c5-262">Se crea una instancia para cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-262">An instance is created for each request.</span></span>
* <span data-ttu-id="6d3c5-263">Las dependencias de constructor se rellenan mediante la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-263">Any constructor dependencies are populated by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span>

<span data-ttu-id="6d3c5-264">Los filtros que se implementan como atributos y se agregan directamente a las clases de controlador o a los métodos de acción no pueden tener dependencias de constructor proporcionadas por la [inserción de dependencias](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-264">Filters that are implemented as attributes and added directly to controller classes or action methods cannot have constructor dependencies provided by [dependency injection](xref:fundamentals/dependency-injection) (DI).</span></span> <span data-ttu-id="6d3c5-265">Las dependencias de constructor no pueden proporcionarse mediante la inserción de dependencias porque:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-265">Constructor dependencies cannot be provided by DI because:</span></span>

* <span data-ttu-id="6d3c5-266">Los atributos deben tener los parámetros de constructor proporcionados allá donde se apliquen.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-266">Attributes must have their constructor parameters supplied where they're applied.</span></span> 
* <span data-ttu-id="6d3c5-267">Se trata de una limitación de cómo funcionan los atributos.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-267">This is a limitation of how attributes work.</span></span>

<span data-ttu-id="6d3c5-268">Los filtros siguientes admiten dependencias de constructor proporcionadas en la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-268">The following filters support constructor dependencies provided from DI:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>
* <xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute>
* <span data-ttu-id="6d3c5-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> se implementa en el atributo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-269"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implemented on the attribute.</span></span>

<span data-ttu-id="6d3c5-270">Los filtros anteriores se pueden aplicar a un método de controlador o de acción:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-270">The preceding filters can be applied to a controller or action method:</span></span>

<span data-ttu-id="6d3c5-271">Los registradores están disponibles en la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-271">Loggers are available from DI.</span></span> <span data-ttu-id="6d3c5-272">Sin embargo, evite crear y utilizar filtros únicamente con fines de registro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-272">However, avoid creating and using filters purely for logging purposes.</span></span> <span data-ttu-id="6d3c5-273">El [registro del marco integrado](xref:fundamentals/logging/index) proporciona normalmente lo que se necesita para el registro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-273">The [built-in framework logging](xref:fundamentals/logging/index) typically provides what's needed for logging.</span></span> <span data-ttu-id="6d3c5-274">Registro agregado a los filtros:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-274">Logging added to filters:</span></span>

* <span data-ttu-id="6d3c5-275">Debe centrarse en cuestiones de dominio empresarial o en el comportamiento específico del filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-275">Should focus on business domain concerns or behavior specific to the filter.</span></span>
* <span data-ttu-id="6d3c5-276">**No** debe registrar acciones u otros eventos del marco.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-276">Should **not** log actions or other framework events.</span></span> <span data-ttu-id="6d3c5-277">Los filtros integrados registran acciones y eventos del marco.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-277">The built in filters log actions and framework events.</span></span>

### <a name="servicefilterattribute"></a><span data-ttu-id="6d3c5-278">ServiceFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="6d3c5-278">ServiceFilterAttribute</span></span>

<span data-ttu-id="6d3c5-279">Los tipos de implementación de filtro de servicio se registran en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-279">Service filter implementation types are registered in `ConfigureServices`.</span></span> <span data-ttu-id="6d3c5-280"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> recupera una instancia del filtro de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-280">A <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> retrieves an instance of the filter from DI.</span></span>

<span data-ttu-id="6d3c5-281">El código siguiente muestra `AddHeaderResultServiceFilter`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-281">The following code shows the `AddHeaderResultServiceFilter`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="6d3c5-282">En el código siguiente, `AddHeaderResultServiceFilter` se agrega al contenedor de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-282">In the following code, `AddHeaderResultServiceFilter` is added to the DI container:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="6d3c5-283">En el código siguiente, el atributo `ServiceFilter` recupera una instancia del filtro `AddHeaderResultServiceFilter` desde la inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-283">In the following code, the `ServiceFilter` attribute retrieves an instance of the `AddHeaderResultServiceFilter` filter from DI:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_ServiceFilter&highlight=1)]

<span data-ttu-id="6d3c5-284">Al usar `ServiceFilterAttribute`, si se establece [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-284">When using `ServiceFilterAttribute`, setting [ServiceFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute.IsReusable):</span></span>

* <span data-ttu-id="6d3c5-285">Proporciona una sugerencia que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-285">Provides a hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="6d3c5-286">El entorno de ejecución de ASP.NET Core no garantiza:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-286">The ASP.NET Core runtime doesn't guarantee:</span></span>

  * <span data-ttu-id="6d3c5-287">Que se creará una única instancia del filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-287">That a single instance of the filter will be created.</span></span>
  * <span data-ttu-id="6d3c5-288">El filtro no volverá a solicitarse desde el contenedor de inserción de dependencias en algún momento posterior.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-288">The filter will not be re-requested from the DI container at some later point.</span></span>

* <span data-ttu-id="6d3c5-289">No debe usarse con un filtro que depende de servicios con una duración distinta de singleton.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-289">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

 <span data-ttu-id="6d3c5-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-290"><xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="6d3c5-291">`IFilterFactory` expone el método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear una instancia de <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-291">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="6d3c5-292">`CreateInstance` carga el tipo especificado desde la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-292">`CreateInstance` loads the specified type from DI.</span></span>

### <a name="typefilterattribute"></a><span data-ttu-id="6d3c5-293">TypeFilterAttribute</span><span class="sxs-lookup"><span data-stu-id="6d3c5-293">TypeFilterAttribute</span></span>

<span data-ttu-id="6d3c5-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> es similar a <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, pero su tipo no se resuelve directamente desde el contenedor de inserción de dependencias,</span><span class="sxs-lookup"><span data-stu-id="6d3c5-294"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> is similar to <xref:Microsoft.AspNetCore.Mvc.ServiceFilterAttribute>, but its type isn't resolved directly from the DI container.</span></span> <span data-ttu-id="6d3c5-295">sino que crea una instancia del tipo usando el elemento <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-295">It instantiates the type by using <xref:Microsoft.Extensions.DependencyInjection.ObjectFactory?displayProperty=fullName>.</span></span>

<span data-ttu-id="6d3c5-296">Dado que los tipos `TypeFilterAttribute` no se resuelven directamente desde el contenedor de inserción de dependencias:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-296">Because `TypeFilterAttribute` types aren't resolved directly from the DI container:</span></span>

* <span data-ttu-id="6d3c5-297">Los tipos a los que se hace referencia con `TypeFilterAttribute` no tienen que estar registrados con el contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-297">Types that are referenced using the `TypeFilterAttribute` don't need to be registered with the DI container.</span></span>  <span data-ttu-id="6d3c5-298">Sus dependencias se completan a través del contenedor de inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-298">They do have their dependencies fulfilled by the DI container.</span></span>
* <span data-ttu-id="6d3c5-299">`TypeFilterAttribute` puede aceptar opcionalmente argumentos de constructor del tipo en cuestión.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-299">`TypeFilterAttribute` can optionally accept constructor arguments for the type.</span></span>

<span data-ttu-id="6d3c5-300">Al usar `TypeFilterAttribute`, si se establece [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-300">When using `TypeFilterAttribute`, setting [TypeFilterAttribute.IsReusable](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute.IsReusable):</span></span>
* <span data-ttu-id="6d3c5-301">Proporciona una sugerencia que indica que la instancia de filtro *podría* reutilizarse fuera del ámbito de la solicitud en la que se creó.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-301">Provides hint that the filter instance *may* be reused outside of the request scope it was created within.</span></span> <span data-ttu-id="6d3c5-302">El entorno de ejecución de ASP.NET Core no ofrece ninguna garantía de que se vaya a crear una única instancia del filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-302">The ASP.NET Core runtime provides no guarantees that a single instance of the filter will be created.</span></span>

* <span data-ttu-id="6d3c5-303">No debe usarse con un filtro que depende de servicios con una duración distinta de singleton.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-303">Should not be used with a filter that depends on services with a lifetime other than singleton.</span></span>

<span data-ttu-id="6d3c5-304">En el siguiente ejemplo se muestra cómo pasar argumentos a un tipo mediante `TypeFilterAttribute`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-304">The following example shows how to pass arguments to a type using `TypeFilterAttribute`:</span></span>

[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_TypeFilter&highlight=1,2)]
[!code-csharp[](../../mvc/controllers/filters/sample/FiltersSample/Filters/LogConstantFilter.cs?name=snippet_TypeFilter_Implementation&highlight=6)]

<!-- 
https://localhost:5001/home/hi?name=joe
VS debug window shows 
FiltersSample.Filters.LogConstantFilter:Information: Method 'Hi' called
-->

## <a name="authorization-filters"></a><span data-ttu-id="6d3c5-305">Filtros de autorización</span><span class="sxs-lookup"><span data-stu-id="6d3c5-305">Authorization filters</span></span>

<span data-ttu-id="6d3c5-306">Filtros de autorización:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-306">Authorization filters:</span></span>

* <span data-ttu-id="6d3c5-307">Son los primeros filtros que se ejecutan en la canalización del filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-307">Are the first filters run in the filter pipeline.</span></span>
* <span data-ttu-id="6d3c5-308">Controlan el acceso a los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-308">Control access to action methods.</span></span>
* <span data-ttu-id="6d3c5-309">Tienen un método anterior, pero no uno posterior.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-309">Have a before method, but no after method.</span></span>

<span data-ttu-id="6d3c5-310">Los filtros de autorización personalizados requieren un marco de autorización personalizado.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-310">Custom authorization filters require a custom authorization framework.</span></span> <span data-ttu-id="6d3c5-311">Es preferible configurar directivas de autorización o escribir una directiva de autorización personalizada a escribir un filtro personalizado.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-311">Prefer configuring the authorization policies or writing a custom authorization policy over writing a custom filter.</span></span> <span data-ttu-id="6d3c5-312">El filtro de autorización integrado:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-312">The built-in authorization filter:</span></span>

* <span data-ttu-id="6d3c5-313">Llama a la autorización del sistema.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-313">Calls the authorization system.</span></span>
* <span data-ttu-id="6d3c5-314">No autoriza las solicitudes.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-314">Does not authorize requests.</span></span>

<span data-ttu-id="6d3c5-315">**No** inicie excepciones dentro de los filtros de autorización:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-315">Do **not** throw exceptions within authorization filters:</span></span>

* <span data-ttu-id="6d3c5-316">La excepción no se controlará.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-316">The exception will not be handled.</span></span>
* <span data-ttu-id="6d3c5-317">Los filtros de excepciones no controlarán la excepción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-317">Exception filters will not handle the exception.</span></span>

<span data-ttu-id="6d3c5-318">Considere la posibilidad de emitir un desafío cuando se produzca una excepción en un filtro de autorizaciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-318">Consider issuing a challenge when an exception occurs in an authorization filter.</span></span>

<span data-ttu-id="6d3c5-319">[Aquí](xref:security/authorization/introduction) encontrará más información sobre la autorización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-319">Learn more about [Authorization](xref:security/authorization/introduction).</span></span>

## <a name="resource-filters"></a><span data-ttu-id="6d3c5-320">Filtros de recursos</span><span class="sxs-lookup"><span data-stu-id="6d3c5-320">Resource filters</span></span>

<span data-ttu-id="6d3c5-321">Filtros de recursos:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-321">Resource filters:</span></span>

* <span data-ttu-id="6d3c5-322">Implementan la interfaz <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-322">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IResourceFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResourceFilter> interface.</span></span>
* <span data-ttu-id="6d3c5-323">La ejecución encapsula la mayor parte de la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-323">Execution wraps most of the filter pipeline.</span></span>
* <span data-ttu-id="6d3c5-324">Los [filtros de autorizaciones](#authorization-filters) son los únicos que se ejecutan antes que los filtros de recursos.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-324">Only [Authorization filters](#authorization-filters) run before resource filters.</span></span>

<span data-ttu-id="6d3c5-325">Los filtros de recursos son útiles para cortocircuitar la mayor parte de la canalización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-325">Resource filters are useful to short-circuit most of the pipeline.</span></span> <span data-ttu-id="6d3c5-326">Por ejemplo, un filtro de almacenamiento en caché puede evitar que se ejecute el resto de la canalización en un acierto de caché.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-326">For example, a caching filter can avoid the rest of the pipeline on a cache hit.</span></span>

<span data-ttu-id="6d3c5-327">Ejemplos de filtros de recursos:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-327">Resource filter examples:</span></span>

* <span data-ttu-id="6d3c5-328">[El filtro de recursos de cortocircuito](#short-circuiting-resource-filter) mostrado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-328">[The short-circuiting resource filter](#short-circuiting-resource-filter) shown previously.</span></span>
* <span data-ttu-id="6d3c5-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-329">[DisableFormValueModelBindingAttribute](https://github.com/aspnet/Entropy/blob/rel/2.0.0-preview2/samples/Mvc.FileUpload/Filters/DisableFormValueModelBindingAttribute.cs):</span></span>

  * <span data-ttu-id="6d3c5-330">Evita que el enlace de modelos tenga acceso a los datos del formulario.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-330">Prevents model binding from accessing the form data.</span></span>
  * <span data-ttu-id="6d3c5-331">Se utiliza cuando hay cargas de archivos muy voluminosos para impedir que los datos del formulario se lean en la memoria.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-331">Used for large file uploads to prevent the form data from being read into memory.</span></span>

## <a name="action-filters"></a><span data-ttu-id="6d3c5-332">Filtros de acciones</span><span class="sxs-lookup"><span data-stu-id="6d3c5-332">Action filters</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6d3c5-333">Los filtros de acción **no** se aplican a Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-333">Action filters do **not** apply to Razor Pages.</span></span> <span data-ttu-id="6d3c5-334">Razor Pages admite <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> y <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-334">Razor Pages supports <xref:Microsoft.AspNetCore.Mvc.Filters.IPageFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter> .</span></span> <span data-ttu-id="6d3c5-335">Para más información, vea [Filter methods for Razor Pages](xref:razor-pages/filter) (Métodos de filtrado para páginas de Razor).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-335">For more information, see [Filter methods for Razor Pages](xref:razor-pages/filter).</span></span>

<span data-ttu-id="6d3c5-336">Filtros de acciones:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-336">Action filters:</span></span>

* <span data-ttu-id="6d3c5-337">Implementan la interfaz <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-337">Implement either the <xref:Microsoft.AspNetCore.Mvc.Filters.IActionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncActionFilter> interface.</span></span>
* <span data-ttu-id="6d3c5-338">Su ejecución rodea la ejecución de los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-338">Their execution surrounds the execution of action methods.</span></span>

<span data-ttu-id="6d3c5-339">El código siguiente muestra un ejemplo de filtro de acciones:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-339">The following code shows a sample action filter:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MySampleActionFilter.cs?name=snippet_ActionFilter)]

<span data-ttu-id="6d3c5-340"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> ofrece las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-340">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext> provides the following properties:</span></span>

* <span data-ttu-id="6d3c5-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments>: permite leer las entradas de un método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-341"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.ActionArguments> - enables the inputs to an action method be read.</span></span>
* <span data-ttu-id="6d3c5-342"><xref:Microsoft.AspNetCore.Mvc.Controller>: permite manipular la instancia del controlador.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-342"><xref:Microsoft.AspNetCore.Mvc.Controller> - enables manipulating the controller instance.</span></span>
* <span data-ttu-id="6d3c5-343"><xref:System.Web.Mvc.ActionExecutingContext.Result>: si se establece `Result`, se cortocircuita la ejecución del método de acción y de los filtros de acciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-343"><xref:System.Web.Mvc.ActionExecutingContext.Result> - setting `Result` short-circuits execution of the action method and subsequent action filters.</span></span>

<span data-ttu-id="6d3c5-344">Inicio de una excepción en un método de acción:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-344">Throwing an exception in an action method:</span></span>

* <span data-ttu-id="6d3c5-345">Impide la ejecución de los filtros subsiguientes.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-345">Prevents running of subsequent filters.</span></span>
* <span data-ttu-id="6d3c5-346">A diferencia del establecimiento de `Result`, se trata como un error en lugar de como un resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-346">Unlike setting `Result`, is treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="6d3c5-347"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> proporciona `Controller` y `Result`, además de las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-347">The <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext> provides `Controller` and `Result` plus the following properties:</span></span>

* <span data-ttu-id="6d3c5-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled>: es true si otro filtro ha cortocircuitado la ejecución de la acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-348"><xref:System.Web.Mvc.ActionExecutedContext.Canceled> - True if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="6d3c5-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception>: es un valor distinto de NULL si la acción o un filtro de acción de ejecución anterior han producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-349"><xref:System.Web.Mvc.ActionExecutedContext.Exception> - Non-null if the action or a previously run action filter threw an exception.</span></span> <span data-ttu-id="6d3c5-350">Si se establece esta propiedad en un valor NULL:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-350">Setting this property to null:</span></span>

  * <span data-ttu-id="6d3c5-351">Controla la excepción eficazmente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-351">Effectively handles the exception.</span></span>
  * <span data-ttu-id="6d3c5-352">`Result` se ejecuta como si se devolviera desde el método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-352">`Result` is executed as if it was returned from the action method.</span></span>

<span data-ttu-id="6d3c5-353">En un `IAsyncActionFilter`, una llamada a <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-353">For an `IAsyncActionFilter`, a call to the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutionDelegate>:</span></span>

* <span data-ttu-id="6d3c5-354">Ejecuta cualquier filtro de acciones posterior y el método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-354">Executes any subsequent action filters and the action method.</span></span>
* <span data-ttu-id="6d3c5-355">Devuelve `ActionExecutedContext`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-355">Returns `ActionExecutedContext`.</span></span>

<span data-ttu-id="6d3c5-356">Para cortocircuitar esto, asigne <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> a una instancia de resultado y no llame a `next` (la clase `ActionExecutionDelegate`).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-356">To short-circuit, assign <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutingContext.Result?displayProperty=fullName> to a result instance and don't call `next` (the `ActionExecutionDelegate`).</span></span>

<span data-ttu-id="6d3c5-357">El marco proporciona una clase <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> abstracta de la que se pueden crear subclases.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-357">The framework provides an abstract <xref:Microsoft.AspNetCore.Mvc.Filters.ActionFilterAttribute> that can be subclassed.</span></span>

<span data-ttu-id="6d3c5-358">Se puede usar el filtro de acción `OnActionExecuting` para:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-358">The `OnActionExecuting` action filter can be used to:</span></span>

* <span data-ttu-id="6d3c5-359">Validar el estado del modelo.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-359">Validate model state.</span></span>
* <span data-ttu-id="6d3c5-360">Devolver un error si el estado no es válido.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-360">Return an error if the state is invalid.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet)]

<span data-ttu-id="6d3c5-361">El método `OnActionExecuted` se ejecuta después del método de acción:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-361">The `OnActionExecuted` method runs after the action method:</span></span>

* <span data-ttu-id="6d3c5-362">Y puede ver y manipular los resultados de la acción a través de la propiedad <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-362">And can see and manipulate the results of the action through the <xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Result> property.</span></span>
* <span data-ttu-id="6d3c5-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> se establece en true si otro filtro ha cortocircuitado la ejecución de la acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-363"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Canceled> is set to true if the action execution was short-circuited by another filter.</span></span>
* <span data-ttu-id="6d3c5-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> se establece en un valor distinto de NULL si la acción o un filtro de acción posterior han producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-364"><xref:Microsoft.AspNetCore.Mvc.Filters.ActionExecutedContext.Exception> is set to a non-null value if the action or a subsequent action filter threw an exception.</span></span> <span data-ttu-id="6d3c5-365">Si `Exception` se establece como nulo:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-365">Setting `Exception` to null:</span></span>

  * <span data-ttu-id="6d3c5-366">Controla una excepción eficazmente.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-366">Effectively handles an exception.</span></span>
  * <span data-ttu-id="6d3c5-367">`ActionExecutedContext.Result` se ejecuta como si se devolviera con normalidad desde el método de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-367">`ActionExecutedContext.Result` is executed as if it were returned normally from the action method.</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/ValidateModelAttribute.cs?name=snippet2&higlight=12-99)]

## <a name="exception-filters"></a><span data-ttu-id="6d3c5-368">Filtros de excepciones</span><span class="sxs-lookup"><span data-stu-id="6d3c5-368">Exception filters</span></span>

<span data-ttu-id="6d3c5-369">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-369">Exception filters:</span></span>

* <span data-ttu-id="6d3c5-370">Implementan <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-370">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter>.</span></span> 
* <span data-ttu-id="6d3c5-371">Se pueden usar para implementar directivas de control de errores comunes.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-371">Can be used to implement common error handling policies.</span></span>

<span data-ttu-id="6d3c5-372">En el siguiente filtro de excepciones de ejemplo se usa una vista de error personalizada para mostrar los detalles sobre las excepciones que se producen cuando la aplicación está en fase de desarrollo:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-372">The following sample exception filter uses a custom error view to display details about exceptions that occur when the app is in development:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/CustomExceptionFilterAttribute.cs?name=snippet_ExceptionFilter&highlight=16-19)]

<span data-ttu-id="6d3c5-373">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-373">Exception filters:</span></span>

* <span data-ttu-id="6d3c5-374">No tienen eventos anteriores ni posteriores.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-374">Don't have before and after events.</span></span>
* <span data-ttu-id="6d3c5-375">Implementan <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-375">Implement <xref:Microsoft.AspNetCore.Mvc.Filters.IExceptionFilter.OnException*> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncExceptionFilter.OnExceptionAsync*>.</span></span>
* <span data-ttu-id="6d3c5-376">Controlan las excepciones sin controlar que se producen en Razor Pages, al crear controladores, en el [enlace de modelos](xref:mvc/models/model-binding), en los filtros de acciones o en los métodos de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-376">Handle unhandled exceptions that occur in Razor Page or controller creation, [model binding](xref:mvc/models/model-binding), action filters, or action methods.</span></span>
* <span data-ttu-id="6d3c5-377">**No** detectan aquellas excepciones que se produzcan en los filtros de recursos, en los filtros de resultados o en la ejecución de resultados de MVC.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-377">Do **not** catch exceptions that occur in resource filters, result filters, or MVC result execution.</span></span>

<span data-ttu-id="6d3c5-378">Para controlar una excepción, establezca la propiedad <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> en `true` o escriba una respuesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-378">To handle an exception, set the <xref:System.Web.Mvc.ExceptionContext.ExceptionHandled> property to `true` or write a response.</span></span> <span data-ttu-id="6d3c5-379">Esto detiene la propagación de la excepción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-379">This stops propagation of the exception.</span></span> <span data-ttu-id="6d3c5-380">Un filtro de excepciones no tiene capacidad para convertir una excepción en un proceso "correcto".</span><span class="sxs-lookup"><span data-stu-id="6d3c5-380">An exception filter can't turn an exception into a "success".</span></span> <span data-ttu-id="6d3c5-381">Eso solo lo pueden hacer los filtros de acciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-381">Only an action filter can do that.</span></span>

<span data-ttu-id="6d3c5-382">Los filtros de excepciones:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-382">Exception filters:</span></span>

* <span data-ttu-id="6d3c5-383">Son adecuados para interceptar las excepciones que se producen en las acciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-383">Are good for trapping exceptions that occur within actions.</span></span>
* <span data-ttu-id="6d3c5-384">No son tan flexibles como el middleware de control de errores.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-384">Are not as flexible as error handling middleware.</span></span>

<span data-ttu-id="6d3c5-385">Es preferible usar middleware de control de excepciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-385">Prefer middleware for exception handling.</span></span> <span data-ttu-id="6d3c5-386">Utilice los filtros de excepciones solo cuando el control de errores *es diferente* en función del método de acción que se llama.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-386">Use exception filters only where error handling *differs* based on which action method is called.</span></span> <span data-ttu-id="6d3c5-387">Por ejemplo, puede que una aplicación tenga métodos de acción tanto para los puntos de conexión de API como para las vistas/HTML.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-387">For example, an app might have action methods for both API endpoints and for views/HTML.</span></span> <span data-ttu-id="6d3c5-388">Los puntos de conexión de API podrían devolver información sobre errores como JSON, mientras que las acciones basadas en vistas podrían devolver una página de error como HTML.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-388">The API endpoints could return error information as JSON, while the view-based actions could return an error page as HTML.</span></span>

## <a name="result-filters"></a><span data-ttu-id="6d3c5-389">Filtros de resultados</span><span class="sxs-lookup"><span data-stu-id="6d3c5-389">Result filters</span></span>

<span data-ttu-id="6d3c5-390">Filtros de resultados:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-390">Result filters:</span></span>

* <span data-ttu-id="6d3c5-391">Implementar una interfaz:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-391">Implement an interface:</span></span>
  * <span data-ttu-id="6d3c5-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span><span class="sxs-lookup"><span data-stu-id="6d3c5-392"><xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter></span></span>
  * <span data-ttu-id="6d3c5-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> o <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span><span class="sxs-lookup"><span data-stu-id="6d3c5-393"><xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> or <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter></span></span>
* <span data-ttu-id="6d3c5-394">Su ejecución rodea la ejecución de los resultados de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-394">Their execution surrounds the execution of action results.</span></span>

### <a name="iresultfilter-and-iasyncresultfilter"></a><span data-ttu-id="6d3c5-395">IResultFilter e IAsyncResultFilter</span><span class="sxs-lookup"><span data-stu-id="6d3c5-395">IResultFilter and IAsyncResultFilter</span></span>

<span data-ttu-id="6d3c5-396">El código siguiente muestra un filtro de resultados que agrega un encabezado HTTP:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-396">The following code shows a result filter that adds an HTTP header:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LoggingAddHeaderFilter.cs?name=snippet_ResultFilter)]

<span data-ttu-id="6d3c5-397">El tipo de resultado que se ejecute dependerá de la acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-397">The kind of result being executed depends on the action.</span></span> <span data-ttu-id="6d3c5-398">Una acción que devuelve una vista incluye todo el procesamiento de Razor como parte del elemento <xref:Microsoft.AspNetCore.Mvc.ViewResult> que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-398">An action returning a view would include all razor processing as part of the <xref:Microsoft.AspNetCore.Mvc.ViewResult> being executed.</span></span> <span data-ttu-id="6d3c5-399">Un método API puede llevar a cabo algunas funciones de serialización como parte de la ejecución del resultado.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-399">An API method might perform some serialization as part of the execution of the result.</span></span> <span data-ttu-id="6d3c5-400">[Aquí](xref:mvc/controllers/actions) encontrará más información sobre los resultados de acciones.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-400">Learn more about [action results](xref:mvc/controllers/actions).</span></span>

<span data-ttu-id="6d3c5-401">Los filtros de resultados solo se ejecutan cuando una acción o un filtro de acción genera un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-401">Result filters are only executed when an action or action filter produces an action result.</span></span> <span data-ttu-id="6d3c5-402">Los filtros de resultados no se ejecutan cuando:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-402">Result filters are not executed when:</span></span>

* <span data-ttu-id="6d3c5-403">Un filtro de autorización o un filtro de recursos genera un cortocircuito en la canalización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-403">An authorization filter or resource filter short-circuits the pipeline.</span></span>
* <span data-ttu-id="6d3c5-404">Un filtro de excepciones controla una excepción al producir un resultado de acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-404">An exception filter handles an exception by producing an action result.</span></span>

<span data-ttu-id="6d3c5-405">El método <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> puede cortocircuitar la ejecución del resultado de la acción y de los filtros de resultados posteriores mediante el establecimiento de <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> en `true`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-405">The <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuting*?displayProperty=fullName> method can short-circuit execution of the action result and subsequent result filters by setting <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel?displayProperty=fullName> to `true`.</span></span> <span data-ttu-id="6d3c5-406">Escriba en el objeto de respuesta cuando el proceso se cortocircuite, ya que así evitará que se genere una respuesta vacía.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-406">Write to the response object when short-circuiting to avoid generating an empty response.</span></span> <span data-ttu-id="6d3c5-407">Si se inicia una excepción en `IResultFilter.OnResultExecuting`, sucederá lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-407">Throwing an exception in `IResultFilter.OnResultExecuting` will:</span></span>

* <span data-ttu-id="6d3c5-408">Se impedirá la ejecución del resultado de la acción y de los filtros subsiguientes.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-408">Prevent execution of the action result and subsequent filters.</span></span>
* <span data-ttu-id="6d3c5-409">Se tratará como un error en lugar de como un resultado correcto.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-409">Be treated as a failure instead of a successful result.</span></span>

<span data-ttu-id="6d3c5-410">Cuando se ejecuta el método <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName>:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-410">When the <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter.OnResultExecuted*?displayProperty=fullName> method runs:</span></span>

* <span data-ttu-id="6d3c5-411">Es probable que la respuesta se haya enviado al cliente y no se pueda cambiar.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-411">The response has likely been sent to the client and cannot be changed.</span></span>
* <span data-ttu-id="6d3c5-412">Si se inicia una excepción, no se envía el cuerpo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-412">If an exception was thrown, the response body is not sent.</span></span>

<!-- Review preceding "If an exception was thrown: Original 
When the OnResultExecuted method runs, the response has likely been sent to the client and cannot be changed further (unless an exception was thrown).

SHould that be , 
If an exception was thrown **IN THE RESULT FILTER**, the response body is not sent.

 -->

<span data-ttu-id="6d3c5-413">`ResultExecutedContext.Canceled` se establece en `true` si otro filtro ha cortocircuitado la ejecución del resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-413">`ResultExecutedContext.Canceled` is set to `true` if the action result execution was short-circuited by another filter.</span></span>

<span data-ttu-id="6d3c5-414">`ResultExecutedContext.Exception` se establece en un valor distinto de NULL si el resultado de la acción o un filtro de resultado posterior ha producido una excepción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-414">`ResultExecutedContext.Exception` is set to a non-null value if the action result or a subsequent result filter threw an exception.</span></span> <span data-ttu-id="6d3c5-415">Establecer `Exception` en un valor NULL hace que una excepción se "controle" de forma eficaz y evita que ASP.NET Core vuelva a producir dicha excepción más adelante en la canalización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-415">Setting `Exception` to null effectively handles an exception and prevents the exception from being rethrown by ASP.NET Core later in the pipeline.</span></span> <span data-ttu-id="6d3c5-416">No hay una forma confiable de escribir datos en una respuesta cuando se controla una excepción en un filtro de resultados.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-416">There is no reliable way to write data to a response when handling an exception in a result filter.</span></span> <span data-ttu-id="6d3c5-417">Si los encabezados ya se han vaciado en el cliente si el resultado de una acción inicia una excepción, no hay ningún mecanismo confiable que permita enviar un código de error.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-417">If the headers have been flushed to the client when an action result throws an exception, there's no reliable mechanism to send a failure code.</span></span>

<span data-ttu-id="6d3c5-418">En un elemento <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, una llamada a `await next` en <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> ejecuta cualquier filtro de resultados posterior y el resultado de la acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-418">For an <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter>, a call to `await next` on the <xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutionDelegate> executes any subsequent result filters and the action result.</span></span> <span data-ttu-id="6d3c5-419">Para cortocircuitarlo, establezca [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) en `true` y no llame a `ResultExecutionDelegate`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-419">To short-circuit, set [ResultExecutingContext.Cancel](xref:Microsoft.AspNetCore.Mvc.Filters.ResultExecutingContext.Cancel) to `true` and don't call the `ResultExecutionDelegate`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/MyAsyncResponseFilter.cs?name=snippet)]

<span data-ttu-id="6d3c5-420">El marco proporciona una clase abstracta `ResultFilterAttribute` de la que se pueden crear subclases.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-420">The framework provides an abstract `ResultFilterAttribute` that can be subclassed.</span></span> <span data-ttu-id="6d3c5-421">La clase [AddHeaderAttribute](#add-header-attribute) mostrada anteriormente es un ejemplo de un atributo de filtro de resultados.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-421">The [AddHeaderAttribute](#add-header-attribute) class shown previously is an example of a result filter attribute.</span></span>

### <a name="ialwaysrunresultfilter-and-iasyncalwaysrunresultfilter"></a><span data-ttu-id="6d3c5-422">IAlwaysRunResultFilter e IAsyncAlwaysRunResultFilter</span><span class="sxs-lookup"><span data-stu-id="6d3c5-422">IAlwaysRunResultFilter and IAsyncAlwaysRunResultFilter</span></span>

<span data-ttu-id="6d3c5-423">Las interfaces <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> e <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> declaran una implementación <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> que se ejecuta para obtener todos los resultados de la acción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-423">The <xref:Microsoft.AspNetCore.Mvc.Filters.IAlwaysRunResultFilter> and <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAlwaysRunResultFilter> interfaces declare an <xref:Microsoft.AspNetCore.Mvc.Filters.IResultFilter> implementation that runs for all action results.</span></span> <span data-ttu-id="6d3c5-424">Esto incluye los resultados de la acción generados por:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-424">This includes action results produced by:</span></span>

* <span data-ttu-id="6d3c5-425">Filtros de autorización y filtros de recursos que generan un cortocircuito.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-425">Authorization filters and resource filters that short-circuit.</span></span>
* <span data-ttu-id="6d3c5-426">Filtros de excepción.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-426">Exception filters.</span></span>

<span data-ttu-id="6d3c5-427">Por ejemplo, el siguiente filtro siempre ejecuta y establece un resultado de la acción (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) con un código de estado *422 - Entidad no procesable* cuando se produce un error en la negociación de contenido:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-427">For example, the following filter always runs and sets an action result (<xref:Microsoft.AspNetCore.Mvc.ObjectResult>) with a *422 Unprocessable Entity* status code when content negotiation fails:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/UnprocessableResultFilter.cs?name=snippet)]

### <a name="ifilterfactory"></a><span data-ttu-id="6d3c5-428">IFilterFactory</span><span class="sxs-lookup"><span data-stu-id="6d3c5-428">IFilterFactory</span></span>

<span data-ttu-id="6d3c5-429"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-429"><xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span></span> <span data-ttu-id="6d3c5-430">Por tanto, una instancia de `IFilterFactory` se puede usar como una instancia de `IFilterMetadata` en cualquier parte de la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-430">Therefore, an `IFilterFactory` instance can be used as an `IFilterMetadata` instance anywhere in the filter pipeline.</span></span> <span data-ttu-id="6d3c5-431">Cuando el entorno de ejecución se prepara para invocar el filtro, intenta convertirlo a un `IFilterFactory`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-431">When the runtime prepares to invoke the filter, it attempts to cast it to an `IFilterFactory`.</span></span> <span data-ttu-id="6d3c5-432">Si esa conversión se realiza correctamente, se llama al método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear la instancia `IFilterMetadata` que se va a invocar.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-432">If that cast succeeds, the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method is called to create the `IFilterMetadata` instance that is invoked.</span></span> <span data-ttu-id="6d3c5-433">Esto proporciona un diseño flexible, dado que no hay que establecer la canalización de filtro exacta de forma explícita cuando la aplicación se inicia.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-433">This provides a flexible design, since the precise filter pipeline doesn't need to be set explicitly when the app starts.</span></span>

<span data-ttu-id="6d3c5-434">Puede implementar `IFilterFactory` con las implementaciones de atributos personalizados como método alternativo para crear filtros:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-434">`IFilterFactory` can be implemented using custom attribute implementations as another approach to creating filters:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/AddHeaderWithFactoryAttribute.cs?name=snippet_IFilterFactory&highlight=1,4,5,6,7)]

<span data-ttu-id="6d3c5-435">El código anterior se puede probar mediante la ejecución del [ejemplo de descargar](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span><span class="sxs-lookup"><span data-stu-id="6d3c5-435">The preceding code can be tested by running the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample):</span></span>

* <span data-ttu-id="6d3c5-436">Invoque las herramientas de desarrollador de F12.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-436">Invoke the F12 developer tools.</span></span>
* <span data-ttu-id="6d3c5-437">Navegue a `https://localhost:5001/Sample/HeaderWithFactory`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-437">Navigate to `https://localhost:5001/Sample/HeaderWithFactory`</span></span>

<span data-ttu-id="6d3c5-438">Las herramientas de desarrollador F12 muestran los siguientes encabezados de respuesta agregados por el código de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-438">The F12 developer tools display the following response headers added by the sample code:</span></span>

* <span data-ttu-id="6d3c5-439">**author:** `Joe Smith`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-439">**author:** `Joe Smith`</span></span>
* <span data-ttu-id="6d3c5-440">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-440">**globaladdheader:** `Result filter added to MvcOptions.Filters`</span></span>
* <span data-ttu-id="6d3c5-441">**internal:** `My header`</span><span class="sxs-lookup"><span data-stu-id="6d3c5-441">**internal:** `My header`</span></span>

<span data-ttu-id="6d3c5-442">El código anterior crea el encabezado de respuesta **internal:** `My header`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-442">The preceding code creates the **internal:** `My header` response header.</span></span>

### <a name="ifilterfactory-implemented-on-an-attribute"></a><span data-ttu-id="6d3c5-443">IFilterFactory implementado en un atributo</span><span class="sxs-lookup"><span data-stu-id="6d3c5-443">IFilterFactory implemented on an attribute</span></span>

<!-- Review 
This section needs to be rewritten.
What's a non-named attribute?
-->

<span data-ttu-id="6d3c5-444">Los filtros que implementan `IFilterFactory` son útiles para los filtros que:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-444">Filters that implement `IFilterFactory` are useful for filters that:</span></span>

* <span data-ttu-id="6d3c5-445">No requieren pasar parámetros.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-445">Don't require passing parameters.</span></span>
* <span data-ttu-id="6d3c5-446">Tienen dependencias de constructor que deben completarse por medio de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-446">Have constructor dependencies that need to be filled by DI.</span></span>

<span data-ttu-id="6d3c5-447"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implementa <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-447"><xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute> implements <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>.</span></span> <span data-ttu-id="6d3c5-448">`IFilterFactory` expone el método <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> para crear una instancia de <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata>.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-448">`IFilterFactory` exposes the <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory.CreateInstance*> method for creating an <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterMetadata> instance.</span></span> <span data-ttu-id="6d3c5-449">`CreateInstance` carga el tipo especificado desde el contenedor de servicios (inserción de dependencias).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-449">`CreateInstance` loads the specified type from the services container (DI).</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/SampleActionFilterAttribute.cs?name=snippet_TypeFilterAttribute&highlight=1,3,7)]

<span data-ttu-id="6d3c5-450">El código siguiente muestra tres métodos para aplicar `[SampleActionFilter]`:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-450">The following code shows three approaches to applying the `[SampleActionFilter]`:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet&highlight=1)]

<span data-ttu-id="6d3c5-451">En el código anterior, decorar el método con `[SampleActionFilter]` es el enfoque preferido para aplicar `SampleActionFilter`.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-451">In the preceding code, decorating the method with `[SampleActionFilter]` is the preferred approach to applying the `SampleActionFilter`.</span></span>

## <a name="using-middleware-in-the-filter-pipeline"></a><span data-ttu-id="6d3c5-452">Uso de middleware en la canalización de filtro</span><span class="sxs-lookup"><span data-stu-id="6d3c5-452">Using middleware in the filter pipeline</span></span>

<span data-ttu-id="6d3c5-453">Los filtros de recursos funcionan como el [middleware](xref:fundamentals/middleware/index), en el sentido de que se encargan de la ejecución de todo lo que viene después en la canalización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-453">Resource filters work like [middleware](xref:fundamentals/middleware/index) in that they surround the execution of everything that comes later in the pipeline.</span></span> <span data-ttu-id="6d3c5-454">Pero los filtros se diferencian del middleware en que forman parte del entorno de ejecución de ASP.NET Core, lo que significa que tienen acceso al contexto y las construcciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-454">But filters differ from middleware in that they're part of the ASP.NET Core runtime, which means that they have access to ASP.NET Core context and constructs.</span></span>

<span data-ttu-id="6d3c5-455">Para usar middleware como un filtro, cree un tipo con un método `Configure` en el que se especifique el middleware para insertar en la canalización de filtro.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-455">To use middleware as a filter, create a type with a `Configure` method that specifies the middleware to inject into the filter pipeline.</span></span> <span data-ttu-id="6d3c5-456">El ejemplo siguiente usa el middleware de localización para establecer la referencia cultural actual de una solicitud:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-456">The following example uses the localization middleware to establish the current culture for a request:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Filters/LocalizationPipeline.cs?name=snippet_MiddlewareFilter&highlight=3,21)]

<span data-ttu-id="6d3c5-457">Use <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> para ejecutar el middleware:</span><span class="sxs-lookup"><span data-stu-id="6d3c5-457">Use the <xref:Microsoft.AspNetCore.Mvc.MiddlewareFilterAttribute> to run the middleware:</span></span>

[!code-csharp[](./filters/sample/FiltersSample/Controllers/HomeController.cs?name=snippet_MiddlewareFilter&highlight=2)]

<span data-ttu-id="6d3c5-458">Los filtros de middleware se ejecutan en la misma fase de la canalización de filtro que los filtros de recursos, antes del enlace de modelos y después del resto de la canalización.</span><span class="sxs-lookup"><span data-stu-id="6d3c5-458">Middleware filters run at the same stage of the filter pipeline as Resource filters, before model binding and after the rest of the pipeline.</span></span>

## <a name="next-actions"></a><span data-ttu-id="6d3c5-459">Siguientes acciones</span><span class="sxs-lookup"><span data-stu-id="6d3c5-459">Next actions</span></span>

* <span data-ttu-id="6d3c5-460">Consulte [Métodos de filtrado para páginas de Razor](xref:razor-pages/filter).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-460">See [Filter methods for Razor Pages](xref:razor-pages/filter)</span></span>
* <span data-ttu-id="6d3c5-461">Para experimentar con los filtros, [descargue, pruebe y modifique el ejemplo de GitHub](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span><span class="sxs-lookup"><span data-stu-id="6d3c5-461">To experiment with filters, [download, test, and modify the GitHub sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/controllers/filters/sample).</span></span>
