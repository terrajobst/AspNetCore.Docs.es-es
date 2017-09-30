---
title: "Configuración en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo usar la API de configuración para configurar una aplicación ASP.NET Core desde varios orígenes."
keywords: "Núcleo de ASP.NET, configuración, JSON, configuración"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: 379030df4ca91a38fce251aeaab9c5dfaf11e915
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="f7086-104">Configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7086-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="f7086-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f7086-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f7086-106">La API de configuración proporciona una manera de configurar una aplicación basada en una lista de pares nombre / valor.</span><span class="sxs-lookup"><span data-stu-id="f7086-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="f7086-107">Se lee la configuración en tiempo de ejecución de varios orígenes.</span><span class="sxs-lookup"><span data-stu-id="f7086-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="f7086-108">Los pares de nombre-valor se pueden agrupar en una jerarquía multinivel.</span><span class="sxs-lookup"><span data-stu-id="f7086-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="f7086-109">Existen proveedores de configuración para:</span><span class="sxs-lookup"><span data-stu-id="f7086-109">There are configuration providers for:</span></span>

* <span data-ttu-id="f7086-110">Formatos de archivo (INI, JSON y XML)</span><span class="sxs-lookup"><span data-stu-id="f7086-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="f7086-111">Argumentos de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f7086-111">Command-line arguments</span></span>
* <span data-ttu-id="f7086-112">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="f7086-112">Environment variables</span></span>
* <span data-ttu-id="f7086-113">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="f7086-113">In-memory .NET objects</span></span>
* <span data-ttu-id="f7086-114">Un almacén de usuario cifrado</span><span class="sxs-lookup"><span data-stu-id="f7086-114">An encrypted user store</span></span>
* [<span data-ttu-id="f7086-115">Almacén de claves de Azure</span><span class="sxs-lookup"><span data-stu-id="f7086-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="f7086-116">Proveedores personalizados, que se instale o cree</span><span class="sxs-lookup"><span data-stu-id="f7086-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="f7086-117">Cada valor de configuración se asigna a una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="f7086-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="f7086-118">No hay compatibilidad de enlace integrado para deserializar la configuración en un personalizado [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (una clase .NET simple con propiedades).</span><span class="sxs-lookup"><span data-stu-id="f7086-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="f7086-119">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="f7086-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="f7086-120">Muestra una configuración sencilla</span><span class="sxs-lookup"><span data-stu-id="f7086-120">Simple configuration</span></span>

<span data-ttu-id="f7086-121">La siguiente aplicación de consola utiliza el proveedor de configuración de JSON:</span><span class="sxs-lookup"><span data-stu-id="f7086-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="f7086-122">La aplicación lee y muestra los valores de configuración siguientes:</span><span class="sxs-lookup"><span data-stu-id="f7086-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="f7086-123">Configuración consta de una lista jerárquica de los pares de nombre / valor en el que los nodos están separados por un coma.</span><span class="sxs-lookup"><span data-stu-id="f7086-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="f7086-124">Para recuperar un valor, tener acceso a la `Configuration` indizador con la correspondiente clave del elemento:</span><span class="sxs-lookup"><span data-stu-id="f7086-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="f7086-125">Para trabajar con matrices en orígenes de configuración con formato JSON, usar un índice de matriz como parte de la cadena separada por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="f7086-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="f7086-126">En el ejemplo siguiente se obtiene el nombre del primer elemento en el anterior `wizards` matriz:</span><span class="sxs-lookup"><span data-stu-id="f7086-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="f7086-127">Pares de nombre / valor que se escriben en integrado en `Configuration` proveedores son **no** es persistente, sin embargo, puede crear un proveedor personalizado que guarda los valores.</span><span class="sxs-lookup"><span data-stu-id="f7086-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="f7086-128">Vea [proveedor de configuración personalizada](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="f7086-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="f7086-129">En el ejemplo anterior utiliza el indizador de configuración para leer los valores.</span><span class="sxs-lookup"><span data-stu-id="f7086-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="f7086-130">Acceso a la configuración fuera de `Startup`, use la [patrón opciones](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="f7086-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="f7086-131">El *patrón opciones* se muestra más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="f7086-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="f7086-132">Es habitual tener distintos valores de configuración para los entornos diferentes, por ejemplo, desarrollo, prueba y producción.</span><span class="sxs-lookup"><span data-stu-id="f7086-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="f7086-133">El `CreateDefaultBuilder` método de extensión en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y sistema de orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="f7086-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="f7086-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="f7086-134">*appsettings.json*</span></span>
* <span data-ttu-id="f7086-135">*appSettings. \<EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="f7086-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="f7086-136">variables de entorno</span><span class="sxs-lookup"><span data-stu-id="f7086-136">environment variables</span></span>

<span data-ttu-id="f7086-137">Vea [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="f7086-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="f7086-138">`reloadOnChange`solo se admite en ASP.NET Core 1.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="f7086-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="f7086-139">Orígenes de configuración se leen en el orden en que se especifican.</span><span class="sxs-lookup"><span data-stu-id="f7086-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="f7086-140">En el código anterior, las variables de entorno se leen de la última.</span><span class="sxs-lookup"><span data-stu-id="f7086-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="f7086-141">Los valores de configuración establecido a través del entorno podría reemplazarlas que se establecen en los dos proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="f7086-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="f7086-142">El entorno se establece normalmente en uno de `Development`, `Staging`, o `Production`.</span><span class="sxs-lookup"><span data-stu-id="f7086-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="f7086-143">Vea [trabajar con varios entornos](environments.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f7086-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="f7086-144">Consideraciones sobre la configuración:</span><span class="sxs-lookup"><span data-stu-id="f7086-144">Configuration considerations:</span></span>

* <span data-ttu-id="f7086-145">`IOptionsSnapshot`puede volver a cargar los datos de configuración, cuando el estado cambia.</span><span class="sxs-lookup"><span data-stu-id="f7086-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="f7086-146">Use `IOptionsSnapshot` si tiene que volver a cargar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="f7086-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="f7086-147">Vea [IOptionsSnapshot](#ioptionssnapshot) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f7086-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="f7086-148">Claves de configuración que distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f7086-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="f7086-149">Una práctica recomendada es especificar variables de entorno por último, para que el entorno local puedan reemplazar todo lo establecido en los archivos de configuración implementada.</span><span class="sxs-lookup"><span data-stu-id="f7086-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="f7086-150">**Nunca** almacenar contraseñas u otros datos confidenciales en el código de proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="f7086-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="f7086-151">No utilice secretos de producción en el desarrollo o entornos de prueba.</span><span class="sxs-lookup"><span data-stu-id="f7086-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="f7086-152">En su lugar, especifique secretos fuera del árbol del proyecto, por lo que no se pueden confirmar accidentalmente en el repositorio.</span><span class="sxs-lookup"><span data-stu-id="f7086-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="f7086-153">Obtenga más información sobre [trabajar con varios entornos](environments.md) y administrar [almacenamiento seguro para la ejecución de secretos de aplicación durante el desarrollo](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="f7086-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="f7086-154">Si `:` no se puede utilizar en variables de entorno en el sistema, reemplace `:` con `__` (subrayado doble).</span><span class="sxs-lookup"><span data-stu-id="f7086-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="f7086-155">Uso de opciones y objetos de configuración</span><span class="sxs-lookup"><span data-stu-id="f7086-155">Using Options and configuration objects</span></span>

<span data-ttu-id="f7086-156">El patrón de opciones usa las clases de opciones personalizadas para representar un grupo de valores relacionados.</span><span class="sxs-lookup"><span data-stu-id="f7086-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="f7086-157">Se recomienda crear clases desacopladas para cada característica dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7086-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="f7086-158">Clases desacopladas siguen:</span><span class="sxs-lookup"><span data-stu-id="f7086-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="f7086-159">El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/) : clases dependen únicamente de los valores de configuración que se usan.</span><span class="sxs-lookup"><span data-stu-id="f7086-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="f7086-160">[Separación de intereses](http://deviq.com/separation-of-concerns/) : configuración para las distintas partes de la aplicación no es dependientes o acoplamiento entre sí.</span><span class="sxs-lookup"><span data-stu-id="f7086-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="f7086-161">La clase de opciones debe ser no abstracto con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="f7086-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="f7086-162">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f7086-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

<span data-ttu-id="f7086-163">En el código siguiente, se habilita el proveedor de configuración de JSON.</span><span class="sxs-lookup"><span data-stu-id="f7086-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="f7086-164">La `MyOptions` clase se agrega al contenedor de servicios y se enlaza a la configuración.</span><span class="sxs-lookup"><span data-stu-id="f7086-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="f7086-165">El siguiente [controlador](../mvc/controllers/index.md) utiliza [constructor inyección de dependencia](xref:fundamentals/dependency-injection#what-is-dependency-injection) en [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) acceso a la configuración:</span><span class="sxs-lookup"><span data-stu-id="f7086-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="f7086-166">Con el siguiente *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="f7086-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="f7086-167">El `HomeController.Index` método `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="f7086-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="f7086-168">Las aplicaciones típicas no enlazar toda la configuración a un archivo de opciones de inicio único.</span><span class="sxs-lookup"><span data-stu-id="f7086-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="f7086-169">Más adelante voy a explicar cómo usar `GetSection` para enlazar a una sección.</span><span class="sxs-lookup"><span data-stu-id="f7086-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="f7086-170">En el código siguiente, un segundo `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="f7086-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="f7086-171">Usa un delegado para configurar el enlace con `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="f7086-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="f7086-172">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="f7086-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="f7086-173">Proveedores de configuración están disponibles en paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="f7086-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="f7086-174">Se aplican en orden que están registrados.</span><span class="sxs-lookup"><span data-stu-id="f7086-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="f7086-175">Cada llamada a `Configure<TOptions>` agrega un `IConfigureOptions<TOptions>` servicio al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="f7086-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="f7086-176">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.JSON que se* --pero el valor de `Option1` se reemplaza por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="f7086-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="f7086-177">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificado "gana" (que establece el valor de configuración).</span><span class="sxs-lookup"><span data-stu-id="f7086-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="f7086-178">En el código anterior, el `HomeController.Index` método `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="f7086-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="f7086-179">Al enlazar opciones de configuración, cada propiedad en el tipo de opciones está enlazado a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="f7086-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="f7086-180">Por ejemplo, el `MyOptions.Option1` propiedad se enlaza a la clave `Option1`, que se lee desde el `option1` propiedad en *appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="f7086-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="f7086-181">Más adelante en este artículo, se muestra un ejemplo de la subpropiedad.</span><span class="sxs-lookup"><span data-stu-id="f7086-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="f7086-182">En el código siguiente, una tercera `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="f7086-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="f7086-183">Enlaza `MySubOptions` a la sección `subsection` de la *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="f7086-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="f7086-184">Nota: Este método de extensión requiere el `Microsoft.Extensions.Options.ConfigurationExtensions` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="f7086-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="f7086-185">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="f7086-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="f7086-186">La `MySubOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="f7086-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="f7086-187">Con los siguientes `Controller`:</span><span class="sxs-lookup"><span data-stu-id="f7086-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="f7086-188">`subOption1 = subvalue1_from_json, subOption2 = 200`se devuelve.</span><span class="sxs-lookup"><span data-stu-id="f7086-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="f7086-189">También puede proporcionar opciones en un modelo de vista o insertar `IOptions<TOptions>` directamente en una vista:</span><span class="sxs-lookup"><span data-stu-id="f7086-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="f7086-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="f7086-190">IOptionsSnapshot</span></span>

<span data-ttu-id="f7086-191">*Requiere ASP.NET Core 1.1 o posterior.*</span><span class="sxs-lookup"><span data-stu-id="f7086-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="f7086-192">`IOptionsSnapshot`permite volver a cargar los datos de configuración cuando ha cambiado el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="f7086-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="f7086-193">Tiene una sobrecarga mínima.</span><span class="sxs-lookup"><span data-stu-id="f7086-193">It has minimal overhead.</span></span> <span data-ttu-id="f7086-194">Usar `IOptionsSnapshot` con `reloadOnChange: true`, las opciones están enlazadas a `IConfiguration` y volver a cargar cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="f7086-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="f7086-195">El siguiente ejemplo se muestra cómo un nuevo `IOptionsSnapshot` se crea después *config.json* cambios.</span><span class="sxs-lookup"><span data-stu-id="f7086-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="f7086-196">Las solicitudes realizadas al servidor devolverán la misma hora *config.json* tiene **no** cambiado.</span><span class="sxs-lookup"><span data-stu-id="f7086-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="f7086-197">La primera solicitud después de *config.json* cambios mostrará una nueva hora.</span><span class="sxs-lookup"><span data-stu-id="f7086-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="f7086-198">La siguiente imagen muestra la salida de servidor:</span><span class="sxs-lookup"><span data-stu-id="f7086-198">The following image shows the server output:</span></span>

![que muestra de imagen de explorador "última actualización: 11/22/2016 4:43 PM"](configuration/_static/first.png)

<span data-ttu-id="f7086-200">Actualizar el explorador no cambia el valor de mensaje o el tiempo que se muestra (cuando *config.json* no ha cambiado).</span><span class="sxs-lookup"><span data-stu-id="f7086-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="f7086-201">Cambiar y guardar la *config.json* y, a continuación, actualice el explorador:</span><span class="sxs-lookup"><span data-stu-id="f7086-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![que muestra de imagen de explorador "se actualizó por última vez a e: 11/22/2016 4:53 P.M."](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="f7086-203">Proveedor en memoria y el enlace a una clase POCO</span><span class="sxs-lookup"><span data-stu-id="f7086-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="f7086-204">El ejemplo siguiente muestra cómo usar el proveedor en memoria y enlazar a una clase:</span><span class="sxs-lookup"><span data-stu-id="f7086-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="f7086-205">Valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos.</span><span class="sxs-lookup"><span data-stu-id="f7086-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="f7086-206">Enlace le permite recuperar objetos POCO o gráficos de objetos incluso todo.</span><span class="sxs-lookup"><span data-stu-id="f7086-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="f7086-207">El ejemplo siguiente muestra cómo enlazar a `MyWindow` y utiliza el patrón de opciones con una aplicación de MVC de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f7086-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="f7086-208">Enlazar la clase personalizada de `ConfigureServices` al crear el host:</span><span class="sxs-lookup"><span data-stu-id="f7086-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="f7086-209">Mostrar la configuración de la `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="f7086-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="f7086-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="f7086-210">GetValue</span></span>

<span data-ttu-id="f7086-211">El ejemplo siguiente se muestra la [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensión:</span><span class="sxs-lookup"><span data-stu-id="f7086-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="f7086-212">El ConfigurationBinder `GetValue<T>` método le permite especificar un valor predeterminado (80 en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="f7086-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="f7086-213">`GetValue<T>`es para escenarios sencillos y no se enlaza con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="f7086-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="f7086-214">`GetValue<T>`Obtiene los valores escalares de `GetSection(key).Value` convertir a un tipo específico.</span><span class="sxs-lookup"><span data-stu-id="f7086-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="f7086-215">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="f7086-215">Binding to an object graph</span></span>

<span data-ttu-id="f7086-216">Puede enlazar de forma recursiva para cada objeto en una clase.</span><span class="sxs-lookup"><span data-stu-id="f7086-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="f7086-217">Tenga en cuenta la siguiente `AppOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="f7086-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="f7086-218">El ejemplo siguiente se enlaza a la `AppOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="f7086-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="f7086-219">**Núcleo de ASP.NET 1.1** y versiones posteriores pueden usar `Get<T>`, que funciona con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="f7086-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="f7086-220">`Get<T>`puede ser más convienent que el uso de `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f7086-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="f7086-221">El código siguiente muestra cómo utilizar `Get<T>` con el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="f7086-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="f7086-222">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="f7086-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="f7086-223">El programa mostrará `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="f7086-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="f7086-224">El código siguiente puede utilizarse para la unidad de la configuración de pruebas:</span><span class="sxs-lookup"><span data-stu-id="f7086-224">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="f7086-225">Ejemplo básico de proveedor personalizado de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f7086-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="f7086-226">En esta sección, se crea un proveedor de configuración básica que lee los pares nombre / valor de una base de datos usan EF.</span><span class="sxs-lookup"><span data-stu-id="f7086-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="f7086-227">Definir una `ConfigurationValue` entidad para almacenar los valores de configuración en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="f7086-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="f7086-228">Agregar un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:</span><span class="sxs-lookup"><span data-stu-id="f7086-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="f7086-229">Cree una clase que implemente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="f7086-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="f7086-230">Crear el proveedor de configuración personalizados heredando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="f7086-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="f7086-231">El proveedor de configuración inicializa la base de datos cuando está vacía:</span><span class="sxs-lookup"><span data-stu-id="f7086-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="f7086-232">Cuando se ejecuta el ejemplo, se muestran los valores de resaltado de la base de datos ("value_from_ef_1" y "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="f7086-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="f7086-233">Puede agregar un `EFConfigSource` método de extensión para agregar el origen de configuración:</span><span class="sxs-lookup"><span data-stu-id="f7086-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="f7086-234">El código siguiente muestra cómo utilizar la opción de instalación `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="f7086-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="f7086-235">Tenga en cuenta el ejemplo agrega personalizado `EFConfigProvider` una vez que el proveedor JSON, por lo que cualquier configuración de la base de datos invalidará la configuración de la *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="f7086-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="f7086-236">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="f7086-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="f7086-237">Se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7086-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="f7086-238">Proveedor de configuración de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f7086-238">CommandLine configuration provider</span></span>

<span data-ttu-id="f7086-239">El [proveedor de configuración de la línea de comandos](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recibe pares de clave y valor de argumento de línea de comandos para la configuración en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f7086-239">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="f7086-240">Ver o descargar el ejemplo de configuración de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f7086-240">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a><span data-ttu-id="f7086-241">Configurar el proveedor</span><span class="sxs-lookup"><span data-stu-id="f7086-241">Setting up the provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="f7086-242">Configuración básica</span><span class="sxs-lookup"><span data-stu-id="f7086-242">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="f7086-243">Para activar la configuración de línea de comandos, llame a la `AddCommandLine` método de extensión en una instancia de [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="f7086-243">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="f7086-244">Ejecutar el código, se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="f7086-244">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="f7086-245">Pasar pares de clave y valor de argumento en la línea de comandos cambia los valores de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="f7086-245">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="f7086-246">Muestra la ventana de consola:</span><span class="sxs-lookup"><span data-stu-id="f7086-246">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="f7086-247">Para invalidar la configuración proporcionada por otros proveedores de configuración con la configuración de línea de comandos, llame a `AddCommandLine` último en `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="f7086-247">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f7086-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f7086-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f7086-249">Las aplicaciones de 2.x típicas de ASP.NET Core usan el método estáticos `CreateDefaultBuilder` para crear el host:</span><span class="sxs-lookup"><span data-stu-id="f7086-249">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

<span data-ttu-id="f7086-250">`CreateDefaultBuilder`carga la configuración opcional de *appSettings.JSON que se*, *appsettings. {} Entorno} .json*, [secretos del usuario](xref:security/app-secrets) (en el `Development` entorno), variables de entorno y los argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f7086-250">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="f7086-251">El proveedor de configuración de la línea de comandos es llamado en último lugar.</span><span class="sxs-lookup"><span data-stu-id="f7086-251">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="f7086-252">Llamando al proveedor de última permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución para invalidar la configuración establecida por los otros proveedores de configuración llamado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f7086-252">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="f7086-253">Tenga en cuenta que para *appsettings* archivos que `reloadOnChange` está habilitado.</span><span class="sxs-lookup"><span data-stu-id="f7086-253">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="f7086-254">Argumentos de línea de comandos se invalidan si un valor de configuración coincidente en un *appsettings* archivo se cambia después de la aplicación se inicia.</span><span class="sxs-lookup"><span data-stu-id="f7086-254">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="f7086-255">Como alternativa al uso de la `CreateDefaultBuilder` (método), crear un host con [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) y crear manualmente la configuración con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) es compatible con ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="f7086-255">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="f7086-256">Consulte la ficha de 1.x ASP.NET Core para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="f7086-256">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f7086-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f7086-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f7086-258">Crear un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) y llamar a la `AddCommandLine` método se debe utilizar el proveedor de configuración de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f7086-258">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="f7086-259">Llamando al proveedor de última permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución para invalidar la configuración establecida por los otros proveedores de configuración llamado anteriormente.</span><span class="sxs-lookup"><span data-stu-id="f7086-259">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="f7086-260">Aplicar la configuración de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con el `UseConfiguration` método:</span><span class="sxs-lookup"><span data-stu-id="f7086-260">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="f7086-261">Argumentos</span><span class="sxs-lookup"><span data-stu-id="f7086-261">Arguments</span></span>

<span data-ttu-id="f7086-262">Argumentos que se pasan en la línea de comandos deben ajustarse a uno de los dos formatos que se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="f7086-262">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="f7086-263">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="f7086-263">Argument format</span></span>                                                     | <span data-ttu-id="f7086-264">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="f7086-264">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="f7086-265">Único argumento: un par de clave y valor separados por un signo igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="f7086-265">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="f7086-266">Secuencia de dos argumentos: un par de clave y valor separados por un espacio</span><span class="sxs-lookup"><span data-stu-id="f7086-266">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="f7086-267">**Argumento único**</span><span class="sxs-lookup"><span data-stu-id="f7086-267">**Single argument**</span></span>

<span data-ttu-id="f7086-268">El valor debe seguir un signo igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="f7086-268">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="f7086-269">El valor puede ser null (por ejemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="f7086-269">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="f7086-270">La clave puede tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="f7086-270">The key may have a prefix.</span></span>

| <span data-ttu-id="f7086-271">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="f7086-271">Key prefix</span></span>               | <span data-ttu-id="f7086-272">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="f7086-272">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="f7086-273">Ningún prefijo</span><span class="sxs-lookup"><span data-stu-id="f7086-273">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="f7086-274">Único guión (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="f7086-274">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="f7086-275">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="f7086-275">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="f7086-276">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="f7086-276">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="f7086-277">&#8224; Una clave con un prefijo único guión (`-`) deben indicarse en [cambiar las asignaciones de](#switch-mappings), que se describen a continuación.</span><span class="sxs-lookup"><span data-stu-id="f7086-277">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="f7086-278">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f7086-278">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="f7086-279">Nota: Si `-key1` no está presente en el [cambiar las asignaciones de](#switch-mappings) dado al proveedor de configuración, un `FormatException` se produce.</span><span class="sxs-lookup"><span data-stu-id="f7086-279">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="f7086-280">**Secuencia de dos argumentos**</span><span class="sxs-lookup"><span data-stu-id="f7086-280">**Sequence of two arguments**</span></span>

<span data-ttu-id="f7086-281">El valor no puede ser nulo y debe seguir la clave separada por un espacio.</span><span class="sxs-lookup"><span data-stu-id="f7086-281">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="f7086-282">La clave debe tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="f7086-282">The key must have a prefix.</span></span>

| <span data-ttu-id="f7086-283">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="f7086-283">Key prefix</span></span>               | <span data-ttu-id="f7086-284">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="f7086-284">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="f7086-285">Único guión (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="f7086-285">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="f7086-286">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="f7086-286">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="f7086-287">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="f7086-287">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="f7086-288">&#8224; Una clave con un prefijo único guión (`-`) deben indicarse en [cambiar las asignaciones de](#switch-mappings), que se describen a continuación.</span><span class="sxs-lookup"><span data-stu-id="f7086-288">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="f7086-289">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f7086-289">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="f7086-290">Nota: Si `-key1` no está presente en el [cambiar las asignaciones de](#switch-mappings) dado al proveedor de configuración, un `FormatException` se produce.</span><span class="sxs-lookup"><span data-stu-id="f7086-290">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="f7086-291">Claves duplicadas</span><span class="sxs-lookup"><span data-stu-id="f7086-291">Duplicate keys</span></span>

<span data-ttu-id="f7086-292">Si se proporcionan las claves duplicadas, se utiliza el último par de clave-valor.</span><span class="sxs-lookup"><span data-stu-id="f7086-292">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="f7086-293">Cambiar las asignaciones</span><span class="sxs-lookup"><span data-stu-id="f7086-293">Switch mappings</span></span>

<span data-ttu-id="f7086-294">Cuando se compila manualmente la configuración con `ConfigurationBuilder`, también puede proporcionar un diccionario de asignaciones de conmutador para la `AddCommandLine` método.</span><span class="sxs-lookup"><span data-stu-id="f7086-294">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="f7086-295">Asignaciones de conmutador permiten proporcionar lógica de sustitución de nombre de clave.</span><span class="sxs-lookup"><span data-stu-id="f7086-295">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="f7086-296">Cuando se utiliza el diccionario de asignaciones de conmutador, se comprueba el diccionario para una clave que coincida con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f7086-296">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="f7086-297">Si la clave de la línea de comandos se encuentra en el diccionario, se pasa el valor del diccionario (sustitución de claves) atrás para establecer la configuración.</span><span class="sxs-lookup"><span data-stu-id="f7086-297">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="f7086-298">Se requiere para cualquier clave de línea de comandos como precedida un guion solo una asignación de conmutador (`-`).</span><span class="sxs-lookup"><span data-stu-id="f7086-298">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="f7086-299">Cambiar las reglas de clave de diccionario de asignaciones:</span><span class="sxs-lookup"><span data-stu-id="f7086-299">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="f7086-300">Conmutadores deben empezar por un guión (`-`) o guión doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="f7086-300">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="f7086-301">El diccionario de asignaciones del conmutador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="f7086-301">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="f7086-302">En el ejemplo siguiente, la `GetSwitchMappings` método permite a los argumentos de línea de comandos usar un solo guión (`-`) la clave de prefijo y evitar prefijos subclaves iniciales.</span><span class="sxs-lookup"><span data-stu-id="f7086-302">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="f7086-303">Sin proporcionar argumentos de línea de comandos, el diccionario proporcionadas para `AddInMemoryCollection` establece los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="f7086-303">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="f7086-304">Ejecute la aplicación con el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7086-304">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="f7086-305">Muestra la ventana de consola:</span><span class="sxs-lookup"><span data-stu-id="f7086-305">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="f7086-306">Para pasar valores de configuración, utilice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7086-306">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="f7086-307">Muestra la ventana de consola:</span><span class="sxs-lookup"><span data-stu-id="f7086-307">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="f7086-308">Después de crea el diccionario de asignaciones de conmutador, que contiene los datos que se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="f7086-308">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="f7086-309">Key</span><span class="sxs-lookup"><span data-stu-id="f7086-309">Key</span></span>            | <span data-ttu-id="f7086-310">Valor</span><span class="sxs-lookup"><span data-stu-id="f7086-310">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="f7086-311">Para mostrar el cambio de clave utilizando el diccionario, ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="f7086-311">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="f7086-312">Se intercambian las claves de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f7086-312">The command-line keys are swapped.</span></span> <span data-ttu-id="f7086-313">La ventana de consola muestra los valores de configuración de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="f7086-313">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="f7086-314">El archivo web.config</span><span class="sxs-lookup"><span data-stu-id="f7086-314">The web.config file</span></span>

<span data-ttu-id="f7086-315">A *web.config* archivo es necesario cuando se hospeda la aplicación en IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="f7086-315">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="f7086-316">*Web.config* activa el AspNetCoreModule en IIS para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f7086-316">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="f7086-317">Configuración de *web.config* habilitar la AspNetCoreModule en IIS para que inicie la aplicación y configurar otros módulos y la configuración de IIS.</span><span class="sxs-lookup"><span data-stu-id="f7086-317">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="f7086-318">Si usa Visual Studio y eliminar *web.config*, Visual Studio creará uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="f7086-318">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="f7086-319">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="f7086-319">Additional notes</span></span>

* <span data-ttu-id="f7086-320">Inyección de dependencia (DI) no se establece hasta después de `ConfigureServices` se invoca.</span><span class="sxs-lookup"><span data-stu-id="f7086-320">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="f7086-321">El sistema de configuración no es compatible con DI.</span><span class="sxs-lookup"><span data-stu-id="f7086-321">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="f7086-322">`IConfiguration`tiene dos especializaciones:</span><span class="sxs-lookup"><span data-stu-id="f7086-322">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="f7086-323">`IConfigurationRoot`Se usa para el nodo raíz.</span><span class="sxs-lookup"><span data-stu-id="f7086-323">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="f7086-324">Puede desencadenar una recarga.</span><span class="sxs-lookup"><span data-stu-id="f7086-324">Can trigger a reload.</span></span>
  * <span data-ttu-id="f7086-325">`IConfigurationSection`Representa una sección de valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="f7086-325">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="f7086-326">El `GetSection` y `GetChildren` métodos devuelven un `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="f7086-326">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7086-327">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f7086-327">Additional resources</span></span>

* [<span data-ttu-id="f7086-328">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="f7086-328">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="f7086-329">Almacenamiento seguro de secretos de aplicación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="f7086-329">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="f7086-330">Hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7086-330">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="f7086-331">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="f7086-331">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="f7086-332">Proveedor de configuración de Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f7086-332">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
