---
title: Patrón de opciones en ASP.NET Core
author: guardrex
description: Descubra cómo usar el patrón de opciones para representar grupos de valores de configuración relacionados en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/19/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 22dde4b05ea20fedb696c6a4b144755a957e8c0d
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886369"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="b951e-103">Patrón de opciones en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b951e-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="b951e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b951e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b951e-105">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="b951e-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b951e-106">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="b951e-106">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b951e-107">El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="b951e-107">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b951e-108">[Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="b951e-108">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b951e-109">Las opciones también proporcionan un mecanismo para validar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-109">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b951e-110">Para obtener más información, consulte la sección [Opciones de validación](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="b951e-110">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b951e-111">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b951e-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b951e-112">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b951e-112">Prerequisites</span></span>

<span data-ttu-id="b951e-113">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="b951e-113">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b951e-114">Interfaces de opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-114">Options interfaces</span></span>

<span data-ttu-id="b951e-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b951e-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> admite las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b951e-117">Notificaciones de cambios</span><span class="sxs-lookup"><span data-stu-id="b951e-117">Change notifications</span></span>
* [<span data-ttu-id="b951e-118">Opciones con nombre</span><span class="sxs-lookup"><span data-stu-id="b951e-118">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b951e-119">Configuración que se puede recargar</span><span class="sxs-lookup"><span data-stu-id="b951e-119">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b951e-120">Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b951e-120">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b951e-121">Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-121">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b951e-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-122"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b951e-123">Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-123">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b951e-124">La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="b951e-124">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b951e-125">Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> y <xref:Microsoft.Extensions.Options.IConfigureOptions%601>, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="b951e-125">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b951e-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para almacenar en caché las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b951e-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b951e-127">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b951e-128">Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-128">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b951e-129">Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="b951e-129">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b951e-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="b951e-130"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b951e-131">Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="b951e-131">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b951e-132"><xref:Microsoft.Extensions.Options.IOptions%601> puede utilizarse para admitir las opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-132"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b951e-133">Sin embargo, <xref:Microsoft.Extensions.Options.IOptions%601> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-133">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b951e-134">Aún puede usar <xref:Microsoft.Extensions.Options.IOptions%601> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions%601> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-134">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b951e-135">Configuración de opciones generales</span><span class="sxs-lookup"><span data-stu-id="b951e-135">General options configuration</span></span>

<span data-ttu-id="b951e-136">La configuración de opciones generales se muestra en el ejemplo &num;1 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-136">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="b951e-137">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="b951e-137">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b951e-138">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b951e-138">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b951e-139">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b951e-139">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b951e-140">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-140">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b951e-141">La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="b951e-141">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b951e-142">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-142">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b951e-143">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="b951e-143">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b951e-144">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="b951e-144">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b951e-145">Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="b951e-145">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b951e-146">Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="b951e-146">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b951e-147">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="b951e-147">Configure simple options with a delegate</span></span>

<span data-ttu-id="b951e-148">La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-148">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="b951e-149">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-149">Use a delegate to set options values.</span></span> <span data-ttu-id="b951e-150">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-150">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b951e-151">En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-151">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b951e-152">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b951e-152">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b951e-153">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b951e-153">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b951e-154">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-154">You can add multiple configuration providers.</span></span> <span data-ttu-id="b951e-155">Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="b951e-155">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b951e-156">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b951e-156">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b951e-157">Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-157">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b951e-158">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="b951e-158">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b951e-159">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-159">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b951e-160">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="b951e-160">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b951e-161">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="b951e-161">Suboptions configuration</span></span>

<span data-ttu-id="b951e-162">La configuración de subopciones se muestra en el ejemplo &num;3 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-162">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="b951e-163">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b951e-163">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b951e-164">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="b951e-164">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b951e-165">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b951e-165">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b951e-166">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-166">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b951e-167">En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-167">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b951e-168">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b951e-168">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b951e-169">El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="b951e-169">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="b951e-170">Si la aplicación usa el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior), el paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b951e-170">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="b951e-171">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b951e-171">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b951e-172">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-172">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b951e-173">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-173">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b951e-174">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-174">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b951e-175">Opciones proporcionadas por un modelo de vista o con inserción de vista directa</span><span class="sxs-lookup"><span data-stu-id="b951e-175">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b951e-176">Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-176">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="b951e-177">Las opciones se pueden suministrar en un modelo de vista o insertando <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-177">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b951e-178">La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b951e-178">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b951e-179">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="b951e-179">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b951e-181">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b951e-181">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b951e-182">El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> se muestra en el ejemplo &num;5 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-182">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="b951e-183"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> admite volver a cargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="b951e-183"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="b951e-184">Cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b951e-184">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b951e-185">En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="b951e-185">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b951e-186">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-186">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b951e-187">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b951e-187">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b951e-188">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="b951e-188">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b951e-189">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-189">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b951e-190">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="b951e-190">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b951e-191">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b951e-191">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b951e-192">La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> se muestra en el ejemplo &num;6 de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-192">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="b951e-193">La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="b951e-193">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b951e-194">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="b951e-194">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b951e-195">La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-195">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b951e-196">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="b951e-196">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b951e-197">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-197">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b951e-198">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="b951e-198">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b951e-199">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b951e-199">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b951e-200">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-200">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b951e-201">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b951e-201">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b951e-202">Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-202">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b951e-203">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="b951e-203">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b951e-204">Agregue manualmente este código al método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b951e-204">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b951e-205">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="b951e-205">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b951e-206">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="b951e-206">All options are named instances.</span></span> <span data-ttu-id="b951e-207">Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b951e-207">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b951e-208"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-208"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b951e-209">La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="b951e-209">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b951e-210">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="b951e-210">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b951e-211">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b951e-211">OptionsBuilder API</span></span>

<span data-ttu-id="b951e-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-212"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b951e-213">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="b951e-213">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b951e-214">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b951e-214">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b951e-215">Uso de servicios de DI para configurar opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-215">Use DI services to configure options</span></span>

<span data-ttu-id="b951e-216">Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-216">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b951e-217">Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b951e-217">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b951e-218">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-218">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b951e-219">Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, y se registrar como un servicio.</span><span class="sxs-lookup"><span data-stu-id="b951e-219">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b951e-220">Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada.</span><span class="sxs-lookup"><span data-stu-id="b951e-220">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b951e-221">La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b951e-221">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b951e-222">La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="b951e-222">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="b951e-223">Opciones de validación</span><span class="sxs-lookup"><span data-stu-id="b951e-223">Options validation</span></span>

<span data-ttu-id="b951e-224">Las opciones de validación permiten validar las opciones cuando se configuran.</span><span class="sxs-lookup"><span data-stu-id="b951e-224">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="b951e-225">Llame a `Validate` con un método de validación que devuelve `true` si las opciones son válidas y `false` si no lo son:</span><span class="sxs-lookup"><span data-stu-id="b951e-225">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="b951e-226">El ejemplo anterior establece la instancia de opciones con nombre en `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="b951e-226">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="b951e-227">La instancia predeterminada es `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="b951e-227">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="b951e-228">La validación se ejecuta cuando se crea la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-228">Validation runs when the options instance is created.</span></span> <span data-ttu-id="b951e-229">La instancia de opciones pasa seguro la validación la primera vez que se accede.</span><span class="sxs-lookup"><span data-stu-id="b951e-229">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b951e-230">La validación de opciones no protege contra las modificaciones de opciones después de configurarse y validarse inicialmente.</span><span class="sxs-lookup"><span data-stu-id="b951e-230">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="b951e-231">El método `Validate` acepta una expresión `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="b951e-231">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="b951e-232">Para personalizar completamente la validación, implemente `IValidateOptions<TOptions>`, que permite:</span><span class="sxs-lookup"><span data-stu-id="b951e-232">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="b951e-233">Validación de varios tipos de opciones: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="b951e-233">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="b951e-234">Validación que depende de otro tipo de opción: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="b951e-234">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="b951e-235">`IValidateOptions` valida:</span><span class="sxs-lookup"><span data-stu-id="b951e-235">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="b951e-236">Una instancia de opciones con nombre específica.</span><span class="sxs-lookup"><span data-stu-id="b951e-236">A specific named options instance.</span></span>
* <span data-ttu-id="b951e-237">Todas las opciones cuando `name` es `null`.</span><span class="sxs-lookup"><span data-stu-id="b951e-237">All options when `name` is `null`.</span></span>

<span data-ttu-id="b951e-238">Devuelve `ValidateOptionsResult` de la implementación de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="b951e-238">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="b951e-239">La validación basada en la anotación de datos está disponible en el paquete [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) llamando al método <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> en `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="b951e-239">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="b951e-240">`Microsoft.Extensions.Options.DataAnnotations` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 o versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="b951e-240">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="b951e-241">La validación diligente (con respuesta rápida a errores en el inicio) se está teniendo en cuenta de cara a una versión futura.</span><span class="sxs-lookup"><span data-stu-id="b951e-241">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="b951e-242">Configuración posterior de las opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-242">Options post-configuration</span></span>

<span data-ttu-id="b951e-243">Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-243">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b951e-244">La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b951e-244">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b951e-245"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="b951e-245"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b951e-246">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="b951e-246">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b951e-247">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="b951e-247">Accessing options during startup</span></span>

<span data-ttu-id="b951e-248"><xref:Microsoft.Extensions.Options.IOptions%601> y <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b951e-248"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b951e-249">No use <xref:Microsoft.Extensions.Options.IOptions%601> o <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b951e-249">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b951e-250">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="b951e-250">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="b951e-251">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="b951e-251">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b951e-252">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="b951e-252">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b951e-253">El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="b951e-253">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b951e-254">[Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="b951e-254">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b951e-255">Las opciones también proporcionan un mecanismo para validar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-255">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b951e-256">Para obtener más información, consulte la sección [Opciones de validación](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="b951e-256">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b951e-257">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b951e-257">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b951e-258">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b951e-258">Prerequisites</span></span>

<span data-ttu-id="b951e-259">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="b951e-259">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b951e-260">Interfaces de opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-260">Options interfaces</span></span>

<span data-ttu-id="b951e-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-261"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b951e-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> admite las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-262"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b951e-263">Notificaciones de cambios</span><span class="sxs-lookup"><span data-stu-id="b951e-263">Change notifications</span></span>
* [<span data-ttu-id="b951e-264">Opciones con nombre</span><span class="sxs-lookup"><span data-stu-id="b951e-264">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b951e-265">Configuración que se puede recargar</span><span class="sxs-lookup"><span data-stu-id="b951e-265">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b951e-266">Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b951e-266">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b951e-267">Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-267">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b951e-268"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-268"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b951e-269">Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-269">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b951e-270">La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="b951e-270">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b951e-271">Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> y <xref:Microsoft.Extensions.Options.IConfigureOptions%601>, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="b951e-271">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b951e-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para almacenar en caché las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-272"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b951e-273"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b951e-273">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b951e-274">Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-274">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b951e-275">Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="b951e-275">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b951e-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="b951e-276"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b951e-277">Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="b951e-277">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b951e-278"><xref:Microsoft.Extensions.Options.IOptions%601> puede utilizarse para admitir las opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-278"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b951e-279">Sin embargo, <xref:Microsoft.Extensions.Options.IOptions%601> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-279">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b951e-280">Aún puede usar <xref:Microsoft.Extensions.Options.IOptions%601> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions%601> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-280">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b951e-281">Configuración de opciones generales</span><span class="sxs-lookup"><span data-stu-id="b951e-281">General options configuration</span></span>

<span data-ttu-id="b951e-282">La configuración de opciones generales se muestra en el ejemplo &num;1 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-282">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="b951e-283">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="b951e-283">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b951e-284">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b951e-284">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b951e-285">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b951e-285">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b951e-286">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-286">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b951e-287">La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="b951e-287">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b951e-288">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-288">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b951e-289">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="b951e-289">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b951e-290">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="b951e-290">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b951e-291">Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="b951e-291">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b951e-292">Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="b951e-292">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b951e-293">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="b951e-293">Configure simple options with a delegate</span></span>

<span data-ttu-id="b951e-294">La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-294">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="b951e-295">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-295">Use a delegate to set options values.</span></span> <span data-ttu-id="b951e-296">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-296">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b951e-297">En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-297">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b951e-298">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b951e-298">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b951e-299">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b951e-299">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b951e-300">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-300">You can add multiple configuration providers.</span></span> <span data-ttu-id="b951e-301">Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="b951e-301">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b951e-302">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b951e-302">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b951e-303">Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-303">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b951e-304">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="b951e-304">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b951e-305">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-305">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b951e-306">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="b951e-306">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b951e-307">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="b951e-307">Suboptions configuration</span></span>

<span data-ttu-id="b951e-308">La configuración de subopciones se muestra en el ejemplo &num;3 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-308">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="b951e-309">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b951e-309">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b951e-310">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="b951e-310">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b951e-311">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b951e-311">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b951e-312">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-312">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b951e-313">En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-313">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b951e-314">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b951e-314">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b951e-315">El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="b951e-315">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="b951e-316">Si la aplicación usa el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior), el paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b951e-316">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="b951e-317">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b951e-317">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b951e-318">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-318">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b951e-319">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-319">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b951e-320">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-320">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b951e-321">Opciones proporcionadas por un modelo de vista o con inserción de vista directa</span><span class="sxs-lookup"><span data-stu-id="b951e-321">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b951e-322">Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-322">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="b951e-323">Las opciones se pueden suministrar en un modelo de vista o insertando <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-323">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b951e-324">La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b951e-324">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b951e-325">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="b951e-325">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b951e-327">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b951e-327">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b951e-328">El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> se muestra en el ejemplo &num;5 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-328">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="b951e-329"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> admite volver a cargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="b951e-329"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="b951e-330">Cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b951e-330">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b951e-331">En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="b951e-331">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b951e-332">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-332">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b951e-333">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b951e-333">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b951e-334">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="b951e-334">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b951e-335">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-335">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b951e-336">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="b951e-336">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b951e-337">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b951e-337">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b951e-338">La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> se muestra en el ejemplo &num;6 de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-338">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="b951e-339">La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="b951e-339">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b951e-340">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="b951e-340">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b951e-341">La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-341">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b951e-342">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="b951e-342">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b951e-343">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-343">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b951e-344">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="b951e-344">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b951e-345">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b951e-345">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b951e-346">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-346">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b951e-347">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b951e-347">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b951e-348">Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-348">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b951e-349">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="b951e-349">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b951e-350">Agregue manualmente este código al método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b951e-350">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b951e-351">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="b951e-351">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b951e-352">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="b951e-352">All options are named instances.</span></span> <span data-ttu-id="b951e-353">Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b951e-353">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b951e-354"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-354"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b951e-355">La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="b951e-355">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b951e-356">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="b951e-356">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b951e-357">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b951e-357">OptionsBuilder API</span></span>

<span data-ttu-id="b951e-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-358"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b951e-359">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="b951e-359">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b951e-360">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b951e-360">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b951e-361">Uso de servicios de DI para configurar opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-361">Use DI services to configure options</span></span>

<span data-ttu-id="b951e-362">Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-362">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b951e-363">Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b951e-363">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b951e-364">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-364">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b951e-365">Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, y se registrar como un servicio.</span><span class="sxs-lookup"><span data-stu-id="b951e-365">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b951e-366">Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada.</span><span class="sxs-lookup"><span data-stu-id="b951e-366">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b951e-367">La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b951e-367">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b951e-368">La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="b951e-368">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="b951e-369">Opciones de validación</span><span class="sxs-lookup"><span data-stu-id="b951e-369">Options validation</span></span>

<span data-ttu-id="b951e-370">Las opciones de validación permiten validar las opciones cuando se configuran.</span><span class="sxs-lookup"><span data-stu-id="b951e-370">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="b951e-371">Llame a `Validate` con un método de validación que devuelve `true` si las opciones son válidas y `false` si no lo son:</span><span class="sxs-lookup"><span data-stu-id="b951e-371">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="b951e-372">El ejemplo anterior establece la instancia de opciones con nombre en `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="b951e-372">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="b951e-373">La instancia predeterminada es `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="b951e-373">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="b951e-374">La validación se ejecuta cuando se crea la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-374">Validation runs when the options instance is created.</span></span> <span data-ttu-id="b951e-375">La instancia de opciones pasa seguro la validación la primera vez que se accede.</span><span class="sxs-lookup"><span data-stu-id="b951e-375">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b951e-376">La validación de opciones no protege contra las modificaciones de opciones después de configurarse y validarse inicialmente.</span><span class="sxs-lookup"><span data-stu-id="b951e-376">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="b951e-377">El método `Validate` acepta una expresión `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="b951e-377">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="b951e-378">Para personalizar completamente la validación, implemente `IValidateOptions<TOptions>`, que permite:</span><span class="sxs-lookup"><span data-stu-id="b951e-378">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="b951e-379">Validación de varios tipos de opciones: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="b951e-379">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="b951e-380">Validación que depende de otro tipo de opción: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="b951e-380">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="b951e-381">`IValidateOptions` valida:</span><span class="sxs-lookup"><span data-stu-id="b951e-381">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="b951e-382">Una instancia de opciones con nombre específica.</span><span class="sxs-lookup"><span data-stu-id="b951e-382">A specific named options instance.</span></span>
* <span data-ttu-id="b951e-383">Todas las opciones cuando `name` es `null`.</span><span class="sxs-lookup"><span data-stu-id="b951e-383">All options when `name` is `null`.</span></span>

<span data-ttu-id="b951e-384">Devuelve `ValidateOptionsResult` de la implementación de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="b951e-384">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="b951e-385">La validación basada en la anotación de datos está disponible en el paquete [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) llamando al método <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> en `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="b951e-385">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="b951e-386">`Microsoft.Extensions.Options.DataAnnotations` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 o versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="b951e-386">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

```csharp
using Microsoft.Extensions.DependencyInjection;

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="b951e-387">La validación diligente (con respuesta rápida a errores en el inicio) se está teniendo en cuenta de cara a una versión futura.</span><span class="sxs-lookup"><span data-stu-id="b951e-387">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="b951e-388">Configuración posterior de las opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-388">Options post-configuration</span></span>

<span data-ttu-id="b951e-389">Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-389">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b951e-390">La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b951e-390">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b951e-391"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="b951e-391"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b951e-392">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="b951e-392">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b951e-393">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="b951e-393">Accessing options during startup</span></span>

<span data-ttu-id="b951e-394"><xref:Microsoft.Extensions.Options.IOptions%601> y <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b951e-394"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b951e-395">No use <xref:Microsoft.Extensions.Options.IOptions%601> o <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b951e-395">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b951e-396">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="b951e-396">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="b951e-397">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="b951e-397">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b951e-398">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="b951e-398">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="b951e-399">El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="b951e-399">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="b951e-400">[Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="b951e-400">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="b951e-401">Las opciones también proporcionan un mecanismo para validar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-401">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="b951e-402">Para obtener más información, consulte la sección [Opciones de validación](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="b951e-402">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="b951e-403">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b951e-403">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b951e-404">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b951e-404">Prerequisites</span></span>

<span data-ttu-id="b951e-405">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="b951e-405">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="b951e-406">Interfaces de opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-406">Options interfaces</span></span>

<span data-ttu-id="b951e-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-407"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="b951e-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> admite las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-408"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="b951e-409">Notificaciones de cambios</span><span class="sxs-lookup"><span data-stu-id="b951e-409">Change notifications</span></span>
* [<span data-ttu-id="b951e-410">Opciones con nombre</span><span class="sxs-lookup"><span data-stu-id="b951e-410">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="b951e-411">Configuración que se puede recargar</span><span class="sxs-lookup"><span data-stu-id="b951e-411">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="b951e-412">Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="b951e-412">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="b951e-413">Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-413">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="b951e-414"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-414"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="b951e-415">Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-415">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="b951e-416">La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="b951e-416">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="b951e-417">Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> y <xref:Microsoft.Extensions.Options.IConfigureOptions%601>, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="b951e-417">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="b951e-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para almacenar en caché las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-418"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="b951e-419"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="b951e-419">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="b951e-420">Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-420">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="b951e-421">Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="b951e-421">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="b951e-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="b951e-422"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="b951e-423">Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="b951e-423">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="b951e-424"><xref:Microsoft.Extensions.Options.IOptions%601> puede utilizarse para admitir las opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-424"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="b951e-425">Sin embargo, <xref:Microsoft.Extensions.Options.IOptions%601> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-425">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="b951e-426">Aún puede usar <xref:Microsoft.Extensions.Options.IOptions%601> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions%601> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-426">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="b951e-427">Configuración de opciones generales</span><span class="sxs-lookup"><span data-stu-id="b951e-427">General options configuration</span></span>

<span data-ttu-id="b951e-428">La configuración de opciones generales se muestra en el ejemplo &num;1 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-428">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="b951e-429">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="b951e-429">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="b951e-430">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="b951e-430">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="b951e-431">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b951e-431">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="b951e-432">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-432">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="b951e-433">La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="b951e-433">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="b951e-434">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-434">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="b951e-435">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="b951e-435">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="b951e-436">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="b951e-436">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="b951e-437">Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="b951e-437">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="b951e-438">Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="b951e-438">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="b951e-439">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="b951e-439">Configure simple options with a delegate</span></span>

<span data-ttu-id="b951e-440">La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-440">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="b951e-441">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="b951e-441">Use a delegate to set options values.</span></span> <span data-ttu-id="b951e-442">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-442">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="b951e-443">En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-443">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b951e-444">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="b951e-444">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="b951e-445">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="b951e-445">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="b951e-446">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-446">You can add multiple configuration providers.</span></span> <span data-ttu-id="b951e-447">Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="b951e-447">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="b951e-448">Para más información, consulte <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b951e-448">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="b951e-449">Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-449">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="b951e-450">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="b951e-450">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="b951e-451">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-451">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="b951e-452">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="b951e-452">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="b951e-453">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="b951e-453">Suboptions configuration</span></span>

<span data-ttu-id="b951e-454">La configuración de subopciones se muestra en el ejemplo &num;3 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-454">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="b951e-455">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b951e-455">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="b951e-456">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="b951e-456">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="b951e-457">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="b951e-457">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="b951e-458">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-458">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="b951e-459">En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="b951e-459">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="b951e-460">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b951e-460">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="b951e-461">El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="b951e-461">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="b951e-462">Si la aplicación usa el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior), el paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b951e-462">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="b951e-463">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="b951e-463">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="b951e-464">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-464">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="b951e-465">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-465">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="b951e-466">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-466">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="b951e-467">Opciones proporcionadas por un modelo de vista o con inserción de vista directa</span><span class="sxs-lookup"><span data-stu-id="b951e-467">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="b951e-468">Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-468">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="b951e-469">Las opciones se pueden suministrar en un modelo de vista o insertando <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-469">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="b951e-470">La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="b951e-470">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="b951e-471">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="b951e-471">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="b951e-473">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="b951e-473">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="b951e-474">El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> se muestra en el ejemplo &num;5 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-474">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="b951e-475"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> admite volver a cargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="b951e-475"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="b951e-476">Cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="b951e-476">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="b951e-477">En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="b951e-477">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="b951e-478">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="b951e-478">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="b951e-479">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="b951e-479">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="b951e-480">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="b951e-480">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="b951e-481">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-481">Save the *appsettings.json* file.</span></span> <span data-ttu-id="b951e-482">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="b951e-482">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="b951e-483">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="b951e-483">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="b951e-484">La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> se muestra en el ejemplo &num;6 de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b951e-484">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="b951e-485">La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="b951e-485">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="b951e-486">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="b951e-486">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="b951e-487">La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="b951e-487">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="b951e-488">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="b951e-488">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="b951e-489">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b951e-489">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="b951e-490">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="b951e-490">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="b951e-491">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="b951e-491">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="b951e-492">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-492">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="b951e-493">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="b951e-493">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="b951e-494">Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="b951e-494">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="b951e-495">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="b951e-495">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="b951e-496">Agregue manualmente este código al método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="b951e-496">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="b951e-497">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="b951e-497">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="b951e-498">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="b951e-498">All options are named instances.</span></span> <span data-ttu-id="b951e-499">Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="b951e-499">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="b951e-500"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-500"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="b951e-501">La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="b951e-501">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="b951e-502">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="b951e-502">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="b951e-503">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="b951e-503">OptionsBuilder API</span></span>

<span data-ttu-id="b951e-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="b951e-504"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="b951e-505">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="b951e-505">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="b951e-506">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b951e-506">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="b951e-507">Uso de servicios de DI para configurar opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-507">Use DI services to configure options</span></span>

<span data-ttu-id="b951e-508">Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-508">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="b951e-509">Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="b951e-509">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="b951e-510">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="b951e-510">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="b951e-511">Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, y se registrar como un servicio.</span><span class="sxs-lookup"><span data-stu-id="b951e-511">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="b951e-512">Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada.</span><span class="sxs-lookup"><span data-stu-id="b951e-512">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="b951e-513">La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="b951e-513">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="b951e-514">La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="b951e-514">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="b951e-515">Configuración posterior de las opciones</span><span class="sxs-lookup"><span data-stu-id="b951e-515">Options post-configuration</span></span>

<span data-ttu-id="b951e-516">Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="b951e-516">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="b951e-517">La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="b951e-517">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b951e-518"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="b951e-518"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="b951e-519">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="b951e-519">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="b951e-520">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="b951e-520">Accessing options during startup</span></span>

<span data-ttu-id="b951e-521"><xref:Microsoft.Extensions.Options.IOptions%601> y <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="b951e-521"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="b951e-522">No use <xref:Microsoft.Extensions.Options.IOptions%601> o <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b951e-522">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b951e-523">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="b951e-523">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="b951e-524">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b951e-524">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
