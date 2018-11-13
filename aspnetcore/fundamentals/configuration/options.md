---
title: Patrón de opciones en ASP.NET Core
author: guardrex
description: Descubra cómo usar el patrón de opciones para representar grupos de valores de configuración relacionados en aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/09/2018
uid: fundamentals/configuration/options
ms.openlocfilehash: 99aa5028a8704c7e9e3010415137e2560213a2ad
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505802"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="5f1a0-103">Patrón de opciones en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5f1a0-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="5f1a0-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5f1a0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5f1a0-105">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="5f1a0-106">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="5f1a0-107">El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/): los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="5f1a0-108">[Separación de intereses](http://deviq.com/separation-of-concerns/): los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="5f1a0-109">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([cómo descargarlo](xref:index#how-to-download-a-sample)). Este artículo es más fácil de seguir con la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f1a0-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="5f1a0-110">Prerequisites</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="5f1a0-111">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-111">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="5f1a0-112">Haga referencia al [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-112">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5f1a0-113">Agregue una referencia al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-113">Add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

::: moniker-end

## <a name="basic-options-configuration"></a><span data-ttu-id="5f1a0-114">Configuración de opciones básicas</span><span class="sxs-lookup"><span data-stu-id="5f1a0-114">Basic options configuration</span></span>

<span data-ttu-id="5f1a0-115">La configuración de opciones básicas se muestra en el ejemplo &num;1 en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-115">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5f1a0-116">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-116">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="5f1a0-117">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-117">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="5f1a0-118">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-118">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="5f1a0-119">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-119">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="5f1a0-120">La clase `MyOptions` se agrega al contenedor de servicios con [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) y enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-120">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="5f1a0-121">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-121">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="5f1a0-122">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-122">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

<span data-ttu-id="5f1a0-123">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-123">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="5f1a0-124">Al usar una instancia de [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-124">When using a custom [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder) to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> <span data-ttu-id="5f1a0-125">Al cargar las opciones de configuración desde el archivo de configuración a través de [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-125">Explicitly setting the base path isn't required when loading options configuration from the settings file via [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="5f1a0-126">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="5f1a0-126">Configure simple options with a delegate</span></span>

<span data-ttu-id="5f1a0-127">La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-127">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5f1a0-128">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-128">Use a delegate to set options values.</span></span> <span data-ttu-id="5f1a0-129">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-129">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="5f1a0-130">En el código siguiente, un segundo servicio `IConfigureOptions<TOptions>` se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-130">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5f1a0-131">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-131">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="5f1a0-132">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-132">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="5f1a0-133">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-133">You can add multiple configuration providers.</span></span> <span data-ttu-id="5f1a0-134">Los proveedores de configuración están disponibles en paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-134">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="5f1a0-135">Se aplican en el orden en que se hayan registrado.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-135">They're applied in the order that they're registered.</span></span>

<span data-ttu-id="5f1a0-136">Cada llamada a [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) agrega un servicio `IConfigureOptions<TOptions>` al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-136">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="5f1a0-137">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-137">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="5f1a0-138">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-138">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="5f1a0-139">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-139">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="5f1a0-140">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="5f1a0-140">Suboptions configuration</span></span>

<span data-ttu-id="5f1a0-141">La configuración de subopciones se muestra en el ejemplo &num;3 en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-141">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5f1a0-142">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-142">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="5f1a0-143">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-143">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="5f1a0-144">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-144">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="5f1a0-145">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-145">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="5f1a0-146">En el código siguiente, se agrega un tercer servicio `IConfigureOptions<TOptions>` al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-146">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="5f1a0-147">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-147">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="5f1a0-148">El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-148">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="5f1a0-149">Si la aplicación usa el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior), el paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-149">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="5f1a0-150">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-150">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="5f1a0-151">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-151">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="5f1a0-152">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-152">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="5f1a0-153">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-153">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="5f1a0-154">Opciones proporcionadas por un modelo de vista o con inserción de vista directa</span><span class="sxs-lookup"><span data-stu-id="5f1a0-154">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="5f1a0-155">Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-155">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5f1a0-156">Las opciones se pueden suministrar en un modelo de vista o insertando `IOptions<TOptions>` directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-156">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="5f1a0-157">Para la inserción directa, inserte `IOptions<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-157">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="5f1a0-158">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-158">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="5f1a0-160">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="5f1a0-160">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="5f1a0-161">El procedimiento de volver a cargar los datos de configuración con `IOptionsSnapshot` se muestra en el ejemplo &num;5 en la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-161">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5f1a0-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) admite volver a cargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-162">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5f1a0-163">Cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-163">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="5f1a0-164">`IOptionsSnapshot` es una instantánea de [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) y se actualiza automáticamente cada vez que el monitor desencadena cambios basados en los cambios del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-164">`IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="5f1a0-165">En el ejemplo siguiente se muestra cómo se crea un nuevo `IOptionsSnapshot` después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-165">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="5f1a0-166">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-166">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="5f1a0-167">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-167">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="5f1a0-168">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-168">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="5f1a0-169">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-169">Save the *appsettings.json* file.</span></span> <span data-ttu-id="5f1a0-170">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-170">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="5f1a0-171">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="5f1a0-171">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="5f1a0-172">La compatibilidad de opciones con nombre con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) se muestra en el ejemplo &num;6 de la [aplicación de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-172">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="5f1a0-173">La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-173">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="5f1a0-174">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, cadena, acción&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) que, a su vez, llama al método de extensión [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-174">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="5f1a0-175">La aplicación de ejemplo accede a las opciones con nombre con [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-175">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="5f1a0-176">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-176">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="5f1a0-177">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-177">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="5f1a0-178">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-178">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="5f1a0-179">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-179">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="5f1a0-180">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-180">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="5f1a0-181">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="5f1a0-181">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="5f1a0-182">Configure todas las instancias de opciones con el método [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-182">Configure all options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="5f1a0-183">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-183">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="5f1a0-184">Agregue manualmente este código al método `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-184">Add the following code manually to the `ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="5f1a0-185">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-185">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="5f1a0-186">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-186">All options are named instances.</span></span> <span data-ttu-id="5f1a0-187">Las instancias de `IConfigureOption` existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-187">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="5f1a0-188">`IConfigureNamedOptions` también implementa `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-188">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="5f1a0-189">La implementación predeterminada de [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origen de referencia](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-189">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="5f1a0-190">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) y [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-190">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="optionsbuilder-api"></a><span data-ttu-id="5f1a0-191">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="5f1a0-191">OptionsBuilder API</span></span>

<span data-ttu-id="5f1a0-192"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-192"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="5f1a0-193">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-193">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="5f1a0-194">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-194">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");
    
services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="configurelttoptions-tdep1--tdep4gt-method"></a><span data-ttu-id="5f1a0-195">Método Configure&lt;TOptions, TDep1, ... TDep4&gt;</span><span class="sxs-lookup"><span data-stu-id="5f1a0-195">Configure&lt;TOptions, TDep1, ... TDep4&gt; method</span></span>

<span data-ttu-id="5f1a0-196">El uso de servicios de DI para configurar opciones implementando `IConfigure[Named]Options` en forma de texto reutilizable es detallado.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-196">Using services from DI to configure options by implementing `IConfigure[Named]Options` in a boilerplate manner is verbose.</span></span> <span data-ttu-id="5f1a0-197">Las sobrecargas de `ConfigureOptions` en `OptionsBuilder<TOptions>` le permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-197">Overloads for `ConfigureOptions` on `OptionsBuilder<TOptions>` allow you to use up to five services to configure options:</span></span>

```csharp
services.AddOptions<MyOptions>("optionalName")
    .Configure<Service1, Service2, Service3, Service4, Service5>(
        (o, s, s2, s3, s4, s5) => 
            o.Property = DoSomethingWith(s, s2, s3, s4, s5));
```

<span data-ttu-id="5f1a0-198">La sobrecarga registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> genérica y transitoria, que tiene un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-198">The overload registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, which has a constructor that accepts the generic service types specified.</span></span> 

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="5f1a0-199">Opciones de validación</span><span class="sxs-lookup"><span data-stu-id="5f1a0-199">Options validation</span></span>

<span data-ttu-id="5f1a0-200">Las opciones de validación permiten validar las opciones cuando se configuran.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-200">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="5f1a0-201">Llame a `Validate` con un método de validación que devuelve `true` si las opciones son válidas y `false` si no lo son:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-201">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

<span data-ttu-id="5f1a0-202">El ejemplo anterior establece la instancia de opciones con nombre en `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-202">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="5f1a0-203">La instancia predeterminada es `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-203">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="5f1a0-204">La validación se ejecuta cuando se crea la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-204">Validation runs when the options instance is created.</span></span> <span data-ttu-id="5f1a0-205">La instancia de opciones pasa seguro la validación la primera vez que se accede.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-205">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5f1a0-206">La validación de opciones no protege contra las modificaciones de opciones después de configurarse y validarse inicialmente.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-206">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="5f1a0-207">El método `Validate` acepta una expresión `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-207">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="5f1a0-208">Para personalizar completamente la validación, implemente `IValidateOptions<TOptions>`, que permite:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-208">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="5f1a0-209">Validación de varios tipos de opciones: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="5f1a0-209">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="5f1a0-210">Validación que depende de otro tipo de opción: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="5f1a0-210">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`</span></span>

<span data-ttu-id="5f1a0-211">`IValidateOptions` valida:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-211">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="5f1a0-212">Una instancia de opciones con nombre específica.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-212">A specific named options instance.</span></span>
* <span data-ttu-id="5f1a0-213">Todas las opciones cuando `name` es `null`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-213">All options when `name` is `null`.</span></span>

<span data-ttu-id="5f1a0-214">Devuelve `ValidateOptionsResult` de la implementación de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-214">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="5f1a0-215">La validación basada en la anotación de datos está disponible en el paquete [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) llamando al método `ValidateDataAnnotations` en `OptionsBuilder<TOptions>`:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-215">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the `ValidateDataAnnotations` method on `OptionsBuilder<TOptions>`:</span></span>

```csharp
private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}
    
[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptions<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}    
```

<span data-ttu-id="5f1a0-216">La validación diligente (con respuesta rápida a errores en el inicio) se está teniendo en cuenta de cara a una versión futura.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-216">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a><span data-ttu-id="5f1a0-217">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="5f1a0-217">IPostConfigureOptions</span></span>

<span data-ttu-id="5f1a0-218">Establezca la postconfiguración con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-218">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="5f1a0-219">La postconfiguración se ejecuta después de que tenga lugar toda la configuración de [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1):</span><span class="sxs-lookup"><span data-stu-id="5f1a0-219">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5f1a0-220">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponible para postconfigurar las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-220">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="5f1a0-221">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="5f1a0-221">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="5f1a0-222">Generador de opciones, supervisión y memoria caché</span><span class="sxs-lookup"><span data-stu-id="5f1a0-222">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="5f1a0-223">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) se usa para crear notificaciones cuando cambian las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-223">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="5f1a0-224">`IOptionsMonitor` admite opciones recargables, notificaciones de cambios y `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-224">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="5f1a0-225">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-225">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) is responsible for creating new options instances.</span></span> <span data-ttu-id="5f1a0-226">Tiene un solo método [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-226">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="5f1a0-227">La implementación predeterminada toma todas las instancias registradas de `IConfigureOptions` y `IPostConfigureOptions`, y configura todas las configuraciones primero, seguidas de las postconfiguraciones.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-227">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="5f1a0-228">Distingue entre `IConfigureNamedOptions` y `IConfigureOptions`, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-228">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="5f1a0-229">`IOptionsMonitor` usa [IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) para almacenar instancias de `TOptions` en caché.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-229">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="5f1a0-230">`IOptionsMonitorCache` invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-230">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="5f1a0-231">Los valores se pueden introducir manualmente y mediante [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="5f1a0-231">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="5f1a0-232">Se usa el método [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-232">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

::: moniker-end

## <a name="accessing-options-during-startup"></a><span data-ttu-id="5f1a0-233">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="5f1a0-233">Accessing options during startup</span></span>

<span data-ttu-id="5f1a0-234">`IOptions` puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-234">`IOptions` can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

<span data-ttu-id="5f1a0-235">`IOptions` no se debe usar `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-235">`IOptions` shouldn't be used in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="5f1a0-236">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="5f1a0-236">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5f1a0-237">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="5f1a0-237">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
