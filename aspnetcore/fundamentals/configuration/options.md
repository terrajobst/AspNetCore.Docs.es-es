---
title: "Patrón de opciones en ASP.NET Core"
author: guardrex
description: "Descubra cómo usar el patrón de opciones para representar grupos de configuraciones relacionadas en las aplicaciones de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: 7d89416626433bf737b63eda4b17e65b089ae142
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2017
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="1f515-103">Patrón de opciones en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1f515-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="1f515-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="1f515-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="1f515-105">El patrón de opciones usa las clases de opciones para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="1f515-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="1f515-106">Cuando valores de configuración están aislados por característica en clases de opciones independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="1f515-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="1f515-107">El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/): características (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que utilizan.</span><span class="sxs-lookup"><span data-stu-id="1f515-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="1f515-108">[Separación de intereses](http://deviq.com/separation-of-concerns/): configuración para las distintas partes de la aplicación no es dependiente o acoplamiento entre sí.</span><span class="sxs-lookup"><span data-stu-id="1f515-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="1f515-109">[Ver o descargar el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample)) en este artículo es más fácil de seguir con la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="1f515-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="1f515-110">Configuración de opciones básicas</span><span class="sxs-lookup"><span data-stu-id="1f515-110">Basic options configuration</span></span>

<span data-ttu-id="1f515-111">Configuración de las opciones básicas se muestra como ejemplo &num;1 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="1f515-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="1f515-112">Una clase de opciones debe ser no abstracto con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="1f515-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="1f515-113">La siguiente clase, `MyOptions`, tiene dos propiedades, `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="1f515-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="1f515-114">Valores predeterminados de configuración es opcional, pero el constructor de clase en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1f515-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="1f515-115">`Option2`tiene un valor predeterminado establecido por Inicializando la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1f515-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="1f515-116">El `MyOptions` clase se agrega al contenedor de servicios con [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) y enlazado a la configuración:</span><span class="sxs-lookup"><span data-stu-id="1f515-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="1f515-117">La siguiente página utiliza modelo [inyección de dependencia de constructor](xref:fundamentals/dependency-injection#what-is-dependency-injection) con [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) para tener acceso a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1f515-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="1f515-118">El ejemplo *appSettings.JSON que se* archivo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="1f515-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="1f515-119">Cuando se ejecuta la aplicación, el modelo de páginas `OnGet` método devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="1f515-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="1f515-120">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="1f515-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="1f515-121">Configuración de las opciones simples con un delegado se muestra como ejemplo &num;2 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="1f515-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="1f515-122">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="1f515-122">Use a delegate to set options values.</span></span> <span data-ttu-id="1f515-123">La aplicación de ejemplo se usa el `MyOptionsWithDelegateConfig` clase (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="1f515-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="1f515-124">En el código siguiente, un segundo `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="1f515-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="1f515-125">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="1f515-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="1f515-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="1f515-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="1f515-127">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="1f515-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="1f515-128">Proveedores de configuración están disponibles en paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1f515-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="1f515-129">Se aplican para que se registran.</span><span class="sxs-lookup"><span data-stu-id="1f515-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="1f515-130">Cada llamada a [configurar&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) agrega un `IConfigureOptions<TOptions>` servicio al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="1f515-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="1f515-131">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.JSON que se*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="1f515-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="1f515-132">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificado *wins* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="1f515-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="1f515-133">Cuando se ejecuta la aplicación, el modelo de páginas `OnGet` método devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="1f515-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="1f515-134">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="1f515-134">Suboptions configuration</span></span>

<span data-ttu-id="1f515-135">Configuración de subopciones se muestra como ejemplo &num;3 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="1f515-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="1f515-136">Las aplicaciones deben crear clases de opciones que pertenecen a grupos específicos de característica (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1f515-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="1f515-137">Partes de la aplicación que requieren valores de configuración solamente deben tener acceso a los valores de configuración que utilizan.</span><span class="sxs-lookup"><span data-stu-id="1f515-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="1f515-138">Al enlazar opciones de configuración, cada propiedad en el tipo de opciones está enlazado a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="1f515-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="1f515-139">Por ejemplo, el `MyOptions.Option1` propiedad se enlaza a la clave `Option1`, que se lee desde el `option1` propiedad en *appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="1f515-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="1f515-140">En el código siguiente, una tercera `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="1f515-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="1f515-141">Enlaza `MySubOptions` a la sección `subsection` de la *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="1f515-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="1f515-142">El `GetSection` método de extensión requiere la [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="1f515-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="1f515-143">Si la aplicación usa el [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, el paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="1f515-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="1f515-144">El ejemplo *appSettings.JSON que se* archivo define un `subsection` miembro con las claves de `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="1f515-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="1f515-145">El `MySubOptions` clase define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opción secundario (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="1f515-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="1f515-146">El modelo de páginas `OnGet` método devuelve una cadena con los valores de opción secundario (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1f515-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="1f515-147">Cuando se ejecuta la aplicación, el `OnGet` método devuelve una cadena que muestra la opción subcarpetas valores de clase:</span><span class="sxs-lookup"><span data-stu-id="1f515-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="1f515-148">Opciones proporcionadas por un modelo de vista o de inyección de vista directa</span><span class="sxs-lookup"><span data-stu-id="1f515-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="1f515-149">Opciones proporcionadas por un modelo de vista o de inserción directa de vista se muestra como ejemplo &num;4 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="1f515-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="1f515-150">Opciones pueden especificarse en un modelo de vista o insertando `IOptions<TOptions>` directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1f515-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="1f515-151">Para que la inserción directa, insertar `IOptions<MyOptions>` con un `@inject` directiva:</span><span class="sxs-lookup"><span data-stu-id="1f515-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="1f515-152">Cuando se ejecuta la aplicación, se muestran los valores de opción en la página presentada:</span><span class="sxs-lookup"><span data-stu-id="1f515-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Opciones de valores de opción 1: value1_from_json y opción2: -1 se cargan desde el modelo y por inyección de código en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="1f515-154">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="1f515-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="1f515-155">Volver a cargar los datos de configuración con `IOptionsSnapshot` se muestra en el ejemplo &num;5 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="1f515-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="1f515-156">*Requiere ASP.NET Core 1.1 o posterior.*</span><span class="sxs-lookup"><span data-stu-id="1f515-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="1f515-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) admite recargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="1f515-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="1f515-158">En ASP.NET Core 1.1, `IOptionsSnapshot` es una instantánea de [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) y las actualizaciones automáticamente cada vez que el monitor desencadena cambia de acuerdo con los cambios del origen de datos.</span><span class="sxs-lookup"><span data-stu-id="1f515-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="1f515-159">En el núcleo de ASP.NET 2.0 y versiones posteriores, opciones se calculan una vez por solicitud cuando se tiene acceso y almacenar en caché para la duración de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1f515-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="1f515-160">En el ejemplo siguiente se muestra cómo un nuevo `IOptionsSnapshot` se crea después *appSettings.JSON que se* cambios (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="1f515-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="1f515-161">Varias solicitudes al servidor devuelven valores constantes proporcionados por el *appSettings.JSON que se* hasta que se modifique el archivo y configuración recarga de archivos.</span><span class="sxs-lookup"><span data-stu-id="1f515-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="1f515-162">La siguiente imagen muestra la inicial `option1` y `option2` valores cargan desde el *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="1f515-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="1f515-163">Cambiar los valores de la *appSettings.JSON que se* del archivo a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="1f515-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="1f515-164">Guardar el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="1f515-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="1f515-165">Actualice el explorador para ver que se actualizan los valores de opciones:</span><span class="sxs-lookup"><span data-stu-id="1f515-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="1f515-166">Con el nombre de opciones de la compatibilidad con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="1f515-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="1f515-167">Con el nombre de opciones de la compatibilidad con [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) se muestra como ejemplo &num;6 en el [aplicación de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="1f515-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="1f515-168">*Requiere el núcleo ASP.NET 2.0 o posterior.*</span><span class="sxs-lookup"><span data-stu-id="1f515-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="1f515-169">*Denominado opciones* compatibilidad permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="1f515-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="1f515-170">En la aplicación de ejemplo, las opciones con nombre se declaran con la [ConfigureNamedOptions&lt;TOptions&gt;. Configurar](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) método:</span><span class="sxs-lookup"><span data-stu-id="1f515-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="1f515-171">La aplicación de ejemplo tiene acceso a las opciones con nombre con [IOptionsSnapshot&lt;TOptions&gt;. Obtener](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="1f515-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="1f515-172">Ejecuta la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="1f515-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="1f515-173">`named_options_1`se proporcionan valores de configuración, que se cargan desde el *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="1f515-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="1f515-174">`named_options_2`valores proceden de:</span><span class="sxs-lookup"><span data-stu-id="1f515-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="1f515-175">El `named_options_2` delegar en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="1f515-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="1f515-176">El valor predeterminado de `Option2` proporcionada por el `MyOptions` clase.</span><span class="sxs-lookup"><span data-stu-id="1f515-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="1f515-177">Configure todas las instancias con nombre opciones con el [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) método.</span><span class="sxs-lookup"><span data-stu-id="1f515-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="1f515-178">El siguiente código configura `Option1` para todas las configuración las instancias con nombre con un valor común.</span><span class="sxs-lookup"><span data-stu-id="1f515-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="1f515-179">Agregue el código siguiente manualmente a la `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="1f515-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="1f515-180">Ejecuta la aplicación de ejemplo después de agregar el código genera el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="1f515-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="1f515-181">En el núcleo de ASP.NET 2.0 y versiones posteriores, todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="1f515-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="1f515-182">Existente `IConfigureOption` instancias se tratan como destino el `Options.DefaultName` instancia, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="1f515-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="1f515-183">`IConfigureNamedOptions`También implementa `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="1f515-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="1f515-184">La implementación predeterminada de la [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([origen de referencia](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) tiene una lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="1f515-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="1f515-185">El `null` opción con nombre se utiliza para todas las instancias con nombre en lugar de un determinado con el nombre de instancia de destino ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) y [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="1f515-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="1f515-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="1f515-186">IPostConfigureOptions</span></span>

<span data-ttu-id="1f515-187">*Requiere el núcleo ASP.NET 2.0 o posterior.*</span><span class="sxs-lookup"><span data-stu-id="1f515-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="1f515-188">Establecer postconfiguration con [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="1f515-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="1f515-189">Postconfiguración se ejecuta después de que todos [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) se produce la configuración:</span><span class="sxs-lookup"><span data-stu-id="1f515-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1f515-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) está disponible para posteriores al configurar las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="1f515-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="1f515-191">Use [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) posteriores al configurar todos configuración las instancias con nombre:</span><span class="sxs-lookup"><span data-stu-id="1f515-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="1f515-192">Generador de opciones, la supervisión y la memoria caché</span><span class="sxs-lookup"><span data-stu-id="1f515-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="1f515-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) se usa para notificaciones cuando `TOptions` instancias de cambio.</span><span class="sxs-lookup"><span data-stu-id="1f515-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="1f515-194">`IOptionsMonitor`admite opciones recargable, notificaciones de cambios, y `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="1f515-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="1f515-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (núcleo ASP.NET 2.0 o posterior) es responsable de crear nuevas opciones de instancias.</span><span class="sxs-lookup"><span data-stu-id="1f515-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="1f515-196">Tiene una sola [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) método.</span><span class="sxs-lookup"><span data-stu-id="1f515-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="1f515-197">La implementación predeterminada tiene todos los `IConfigureOptions` y `IPostConfigureOptions` y ejecuta todos los configura primero, a continuación, la configura posteriores a la.</span><span class="sxs-lookup"><span data-stu-id="1f515-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="1f515-198">Distingue entre `IConfigureNamedOptions` y `IConfigureOptions` y solo se llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="1f515-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="1f515-199">[IOptionsMonitorCache&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (núcleo ASP.NET 2.0 o posterior) se usa por `IOptionsMonitor` en caché `TOptions` instancias.</span><span class="sxs-lookup"><span data-stu-id="1f515-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="1f515-200">El `IOptionsMonitorCache` invalida instancias de opciones en el monitor de modo que se vuelve a calcular el valor ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="1f515-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="1f515-201">Los valores pueden ser manualmente presentar también con [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="1f515-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="1f515-202">El [desactive](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) método se utiliza cuando todas las instancias con nombre se deben volver a crearse a petición.</span><span class="sxs-lookup"><span data-stu-id="1f515-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="see-also"></a><span data-ttu-id="1f515-203">Vea también</span><span class="sxs-lookup"><span data-stu-id="1f515-203">See also</span></span>

* [<span data-ttu-id="1f515-204">Configuración</span><span class="sxs-lookup"><span data-stu-id="1f515-204">Configuration</span></span>](xref:fundamentals/configuration/index)
