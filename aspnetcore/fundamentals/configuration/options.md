---
title: Patrón de opciones en ASP.NET Core
author: rick-anderson
description: Descubra cómo usar el patrón de opciones para representar grupos de valores de configuración relacionados en aplicaciones ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/12/2020
uid: fundamentals/configuration/options
ms.openlocfilehash: 756d3d57122642ab10ab671c9accb75975c3799d
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78649991"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="a8065-103">Patrón de opciones en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8065-103">Options pattern in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8065-104">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a8065-104">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a8065-105">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="a8065-105">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="a8065-106">El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="a8065-106">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="a8065-107">[Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="a8065-107">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="a8065-108">Las opciones también proporcionan un mecanismo para validar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-108">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="a8065-109">Para obtener más información, consulte la sección [Opciones de validación](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="a8065-109">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="a8065-110">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8065-110">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="package"></a><span data-ttu-id="a8065-111">Package</span><span class="sxs-lookup"><span data-stu-id="a8065-111">Package</span></span>

<span data-ttu-id="a8065-112">Se hace referencia implícita al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) en las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8065-112">The [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package is implicitly referenced in ASP.NET Core apps.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="a8065-113">Interfaces de opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-113">Options interfaces</span></span>

<span data-ttu-id="a8065-114"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-114"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="a8065-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> admite las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-115"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="a8065-116">Notificaciones de cambios</span><span class="sxs-lookup"><span data-stu-id="a8065-116">Change notifications</span></span>
* [<span data-ttu-id="a8065-117">Opciones con nombre</span><span class="sxs-lookup"><span data-stu-id="a8065-117">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="a8065-118">Configuración que se puede recargar</span><span class="sxs-lookup"><span data-stu-id="a8065-118">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="a8065-119">Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="a8065-119">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="a8065-120">Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-120">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="a8065-121"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-121"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="a8065-122">Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-122">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="a8065-123">La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="a8065-123">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="a8065-124">Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> y <xref:Microsoft.Extensions.Options.IConfigureOptions%601>, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="a8065-124">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="a8065-125"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para almacenar en caché las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-125"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="a8065-126"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="a8065-126">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="a8065-127">Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-127">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="a8065-128">Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="a8065-128">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="a8065-129"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8065-129"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="a8065-130">Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="a8065-130">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="a8065-131"><xref:Microsoft.Extensions.Options.IOptions%601> puede utilizarse para admitir las opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-131"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="a8065-132">Sin embargo, <xref:Microsoft.Extensions.Options.IOptions%601> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-132">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="a8065-133">Aún puede usar <xref:Microsoft.Extensions.Options.IOptions%601> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions%601> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-133">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="a8065-134">Configuración de opciones generales</span><span class="sxs-lookup"><span data-stu-id="a8065-134">General options configuration</span></span>

<span data-ttu-id="a8065-135">La configuración de opciones generales se muestra en el ejemplo 1 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-135">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="a8065-136">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="a8065-136">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="a8065-137">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="a8065-137">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="a8065-138">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a8065-138">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="a8065-139">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-139">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="a8065-140">La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="a8065-140">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="a8065-141">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-141">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="a8065-142">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="a8065-142">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="a8065-143">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="a8065-143">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="a8065-144">Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="a8065-144">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="a8065-145">Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="a8065-145">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="a8065-146">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="a8065-146">Configure simple options with a delegate</span></span>

<span data-ttu-id="a8065-147">La configuración de opciones simples con un delegado se muestra como ejemplo 2 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-147">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="a8065-148">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-148">Use a delegate to set options values.</span></span> <span data-ttu-id="a8065-149">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-149">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="a8065-150">En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-150">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a8065-151">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="a8065-151">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="a8065-152">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8065-152">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="a8065-153">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-153">You can add multiple configuration providers.</span></span> <span data-ttu-id="a8065-154">Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="a8065-154">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="a8065-155">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a8065-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="a8065-156">Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-156">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="a8065-157">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="a8065-157">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="a8065-158">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-158">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="a8065-159">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="a8065-159">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="a8065-160">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="a8065-160">Suboptions configuration</span></span>

<span data-ttu-id="a8065-161">La configuración de subopciones se muestra en el ejemplo 3 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-161">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="a8065-162">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8065-162">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="a8065-163">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="a8065-163">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="a8065-164">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="a8065-164">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="a8065-165">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-165">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="a8065-166">En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-166">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a8065-167">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8065-167">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="a8065-168">El método `GetSection` requiere el espacio de nombres <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="a8065-168">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="a8065-169">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="a8065-169">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/3.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="a8065-170">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-170">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="a8065-171">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-171">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="a8065-172">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-172">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="a8065-173">Inserción de opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-173">Options injection</span></span>

<span data-ttu-id="a8065-174">La inserción de opciones se muestra en el ejemplo 4 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-174">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="a8065-175">Inserte <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en:</span><span class="sxs-lookup"><span data-stu-id="a8065-175">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="a8065-176">Una instancia de Razor Pages o una vista de MVC con la directiva [`@inject`](xref:mvc/views/razor#inject) de Razor.</span><span class="sxs-lookup"><span data-stu-id="a8065-176">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="a8065-177">Un modelo de página o vista.</span><span class="sxs-lookup"><span data-stu-id="a8065-177">A page or view model.</span></span>

<span data-ttu-id="a8065-178">En el ejemplo siguiente de la aplicación de ejemplo, inserte <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en un modelo de página (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-178">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="a8065-179">La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="a8065-179">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/3.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="a8065-180">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="a8065-180">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="a8065-182">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="a8065-182">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="a8065-183">El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> se muestra en el ejemplo 5 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-183">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="a8065-184">Al usar <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8065-184">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="a8065-185">La diferencia entre `IOptionsMonitor` y `IOptionsSnapshot` es que:</span><span class="sxs-lookup"><span data-stu-id="a8065-185">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="a8065-186">`IOptionsMonitor` es un [servicio singleton](xref:fundamentals/dependency-injection#singleton) que recupera los valores de las opciones actuales en cualquier momento, lo que resulta especialmente útil en las dependencias singleton.</span><span class="sxs-lookup"><span data-stu-id="a8065-186">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="a8065-187">`IOptionsSnapshot` es un [servicio con ámbito](xref:fundamentals/dependency-injection#scoped) y proporciona una instantánea de las opciones en el momento en que se construye el objeto `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="a8065-187">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="a8065-188">Las instantáneas de opciones están diseñadas para usarlas con dependencias transitorias y con ámbito.</span><span class="sxs-lookup"><span data-stu-id="a8065-188">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="a8065-189">En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="a8065-189">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="a8065-190">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-190">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="a8065-191">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8065-191">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="a8065-192">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="a8065-192">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="a8065-193">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-193">Save the *appsettings.json* file.</span></span> <span data-ttu-id="a8065-194">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="a8065-194">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="a8065-195">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="a8065-195">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="a8065-196">La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> se muestra en el ejemplo 6 de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-196">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="a8065-197">La compatibilidad con las opciones con nombre permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="a8065-197">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="a8065-198">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a8065-198">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="a8065-199">Las opciones con nombre distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a8065-199">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="a8065-200">La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-200">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/3.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="a8065-201">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="a8065-201">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="a8065-202">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-202">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="a8065-203">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="a8065-203">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="a8065-204">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a8065-204">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="a8065-205">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-205">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="a8065-206">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="a8065-206">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="a8065-207">Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-207">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="a8065-208">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="a8065-208">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="a8065-209">Agregue manualmente este código al método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a8065-209">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="a8065-210">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="a8065-210">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="a8065-211">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="a8065-211">All options are named instances.</span></span> <span data-ttu-id="a8065-212">Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="a8065-212">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="a8065-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-213"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="a8065-214">La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="a8065-214">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="a8065-215">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="a8065-215">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="a8065-216">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="a8065-216">OptionsBuilder API</span></span>

<span data-ttu-id="a8065-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-217"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="a8065-218">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="a8065-218">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="a8065-219">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8065-219">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="a8065-220">Uso de servicios de DI para configurar opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-220">Use DI services to configure options</span></span>

<span data-ttu-id="a8065-221">Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-221">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="a8065-222">Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="a8065-222">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="a8065-223">`OptionsBuilder<TOptions>` proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-223">`OptionsBuilder<TOptions>` provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="a8065-224">Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, y se registrar como un servicio.</span><span class="sxs-lookup"><span data-stu-id="a8065-224">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="a8065-225">Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada.</span><span class="sxs-lookup"><span data-stu-id="a8065-225">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="a8065-226">La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a8065-226">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="a8065-227">La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="a8065-227">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="a8065-228">Opciones de validación</span><span class="sxs-lookup"><span data-stu-id="a8065-228">Options validation</span></span>

<span data-ttu-id="a8065-229">Las opciones de validación permiten validar las opciones cuando se configuran.</span><span class="sxs-lookup"><span data-stu-id="a8065-229">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="a8065-230">Llame a `Validate` con un método de validación que devuelve `true` si las opciones son válidas y `false` si no lo son:</span><span class="sxs-lookup"><span data-stu-id="a8065-230">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="a8065-231">El ejemplo anterior establece la instancia de opciones con nombre en `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="a8065-231">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="a8065-232">La instancia predeterminada es `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="a8065-232">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="a8065-233">La validación se ejecuta cuando se crea la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-233">Validation runs when the options instance is created.</span></span> <span data-ttu-id="a8065-234">Una instancia de opciones pasa seguro la validación la primera vez que se accede.</span><span class="sxs-lookup"><span data-stu-id="a8065-234">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8065-235">La validación de opciones no protege contra las modificaciones de opciones después de crearse la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-235">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="a8065-236">Por ejemplo, las opciones `IOptionsSnapshot` se crean y validan una vez por solicitud al obtenerse acceso a estas por primera vez.</span><span class="sxs-lookup"><span data-stu-id="a8065-236">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="a8065-237">Las opciones `IOptionsSnapshot` no se validarán de nuevo en intentos de acceso posteriores *para la misma solicitud*.</span><span class="sxs-lookup"><span data-stu-id="a8065-237">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="a8065-238">El método `Validate` acepta una expresión `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="a8065-238">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="a8065-239">Para personalizar completamente la validación, implemente `IValidateOptions<TOptions>`, que permite:</span><span class="sxs-lookup"><span data-stu-id="a8065-239">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="a8065-240">Validación de varios tipos de opciones: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="a8065-240">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="a8065-241">Validación que depende de otro tipo de opción: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="a8065-241">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="a8065-242">`IValidateOptions` valida:</span><span class="sxs-lookup"><span data-stu-id="a8065-242">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="a8065-243">Una instancia de opciones con nombre específica.</span><span class="sxs-lookup"><span data-stu-id="a8065-243">A specific named options instance.</span></span>
* <span data-ttu-id="a8065-244">Todas las opciones cuando `name` es `null`.</span><span class="sxs-lookup"><span data-stu-id="a8065-244">All options when `name` is `null`.</span></span>

<span data-ttu-id="a8065-245">Devuelve `ValidateOptionsResult` de la implementación de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="a8065-245">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="a8065-246">La validación basada en la anotación de datos está disponible en el paquete [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) llamando al método <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> en `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="a8065-246">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="a8065-247">Se hace referencia implícita a `Microsoft.Extensions.Options.DataAnnotations` en las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8065-247">`Microsoft.Extensions.Options.DataAnnotations` is implicitly referenced in ASP.NET Core apps.</span></span>

```csharp
using System.ComponentModel.DataAnnotations;
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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="a8065-248">La validación diligente (con respuesta rápida a errores en el inicio) se está teniendo en cuenta de cara a una versión futura.</span><span class="sxs-lookup"><span data-stu-id="a8065-248">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="a8065-249">Configuración posterior de las opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-249">Options post-configuration</span></span>

<span data-ttu-id="a8065-250">Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-250">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="a8065-251">La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="a8065-251">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a8065-252"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="a8065-252"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a8065-253">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8065-253">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="a8065-254">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="a8065-254">Accessing options during startup</span></span>

<span data-ttu-id="a8065-255"><xref:Microsoft.Extensions.Options.IOptions%601> y <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a8065-255"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, 
    IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="a8065-256">No use <xref:Microsoft.Extensions.Options.IOptions%601> o <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8065-256">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8065-257">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="a8065-257">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="a8065-258">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a8065-258">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a8065-259">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="a8065-259">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="a8065-260">El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="a8065-260">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="a8065-261">[Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="a8065-261">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="a8065-262">Las opciones también proporcionan un mecanismo para validar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-262">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="a8065-263">Para obtener más información, consulte la sección [Opciones de validación](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="a8065-263">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="a8065-264">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8065-264">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8065-265">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a8065-265">Prerequisites</span></span>

<span data-ttu-id="a8065-266">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="a8065-266">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="a8065-267">Interfaces de opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-267">Options interfaces</span></span>

<span data-ttu-id="a8065-268"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-268"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="a8065-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> admite las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-269"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="a8065-270">Notificaciones de cambios</span><span class="sxs-lookup"><span data-stu-id="a8065-270">Change notifications</span></span>
* [<span data-ttu-id="a8065-271">Opciones con nombre</span><span class="sxs-lookup"><span data-stu-id="a8065-271">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="a8065-272">Configuración que se puede recargar</span><span class="sxs-lookup"><span data-stu-id="a8065-272">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="a8065-273">Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="a8065-273">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="a8065-274">Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-274">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="a8065-275"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-275"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="a8065-276">Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-276">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="a8065-277">La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="a8065-277">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="a8065-278">Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> y <xref:Microsoft.Extensions.Options.IConfigureOptions%601>, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="a8065-278">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="a8065-279"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para almacenar en caché las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-279"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="a8065-280"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="a8065-280">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="a8065-281">Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-281">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="a8065-282">Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="a8065-282">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="a8065-283"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8065-283"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="a8065-284">Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="a8065-284">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="a8065-285"><xref:Microsoft.Extensions.Options.IOptions%601> puede utilizarse para admitir las opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-285"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="a8065-286">Sin embargo, <xref:Microsoft.Extensions.Options.IOptions%601> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-286">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="a8065-287">Aún puede usar <xref:Microsoft.Extensions.Options.IOptions%601> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions%601> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-287">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="a8065-288">Configuración de opciones generales</span><span class="sxs-lookup"><span data-stu-id="a8065-288">General options configuration</span></span>

<span data-ttu-id="a8065-289">La configuración de opciones generales se muestra en el ejemplo 1 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-289">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="a8065-290">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="a8065-290">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="a8065-291">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="a8065-291">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="a8065-292">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a8065-292">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="a8065-293">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-293">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="a8065-294">La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="a8065-294">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="a8065-295">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-295">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="a8065-296">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="a8065-296">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="a8065-297">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="a8065-297">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="a8065-298">Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="a8065-298">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="a8065-299">Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="a8065-299">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="a8065-300">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="a8065-300">Configure simple options with a delegate</span></span>

<span data-ttu-id="a8065-301">La configuración de opciones simples con un delegado se muestra como ejemplo 2 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-301">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="a8065-302">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-302">Use a delegate to set options values.</span></span> <span data-ttu-id="a8065-303">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-303">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="a8065-304">En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-304">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a8065-305">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="a8065-305">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="a8065-306">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8065-306">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="a8065-307">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-307">You can add multiple configuration providers.</span></span> <span data-ttu-id="a8065-308">Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="a8065-308">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="a8065-309">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a8065-309">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="a8065-310">Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-310">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="a8065-311">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="a8065-311">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="a8065-312">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-312">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="a8065-313">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="a8065-313">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="a8065-314">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="a8065-314">Suboptions configuration</span></span>

<span data-ttu-id="a8065-315">La configuración de subopciones se muestra en el ejemplo 3 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-315">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="a8065-316">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8065-316">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="a8065-317">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="a8065-317">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="a8065-318">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="a8065-318">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="a8065-319">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-319">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="a8065-320">En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-320">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a8065-321">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8065-321">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="a8065-322">El método `GetSection` requiere el espacio de nombres <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="a8065-322">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="a8065-323">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="a8065-323">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="a8065-324">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-324">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="a8065-325">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-325">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="a8065-326">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-326">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-injection"></a><span data-ttu-id="a8065-327">Inserción de opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-327">Options injection</span></span>

<span data-ttu-id="a8065-328">La inserción de opciones se muestra en el ejemplo 4 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-328">Options injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="a8065-329">Inserte <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en:</span><span class="sxs-lookup"><span data-stu-id="a8065-329">Inject <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into:</span></span>

* <span data-ttu-id="a8065-330">Una instancia de Razor Pages o una vista de MVC con la directiva [`@inject`](xref:mvc/views/razor#inject) de Razor.</span><span class="sxs-lookup"><span data-stu-id="a8065-330">A Razor page or MVC view with the [`@inject`](xref:mvc/views/razor#inject) Razor directive.</span></span>
* <span data-ttu-id="a8065-331">Un modelo de página o vista.</span><span class="sxs-lookup"><span data-stu-id="a8065-331">A page or view model.</span></span>

<span data-ttu-id="a8065-332">En el ejemplo siguiente de la aplicación de ejemplo, inserte <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en un modelo de página (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-332">The following example from the sample app injects <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> into a page model (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="a8065-333">La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="a8065-333">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="a8065-334">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="a8065-334">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="a8065-336">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="a8065-336">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="a8065-337">El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> se muestra en el ejemplo 5 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-337">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="a8065-338">Al usar <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8065-338">Using <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="a8065-339">La diferencia entre `IOptionsMonitor` y `IOptionsSnapshot` es que:</span><span class="sxs-lookup"><span data-stu-id="a8065-339">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

* <span data-ttu-id="a8065-340">`IOptionsMonitor` es un [servicio singleton](xref:fundamentals/dependency-injection#singleton) que recupera los valores de las opciones actuales en cualquier momento, lo que resulta especialmente útil en las dependencias singleton.</span><span class="sxs-lookup"><span data-stu-id="a8065-340">`IOptionsMonitor` is a [singleton service](xref:fundamentals/dependency-injection#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
* <span data-ttu-id="a8065-341">`IOptionsSnapshot` es un [servicio con ámbito](xref:fundamentals/dependency-injection#scoped) y proporciona una instantánea de las opciones en el momento en que se construye el objeto `IOptionsSnapshot<T>`.</span><span class="sxs-lookup"><span data-stu-id="a8065-341">`IOptionsSnapshot` is a [scoped service](xref:fundamentals/dependency-injection#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="a8065-342">Las instantáneas de opciones están diseñadas para usarlas con dependencias transitorias y con ámbito.</span><span class="sxs-lookup"><span data-stu-id="a8065-342">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="a8065-343">En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="a8065-343">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="a8065-344">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-344">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="a8065-345">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8065-345">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="a8065-346">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="a8065-346">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="a8065-347">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-347">Save the *appsettings.json* file.</span></span> <span data-ttu-id="a8065-348">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="a8065-348">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="a8065-349">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="a8065-349">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="a8065-350">La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> se muestra en el ejemplo 6 de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-350">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="a8065-351">La compatibilidad con las opciones con nombre permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="a8065-351">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="a8065-352">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a8065-352">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="a8065-353">Las opciones con nombre distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a8065-353">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="a8065-354">La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-354">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="a8065-355">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="a8065-355">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="a8065-356">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-356">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="a8065-357">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="a8065-357">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="a8065-358">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a8065-358">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="a8065-359">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-359">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="a8065-360">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="a8065-360">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="a8065-361">Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-361">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="a8065-362">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="a8065-362">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="a8065-363">Agregue manualmente este código al método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a8065-363">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="a8065-364">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="a8065-364">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="a8065-365">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="a8065-365">All options are named instances.</span></span> <span data-ttu-id="a8065-366">Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="a8065-366">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="a8065-367"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-367"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="a8065-368">La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="a8065-368">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="a8065-369">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="a8065-369">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="a8065-370">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="a8065-370">OptionsBuilder API</span></span>

<span data-ttu-id="a8065-371"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-371"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="a8065-372">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="a8065-372">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="a8065-373">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8065-373">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="a8065-374">Uso de servicios de DI para configurar opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-374">Use DI services to configure options</span></span>

<span data-ttu-id="a8065-375">Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-375">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="a8065-376">Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="a8065-376">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="a8065-377">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-377">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="a8065-378">Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, y se registrar como un servicio.</span><span class="sxs-lookup"><span data-stu-id="a8065-378">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="a8065-379">Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada.</span><span class="sxs-lookup"><span data-stu-id="a8065-379">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="a8065-380">La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a8065-380">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="a8065-381">La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="a8065-381">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-validation"></a><span data-ttu-id="a8065-382">Opciones de validación</span><span class="sxs-lookup"><span data-stu-id="a8065-382">Options validation</span></span>

<span data-ttu-id="a8065-383">Las opciones de validación permiten validar las opciones cuando se configuran.</span><span class="sxs-lookup"><span data-stu-id="a8065-383">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="a8065-384">Llame a `Validate` con un método de validación que devuelve `true` si las opciones son válidas y `false` si no lo son:</span><span class="sxs-lookup"><span data-stu-id="a8065-384">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="a8065-385">El ejemplo anterior establece la instancia de opciones con nombre en `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="a8065-385">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="a8065-386">La instancia predeterminada es `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="a8065-386">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="a8065-387">La validación se ejecuta cuando se crea la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-387">Validation runs when the options instance is created.</span></span> <span data-ttu-id="a8065-388">Una instancia de opciones pasa seguro la validación la primera vez que se accede.</span><span class="sxs-lookup"><span data-stu-id="a8065-388">An options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a8065-389">La validación de opciones no protege contra las modificaciones de opciones después de crearse la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-389">Options validation doesn't guard against options modifications after the options instance is created.</span></span> <span data-ttu-id="a8065-390">Por ejemplo, las opciones `IOptionsSnapshot` se crean y validan una vez por solicitud al obtenerse acceso a estas por primera vez.</span><span class="sxs-lookup"><span data-stu-id="a8065-390">For example, `IOptionsSnapshot` options are created and validated once per request when the options are first accessed.</span></span> <span data-ttu-id="a8065-391">Las opciones `IOptionsSnapshot` no se validarán de nuevo en intentos de acceso posteriores *para la misma solicitud*.</span><span class="sxs-lookup"><span data-stu-id="a8065-391">The `IOptionsSnapshot` options aren't validated again on subsequent access attempts *for the same request*.</span></span>

<span data-ttu-id="a8065-392">El método `Validate` acepta una expresión `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="a8065-392">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="a8065-393">Para personalizar completamente la validación, implemente `IValidateOptions<TOptions>`, que permite:</span><span class="sxs-lookup"><span data-stu-id="a8065-393">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="a8065-394">Validación de varios tipos de opciones: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="a8065-394">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="a8065-395">Validación que depende de otro tipo de opción: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="a8065-395">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="a8065-396">`IValidateOptions` valida:</span><span class="sxs-lookup"><span data-stu-id="a8065-396">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="a8065-397">Una instancia de opciones con nombre específica.</span><span class="sxs-lookup"><span data-stu-id="a8065-397">A specific named options instance.</span></span>
* <span data-ttu-id="a8065-398">Todas las opciones cuando `name` es `null`.</span><span class="sxs-lookup"><span data-stu-id="a8065-398">All options when `name` is `null`.</span></span>

<span data-ttu-id="a8065-399">Devuelve `ValidateOptionsResult` de la implementación de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="a8065-399">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="a8065-400">La validación basada en la anotación de datos está disponible en el paquete [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) llamando al método <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> en `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="a8065-400">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations*> method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="a8065-401">`Microsoft.Extensions.Options.DataAnnotations` se incluye en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a8065-401">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

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
        sp.GetRequiredService<IOptionsMonitor<AnnotatedOptions>>().CurrentValue);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}
```

<span data-ttu-id="a8065-402">La validación diligente (con respuesta rápida a errores en el inicio) se está teniendo en cuenta de cara a una versión futura.</span><span class="sxs-lookup"><span data-stu-id="a8065-402">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

## <a name="options-post-configuration"></a><span data-ttu-id="a8065-403">Configuración posterior de las opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-403">Options post-configuration</span></span>

<span data-ttu-id="a8065-404">Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-404">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="a8065-405">La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="a8065-405">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a8065-406"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="a8065-406"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a8065-407">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8065-407">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="a8065-408">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="a8065-408">Accessing options during startup</span></span>

<span data-ttu-id="a8065-409"><xref:Microsoft.Extensions.Options.IOptions%601> y <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a8065-409"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="a8065-410">No use <xref:Microsoft.Extensions.Options.IOptions%601> o <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8065-410">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8065-411">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="a8065-411">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="a8065-412">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a8065-412">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a8065-413">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="a8065-413">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="a8065-414">El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="a8065-414">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="a8065-415">[Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="a8065-415">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="a8065-416">Las opciones también proporcionan un mecanismo para validar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-416">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="a8065-417">Para obtener más información, consulte la sección [Opciones de validación](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="a8065-417">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="a8065-418">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8065-418">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a8065-419">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="a8065-419">Prerequisites</span></span>

<span data-ttu-id="a8065-420">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="a8065-420">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="a8065-421">Interfaces de opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-421">Options interfaces</span></span>

<span data-ttu-id="a8065-422"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-422"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="a8065-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> admite las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-423"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601> supports the following scenarios:</span></span>

* <span data-ttu-id="a8065-424">Notificaciones de cambios</span><span class="sxs-lookup"><span data-stu-id="a8065-424">Change notifications</span></span>
* [<span data-ttu-id="a8065-425">Opciones con nombre</span><span class="sxs-lookup"><span data-stu-id="a8065-425">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="a8065-426">Configuración que se puede recargar</span><span class="sxs-lookup"><span data-stu-id="a8065-426">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="a8065-427">Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="a8065-427">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="a8065-428">Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-428">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs.</span></span>

<span data-ttu-id="a8065-429"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-429"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="a8065-430">Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-430">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="a8065-431">La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="a8065-431">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="a8065-432">Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> y <xref:Microsoft.Extensions.Options.IConfigureOptions%601>, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="a8065-432">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="a8065-433"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para almacenar en caché las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-433"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="a8065-434"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="a8065-434">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="a8065-435">Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-435">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="a8065-436">Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="a8065-436">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="a8065-437"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8065-437"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="a8065-438">Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="a8065-438">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="a8065-439"><xref:Microsoft.Extensions.Options.IOptions%601> puede utilizarse para admitir las opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-439"><xref:Microsoft.Extensions.Options.IOptions%601> can be used to support options.</span></span> <span data-ttu-id="a8065-440">Sin embargo, <xref:Microsoft.Extensions.Options.IOptions%601> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-440">However, <xref:Microsoft.Extensions.Options.IOptions%601> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span> <span data-ttu-id="a8065-441">Aún puede usar <xref:Microsoft.Extensions.Options.IOptions%601> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions%601> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-441">You may continue to use <xref:Microsoft.Extensions.Options.IOptions%601> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions%601> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="a8065-442">Configuración de opciones generales</span><span class="sxs-lookup"><span data-stu-id="a8065-442">General options configuration</span></span>

<span data-ttu-id="a8065-443">La configuración de opciones generales se muestra en el ejemplo 1 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-443">General options configuration is demonstrated as Example 1 in the sample app.</span></span>

<span data-ttu-id="a8065-444">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="a8065-444">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="a8065-445">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="a8065-445">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="a8065-446">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a8065-446">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="a8065-447">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-447">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="a8065-448">La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="a8065-448">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="a8065-449">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-449">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="a8065-450">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="a8065-450">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="a8065-451">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="a8065-451">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="a8065-452">Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="a8065-452">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="a8065-453">Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="a8065-453">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="a8065-454">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="a8065-454">Configure simple options with a delegate</span></span>

<span data-ttu-id="a8065-455">La configuración de opciones simples con un delegado se muestra como ejemplo 2 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-455">Configuring simple options with a delegate is demonstrated as Example 2 in the sample app.</span></span>

<span data-ttu-id="a8065-456">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="a8065-456">Use a delegate to set options values.</span></span> <span data-ttu-id="a8065-457">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-457">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="a8065-458">En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-458">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a8065-459">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="a8065-459">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="a8065-460">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8065-460">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="a8065-461">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-461">You can add multiple configuration providers.</span></span> <span data-ttu-id="a8065-462">Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="a8065-462">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="a8065-463">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="a8065-463">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="a8065-464">Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-464">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions%601.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service to the service container.</span></span> <span data-ttu-id="a8065-465">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="a8065-465">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="a8065-466">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-466">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="a8065-467">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="a8065-467">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="a8065-468">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="a8065-468">Suboptions configuration</span></span>

<span data-ttu-id="a8065-469">La configuración de subopciones se muestra en el ejemplo 3 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-469">Suboptions configuration is demonstrated as Example 3 in the sample app.</span></span>

<span data-ttu-id="a8065-470">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8065-470">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="a8065-471">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="a8065-471">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="a8065-472">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="a8065-472">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="a8065-473">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-473">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="a8065-474">En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions%601> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="a8065-474">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions%601> service is added to the service container.</span></span> <span data-ttu-id="a8065-475">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8065-475">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="a8065-476">El método `GetSection` requiere el espacio de nombres <xref:Microsoft.Extensions.Configuration?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="a8065-476">The `GetSection` method requires the <xref:Microsoft.Extensions.Configuration?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="a8065-477">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="a8065-477">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="a8065-478">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-478">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="a8065-479">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-479">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="a8065-480">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-480">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="a8065-481">Opciones proporcionadas por un modelo de vista o con inserción de vista directa</span><span class="sxs-lookup"><span data-stu-id="a8065-481">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="a8065-482">Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo 4 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-482">Options provided by a view model or with direct view injection is demonstrated as Example 4 in the sample app.</span></span>

<span data-ttu-id="a8065-483">Las opciones se pueden suministrar en un modelo de vista o insertando <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-483">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="a8065-484">La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="a8065-484">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="a8065-485">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="a8065-485">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="a8065-487">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="a8065-487">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="a8065-488">El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> se muestra en el ejemplo 5 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-488">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is demonstrated in Example 5 in the sample app.</span></span>

<span data-ttu-id="a8065-489"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> admite volver a cargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="a8065-489"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="a8065-490">Cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="a8065-490">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="a8065-491">En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="a8065-491">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="a8065-492">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8065-492">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="a8065-493">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8065-493">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="a8065-494">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="a8065-494">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="a8065-495">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-495">Save the *appsettings.json* file.</span></span> <span data-ttu-id="a8065-496">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="a8065-496">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="a8065-497">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="a8065-497">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="a8065-498">La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> se muestra en el ejemplo 6 de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8065-498">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> is demonstrated as Example 6 in the sample app.</span></span>

<span data-ttu-id="a8065-499">La compatibilidad con las opciones con nombre permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="a8065-499">Named options support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="a8065-500">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a8065-500">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method.</span></span> <span data-ttu-id="a8065-501">Las opciones con nombre distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a8065-501">Named options are case sensitive.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="a8065-502">La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8065-502">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="a8065-503">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="a8065-503">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="a8065-504">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8065-504">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="a8065-505">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="a8065-505">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="a8065-506">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a8065-506">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="a8065-507">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-507">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="a8065-508">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="a8065-508">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="a8065-509">Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="a8065-509">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="a8065-510">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="a8065-510">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="a8065-511">Agregue manualmente este código al método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="a8065-511">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="a8065-512">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="a8065-512">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="a8065-513">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="a8065-513">All options are named instances.</span></span> <span data-ttu-id="a8065-514">Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions%601> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="a8065-514">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="a8065-515"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-515"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="a8065-516">La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory%601> tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="a8065-516">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="a8065-517">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="a8065-517">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="a8065-518">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="a8065-518">OptionsBuilder API</span></span>

<span data-ttu-id="a8065-519"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="a8065-519"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="a8065-520">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="a8065-520">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="a8065-521">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8065-521">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="a8065-522">Uso de servicios de DI para configurar opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-522">Use DI services to configure options</span></span>

<span data-ttu-id="a8065-523">Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-523">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="a8065-524">Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="a8065-524">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="a8065-525">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="a8065-525">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="a8065-526">Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions%601> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, y se registrar como un servicio.</span><span class="sxs-lookup"><span data-stu-id="a8065-526">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="a8065-527">Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada.</span><span class="sxs-lookup"><span data-stu-id="a8065-527">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="a8065-528">La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="a8065-528">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="a8065-529">La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="a8065-529">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span> 

## <a name="options-post-configuration"></a><span data-ttu-id="a8065-530">Configuración posterior de las opciones</span><span class="sxs-lookup"><span data-stu-id="a8065-530">Options post-configuration</span></span>

<span data-ttu-id="a8065-531">Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span><span class="sxs-lookup"><span data-stu-id="a8065-531">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="a8065-532">La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="a8065-532">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a8065-533"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="a8065-533"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a8065-534">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8065-534">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="a8065-535">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="a8065-535">Accessing options during startup</span></span>

<span data-ttu-id="a8065-536"><xref:Microsoft.Extensions.Options.IOptions%601> y <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="a8065-536"><xref:Microsoft.Extensions.Options.IOptions%601> and <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="a8065-537">No use <xref:Microsoft.Extensions.Options.IOptions%601> o <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8065-537">Don't use <xref:Microsoft.Extensions.Options.IOptions%601> or <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8065-538">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="a8065-538">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a8065-539">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a8065-539">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
