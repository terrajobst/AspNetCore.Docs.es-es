---
title: Elementos de aplicación en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo usar elementos de aplicación (que son abstracciones de los recursos de una aplicación) para detectar o evitar la carga de características desde un ensamblado.
ms.author: riande
ms.date: 01/04/2017
uid: mvc/extensibility/app-parts
ms.openlocfilehash: e0290ceadc159d7c3608ec4420d95cd219407d32
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276830"
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="79835-103">Elementos de aplicación en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="79835-103">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="79835-104">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="79835-104">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="79835-105">Un *elemento de aplicación* es una abstracción sobre los recursos de una aplicación desde el que se pueden detectar características de MVC como controladores, componentes de vista o aplicaciones auxiliares de etiquetas.</span><span class="sxs-lookup"><span data-stu-id="79835-105">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="79835-106">Un ejemplo de un elemento de aplicación es AssemblyPart, que encapsula una referencia de ensamblado y expone los tipos y las referencias de la compilación.</span><span class="sxs-lookup"><span data-stu-id="79835-106">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="79835-107">Los *proveedores de características* trabajan con los elementos de aplicación para rellenar las características de una aplicación de ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="79835-107">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="79835-108">El uso principal de los elementos de aplicación es permitir la configuración de la aplicación para detectar características MVC (o evitar su carga) de un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="79835-108">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="79835-109">Introducción a los elementos de aplicación</span><span class="sxs-lookup"><span data-stu-id="79835-109">Introducing Application Parts</span></span>

<span data-ttu-id="79835-110">Las aplicaciones MVC cargan sus características desde [elementos de aplicación](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="79835-110">MVC apps load their features from [application parts](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="79835-111">En particular, la clase [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) representa un elemento de aplicación que está respaldado por un ensamblado.</span><span class="sxs-lookup"><span data-stu-id="79835-111">In particular, the [AssemblyPart](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that's backed by an assembly.</span></span> <span data-ttu-id="79835-112">Puede utilizar estas clases para detectar y cargar las características MVC, tales como controladores, componentes de vista, aplicaciones auxiliares de etiquetas y orígenes de compilación de Razor.</span><span class="sxs-lookup"><span data-stu-id="79835-112">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="79835-113">[ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) es responsable del seguimiento de los elementos de aplicación y los proveedores de características disponibles para la aplicación MVC.</span><span class="sxs-lookup"><span data-stu-id="79835-113">The [ApplicationPartManager](/dotnet/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="79835-114">Puede interactuar con `ApplicationPartManager` en `Startup` cuando configura MVC:</span><span class="sxs-lookup"><span data-stu-id="79835-114">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => apm.ApplicationParts.Add(part));
```

<span data-ttu-id="79835-115">De forma predeterminada, MVC examinará el árbol de dependencias y buscará controladores (incluso en otros ensamblados).</span><span class="sxs-lookup"><span data-stu-id="79835-115">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="79835-116">Para cargar un ensamblado arbitrario (por ejemplo, desde un complemento al que no se hace referencia en tiempo de compilación), puede utilizar un elemento de aplicación.</span><span class="sxs-lookup"><span data-stu-id="79835-116">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="79835-117">Los elementos de aplicación se pueden usar para *evitar* tener que buscar controladores en una determinada ubicación o ensamblado.</span><span class="sxs-lookup"><span data-stu-id="79835-117">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="79835-118">Modifique la colección `ApplicationParts` de `ApplicationPartManager` para controlar qué elementos (o ensamblados) están disponibles para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="79835-118">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="79835-119">El orden de las entradas de la colección `ApplicationParts` es irrelevante.</span><span class="sxs-lookup"><span data-stu-id="79835-119">The order of the entries in the `ApplicationParts` collection isn't important.</span></span> <span data-ttu-id="79835-120">Es importante configurar totalmente `ApplicationPartManager` antes de usarlo para configurar los servicios en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="79835-120">It's important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="79835-121">Por ejemplo, debe configurar totalmente `ApplicationPartManager` antes de invocar a `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="79835-121">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="79835-122">Si no lo hace, significará que los controladores de elementos de aplicación que se han agregado después de esa llamada al método no se verán afectados (no se registrarán como servicios), lo que podría dar lugar a un comportamiento incorrecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="79835-122">Failing to do so, will mean that controllers in application parts added after that method call won't be affected (won't get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="79835-123">Si tiene un ensamblado que contiene controladores que no quiere usar, quítelo de `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="79835-123">If you have an assembly that contains controllers you don't want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm =>
    {
        var dependentLibrary = apm.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="79835-124">Además del ensamblado del proyecto y sus ensamblados dependientes, `ApplicationPartManager` incluirá elementos de `Microsoft.AspNetCore.Mvc.TagHelpers` y `Microsoft.AspNetCore.Mvc.Razor` de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="79835-124">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="79835-125">Proveedores de características de la aplicación</span><span class="sxs-lookup"><span data-stu-id="79835-125">Application Feature Providers</span></span>

<span data-ttu-id="79835-126">Los proveedores de características de la aplicación examinan los elementos de aplicación y proporcionan características para esos elementos.</span><span class="sxs-lookup"><span data-stu-id="79835-126">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="79835-127">Existen proveedores de características integrados para las siguientes características MVC:</span><span class="sxs-lookup"><span data-stu-id="79835-127">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="79835-128">Controladores</span><span class="sxs-lookup"><span data-stu-id="79835-128">Controllers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="79835-129">Referencia de los metadatos</span><span class="sxs-lookup"><span data-stu-id="79835-129">Metadata Reference</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="79835-130">Aplicaciones auxiliares de etiquetas</span><span class="sxs-lookup"><span data-stu-id="79835-130">Tag Helpers</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="79835-131">Componentes de vista</span><span class="sxs-lookup"><span data-stu-id="79835-131">View Components</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="79835-132">Los proveedores de características heredan de `IApplicationFeatureProvider<T>`, donde `T` es el tipo de la característica.</span><span class="sxs-lookup"><span data-stu-id="79835-132">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="79835-133">Puede implementar sus propios proveedores de características para cualquiera de los tipos de características de MVC mencionados anteriormente.</span><span class="sxs-lookup"><span data-stu-id="79835-133">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="79835-134">El orden de los proveedores de características en la colección `ApplicationPartManager.FeatureProviders` puede ser importante, puesto que los proveedores posteriores pueden reaccionar ante las acciones realizadas por los proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="79835-134">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="79835-135">Ejemplo: característica de controlador genérico</span><span class="sxs-lookup"><span data-stu-id="79835-135">Sample: Generic controller feature</span></span>

<span data-ttu-id="79835-136">De forma predeterminada, ASP.NET Core MVC omite los controladores genéricos (por ejemplo, `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="79835-136">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="79835-137">En este ejemplo se usa un proveedor de características de controlador que se ejecuta después del proveedor predeterminado y agrega instancias de controlador genérico para una lista especificada de tipos (definida en `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="79835-137">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="79835-138">Tipos de entidad:</span><span class="sxs-lookup"><span data-stu-id="79835-138">The entity types:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="79835-139">El proveedor de características se agrega en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="79835-139">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(apm => 
        apm.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="79835-140">De forma predeterminada, los nombres de controlador genérico utilizados para el enrutamiento tendrían el formato *GenericController\`1[Widget]* en lugar de *Widget*.</span><span class="sxs-lookup"><span data-stu-id="79835-140">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="79835-141">El siguiente atributo se usa para modificar el nombre para coincidir con el tipo genérico usado por el controlador:</span><span class="sxs-lookup"><span data-stu-id="79835-141">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="79835-142">La clase `GenericController`:</span><span class="sxs-lookup"><span data-stu-id="79835-142">The `GenericController` class:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="79835-143">Este es el resultado cuando se solicita una ruta coincidente:</span><span class="sxs-lookup"><span data-stu-id="79835-143">The result, when a matching route is requested:</span></span>

![En la salida de la aplicación de ejemplo se lee "Hello from a generic Sproket controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="79835-145">Ejemplo: mostrar las características disponibles</span><span class="sxs-lookup"><span data-stu-id="79835-145">Sample: Display available features</span></span>

<span data-ttu-id="79835-146">Para iterar a través de las características rellenadas disponibles para la aplicación, puede solicitar un administrador `ApplicationPartManager` mediante [inserción de dependencias](../../fundamentals/dependency-injection.md) y usarlo para rellenar las instancias de las características adecuadas:</span><span class="sxs-lookup"><span data-stu-id="79835-146">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="79835-147">Salida del ejemplo:</span><span class="sxs-lookup"><span data-stu-id="79835-147">Example output:</span></span>

![Ejemplo de salida de la aplicación de ejemplo](app-parts/_static/available-features.png)
