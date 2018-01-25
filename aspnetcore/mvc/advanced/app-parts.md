---
title: "Partes de la aplicación de ASP.NET Core"
author: ardalis
description: "Obtenga información acerca de cómo utilizar componentes de la aplicación, que son abstrations sobre los recursos de una aplicación, para configurar la aplicación para detectar o evitar la carga de características desde un ensamblado."
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: 702d7773374f331b25489060b18f752186d7acea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="09f04-103">Partes de la aplicación de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="09f04-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="09f04-104">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="09f04-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="09f04-105">Un *parte de la aplicación* es una abstracción sobre los recursos de una aplicación, desde el que MVC características como controladores, componentes de la vista, o se pueden detectar aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="09f04-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="09f04-106">Un ejemplo de una parte de la aplicación es un AssemblyPart, que encapsula una referencia de ensamblado y expone los tipos y las referencias de la compilación.</span><span class="sxs-lookup"><span data-stu-id="09f04-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="09f04-107">*Proveedores de características* funcionan con los componentes de la aplicación para rellenar las características de una aplicación de MVC de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="09f04-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="09f04-108">Es el caso de uso principal para las partes de la aplicación para que pueda configurar la aplicación para detectar (o si se evita la carga) características MVC desde un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="09f04-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="09f04-109">Introducción a partes de la aplicación</span><span class="sxs-lookup"><span data-stu-id="09f04-109">Introducing Application Parts</span></span>

<span data-ttu-id="09f04-110">Aplicaciones MVC cargar sus características de [partes de la aplicación](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="09f04-110">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="09f04-111">En concreto, el [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) clase representa una parte de la aplicación que está respaldada por un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="09f04-111">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="09f04-112">Puede utilizar estas clases para detectar y cargar las características MVC, tales como controladores, componentes de la vista, aplicaciones auxiliares de etiquetas y orígenes de compilación de razor.</span><span class="sxs-lookup"><span data-stu-id="09f04-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="09f04-113">El [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) es responsable de seguimiento de los componentes de la aplicación y proveedores de características disponibles para la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="09f04-113">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="09f04-114">Puede interactuar con el `ApplicationPartManager` en `Startup` cuando configura MVC:</span><span class="sxs-lookup"><span data-stu-id="09f04-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="09f04-115">De forma predeterminada MVC buscará el árbol de dependencias y buscar controladores (incluso en otros ensamblados).</span><span class="sxs-lookup"><span data-stu-id="09f04-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="09f04-116">Para cargar un ensamblado arbitrario (por ejemplo, desde un complemento que no se hace referencia en tiempo de compilación), puede utilizar una parte de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09f04-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="09f04-117">Puede utilizar elementos de aplicación para *evitar* buscando controladores en una ubicación o un ensamblado concreto.</span><span class="sxs-lookup"><span data-stu-id="09f04-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="09f04-118">Puede controlar qué elementos (o ensamblados) están disponibles para la aplicación mediante la modificación de la `ApplicationParts` colección de la `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="09f04-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="09f04-119">El orden de las entradas de la `ApplicationParts` colección no es importante.</span><span class="sxs-lookup"><span data-stu-id="09f04-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="09f04-120">Es importante configurar totalmente el `ApplicationPartManager` antes de usarlo para configurar los servicios en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="09f04-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="09f04-121">Por ejemplo, debe configurar totalmente el `ApplicationPartManager` antes de invocar `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="09f04-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="09f04-122">Si no lo hace así, significará que agregar controladores en partes de la aplicación después de que no se verá afectada llamada al método (no obtener registrado como servicios) que podría dar lugar a bevavior incorrecta de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="09f04-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="09f04-123">Si tiene un ensamblado que contiene los controladores que no desea usar, quítelo de la `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="09f04-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="09f04-124">Además de ensamblado del proyecto y sus ensamblados dependientes, la `ApplicationPartManager` incluirá partes de `Microsoft.AspNetCore.Mvc.TagHelpers` y `Microsoft.AspNetCore.Mvc.Razor` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="09f04-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="09f04-125">Proveedores de características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="09f04-125">Application Feature Providers</span></span>

<span data-ttu-id="09f04-126">Proveedores de características de la aplicación examinar los componentes de la aplicación y proporciona características para las partes.</span><span class="sxs-lookup"><span data-stu-id="09f04-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="09f04-127">Existen proveedores de características integradas para las siguientes características MVC:</span><span class="sxs-lookup"><span data-stu-id="09f04-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="09f04-128">Controladores</span><span class="sxs-lookup"><span data-stu-id="09f04-128">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="09f04-129">Referencia de metadatos</span><span class="sxs-lookup"><span data-stu-id="09f04-129">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="09f04-130">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="09f04-130">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="09f04-131">Componentes de la vista</span><span class="sxs-lookup"><span data-stu-id="09f04-131">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="09f04-132">Proveedores de características que se heredan de `IApplicationFeatureProvider<T>`, donde `T` es el tipo de la característica.</span><span class="sxs-lookup"><span data-stu-id="09f04-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="09f04-133">Puede implementar su propia característica proveedores para cualquiera de los tipos de características de MVC mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="09f04-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="09f04-134">El orden de los proveedores de características en el `ApplicationPartManager.FeatureProviders` colección puede ser importante, puesto que proveedores posteriores pueden reaccionar ante las acciones realizadas por proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="09f04-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="09f04-135">Ejemplo: Característica de controladora genérica</span><span class="sxs-lookup"><span data-stu-id="09f04-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="09f04-136">De forma predeterminada, ASP.NET Core MVC omite controladores genéricos (por ejemplo, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="09f04-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="09f04-137">Este ejemplo usa un proveedor de la característica de controlador que se ejecuta una vez que el proveedor predeterminado y agrega instancias de controlador genérico de una lista especificada de tipos (definido en `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="09f04-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="09f04-138">Los tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="09f04-138">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="09f04-139">El proveedor de características se agrega en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="09f04-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="09f04-140">De forma predeterminada, los nombres de controlador genérico utilizados para el enrutamiento sería del formulario *GenericController'1 [Widget]* en lugar de *Widget*.</span><span class="sxs-lookup"><span data-stu-id="09f04-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="09f04-141">El siguiente atributo se usa para modificar el nombre que corresponda al tipo genérico usado por el controlador:</span><span class="sxs-lookup"><span data-stu-id="09f04-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="09f04-142">La `GenericController` clase:</span><span class="sxs-lookup"><span data-stu-id="09f04-142">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="09f04-143">El resultado, cuando se solicita una ruta coincidente:</span><span class="sxs-lookup"><span data-stu-id="09f04-143">The result, when a matching route is requested:</span></span>

![Ejemplo de salida de la aplicación de ejemplo que se lee, 'Hello desde un controlador de Sproket genérico'.](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="09f04-145">Ejemplo: Características disponibles de pantalla</span><span class="sxs-lookup"><span data-stu-id="09f04-145">Sample: Display available features</span></span>

<span data-ttu-id="09f04-146">Puede iterar a través de las características rellenos disponibles para la aplicación solicitando una `ApplicationPartManager` a través de [inyección de dependencia](../../fundamentals/dependency-injection.md) y usarlo para rellenar las instancias de la característica adecuada:</span><span class="sxs-lookup"><span data-stu-id="09f04-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="09f04-147">Resultado de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="09f04-147">Example output:</span></span>

![Ejemplo de salida de la aplicación de ejemplo](app-parts/_static/available-features.png)
