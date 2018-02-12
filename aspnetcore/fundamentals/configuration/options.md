---
title: "Patrón de opciones en ASP.NET Core"
author: guardrex
description: "Descubra cómo usar el patrón de opciones para representar grupos de valores de configuración relacionados en aplicaciones ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: abb3b92af07a7b3b199712fcfdc459ca283d0017
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="66c1b-103">Patrón de opciones en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="66c1b-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="66c1b-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="66c1b-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="66c1b-105">El patrón de opciones usa las clases de opciones para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="66c1b-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="66c1b-106">Cuando los valores de configuración están aislados por característica en clases de opciones independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="66c1b-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="66c1b-107">El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/): las características (clases) que dependen de valores de configuración dependerán únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="66c1b-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="66c1b-108">[Separación de intereses](http://deviq.com/separation-of-concerns/): los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="66c1b-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="66c1b-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample)). Este artículo es más fácil de seguir con la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="66c1b-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="66c1b-110">Configuración de opciones básicas</span><span class="sxs-lookup"><span data-stu-id="66c1b-110">Basic options configuration</span></span>

<span data-ttu-id="66c1b-111">La configuración de opciones básicas se muestra en el ejemplo &num;1 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="66c1b-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="66c1b-112">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="66c1b-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="66c1b-113">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="66c1b-114">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="66c1b-115">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="66c1b-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="66c1b-116">La clase `MyOptions` se agrega al contenedor de servicios con [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) y enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="66c1b-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="66c1b-117">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:fundamentals/dependency-injection#what-is-dependency-injection) con [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="66c1b-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="66c1b-118">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="66c1b-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="66c1b-119">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="66c1b-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="66c1b-120">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="66c1b-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="66c1b-121">La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="66c1b-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="66c1b-122">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="66c1b-122">Use a delegate to set options values.</span></span> <span data-ttu-id="66c1b-123">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="66c1b-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="66c1b-124">En el código siguiente, un segundo servicio `IConfigureOptions<TOptions>` se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="66c1b-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="66c1b-125">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="66c1b-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="66c1b-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="66c1b-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="66c1b-127">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="66c1b-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="66c1b-128">Los proveedores de configuración están disponibles en paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="66c1b-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="66c1b-129">Se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="66c1b-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="66c1b-130">Cada llamada a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) agrega un servicio `IConfigureOptions<TOptions>` al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="66c1b-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="66c1b-131">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="66c1b-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="66c1b-132">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="66c1b-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="66c1b-133">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="66c1b-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="66c1b-134">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="66c1b-134">Suboptions configuration</span></span>

<span data-ttu-id="66c1b-135">La configuración de subopciones se muestra en el ejemplo &num;3 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="66c1b-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="66c1b-136">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de características (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="66c1b-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="66c1b-137">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="66c1b-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="66c1b-138">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="66c1b-139">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="66c1b-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="66c1b-140">En el código siguiente, se agrega un tercer servicio `IConfigureOptions<TOptions>` al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="66c1b-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="66c1b-141">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="66c1b-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="66c1b-142">El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="66c1b-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="66c1b-143">Si la aplicación usa el metapaquete [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/), el paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="66c1b-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="66c1b-144">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="66c1b-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="66c1b-145">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de subopciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="66c1b-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="66c1b-146">El método `OnGet` del modelo de página devuelve una cadena con los valores de subopciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="66c1b-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="66c1b-147">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="66c1b-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="66c1b-148">Opciones proporcionadas por un modelo de vista o con inserción de vista directa</span><span class="sxs-lookup"><span data-stu-id="66c1b-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="66c1b-149">Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="66c1b-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="66c1b-150">Las opciones se pueden suministrar en un modelo de vista o insertando `IOptions<TOptions>` directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="66c1b-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="66c1b-151">Para la inserción directa, inserte `IOptions<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="66c1b-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="66c1b-152">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="66c1b-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="66c1b-154">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="66c1b-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="66c1b-155">El procedimiento de volver a cargar los datos de configuración con `IOptionsSnapshot` se muestra en el ejemplo &num;5 en la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="66c1b-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="66c1b-156">*Requiere ASP.NET Core 1.1 o versiones posteriores.*</span><span class="sxs-lookup"><span data-stu-id="66c1b-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="66c1b-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) admite volver a cargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="66c1b-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="66c1b-158">En ASP.NET Core 1.1, `IOptionsSnapshot` es una instantánea de [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) y se actualiza automáticamente cada vez que el monitor desencadena cambios de acuerdo con los cambios del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="66c1b-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="66c1b-159">En ASP.NET Core 2.0 y versiones posteriores, las opciones se calculan una vez por solicitud cuando se accede a ellas y se almacenan en caché en el tiempo que dura la solicitud.</span><span class="sxs-lookup"><span data-stu-id="66c1b-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="66c1b-160">En el ejemplo siguiente se muestra cómo se crea un nuevo `IOptionsSnapshot` después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="66c1b-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="66c1b-161">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="66c1b-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="66c1b-162">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="66c1b-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="66c1b-163">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="66c1b-164">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="66c1b-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="66c1b-165">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="66c1b-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="66c1b-166">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="66c1b-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="66c1b-167">La compatibilidad de opciones con nombre con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) se muestra en el ejemplo &num;6 de la [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="66c1b-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="66c1b-168">*Requiere ASP.NET Core 2.0 o versiones posteriores.*</span><span class="sxs-lookup"><span data-stu-id="66c1b-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="66c1b-169">La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="66c1b-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="66c1b-170">En la aplicación de ejemplo, las opciones con nombre se declaran con el método [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):</span><span class="sxs-lookup"><span data-stu-id="66c1b-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="66c1b-171">La aplicación de ejemplo accede a las opciones con nombre con [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="66c1b-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="66c1b-172">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="66c1b-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="66c1b-173">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="66c1b-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="66c1b-174">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="66c1b-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="66c1b-175">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="66c1b-176">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="66c1b-177">Configure todas las instancias de opciones con nombre con el método [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="66c1b-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="66c1b-178">El siguiente código configura `Option1` para todas las instancias de configuración con nombre con un valor común.</span><span class="sxs-lookup"><span data-stu-id="66c1b-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="66c1b-179">Agregue manualmente este código al método `Configure`:</span><span class="sxs-lookup"><span data-stu-id="66c1b-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="66c1b-180">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="66c1b-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="66c1b-181">En ASP.NET 2.0 Core y versiones posteriores, todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="66c1b-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="66c1b-182">Las instancias de `IConfigureOption` existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="66c1b-183">`IConfigureNamedOptions` también implementa `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="66c1b-184">La implementación predeterminada de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origen de referencia](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="66c1b-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="66c1b-185">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) y [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="66c1b-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="66c1b-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="66c1b-186">IPostConfigureOptions</span></span>

<span data-ttu-id="66c1b-187">*Requiere ASP.NET Core 2.0 o versiones posteriores.*</span><span class="sxs-lookup"><span data-stu-id="66c1b-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="66c1b-188">Establezca la postconfiguración con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="66c1b-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="66c1b-189">La postconfiguración se ejecuta después de que tenga lugar toda la configuración de [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):</span><span class="sxs-lookup"><span data-stu-id="66c1b-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="66c1b-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponible para postconfigurar las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="66c1b-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="66c1b-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) para postconfigurar todas las instancias de configuración con nombre:</span><span class="sxs-lookup"><span data-stu-id="66c1b-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="66c1b-192">Generador de opciones, supervisión y memoria caché</span><span class="sxs-lookup"><span data-stu-id="66c1b-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="66c1b-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) se usa para crear notificaciones cuando cambian las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="66c1b-194">`IOptionsMonitor` admite opciones recargables, notificaciones de cambios y `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="66c1b-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 o versiones posteriores) es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="66c1b-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="66c1b-196">Tiene un solo método [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create).</span><span class="sxs-lookup"><span data-stu-id="66c1b-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="66c1b-197">La implementación predeterminada toma todas las instancias registradas de `IConfigureOptions` y `IPostConfigureOptions`, y configura todas las configuraciones primero, seguidas de las postconfiguraciones.</span><span class="sxs-lookup"><span data-stu-id="66c1b-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="66c1b-198">Distingue entre `IConfigureNamedOptions` y `IConfigureOptions`, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="66c1b-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="66c1b-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 o versiones posteriores) lo usa `IOptionsMonitor` para almacenar en caché instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="66c1b-200">`IOptionsMonitorCache` invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="66c1b-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="66c1b-201">Los valores se pueden introducir manualmente y mediante [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="66c1b-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="66c1b-202">Se usa el método [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="66c1b-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="66c1b-203">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="66c1b-203">Accessing options during startup</span></span>

<span data-ttu-id="66c1b-204">`IOptions` puede usarse en `Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-204">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="66c1b-205">Si se compila un proveedor de servicios en `ConfigureServices` para acceder a opciones, no incluirá ninguna configuración de opción proporcionada después de que se compile el proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="66c1b-205">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="66c1b-206">Por tanto, puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="66c1b-206">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="66c1b-207">Puesto que las opciones suelen cargarse a partir de la configuración, puede usar la configuración al inicio en `Configure` y `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="66c1b-207">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="66c1b-208">Para obtener ejemplos de uso de la configuración durante el inicio, vea el tema [Inicio de la aplicación ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="66c1b-208">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="66c1b-209">Vea también</span><span class="sxs-lookup"><span data-stu-id="66c1b-209">See also</span></span>

* [<span data-ttu-id="66c1b-210">Configuración</span><span class="sxs-lookup"><span data-stu-id="66c1b-210">Configuration</span></span>](xref:fundamentals/configuration/index)
