---
title: Elementos de aplicación en ASP.NET Core
author: rick-anderson
description: Uso compartido de controladores, vistas, Razor Pages y mucho más con los elementos de aplicación en ASP.NET Core
ms.author: riande
ms.date: 11/10/2019
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 92c408adfad8af3dfd2572b625ae28ef24f64948
ms.sourcegitcommit: a7bbe3890befead19440075b05b9674351f98872
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905761"
---
# <a name="share-controllers-views-razor-pages-and-more-with-application-parts-in-aspnet-core"></a><span data-ttu-id="b40f9-103">Uso compartido de controladores, vistas, Razor Pages y mucho más con los elementos de aplicación en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b40f9-103">Share controllers, views, Razor Pages and more with Application Parts in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b40f9-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b40f9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b40f9-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b40f9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b40f9-106">Un *elemento de aplicación* es una abstracción sobre los recursos de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-106">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="b40f9-107">Los elementos de aplicación permiten a ASP.NET Core detectar controladores, componentes de vista, aplicaciones auxiliares de etiquetas, Razor Pages, orígenes de compilación de Razor, etc.</span><span class="sxs-lookup"><span data-stu-id="b40f9-107">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="b40f9-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is un elemento de aplicación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-108">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="b40f9-109">`AssemblyPart` encapsula una referencia de ensamblado y expone los tipos y las referencias de compilación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-109">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="b40f9-110">Los *proveedores de características* trabajan con los elementos de aplicación para rellenar las características de una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b40f9-110">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="b40f9-111">El caso de uso principal de los elementos de aplicación es configurar una aplicación para detectar (o evitar cargar) características de ASP.NET Core de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b40f9-111">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="b40f9-112">Por ejemplo, puede que desee compartir la funcionalidad común entre varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b40f9-112">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="b40f9-113">Mediante el uso de elementos de aplicación, puede compartir un ensamblado (DLL) que contenga controladores, vistas, Razor Pages, orígenes de compilación de Razor, aplicaciones auxiliares de etiquetas y mucho más con varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b40f9-113">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="b40f9-114">Es preferible compartir un ensamblado a duplicar el código en varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="b40f9-114">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="b40f9-115">Las aplicaciones ASP.NET Core cargan características de <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="b40f9-115">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="b40f9-116">La clase <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> representa un elemento de aplicación que está respaldado por un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b40f9-116">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="b40f9-117">Carga de características de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b40f9-117">Load ASP.NET Core features</span></span>

<span data-ttu-id="b40f9-118">Use las clases `ApplicationPart` y `AssemblyPart` para detectar y cargar características de ASP.NET Core (controladores, componentes de vista, etc.).</span><span class="sxs-lookup"><span data-stu-id="b40f9-118">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="b40f9-119"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> realiza un seguimiento de los elementos de aplicación y los proveedores de características disponibles.</span><span class="sxs-lookup"><span data-stu-id="b40f9-119">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="b40f9-120">`ApplicationPartManager` se configura en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-120">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="b40f9-121">El código siguiente proporciona un enfoque alternativo para configurar `ApplicationPartManager` mediante `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-121">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="b40f9-122">Los dos ejemplos de código anteriores cargan `SharedController` de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b40f9-122">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="b40f9-123">`SharedController` no está en el proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-123">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="b40f9-124">Vea la descarga de ejemplo de la [solución WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts).</span><span class="sxs-lookup"><span data-stu-id="b40f9-124">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="b40f9-125">Inclusión de vistas</span><span class="sxs-lookup"><span data-stu-id="b40f9-125">Include views</span></span>

<span data-ttu-id="b40f9-126">Para incluir vistas en el ensamblado:</span><span class="sxs-lookup"><span data-stu-id="b40f9-126">To include views in the assembly:</span></span>

* <span data-ttu-id="b40f9-127">Agregue el marcado siguiente al archivo de proyecto compartido:</span><span class="sxs-lookup"><span data-stu-id="b40f9-127">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="b40f9-128">Agregue <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> a <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="b40f9-128">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="b40f9-129">Impedimento de la carga de los recursos</span><span class="sxs-lookup"><span data-stu-id="b40f9-129">Prevent loading resources</span></span>

<span data-ttu-id="b40f9-130">Los elementos de aplicación se pueden usar para *evitar* la carga de recursos en un ensamblado o ubicación en concreto.</span><span class="sxs-lookup"><span data-stu-id="b40f9-130">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="b40f9-131">Agregue o quite miembros de la colección <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> para ocultar los recursos o hacer que estos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="b40f9-131">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="b40f9-132">El orden de las entradas de la colección `ApplicationParts` es irrelevante.</span><span class="sxs-lookup"><span data-stu-id="b40f9-132">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="b40f9-133">Configure `ApplicationPartManager` antes de usarlo para configurar los servicios en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="b40f9-133">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="b40f9-134">Por ejemplo, configure `ApplicationPartManager` antes de invocar a `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="b40f9-134">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="b40f9-135">Llame a `Remove` en la colección `ApplicationParts` para quitar un recurso.</span><span class="sxs-lookup"><span data-stu-id="b40f9-135">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="b40f9-136">En el código siguiente se usa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> para quitar `MyDependentLibrary` de la aplicación: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="b40f9-136">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="b40f9-137">`ApplicationPartManager` incluye elementos para:</span><span class="sxs-lookup"><span data-stu-id="b40f9-137">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="b40f9-138">El ensamblado de la aplicación y los ensamblados dependientes.</span><span class="sxs-lookup"><span data-stu-id="b40f9-138">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="b40f9-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="b40f9-139">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="b40f9-140">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="b40f9-140">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="b40f9-141">Proveedores de características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b40f9-141">Application feature providers</span></span>

<span data-ttu-id="b40f9-142">Los proveedores de características de la aplicación examinan los elementos de aplicación y proporcionan características para esos elementos.</span><span class="sxs-lookup"><span data-stu-id="b40f9-142">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="b40f9-143">Existen proveedores de características integrados para las siguientes características de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b40f9-143">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="b40f9-144">Controladores</span><span class="sxs-lookup"><span data-stu-id="b40f9-144">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="b40f9-145">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="b40f9-145">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="b40f9-146">Componentes de vista</span><span class="sxs-lookup"><span data-stu-id="b40f9-146">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="b40f9-147">Los proveedores de características heredan de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, donde `T` es el tipo de la característica.</span><span class="sxs-lookup"><span data-stu-id="b40f9-147">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="b40f9-148">Los proveedores de características se pueden implementar para cualquiera de los tipos de características enumerados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b40f9-148">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="b40f9-149">El orden de los proveedores de características en `ApplicationPartManager.FeatureProviders` puede afectar al comportamiento en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b40f9-149">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="b40f9-150">Los proveedores agregados posteriormente pueden reaccionar ante las acciones realizadas por los proveedores agregados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b40f9-150">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="b40f9-151">característica de controlador genérico</span><span class="sxs-lookup"><span data-stu-id="b40f9-151">Generic controller feature</span></span>

<span data-ttu-id="b40f9-152">ASP.NET Core omite los [controladores genéricos](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="b40f9-152">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="b40f9-153">Un controlador genérico tiene un parámetro de tipo (por ejemplo, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="b40f9-153">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="b40f9-154">En el ejemplo siguiente se agregan instancias de controlador genérico para una lista especificada de tipos:</span><span class="sxs-lookup"><span data-stu-id="b40f9-154">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="b40f9-155">Los tipos se definen en `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-155">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="b40f9-156">El proveedor de características se agrega en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-156">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="b40f9-157">Los nombres de controlador genérico utilizados para el enrutamiento tienen el formato *GenericController\`1[Widget]* en lugar de *Widget*.</span><span class="sxs-lookup"><span data-stu-id="b40f9-157">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="b40f9-158">El siguiente atributo modifica el nombre para coincidir con el tipo genérico usado por el controlador:</span><span class="sxs-lookup"><span data-stu-id="b40f9-158">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="b40f9-159">La clase `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-159">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="b40f9-160">Por ejemplo, si se solicita una dirección URL de `https://localhost:5001/Sprocket`, se obtiene la siguiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="b40f9-160">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="b40f9-161">muestra de las características disponibles</span><span class="sxs-lookup"><span data-stu-id="b40f9-161">Display available features</span></span>

<span data-ttu-id="b40f9-162">Las características disponibles para una aplicación se pueden enumerar solicitando `ApplicationPartManager` a través de la [inserción de dependencias](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="b40f9-162">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="b40f9-163">El [ejemplo de descarga](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) usa el código anterior para mostrar las características de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b40f9-163">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b40f9-164">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b40f9-164">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b40f9-165">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b40f9-165">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b40f9-166">Un *elemento de aplicación* es una abstracción sobre los recursos de una aplicación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-166">An *Application Part* is an abstraction over the resources of an app.</span></span> <span data-ttu-id="b40f9-167">Los elementos de aplicación permiten a ASP.NET Core detectar controladores, componentes de vista, aplicaciones auxiliares de etiquetas, Razor Pages, orígenes de compilación de Razor, etc.</span><span class="sxs-lookup"><span data-stu-id="b40f9-167">Application Parts allow ASP.NET Core to discover controllers, view components, tag helpers, Razor Pages, razor compilation sources, and more.</span></span> <span data-ttu-id="b40f9-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is un elemento de aplicación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-168">[AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) is an Application part.</span></span> <span data-ttu-id="b40f9-169">`AssemblyPart` encapsula una referencia de ensamblado y expone los tipos y las referencias de compilación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-169">`AssemblyPart` encapsulates an assembly reference and exposes types and compilation references.</span></span>

<span data-ttu-id="b40f9-170">Los *proveedores de características* trabajan con los elementos de aplicación para rellenar las características de una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b40f9-170">*Feature providers* work with application parts to populate the features of an ASP.NET Core app.</span></span> <span data-ttu-id="b40f9-171">El caso de uso principal de los elementos de aplicación es configurar una aplicación para detectar (o evitar cargar) características de ASP.NET Core de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b40f9-171">The main use case for application parts is to configure an app to discover (or avoid loading) ASP.NET Core features from an assembly.</span></span> <span data-ttu-id="b40f9-172">Por ejemplo, puede que desee compartir la funcionalidad común entre varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b40f9-172">For example, you might want to share common functionality between multiple apps.</span></span> <span data-ttu-id="b40f9-173">Mediante el uso de elementos de aplicación, puede compartir un ensamblado (DLL) que contenga controladores, vistas, Razor Pages, orígenes de compilación de Razor, aplicaciones auxiliares de etiquetas y mucho más con varias aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b40f9-173">Using Application Parts, you can share an assembly (DLL) containing controllers, views, Razor Pages, razor compilation sources, Tag Helpers, and more with multiple apps.</span></span> <span data-ttu-id="b40f9-174">Es preferible compartir un ensamblado a duplicar el código en varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="b40f9-174">Sharing an assembly is preferred to duplicating code in multiple projects.</span></span>

<span data-ttu-id="b40f9-175">Las aplicaciones ASP.NET Core cargan características de <xref:System.Web.WebPages.ApplicationPart>.</span><span class="sxs-lookup"><span data-stu-id="b40f9-175">ASP.NET Core apps load features from <xref:System.Web.WebPages.ApplicationPart>.</span></span> <span data-ttu-id="b40f9-176">La clase <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> representa un elemento de aplicación que está respaldado por un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b40f9-176">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.AssemblyPart> class represents an application part that's backed by an assembly.</span></span>

## <a name="load-aspnet-core-features"></a><span data-ttu-id="b40f9-177">Carga de características de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b40f9-177">Load ASP.NET Core features</span></span>

<span data-ttu-id="b40f9-178">Use las clases `ApplicationPart` y `AssemblyPart` para detectar y cargar características de ASP.NET Core (controladores, componentes de vista, etc.).</span><span class="sxs-lookup"><span data-stu-id="b40f9-178">Use the `ApplicationPart` and `AssemblyPart` classes to discover and load ASP.NET Core features (controllers, view components, etc.).</span></span> <span data-ttu-id="b40f9-179"><xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> realiza un seguimiento de los elementos de aplicación y los proveedores de características disponibles.</span><span class="sxs-lookup"><span data-stu-id="b40f9-179">The <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.ApplicationPartManager> tracks the application parts and feature providers available.</span></span> <span data-ttu-id="b40f9-180">`ApplicationPartManager` se configura en `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-180">`ApplicationPartManager` is configured in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup.cs?name=snippet)]

<span data-ttu-id="b40f9-181">El código siguiente proporciona un enfoque alternativo para configurar `ApplicationPartManager` mediante `AssemblyPart`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-181">The following code provides an alternative approach to configuring `ApplicationPartManager` using `AssemblyPart`:</span></span>

[!code-csharp[](./app-parts/sample1/WebAppParts/Startup2.cs?name=snippet)]

<span data-ttu-id="b40f9-182">Los dos ejemplos de código anteriores cargan `SharedController` de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="b40f9-182">The preceding two code samples load the `SharedController` from an assembly.</span></span> <span data-ttu-id="b40f9-183">`SharedController` no está en el proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b40f9-183">The `SharedController` is not in the application's project.</span></span> <span data-ttu-id="b40f9-184">Vea la descarga de ejemplo de la [solución WebAppParts](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts).</span><span class="sxs-lookup"><span data-stu-id="b40f9-184">See the [WebAppParts solution](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample1/WebAppParts) sample download.</span></span>

### <a name="include-views"></a><span data-ttu-id="b40f9-185">Inclusión de vistas</span><span class="sxs-lookup"><span data-stu-id="b40f9-185">Include views</span></span>

<span data-ttu-id="b40f9-186">Para incluir vistas en el ensamblado:</span><span class="sxs-lookup"><span data-stu-id="b40f9-186">To include views in the assembly:</span></span>

* <span data-ttu-id="b40f9-187">Agregue el marcado siguiente al archivo de proyecto compartido:</span><span class="sxs-lookup"><span data-stu-id="b40f9-187">Add the following markup to the shared project file:</span></span>

  ```csproj
  <ItemGroup>
      <EmbeddedResource Include="Views\**\*.cshtml" />
  </ItemGroup>
  ```

* <span data-ttu-id="b40f9-188">Agregue <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> a <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span><span class="sxs-lookup"><span data-stu-id="b40f9-188">Add the <xref:Microsoft.Extensions.FileProviders.EmbeddedFileProvider> to the <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngine>:</span></span>

  [!code-csharp[](./app-parts/sample1/WebAppParts/StartupViews.cs?name=snippet&highlight=3-7)]

### <a name="prevent-loading-resources"></a><span data-ttu-id="b40f9-189">Impedimento de la carga de los recursos</span><span class="sxs-lookup"><span data-stu-id="b40f9-189">Prevent loading resources</span></span>

<span data-ttu-id="b40f9-190">Los elementos de aplicación se pueden usar para *evitar* la carga de recursos en un ensamblado o ubicación en concreto.</span><span class="sxs-lookup"><span data-stu-id="b40f9-190">Application parts can be used to *avoid* loading resources in a particular assembly or location.</span></span> <span data-ttu-id="b40f9-191">Agregue o quite miembros de la colección <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> para ocultar los recursos o hacer que estos estén disponibles.</span><span class="sxs-lookup"><span data-stu-id="b40f9-191">Add or remove members of the  <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> collection to hide or make available resources.</span></span> <span data-ttu-id="b40f9-192">El orden de las entradas de la colección `ApplicationParts` es irrelevante.</span><span class="sxs-lookup"><span data-stu-id="b40f9-192">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="b40f9-193">Configure `ApplicationPartManager` antes de usarlo para configurar los servicios en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="b40f9-193">Configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="b40f9-194">Por ejemplo, configure `ApplicationPartManager` antes de invocar a `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="b40f9-194">For example, configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="b40f9-195">Llame a `Remove` en la colección `ApplicationParts` para quitar un recurso.</span><span class="sxs-lookup"><span data-stu-id="b40f9-195">Call `Remove` on the `ApplicationParts` collection to remove a resource.</span></span>

<span data-ttu-id="b40f9-196">En el código siguiente se usa <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> para quitar `MyDependentLibrary` de la aplicación: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span><span class="sxs-lookup"><span data-stu-id="b40f9-196">The following code uses <xref:Microsoft.AspNetCore.Mvc.ApplicationParts> to remove `MyDependentLibrary` from the app: [!code-csharp[](./app-parts/sample1/WebAppParts/StartupRm.cs?name=snippet)]</span></span>

<span data-ttu-id="b40f9-197">`ApplicationPartManager` incluye elementos para:</span><span class="sxs-lookup"><span data-stu-id="b40f9-197">The `ApplicationPartManager` includes parts for:</span></span>

* <span data-ttu-id="b40f9-198">El ensamblado de la aplicación y los ensamblados dependientes.</span><span class="sxs-lookup"><span data-stu-id="b40f9-198">The app's assembly and dependent assemblies.</span></span>
* <span data-ttu-id="b40f9-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span><span class="sxs-lookup"><span data-stu-id="b40f9-199">`Microsoft.AspNetCore.Mvc.TagHelpers`.</span></span>
* <span data-ttu-id="b40f9-200">`Microsoft.AspNetCore.Mvc.Razor`.</span><span class="sxs-lookup"><span data-stu-id="b40f9-200">`Microsoft.AspNetCore.Mvc.Razor`.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="b40f9-201">Proveedores de características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="b40f9-201">Application feature providers</span></span>

<span data-ttu-id="b40f9-202">Los proveedores de características de la aplicación examinan los elementos de aplicación y proporcionan características para esos elementos.</span><span class="sxs-lookup"><span data-stu-id="b40f9-202">Application feature providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="b40f9-203">Existen proveedores de características integrados para las siguientes características de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b40f9-203">There are built-in feature providers for the following ASP.NET Core features:</span></span>

* [<span data-ttu-id="b40f9-204">Controladores</span><span class="sxs-lookup"><span data-stu-id="b40f9-204">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="b40f9-205">Asistentes de etiquetas</span><span class="sxs-lookup"><span data-stu-id="b40f9-205">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="b40f9-206">Componentes de vista</span><span class="sxs-lookup"><span data-stu-id="b40f9-206">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="b40f9-207">Los proveedores de características heredan de <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, donde `T` es el tipo de la característica.</span><span class="sxs-lookup"><span data-stu-id="b40f9-207">Feature providers inherit from <xref:Microsoft.AspNetCore.Mvc.ApplicationParts.IApplicationFeatureProvider`1>, where `T` is the type of the feature.</span></span> <span data-ttu-id="b40f9-208">Los proveedores de características se pueden implementar para cualquiera de los tipos de características enumerados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b40f9-208">Feature providers can be implemented for any of the previously listed feature types.</span></span> <span data-ttu-id="b40f9-209">El orden de los proveedores de características en `ApplicationPartManager.FeatureProviders` puede afectar al comportamiento en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b40f9-209">The order of feature providers in the `ApplicationPartManager.FeatureProviders` can impact run time behavior.</span></span> <span data-ttu-id="b40f9-210">Los proveedores agregados posteriormente pueden reaccionar ante las acciones realizadas por los proveedores agregados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="b40f9-210">Later added providers can react to actions taken by earlier added providers.</span></span>

### <a name="generic-controller-feature"></a><span data-ttu-id="b40f9-211">característica de controlador genérico</span><span class="sxs-lookup"><span data-stu-id="b40f9-211">Generic controller feature</span></span>

<span data-ttu-id="b40f9-212">ASP.NET Core omite los [controladores genéricos](/dotnet/csharp/programming-guide/generics/generic-classes).</span><span class="sxs-lookup"><span data-stu-id="b40f9-212">ASP.NET Core ignores [generic controllers](/dotnet/csharp/programming-guide/generics/generic-classes).</span></span> <span data-ttu-id="b40f9-213">Un controlador genérico tiene un parámetro de tipo (por ejemplo, `MyController<T>`).</span><span class="sxs-lookup"><span data-stu-id="b40f9-213">A generic controller has a type parameter (for example, `MyController<T>`).</span></span> <span data-ttu-id="b40f9-214">En el ejemplo siguiente se agregan instancias de controlador genérico para una lista especificada de tipos:</span><span class="sxs-lookup"><span data-stu-id="b40f9-214">The following sample adds generic controller instances for a specified list of types:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerFeatureProvider.cs?name=snippet)]

<span data-ttu-id="b40f9-215">Los tipos se definen en `EntityTypes.Types`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-215">The types are defined in `EntityTypes.Types`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Models/EntityTypes.cs?name=snippet)]

<span data-ttu-id="b40f9-216">El proveedor de características se agrega en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-216">The feature provider is added in `Startup`:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Startup.cs?name=snippet)]

<span data-ttu-id="b40f9-217">Los nombres de controlador genérico utilizados para el enrutamiento tienen el formato *GenericController\`1[Widget]* en lugar de *Widget*.</span><span class="sxs-lookup"><span data-stu-id="b40f9-217">Generic controller names used for routing are of the form *GenericController\`1[Widget]* rather than *Widget*.</span></span> <span data-ttu-id="b40f9-218">El siguiente atributo modifica el nombre para coincidir con el tipo genérico usado por el controlador:</span><span class="sxs-lookup"><span data-stu-id="b40f9-218">The following attribute modifies the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="b40f9-219">La clase `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="b40f9-219">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/GenericController.cs)]

<span data-ttu-id="b40f9-220">Por ejemplo, si se solicita una dirección URL de `https://localhost:5001/Sprocket`, se obtiene la siguiente respuesta:</span><span class="sxs-lookup"><span data-stu-id="b40f9-220">For example, requesting a URL of `https://localhost:5001/Sprocket` results in the following response:</span></span>

```text
Hello from a generic Sprocket controller.
```

### <a name="display-available-features"></a><span data-ttu-id="b40f9-221">muestra de las características disponibles</span><span class="sxs-lookup"><span data-stu-id="b40f9-221">Display available features</span></span>

<span data-ttu-id="b40f9-222">Las características disponibles para una aplicación se pueden enumerar solicitando `ApplicationPartManager` a través de la [inserción de dependencias](../../fundamentals/dependency-injection.md):</span><span class="sxs-lookup"><span data-stu-id="b40f9-222">The features available to an app can be enumerated by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md):</span></span>

[!code-csharp[](./app-parts/sample2/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="b40f9-223">El [ejemplo de descarga](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) usa el código anterior para mostrar las características de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b40f9-223">The [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample2) uses the preceding code to display the app features:</span></span>

```text
Controllers:
    - FeaturesController
    - HomeController
    - HelloController
    - GenericController`1
    - GenericController`1
Tag Helpers:
    - PrerenderTagHelper
    - AnchorTagHelper
    - CacheTagHelper
    - DistributedCacheTagHelper
    - EnvironmentTagHelper
    - Additional Tag Helpers omitted for brevity.
View Components:
    - SampleViewComponent
```

::: moniker-end
