---
title: "Inyección de dependencia en las vistas"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 80fb9e43-e4db-4af2-b2a8-e1364a712f69
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/dependency-injection
ms.openlocfilehash: 05d64858dd70b45a1e2bb90a86ab3cbdc85264b1
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="dependency-injection-into-views"></a><span data-ttu-id="e82e7-103">Inyección de dependencia en las vistas</span><span class="sxs-lookup"><span data-stu-id="e82e7-103">Dependency injection into views</span></span>

<span data-ttu-id="e82e7-104">Por [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="e82e7-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="e82e7-105">ASP.NET Core es compatible con [inyección de dependencia](xref:fundamentals/dependency-injection) en vistas.</span><span class="sxs-lookup"><span data-stu-id="e82e7-105">ASP.NET Core supports [dependency injection](xref:fundamentals/dependency-injection) into views.</span></span> <span data-ttu-id="e82e7-106">Esto puede ser útil para los servicios específicos de la vista, como localización o solo es necesarios para rellenar los elementos de vista de datos.</span><span class="sxs-lookup"><span data-stu-id="e82e7-106">This can be useful for view-specific services, such as localization or data required only for populating view elements.</span></span> <span data-ttu-id="e82e7-107">Debe intentar mantener [separación de intereses](http://deviq.com/separation-of-concerns) entre los controladores y vistas.</span><span class="sxs-lookup"><span data-stu-id="e82e7-107">You should try to maintain [separation of concerns](http://deviq.com/separation-of-concerns) between your controllers and views.</span></span> <span data-ttu-id="e82e7-108">La mayoría de los datos que se muestran las vistas se debe pasar desde el controlador.</span><span class="sxs-lookup"><span data-stu-id="e82e7-108">Most of the data your views display should be passed in from the controller.</span></span>

[<span data-ttu-id="e82e7-109">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="e82e7-109">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/dependency-injection/sample)

## <a name="a-simple-example"></a><span data-ttu-id="e82e7-110">Un ejemplo sencillo</span><span class="sxs-lookup"><span data-stu-id="e82e7-110">A Simple Example</span></span>

<span data-ttu-id="e82e7-111">También puede insertar un servicio en una vista mediante el `@inject` directiva.</span><span class="sxs-lookup"><span data-stu-id="e82e7-111">You can inject a service into a view using the `@inject` directive.</span></span> <span data-ttu-id="e82e7-112">Puede pensar en `@inject` como agregar una propiedad a la vista y rellenar la propiedad mediante DI.</span><span class="sxs-lookup"><span data-stu-id="e82e7-112">You can think of `@inject` as adding a property to your view, and populating the property using DI.</span></span>

<span data-ttu-id="e82e7-113">La sintaxis de `@inject`:`@inject <type> <name>`</span><span class="sxs-lookup"><span data-stu-id="e82e7-113">The syntax for `@inject`: `@inject <type> <name>`</span></span>

<span data-ttu-id="e82e7-114">Un ejemplo de `@inject` en acción:</span><span class="sxs-lookup"><span data-stu-id="e82e7-114">An example of `@inject` in action:</span></span>

<span data-ttu-id="e82e7-115">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]</span><span class="sxs-lookup"><span data-stu-id="e82e7-115">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/ToDo/Index.cshtml?highlight=4,5,15,16,17)]</span></span>

<span data-ttu-id="e82e7-116">Esta vista muestra una lista de `ToDoItem` instancias, junto con un resumen de estadísticas generales.</span><span class="sxs-lookup"><span data-stu-id="e82e7-116">This view displays a list of `ToDoItem` instances, along with a summary showing overall statistics.</span></span> <span data-ttu-id="e82e7-117">El resumen se rellena desde la insertado `StatisticsService`.</span><span class="sxs-lookup"><span data-stu-id="e82e7-117">The summary is populated from the injected `StatisticsService`.</span></span> <span data-ttu-id="e82e7-118">Este servicio está registrado para la inyección de dependencia en `ConfigureServices` en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e82e7-118">This service is registered for dependency injection in `ConfigureServices` in *Startup.cs*:</span></span>

<span data-ttu-id="e82e7-119">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]</span><span class="sxs-lookup"><span data-stu-id="e82e7-119">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Startup.cs?highlight=6,7&range=15-22)]</span></span>

<span data-ttu-id="e82e7-120">El `StatisticsService` realiza algunos cálculos en el conjunto de `ToDoItem` instancias, que tiene acceso a través de un repositorio:</span><span class="sxs-lookup"><span data-stu-id="e82e7-120">The `StatisticsService` performs some calculations on the set of `ToDoItem` instances, which it accesses via a repository:</span></span>

<span data-ttu-id="e82e7-121">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]</span><span class="sxs-lookup"><span data-stu-id="e82e7-121">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/StatisticsService.cs?highlight=15,20,26)]</span></span>

<span data-ttu-id="e82e7-122">El repositorio de ejemplo utiliza una colección en memoria.</span><span class="sxs-lookup"><span data-stu-id="e82e7-122">The sample repository uses an in-memory collection.</span></span> <span data-ttu-id="e82e7-123">La implementación se muestra arriba (que funciona en todos los datos en memoria) no se recomienda para conjuntos de datos grandes, con acceso de forma remota.</span><span class="sxs-lookup"><span data-stu-id="e82e7-123">The implementation shown above (which operates on all of the data in memory) is not recommended for large, remotely accessed data sets.</span></span>

<span data-ttu-id="e82e7-124">El ejemplo muestra los datos del modelo enlazado a la vista y el servicio que se insertan en la vista:</span><span class="sxs-lookup"><span data-stu-id="e82e7-124">The sample displays data from the model bound to the view and the service injected into the view:</span></span>

![Para ver la lista de elementos total, completa elementos de prioridad Media y una lista de tareas con sus niveles de prioridad y los valores de tipo boolean que indica la finalización.](dependency-injection/_static/screenshot.png)

## <a name="populating-lookup-data"></a><span data-ttu-id="e82e7-126">Rellenar los datos de búsqueda</span><span class="sxs-lookup"><span data-stu-id="e82e7-126">Populating Lookup Data</span></span>

<span data-ttu-id="e82e7-127">Inyección de código de la vista puede ser útil para rellenar las opciones de elementos de interfaz de usuario, como listas desplegables.</span><span class="sxs-lookup"><span data-stu-id="e82e7-127">View injection can be useful to populate options in UI elements, such as dropdown lists.</span></span> <span data-ttu-id="e82e7-128">Considere la posibilidad de un formulario de perfil de usuario que incluye opciones para especificar el género, estado y otras preferencias.</span><span class="sxs-lookup"><span data-stu-id="e82e7-128">Consider a user profile form that includes options for specifying gender, state, and other preferences.</span></span> <span data-ttu-id="e82e7-129">Representar este tipo un formulario mediante un enfoque MVC estándar requeriría el controlador para solicitar servicios de acceso de datos para cada uno de estos conjuntos de opciones y, a continuación, rellenar un modelo o `ViewBag` con cada conjunto de opciones para enlazar.</span><span class="sxs-lookup"><span data-stu-id="e82e7-129">Rendering such a form using a standard MVC approach would require the controller to request data access services for each of these sets of options, and then populate a model or `ViewBag` with each set of options to be bound.</span></span>

<span data-ttu-id="e82e7-130">Un enfoque alternativo inserta services directamente en la vista para obtener las opciones.</span><span class="sxs-lookup"><span data-stu-id="e82e7-130">An alternative approach injects services directly into the view to obtain the options.</span></span> <span data-ttu-id="e82e7-131">Esto reduce la cantidad de código necesario para el controlador, mover esta lógica de construcción del elemento de vista en la propia vista.</span><span class="sxs-lookup"><span data-stu-id="e82e7-131">This minimizes the amount of code required by the controller, moving this view element construction logic into the view itself.</span></span> <span data-ttu-id="e82e7-132">La acción del controlador para mostrar un formulario de edición de perfil sólo necesita pasar el formulario de la instancia de perfil:</span><span class="sxs-lookup"><span data-stu-id="e82e7-132">The controller action to display a profile editing form only needs to pass the form the profile instance:</span></span>

<span data-ttu-id="e82e7-133">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]</span><span class="sxs-lookup"><span data-stu-id="e82e7-133">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Controllers/ProfileController.cs?highlight=9,19)]</span></span>

<span data-ttu-id="e82e7-134">El formulario HTML que se utiliza para actualizar estas preferencias incluye listas desplegables para tres de las propiedades:</span><span class="sxs-lookup"><span data-stu-id="e82e7-134">The HTML form used to update these preferences includes dropdown lists for three of the properties:</span></span>

![Actualizar vista del perfil con un formulario que permite la entrada de nombre, sexo, estado y su Color favorito.](dependency-injection/_static/updateprofile.png)

<span data-ttu-id="e82e7-136">Estas listas se rellenan con un servicio que se ha insertado en la vista:</span><span class="sxs-lookup"><span data-stu-id="e82e7-136">These lists are populated by a service that has been injected into the view:</span></span>

<span data-ttu-id="e82e7-137">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]</span><span class="sxs-lookup"><span data-stu-id="e82e7-137">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Profile/Index.cshtml?highlight=4,16,17,21,22,26,27)]</span></span>

<span data-ttu-id="e82e7-138">El `ProfileOptionsService` es un servicio de nivel de interfaz de usuario diseñado para proporcionar solo los datos necesarios para este formulario:</span><span class="sxs-lookup"><span data-stu-id="e82e7-138">The `ProfileOptionsService` is a UI-level service designed to provide just the data needed for this form:</span></span>

<span data-ttu-id="e82e7-139">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]</span><span class="sxs-lookup"><span data-stu-id="e82e7-139">[!code-csharp[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Model/Services/ProfileOptionsService.cs?highlight=7,13,24)]</span></span>

>[!TIP]
> <span data-ttu-id="e82e7-140">No olvide registrar tipos solicitará a través de la inyección de dependencia en el `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e82e7-140">Don't forget to register types you will request through dependency injection in the  `ConfigureServices` method in *Startup.cs*.</span></span>

## <a name="overriding-services"></a><span data-ttu-id="e82e7-141">Reemplazar servicios</span><span class="sxs-lookup"><span data-stu-id="e82e7-141">Overriding Services</span></span>

<span data-ttu-id="e82e7-142">Además de insertar nuevos servicios, esta técnica también puede usarse para reemplazar servicios previamente insertados en una página.</span><span class="sxs-lookup"><span data-stu-id="e82e7-142">In addition to injecting new services, this technique can also be used to override previously injected services on a page.</span></span> <span data-ttu-id="e82e7-143">La siguiente ilustración muestra todos los campos disponibles en la página que se utiliza en el primer ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e82e7-143">The figure below shows all of the fields available on the page used in the first example:</span></span>

![Menú contextual de IntelliSense en una lista de campos de Html, componente, StatsService y dirección Url del símbolo @](dependency-injection/_static/razor-fields.png)

<span data-ttu-id="e82e7-145">Como puede ver, los campos predeterminados incluyen `Html`, `Component`, y `Url` (así como el `StatsService` que hemos insertado).</span><span class="sxs-lookup"><span data-stu-id="e82e7-145">As you can see, the default fields include `Html`, `Component`, and `Url` (as well as the `StatsService` that we injected).</span></span> <span data-ttu-id="e82e7-146">Si para la instancia que desea reemplazar las aplicaciones auxiliares HTML de forma predeterminada por el suyo propio, puede hacerlo fácilmente mediante `@inject`:</span><span class="sxs-lookup"><span data-stu-id="e82e7-146">If for instance you wanted to replace the default HTML Helpers with your own, you could easily do so using `@inject`:</span></span>

<span data-ttu-id="e82e7-147">[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]</span><span class="sxs-lookup"><span data-stu-id="e82e7-147">[!code-html[Main](../../mvc/views/dependency-injection/sample/src/ViewInjectSample/Views/Helper/Index.cshtml?highlight=3,11)]</span></span>

<span data-ttu-id="e82e7-148">Si desea ampliar los servicios existentes, simplemente puede utilizar esta técnica al heredar o ajuste la implementación existente por el suyo propio.</span><span class="sxs-lookup"><span data-stu-id="e82e7-148">If you want to extend existing services, you can simply use this technique while inheriting from or wrapping the existing implementation with your own.</span></span>

## <a name="see-also"></a><span data-ttu-id="e82e7-149">Vea también</span><span class="sxs-lookup"><span data-stu-id="e82e7-149">See Also</span></span>

* <span data-ttu-id="e82e7-150">Blog de Simon Timms: [obtener datos de búsqueda en la vista](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span><span class="sxs-lookup"><span data-stu-id="e82e7-150">Simon Timms Blog: [Getting Lookup Data Into Your View](http://blog.simontimms.com/2015/06/09/getting-lookup-data-into-you-view/)</span></span>
