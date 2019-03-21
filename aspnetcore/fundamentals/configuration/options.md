---
title: Patrón de opciones en ASP.NET Core
author: guardrex
description: Descubra cómo usar el patrón de opciones para representar grupos de valores de configuración relacionados en aplicaciones ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 02/26/2019
uid: fundamentals/configuration/options
ms.openlocfilehash: 8d219658b7116ad7e1f2a73678ef76ea1ce69595
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58209192"
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="0d8f4-103">Patrón de opciones en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0d8f4-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="0d8f4-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="0d8f4-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="0d8f4-105">Para obtener la versión 1.1 de este tema, descargue [Patrón de opciones en ASP.NET Core (versión 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-105">For the 1.1 version of this topic, download [Options pattern in ASP.NET Core (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Options_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="0d8f4-106">El patrón de opciones usa clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-106">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="0d8f4-107">Cuando los [valores de configuración](xref:fundamentals/configuration/index) están aislados por escenario en clases independientes, la aplicación se ajusta a dos principios de ingeniería de software importantes:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-107">When [configuration settings](xref:fundamentals/configuration/index) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="0d8f4-108">El [principio de segregación de interfaz (ISP) o encapsulación](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Los escenarios (clases) que dependen de valores de configuración dependen únicamente de los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-108">The [Interface Segregation Principle (ISP) or Encapsulation](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#encapsulation) &ndash; Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="0d8f4-109">[Separación de intereses](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Los valores de configuración para distintos elementos de la aplicación no son dependientes entre sí ni están emparejados.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-109">[Separation of Concerns](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns) &ndash; Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="0d8f4-110">Las opciones también proporcionan un mecanismo para validar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-110">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="0d8f4-111">Para obtener más información, consulte la sección [Opciones de validación](#options-validation).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-111">For more information, see the [Options validation](#options-validation) section.</span></span>

<span data-ttu-id="0d8f4-112">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0d8f4-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d8f4-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0d8f4-113">Prerequisites</span></span>

<span data-ttu-id="0d8f4-114">Haga referencia al [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) o agregue una referencia de paquete al paquete [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-114">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) package.</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="0d8f4-115">Interfaces de opciones</span><span class="sxs-lookup"><span data-stu-id="0d8f4-115">Options interfaces</span></span>

<span data-ttu-id="0d8f4-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> se usa para recuperar las opciones y administrar las notificaciones de las opciones para instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-116"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> is used to retrieve options and manage options notifications for `TOptions` instances.</span></span> <span data-ttu-id="0d8f4-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> admite las siguientes situaciones:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-117"><xref:Microsoft.Extensions.Options.IOptionsMonitor`1> supports the following scenarios:</span></span>

* <span data-ttu-id="0d8f4-118">Notificaciones de cambios</span><span class="sxs-lookup"><span data-stu-id="0d8f4-118">Change notifications</span></span>
* [<span data-ttu-id="0d8f4-119">Opciones con nombre</span><span class="sxs-lookup"><span data-stu-id="0d8f4-119">Named options</span></span>](#named-options-support-with-iconfigurenamedoptions)
* [<span data-ttu-id="0d8f4-120">Configuración que se puede recargar</span><span class="sxs-lookup"><span data-stu-id="0d8f4-120">Reloadable configuration</span></span>](#reload-configuration-data-with-ioptionssnapshot)
* <span data-ttu-id="0d8f4-121">Invalidación de opciones de selección (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)</span><span class="sxs-lookup"><span data-stu-id="0d8f4-121">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1>)</span></span>

<span data-ttu-id="0d8f4-122">Los escenarios [posteriores a la configuración](#options-post-configuration) le permiten establecer o cambiar las opciones después de que finalice toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-122">[Post-configuration](#options-post-configuration) scenarios allow you to set or change options after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs.</span></span>

<span data-ttu-id="0d8f4-123"><xref:Microsoft.Extensions.Options.IOptionsFactory`1> es responsable de crear nuevas instancias de opciones.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-123"><xref:Microsoft.Extensions.Options.IOptionsFactory`1> is responsible for creating new options instances.</span></span> <span data-ttu-id="0d8f4-124">Tiene un solo método <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-124">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory`1.Create*> method.</span></span> <span data-ttu-id="0d8f4-125">La implementación predeterminada toma todas las instancias registradas de <xref:Microsoft.Extensions.Options.IConfigureOptions`1> y <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>, y establece todas las configuraciones primero, seguidas de las configuraciones posteriores.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-125">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="0d8f4-126">Distingue entre <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> y <xref:Microsoft.Extensions.Options.IConfigureOptions`1>, y solo llama a la interfaz adecuada.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-126">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> and <xref:Microsoft.Extensions.Options.IConfigureOptions`1> and only calls the appropriate interface.</span></span>

<span data-ttu-id="0d8f4-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> se usa por <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> para almacenar en caché las instancias de `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-127"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to cache `TOptions` instances.</span></span> <span data-ttu-id="0d8f4-128"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> invalida instancias de opciones en la supervisión para que se pueda volver a calcular el valor (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-128">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryRemove*>).</span></span> <span data-ttu-id="0d8f4-129">Los valores se pueden introducir manualmente y mediante <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-129">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.TryAdd*>.</span></span> <span data-ttu-id="0d8f4-130">Se usa el método <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> cuando todas las instancias con nombre se deben volver a crear a petición.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-130">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache`1.Clear*> method is used when all named instances should be recreated on demand.</span></span>

<span data-ttu-id="0d8f4-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> es útil en escenarios donde se deben volver a calcular las opciones en cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-131"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is useful in scenarios where options should be recomputed on every request.</span></span> <span data-ttu-id="0d8f4-132">Para obtener más información, consulte la sección [Volver a cargar los datos de configuración con IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-132">For more information, see the [Reload configuration data with IOptionsSnapshot](#reload-configuration-data-with-ioptionssnapshot) section.</span></span>

<span data-ttu-id="0d8f4-133"><xref:Microsoft.Extensions.Options.IOptions`1> puede utilizarse para admitir las opciones.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-133"><xref:Microsoft.Extensions.Options.IOptions`1> can be used to support options.</span></span> <span data-ttu-id="0d8f4-134">Sin embargo, <xref:Microsoft.Extensions.Options.IOptions`1> no es compatible con los escenarios anteriores de <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-134">However, <xref:Microsoft.Extensions.Options.IOptions`1> doesn't support the preceding scenarios of <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span> <span data-ttu-id="0d8f4-135">Aún puede usar <xref:Microsoft.Extensions.Options.IOptions`1> en marcos y bibliotecas existentes que ya usan la interfaz <xref:Microsoft.Extensions.Options.IOptions`1> y no requieren los escenarios proporcionados por <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-135">You may continue to use <xref:Microsoft.Extensions.Options.IOptions`1> in existing frameworks and libraries that already use the <xref:Microsoft.Extensions.Options.IOptions`1> interface and don't require the scenarios provided by <xref:Microsoft.Extensions.Options.IOptionsMonitor`1>.</span></span>

## <a name="general-options-configuration"></a><span data-ttu-id="0d8f4-136">Configuración de opciones generales</span><span class="sxs-lookup"><span data-stu-id="0d8f4-136">General options configuration</span></span>

<span data-ttu-id="0d8f4-137">La configuración de opciones generales se muestra en el ejemplo &num;1 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-137">General options configuration is demonstrated as Example &num;1 in the sample app.</span></span>

<span data-ttu-id="0d8f4-138">Una clase de opciones debe ser no abstracta con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-138">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="0d8f4-139">La siguiente clase, `MyOptions`, tiene dos propiedades: `Option1` y `Option2`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-139">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="0d8f4-140">Configurar los valores predeterminados es opcional, pero el constructor de clases en el ejemplo siguiente establece el valor predeterminado de `Option1`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-140">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="0d8f4-141">`Option2` tiene un valor predeterminado que se establece al inicializar la propiedad directamente (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-141">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="0d8f4-142">La clase `MyOptions` se agrega al contenedor de servicios con <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> y se enlaza a la configuración:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-142">The `MyOptions` class is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure*> and bound to configuration:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="0d8f4-143">El siguiente modelo de página usa la [inserción de dependencias de constructor](xref:mvc/controllers/dependency-injection) con <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> para acceder a la configuración (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-143">The following page model uses [constructor dependency injection](xref:mvc/controllers/dependency-injection) with <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="0d8f4-144">El archivo *appSettings.json* del ejemplo especifica valores para `option1` y `option2`:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-144">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=2-3)]

<span data-ttu-id="0d8f4-145">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-145">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> <span data-ttu-id="0d8f4-146">Al usar una instancia de <xref:System.Configuration.ConfigurationBuilder> personalizada para cargar las opciones de configuración desde un archivo de configuración, confirme que la ruta de acceso base esté configurada correctamente:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-146">When using a custom <xref:System.Configuration.ConfigurationBuilder> to load options configuration from a settings file, confirm that the base path is set correctly:</span></span>
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
> <span data-ttu-id="0d8f4-147">Al cargar las opciones de configuración desde el archivo de configuración a través de <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, no es necesario establecer de forma explícita la ruta de acceso base.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-147">Explicitly setting the base path isn't required when loading options configuration from the settings file via <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="0d8f4-148">Configurar opciones simples con un delegado</span><span class="sxs-lookup"><span data-stu-id="0d8f4-148">Configure simple options with a delegate</span></span>

<span data-ttu-id="0d8f4-149">La configuración de opciones simples con un delegado se muestra como ejemplo &num;2 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-149">Configuring simple options with a delegate is demonstrated as Example &num;2 in the sample app.</span></span>

<span data-ttu-id="0d8f4-150">Use un delegado para establecer los valores de opciones.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-150">Use a delegate to set options values.</span></span> <span data-ttu-id="0d8f4-151">La aplicación de ejemplo usa la clase `MyOptionsWithDelegateConfig` (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-151">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="0d8f4-152">En el código siguiente, un segundo servicio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-152">In the following code, a second <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="0d8f4-153">Usa un delegado para configurar el enlace con `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-153">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="0d8f4-154">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-154">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="0d8f4-155">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-155">You can add multiple configuration providers.</span></span> <span data-ttu-id="0d8f4-156">Los proveedores de configuración están disponibles en paquetes de NuGet y se aplican en el orden en que están registrados.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-156">Configuration providers are available from NuGet packages and are applied in the order that they're registered.</span></span> <span data-ttu-id="0d8f4-157">Para obtener más información, vea <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-157">For more information, see <xref:fundamentals/configuration/index>.</span></span>

<span data-ttu-id="0d8f4-158">Cada llamada a <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> agrega un servicio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-158">Each call to <xref:Microsoft.Extensions.Options.IConfigureOptions`1.Configure*> adds an <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service to the service container.</span></span> <span data-ttu-id="0d8f4-159">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.json*, pero los valores de `Option1` y `Option2` se reemplazan por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-159">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="0d8f4-160">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificada *gana* y establece el valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-160">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="0d8f4-161">Cuando se ejecuta la aplicación, el método `OnGet` del modelo de página devuelve una cadena que muestra los valores de la clase de opción:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-161">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delegate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="0d8f4-162">Configuración de subopciones</span><span class="sxs-lookup"><span data-stu-id="0d8f4-162">Suboptions configuration</span></span>

<span data-ttu-id="0d8f4-163">La configuración de subopciones se muestra en el ejemplo &num;3 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-163">Suboptions configuration is demonstrated as Example &num;3 in the sample app.</span></span>

<span data-ttu-id="0d8f4-164">Las aplicaciones deben crear clases de opciones que pertenezcan a grupos específicos de escenarios (clases) en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-164">Apps should create options classes that pertain to specific scenario groups (classes) in the app.</span></span> <span data-ttu-id="0d8f4-165">Los elementos de la aplicación que requieran valores de configuración deben acceder solamente a los valores de configuración que usen.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-165">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="0d8f4-166">Al enlazar opciones para la configuración, cada propiedad en el tipo de opciones se enlaza a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-166">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="0d8f4-167">Por ejemplo, la propiedad `MyOptions.Option1` se enlaza a la clave `Option1`, que se lee desde la propiedad `option1` en *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-167">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="0d8f4-168">En el código siguiente, se agrega un tercer servicio <xref:Microsoft.Extensions.Options.IConfigureOptions`1> al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-168">In the following code, a third <xref:Microsoft.Extensions.Options.IConfigureOptions`1> service is added to the service container.</span></span> <span data-ttu-id="0d8f4-169">Enlaza `MySubOptions` a la sección `subsection` del archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-169">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="0d8f4-170">El método de extensión `GetSection` requiere el paquete NuGet [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-170">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="0d8f4-171">Si la aplicación usa el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 o posterior), el paquete se incluye automáticamente.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-171">If the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later), the package is automatically included.</span></span>

<span data-ttu-id="0d8f4-172">El archivo *appSettings.json* del ejemplo define un miembro `subsection` con las claves para `suboption1` y `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-172">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/samples/2.x/OptionsSample/appsettings.json?highlight=4-7)]

<span data-ttu-id="0d8f4-173">La clase `MySubOptions` define propiedades, `SubOption1` y `SubOption2`, para mantener los valores de opciones (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-173">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the options values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="0d8f4-174">El método `OnGet` del modelo de página devuelve una cadena con los valores de opciones (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-174">The page model's `OnGet` method returns a string with the options values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="0d8f4-175">Cuando se ejecuta la aplicación, el método `OnGet` devuelve una cadena que muestra los valores de clase de subopciones:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-175">When the app is run, the `OnGet` method returns a string showing the suboption class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="0d8f4-176">Opciones proporcionadas por un modelo de vista o con inserción de vista directa</span><span class="sxs-lookup"><span data-stu-id="0d8f4-176">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="0d8f4-177">Las opciones proporcionadas por un modelo de vista o de inserción de vista directa se muestran en el ejemplo &num;4 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-177">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the sample app.</span></span>

<span data-ttu-id="0d8f4-178">Las opciones se pueden suministrar en un modelo de vista o insertando <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> directamente en una vista (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-178">Options can be supplied in a view model or by injecting <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="0d8f4-179">La aplicación de ejemplo muestra cómo insertar `IOptionsMonitor<MyOptions>` con una directiva `@inject`:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-179">The sample app shows how to inject `IOptionsMonitor<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/samples/2.x/OptionsSample/Pages/Index.cshtml?range=1-10&highlight=4)]

<span data-ttu-id="0d8f4-180">Cuando se ejecuta la aplicación, se muestran los valores de opciones en la página representada:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-180">When the app is run, the options values are shown in the rendered page:</span></span>

![Los valores de opciones de Option1: value1_from_json y Option2: -1 se cargan desde el modelo y por inserción en la vista.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="0d8f4-182">Volver a cargar los datos de configuración con IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="0d8f4-182">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="0d8f4-183">El procedimiento de volver a cargar los datos de configuración con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> se muestra en el ejemplo &num;5 en la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-183">Reloading configuration data with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is demonstrated in Example &num;5 in the sample app.</span></span>

<span data-ttu-id="0d8f4-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> admite volver a cargar opciones con la mínima sobrecarga de procesamiento.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-184"><xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> supports reloading options with minimal processing overhead.</span></span>

<span data-ttu-id="0d8f4-185">Cuando se accede a las opciones y se las almacena en caché durante la vigencia de la solicitud, se calculan una vez por solicitud.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-185">Options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="0d8f4-186">En el ejemplo siguiente se muestra cómo se crea un nuevo <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> después de cambiar el archivo *appSettings.json* (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-186">The following example demonstrates how a new <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1> is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="0d8f4-187">Varias solicitudes al servidor devuelven valores constantes proporcionados por el archivo *appSettings.json* hasta que se modifique el archivo y vuelva a cargarse la configuración.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-187">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="0d8f4-188">En la siguiente imagen se muestran los valores `option1` y `option2` iniciales cargados desde el archivo *appSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-188">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="0d8f4-189">Cambie los valores del archivo *appSettings.json* a `value1_from_json UPDATED` y `200`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-189">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="0d8f4-190">Guarde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-190">Save the *appsettings.json* file.</span></span> <span data-ttu-id="0d8f4-191">Actualice el explorador para ver qué valores de opciones se han actualizado:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-191">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="0d8f4-192">Compatibilidad de opciones con nombre con IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="0d8f4-192">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="0d8f4-193">La compatibilidad de opciones con nombre con <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> se muestra en el ejemplo &num;6 de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-193">Named options support with <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> is demonstrated as Example &num;6 in the sample app.</span></span>

<span data-ttu-id="0d8f4-194">La compatibilidad con las *opciones con nombre* permite a la aplicación distinguir entre las configuraciones de opciones con nombre.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-194">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="0d8f4-195">En la aplicación de ejemplo, las opciones con nombre se declaran con [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), que llama al método de extensión [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-195">In the sample app, named options are declared with [OptionsServiceCollectionExtensions.Configure](xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure*), which calls the [ConfigureNamedOptions\<TOptions>.Configure](xref:Microsoft.Extensions.Options.ConfigureNamedOptions`1.Configure*) extension method:</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="0d8f4-196">La aplicación de ejemplo accede a las opciones con nombre con <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="0d8f4-196">The sample app accesses the named options with <xref:Microsoft.Extensions.Options.IOptionsSnapshot`1.Get*> (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/samples/2.x/OptionsSample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="0d8f4-197">Al ejecutar la aplicación de ejemplo, se devuelven las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-197">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="0d8f4-198">Se proporcionan valores de `named_options_1` a partir de la configuración, que se cargan desde el archivo *appSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-198">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="0d8f4-199">Los valores de `named_options_2` los proporciona:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-199">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="0d8f4-200">El delegado `named_options_2` en `ConfigureServices` para `Option1`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-200">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="0d8f4-201">El valor predeterminado para `Option2` proporcionado por la clase `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-201">The default value for `Option2` provided by the `MyOptions` class.</span></span>

## <a name="configure-all-options-with-the-configureall-method"></a><span data-ttu-id="0d8f4-202">Configurar todas las opciones con el método ConfigureAll</span><span class="sxs-lookup"><span data-stu-id="0d8f4-202">Configure all options with the ConfigureAll method</span></span>

<span data-ttu-id="0d8f4-203">Configure todas las instancias de opciones con el método <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-203">Configure all options instances with the <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> method.</span></span> <span data-ttu-id="0d8f4-204">El siguiente código configura `Option1` para todas las instancias de configuración con un valor común.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-204">The following code configures `Option1` for all configuration instances with a common value.</span></span> <span data-ttu-id="0d8f4-205">Agregue manualmente este código al método `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-205">Add the following code manually to the `Startup.ConfigureServices` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="0d8f4-206">Al ejecutar la aplicación de ejemplo después de agregar el código se produce el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-206">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="0d8f4-207">Todas las opciones son instancias con nombre.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-207">All options are named instances.</span></span> <span data-ttu-id="0d8f4-208">Las instancias de <xref:Microsoft.Extensions.Options.IConfigureOptions`1> existentes se usan para seleccionar como destino la instancia de `Options.DefaultName`, que es `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-208">Existing <xref:Microsoft.Extensions.Options.IConfigureOptions`1> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="0d8f4-209"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> también implementa <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-209"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions`1>.</span></span> <span data-ttu-id="0d8f4-210">La implementación predeterminada de <xref:Microsoft.Extensions.Options.IOptionsFactory`1> tiene lógica para usar cada una de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-210">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory`1> has logic to use each appropriately.</span></span> <span data-ttu-id="0d8f4-211">La opción con nombre `null` se usa para seleccionar como destino todas las instancias con nombre, en lugar de una instancia con nombre determinada (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> y <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> usan esta convención).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-211">The `null` named option is used to target all of the named instances instead of a specific named instance (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll*> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> use this convention).</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="0d8f4-212">API OptionsBuilder</span><span class="sxs-lookup"><span data-stu-id="0d8f4-212">OptionsBuilder API</span></span>

<span data-ttu-id="0d8f4-213"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> se usa para configurar instancias `TOptions`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-213"><xref:Microsoft.Extensions.Options.OptionsBuilder`1> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="0d8f4-214">`OptionsBuilder` simplifica la creación de opciones con nombre, ya que es un único parámetro para la llamada `AddOptions<TOptions>(string optionsName)` inicial en lugar de aparecer en todas las llamadas posteriores.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-214">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="0d8f4-215">La validación de opciones y las sobrecargas `ConfigureOptions` que aceptan las dependencias de servicio solo están disponibles mediante `OptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-215">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");

services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="0d8f4-216">Uso de servicios de DI para configurar opciones</span><span class="sxs-lookup"><span data-stu-id="0d8f4-216">Use DI services to configure options</span></span>

<span data-ttu-id="0d8f4-217">Hay dos formas de acceder a otros servicios desde la inserción de dependencias durante la configuración de opciones:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-217">You can access other services from dependency injection while configuring options in two ways:</span></span>

* <span data-ttu-id="0d8f4-218">Si se pasa un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) en [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-218">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1).</span></span> <span data-ttu-id="0d8f4-219">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) proporciona sobrecargas de [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) que permiten usar hasta cinco servicios para configurar las opciones:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-219">[OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder`1) provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) that allow you to use up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<Service1, Service2, Service3, Service4, Service5>(
          (o, s, s2, s3, s4, s5) => 
              o.Property = DoSomethingWith(s, s2, s3, s4, s5));
  ```

* <span data-ttu-id="0d8f4-220">Si se crea un tipo propio que implementa <xref:Microsoft.Extensions.Options.IConfigureOptions`1> o <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, y se registrar como un servicio.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-220">Create your own type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions`1> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> and register the type as a service.</span></span>

<span data-ttu-id="0d8f4-221">Se recomienda pasar un delegado de configuración a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), ya que la creación de un servicio es más complicada.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-221">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*), since creating a service is more complex.</span></span> <span data-ttu-id="0d8f4-222">La creación de un tipo propio es equivalente a lo que el marco hace de forma automática cuando se usa [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-222">Creating your own type is equivalent to what the framework does for you when you use [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*).</span></span> <span data-ttu-id="0d8f4-223">La llamada a [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registra una interfaz <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1> genérica y transitoria, con un constructor que acepta los tipos de servicio genéricos especificados.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-223">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder`1.Configure*) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, which has a constructor that accepts the generic service types specified.</span></span> 

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a><span data-ttu-id="0d8f4-224">Opciones de validación</span><span class="sxs-lookup"><span data-stu-id="0d8f4-224">Options validation</span></span>

<span data-ttu-id="0d8f4-225">Las opciones de validación permiten validar las opciones cuando se configuran.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-225">Options validation allows you to validate options when options are configured.</span></span> <span data-ttu-id="0d8f4-226">Llame a `Validate` con un método de validación que devuelve `true` si las opciones son válidas y `false` si no lo son:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-226">Call `Validate` with a validation method that returns `true` if options are valid and `false` if they aren't valid:</span></span>

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

<span data-ttu-id="0d8f4-227">El ejemplo anterior establece la instancia de opciones con nombre en `optionalOptionsName`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-227">The preceding example sets the named options instance to `optionalOptionsName`.</span></span> <span data-ttu-id="0d8f4-228">La instancia predeterminada es `Options.DefaultName`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-228">The default options instance is `Options.DefaultName`.</span></span>

<span data-ttu-id="0d8f4-229">La validación se ejecuta cuando se crea la instancia de opciones.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-229">Validation runs when the options instance is created.</span></span> <span data-ttu-id="0d8f4-230">La instancia de opciones pasa seguro la validación la primera vez que se accede.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-230">Your options instance is guaranteed to pass validation the first time it's accessed.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0d8f4-231">La validación de opciones no protege contra las modificaciones de opciones después de configurarse y validarse inicialmente.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-231">Options validation doesn't guard against options modifications after the options are initially configured and validated.</span></span>

<span data-ttu-id="0d8f4-232">El método `Validate` acepta una expresión `Func<TOptions, bool>`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-232">The `Validate` method accepts a `Func<TOptions, bool>`.</span></span> <span data-ttu-id="0d8f4-233">Para personalizar completamente la validación, implemente `IValidateOptions<TOptions>`, que permite:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-233">To fully customize validation, implement `IValidateOptions<TOptions>`, which allows:</span></span>

* <span data-ttu-id="0d8f4-234">Validación de varios tipos de opciones: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span><span class="sxs-lookup"><span data-stu-id="0d8f4-234">Validation of multiple options types: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`</span></span>
* <span data-ttu-id="0d8f4-235">Validación que depende de otro tipo de opción: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span><span class="sxs-lookup"><span data-stu-id="0d8f4-235">Validation that depends on another option type: `public DependsOnAnotherOptionValidator(IOptionsMonitor<AnotherOption> options)`</span></span>

<span data-ttu-id="0d8f4-236">`IValidateOptions` valida:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-236">`IValidateOptions` validates:</span></span>

* <span data-ttu-id="0d8f4-237">Una instancia de opciones con nombre específica.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-237">A specific named options instance.</span></span>
* <span data-ttu-id="0d8f4-238">Todas las opciones cuando `name` es `null`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-238">All options when `name` is `null`.</span></span>

<span data-ttu-id="0d8f4-239">Devuelve `ValidateOptionsResult` de la implementación de la interfaz:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-239">Return a `ValidateOptionsResult` from your implementation of the interface:</span></span>

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

<span data-ttu-id="0d8f4-240">La validación basada en la anotación de datos está disponible en el paquete [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) llamando al método `ValidateDataAnnotations` en `OptionsBuilder<TOptions>`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-240">Data Annotation-based validation is available from the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) package by calling the `ValidateDataAnnotations` method on `OptionsBuilder<TOptions>`.</span></span> <span data-ttu-id="0d8f4-241">`Microsoft.Extensions.Options.DataAnnotations` está incluido en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 o versiones posteriores).</span><span class="sxs-lookup"><span data-stu-id="0d8f4-241">`Microsoft.Extensions.Options.DataAnnotations` is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.2 or later).</span></span>

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

<span data-ttu-id="0d8f4-242">La validación diligente (con respuesta rápida a errores en el inicio) se está teniendo en cuenta de cara a una versión futura.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-242">Eager validation (fail fast at startup) is under consideration for a future release.</span></span>

::: moniker-end

## <a name="options-post-configuration"></a><span data-ttu-id="0d8f4-243">Configuración posterior de las opciones</span><span class="sxs-lookup"><span data-stu-id="0d8f4-243">Options post-configuration</span></span>

<span data-ttu-id="0d8f4-244">Establezca la configuración posterior con <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-244">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions`1>.</span></span> <span data-ttu-id="0d8f4-245">La configuración posterior se ejecuta una vez completada toda la configuración de <xref:Microsoft.Extensions.Options.IConfigureOptions`1>:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-245">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions`1> configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0d8f4-246"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> está disponible para configurar posteriormente las opciones con nombre:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-246"><xref:Microsoft.Extensions.Options.IPostConfigureOptions`1.PostConfigure*> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="0d8f4-247">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> para configurar posteriormente todas las instancias de configuración:</span><span class="sxs-lookup"><span data-stu-id="0d8f4-247">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll*> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="accessing-options-during-startup"></a><span data-ttu-id="0d8f4-248">Acceso a opciones durante el inicio</span><span class="sxs-lookup"><span data-stu-id="0d8f4-248">Accessing options during startup</span></span>

<span data-ttu-id="0d8f4-249"><xref:Microsoft.Extensions.Options.IOptions`1> y <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> puede usarse en `Startup.Configure`, ya que los servicios se compilan antes de que se ejecute el método `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-249"><xref:Microsoft.Extensions.Options.IOptions`1> and <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> can be used in `Startup.Configure`, since services are built before the `Configure` method executes.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IOptionsMonitor<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.CurrentValue.Option1;
}
```

<span data-ttu-id="0d8f4-250">No use <xref:Microsoft.Extensions.Options.IOptions`1> o <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-250">Don't use <xref:Microsoft.Extensions.Options.IOptions`1> or <xref:Microsoft.Extensions.Options.IOptionsMonitor`1> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="0d8f4-251">Puede que exista un estado incoherente de opciones debido al orden de los registros de servicio.</span><span class="sxs-lookup"><span data-stu-id="0d8f4-251">An inconsistent options state may exist due to the ordering of service registrations.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d8f4-252">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="0d8f4-252">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
