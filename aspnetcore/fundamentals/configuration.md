---
title: "Configuración de ASP.NET Core"
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
ms.openlocfilehash: 39e76b14af85de34b8443bf4e04d18d13ad2aa90
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/08/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a><span data-ttu-id="22dd7-104">Configuración de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22dd7-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="22dd7-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="22dd7-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="22dd7-106">La API de configuración proporciona una manera de configurar una aplicación basada en una lista de pares nombre / valor.</span><span class="sxs-lookup"><span data-stu-id="22dd7-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="22dd7-107">Se lee la configuración en tiempo de ejecución de varios orígenes.</span><span class="sxs-lookup"><span data-stu-id="22dd7-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="22dd7-108">Los pares de nombre-valor se pueden agrupar en una jerarquía multinivel.</span><span class="sxs-lookup"><span data-stu-id="22dd7-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="22dd7-109">Existen proveedores de configuración para:</span><span class="sxs-lookup"><span data-stu-id="22dd7-109">There are configuration providers for:</span></span>

* <span data-ttu-id="22dd7-110">Formatos de archivo (INI, JSON y XML)</span><span class="sxs-lookup"><span data-stu-id="22dd7-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="22dd7-111">Argumentos de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="22dd7-111">Command-line arguments</span></span>
* <span data-ttu-id="22dd7-112">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="22dd7-112">Environment variables</span></span>
* <span data-ttu-id="22dd7-113">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="22dd7-113">In-memory .NET objects</span></span>
* <span data-ttu-id="22dd7-114">Un almacén de usuario cifrado</span><span class="sxs-lookup"><span data-stu-id="22dd7-114">An encrypted user store</span></span>
* [<span data-ttu-id="22dd7-115">Almacén de claves de Azure</span><span class="sxs-lookup"><span data-stu-id="22dd7-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="22dd7-116">Proveedores personalizados, que se instale o cree</span><span class="sxs-lookup"><span data-stu-id="22dd7-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="22dd7-117">Cada valor de configuración se asigna a una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="22dd7-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="22dd7-118">No hay compatibilidad de enlace integrado para deserializar la configuración en un personalizado [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (una clase .NET simple con propiedades).</span><span class="sxs-lookup"><span data-stu-id="22dd7-118">There's built-in binding support to deserialize settings into a custom [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="22dd7-119">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="22dd7-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="22dd7-120">Muestra una configuración sencilla</span><span class="sxs-lookup"><span data-stu-id="22dd7-120">Simple configuration</span></span>

<span data-ttu-id="22dd7-121">La siguiente aplicación de consola utiliza el proveedor de configuración de JSON:</span><span class="sxs-lookup"><span data-stu-id="22dd7-121">The following console app uses the JSON configuration provider:</span></span>

<span data-ttu-id="22dd7-122">[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-122">[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]</span></span>

<span data-ttu-id="22dd7-123">La aplicación lee y muestra los valores de configuración siguientes:</span><span class="sxs-lookup"><span data-stu-id="22dd7-123">The app reads and displays the following configuration settings:</span></span>

<span data-ttu-id="22dd7-124">[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-124">[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]</span></span>

<span data-ttu-id="22dd7-125">Configuración consta de una lista jerárquica de los pares de nombre / valor en el que los nodos están separados por un coma.</span><span class="sxs-lookup"><span data-stu-id="22dd7-125">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="22dd7-126">Para recuperar un valor, tener acceso a la `Configuration` indizador con la correspondiente clave del elemento:</span><span class="sxs-lookup"><span data-stu-id="22dd7-126">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="22dd7-127">Para trabajar con matrices en orígenes de configuración con formato JSON, usar un índice de matriz como parte de la cadena separada por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="22dd7-127">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="22dd7-128">En el ejemplo siguiente se obtiene el nombre del primer elemento en el anterior `wizards` matriz:</span><span class="sxs-lookup"><span data-stu-id="22dd7-128">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="22dd7-129">Pares de nombre / valor que se escriben en integrado en `Configuration` proveedores son **no** es persistente, sin embargo, puede crear un proveedor personalizado que guarda los valores.</span><span class="sxs-lookup"><span data-stu-id="22dd7-129">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="22dd7-130">Vea [proveedor de configuración personalizada](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="22dd7-130">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="22dd7-131">En el ejemplo anterior utiliza el indizador de configuración para leer los valores.</span><span class="sxs-lookup"><span data-stu-id="22dd7-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="22dd7-132">Acceso a la configuración fuera de `Startup`, use la [patrón opciones](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="22dd7-132">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="22dd7-133">El *patrón opciones* se muestra más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="22dd7-133">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="22dd7-134">Es habitual tener distintos valores de configuración para los entornos diferentes, por ejemplo, desarrollo, prueba y producción.</span><span class="sxs-lookup"><span data-stu-id="22dd7-134">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="22dd7-135">El `CreateDefaultBuilder` método de extensión en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y sistema de orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="22dd7-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="22dd7-136">*appSettings.JSON que se*</span><span class="sxs-lookup"><span data-stu-id="22dd7-136">*appsettings.json*</span></span>
* <span data-ttu-id="22dd7-137">* appsettings. \<EnvironmentName > .json</span><span class="sxs-lookup"><span data-stu-id="22dd7-137">*appsettings.\<EnvironmentName>.json</span></span>
* <span data-ttu-id="22dd7-138">variables de entorno</span><span class="sxs-lookup"><span data-stu-id="22dd7-138">environment variables</span></span>

<span data-ttu-id="22dd7-139">Vea [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="22dd7-139">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="22dd7-140">`reloadOnChange`solo se admite en ASP.NET Core 1.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="22dd7-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="22dd7-141">Orígenes de configuración se leen en el orden en que se especifican.</span><span class="sxs-lookup"><span data-stu-id="22dd7-141">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="22dd7-142">En el código anterior, las variables de entorno se leen de la última.</span><span class="sxs-lookup"><span data-stu-id="22dd7-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="22dd7-143">Los valores de configuración establecido a través del entorno podría reemplazarlas que se establecen en los dos proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="22dd7-143">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="22dd7-144">El entorno se establece normalmente en uno de `Development`, `Staging`, o `Production`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-144">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="22dd7-145">Vea [trabajar con varios entornos](environments.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="22dd7-145">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="22dd7-146">Consideraciones sobre la configuración:</span><span class="sxs-lookup"><span data-stu-id="22dd7-146">Configuration considerations:</span></span>

* <span data-ttu-id="22dd7-147">`IOptionsSnapshot`puede volver a cargar los datos de configuración, cuando el estado cambia.</span><span class="sxs-lookup"><span data-stu-id="22dd7-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="22dd7-148">Use `IOptionsSnapshot` si tiene que volver a cargar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="22dd7-148">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="22dd7-149">Vea [IOptionsSnapshot](#ioptionssnapshot) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="22dd7-149">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="22dd7-150">Claves de configuración que distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="22dd7-150">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="22dd7-151">Una práctica recomendada es especificar variables de entorno por último, para que el entorno local puedan reemplazar todo lo establecido en los archivos de configuración implementada.</span><span class="sxs-lookup"><span data-stu-id="22dd7-151">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="22dd7-152">**Nunca** almacenar contraseñas u otros datos confidenciales en el código de proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="22dd7-152">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="22dd7-153">No utilice secretos de producción en el desarrollo o entornos de prueba.</span><span class="sxs-lookup"><span data-stu-id="22dd7-153">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="22dd7-154">En su lugar, especifique secretos fuera del árbol del proyecto, por lo que no se pueden confirmar accidentalmente en el repositorio.</span><span class="sxs-lookup"><span data-stu-id="22dd7-154">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="22dd7-155">Obtenga más información sobre [trabajar con varios entornos](environments.md) y administrar [almacenamiento seguro para la ejecución de secretos de aplicación durante el desarrollo](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="22dd7-155">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="22dd7-156">Si `:` no se puede utilizar en variables de entorno en el sistema, reemplace `:` con `__` (subrayado doble).</span><span class="sxs-lookup"><span data-stu-id="22dd7-156">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="22dd7-157">Uso de opciones y objetos de configuración</span><span class="sxs-lookup"><span data-stu-id="22dd7-157">Using Options and configuration objects</span></span>

<span data-ttu-id="22dd7-158">El patrón de opciones usa las clases de opciones personalizadas para representar un grupo de valores relacionados.</span><span class="sxs-lookup"><span data-stu-id="22dd7-158">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="22dd7-159">Se recomienda crear clases desacopladas para cada característica dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22dd7-159">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="22dd7-160">Clases desacopladas siguen:</span><span class="sxs-lookup"><span data-stu-id="22dd7-160">Decoupled classes follow:</span></span>

* <span data-ttu-id="22dd7-161">El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/) : clases dependen únicamente de los valores de configuración que se usan.</span><span class="sxs-lookup"><span data-stu-id="22dd7-161">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="22dd7-162">[Separación de intereses](http://deviq.com/separation-of-concerns/) : configuración para las distintas partes de la aplicación no es dependientes o acoplamiento entre sí.</span><span class="sxs-lookup"><span data-stu-id="22dd7-162">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="22dd7-163">La clase de opciones debe ser no abstracto con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="22dd7-163">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="22dd7-164">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-164">For example:</span></span>

<span data-ttu-id="22dd7-165">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-165">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]</span></span>

<a name=options-example></a>

<span data-ttu-id="22dd7-166">En el código siguiente, se habilita el proveedor de configuración de JSON.</span><span class="sxs-lookup"><span data-stu-id="22dd7-166">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="22dd7-167">La `MyOptions` clase se agrega al contenedor de servicios y se enlaza a la configuración.</span><span class="sxs-lookup"><span data-stu-id="22dd7-167">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

<span data-ttu-id="22dd7-168">[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-168">[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]</span></span>

<span data-ttu-id="22dd7-169">El siguiente [controlador](../mvc/controllers/index.md) utiliza [constructor inyección de dependencia](xref:fundamentals/dependency-injection#what-is-dependency-injection) en [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) acceso a la configuración:</span><span class="sxs-lookup"><span data-stu-id="22dd7-169">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

<span data-ttu-id="22dd7-170">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-170">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="22dd7-171">Con el siguiente *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-171">With the following *appsettings.json* file:</span></span>

<span data-ttu-id="22dd7-172">[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-172">[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]</span></span>

<span data-ttu-id="22dd7-173">El `HomeController.Index` método `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-173">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="22dd7-174">Las aplicaciones típicas no enlazar toda la configuración a un archivo de opciones de inicio único.</span><span class="sxs-lookup"><span data-stu-id="22dd7-174">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="22dd7-175">Más adelante voy a explicar cómo usar `GetSection` para enlazar a una sección.</span><span class="sxs-lookup"><span data-stu-id="22dd7-175">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="22dd7-176">En el código siguiente, un segundo `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="22dd7-176">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="22dd7-177">Usa un delegado para configurar el enlace con `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-177">It uses a delegate to configure the binding with `MyOptions`.</span></span>

<span data-ttu-id="22dd7-178">[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-178">[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span></span>

<span data-ttu-id="22dd7-179">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="22dd7-179">You can add multiple configuration providers.</span></span> <span data-ttu-id="22dd7-180">Proveedores de configuración están disponibles en paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="22dd7-180">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="22dd7-181">Se aplican en orden que están registrados.</span><span class="sxs-lookup"><span data-stu-id="22dd7-181">They are applied in order they are registered.</span></span>

<span data-ttu-id="22dd7-182">Cada llamada a `Configure<TOptions>` agrega un `IConfigureOptions<TOptions>` servicio al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="22dd7-182">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="22dd7-183">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.JSON que se* --pero el valor de `Option1` se reemplaza por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="22dd7-183">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="22dd7-184">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificado "gana" (que establece el valor de configuración).</span><span class="sxs-lookup"><span data-stu-id="22dd7-184">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="22dd7-185">En el código anterior, el `HomeController.Index` método `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-185">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="22dd7-186">Al enlazar opciones de configuración, cada propiedad en el tipo de opciones está enlazado a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-186">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="22dd7-187">Por ejemplo, el `MyOptions.Option1` propiedad se enlaza a la clave `Option1`, que se lee desde el `option1` propiedad en *appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="22dd7-187">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="22dd7-188">Más adelante en este artículo, se muestra un ejemplo de la subpropiedad.</span><span class="sxs-lookup"><span data-stu-id="22dd7-188">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="22dd7-189">En el código siguiente, una tercera `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="22dd7-189">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="22dd7-190">Enlaza `MySubOptions` a la sección `subsection` de la *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-190">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

<span data-ttu-id="22dd7-191">[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-191">[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span></span>

<span data-ttu-id="22dd7-192">Nota: Este método de extensión requiere el `Microsoft.Extensions.Options.ConfigurationExtensions` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="22dd7-192">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="22dd7-193">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-193">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="22dd7-194">[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-194">[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]</span></span>

<span data-ttu-id="22dd7-195">La `MySubOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="22dd7-195">The `MySubOptions` class:</span></span>

<span data-ttu-id="22dd7-196">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-196">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]</span></span>

<span data-ttu-id="22dd7-197">Con los siguientes `Controller`:</span><span class="sxs-lookup"><span data-stu-id="22dd7-197">With the following `Controller`:</span></span>

<span data-ttu-id="22dd7-198">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-198">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span></span>

<span data-ttu-id="22dd7-199">`subOption1 = subvalue1_from_json, subOption2 = 200`se devuelve.</span><span class="sxs-lookup"><span data-stu-id="22dd7-199">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="22dd7-200">También puede proporcionar opciones en un modelo de vista o insertar `IOptions<TOptions>` directamente en una vista:</span><span class="sxs-lookup"><span data-stu-id="22dd7-200">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

<span data-ttu-id="22dd7-201">[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-201">[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]</span></span>

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="22dd7-202">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="22dd7-202">IOptionsSnapshot</span></span>

<span data-ttu-id="22dd7-203">*Requiere ASP.NET Core 1.1 o posterior.*</span><span class="sxs-lookup"><span data-stu-id="22dd7-203">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="22dd7-204">`IOptionsSnapshot`permite volver a cargar los datos de configuración cuando ha cambiado el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="22dd7-204">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="22dd7-205">Tiene una sobrecarga mínima.</span><span class="sxs-lookup"><span data-stu-id="22dd7-205">It has minimal overhead.</span></span> <span data-ttu-id="22dd7-206">Usar `IOptionsSnapshot` con `reloadOnChange: true`, las opciones están enlazadas a `IConfiguration` y volver a cargar cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="22dd7-206">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="22dd7-207">El siguiente ejemplo se muestra cómo un nuevo `IOptionsSnapshot` se crea después *config.json* cambios.</span><span class="sxs-lookup"><span data-stu-id="22dd7-207">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="22dd7-208">Las solicitudes realizadas al servidor devolverán la misma hora *config.json* tiene **no** cambiado.</span><span class="sxs-lookup"><span data-stu-id="22dd7-208">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="22dd7-209">La primera solicitud después de *config.json* cambios mostrará una nueva hora.</span><span class="sxs-lookup"><span data-stu-id="22dd7-209">The first request after *config.json* changes will show a new time.</span></span>

<span data-ttu-id="22dd7-210">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-210">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span></span>

<span data-ttu-id="22dd7-211">La siguiente imagen muestra la salida de servidor:</span><span class="sxs-lookup"><span data-stu-id="22dd7-211">The following image shows the server output:</span></span>

![que muestra de imagen de explorador "última actualización: 11/22/2016 4:43 PM"](configuration/_static/first.png)

<span data-ttu-id="22dd7-213">Actualizar el explorador no cambia el valor de mensaje o el tiempo que se muestra (cuando *config.json* no ha cambiado).</span><span class="sxs-lookup"><span data-stu-id="22dd7-213">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="22dd7-214">Cambiar y guardar la *config.json* y, a continuación, actualice el explorador:</span><span class="sxs-lookup"><span data-stu-id="22dd7-214">Change and save the  *config.json* and then refresh the browser:</span></span>

![que muestra de imagen de explorador "se actualizó por última vez a e: 11/22/2016 4:53 P.M."](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="22dd7-216">Proveedor en memoria y el enlace a una clase POCO</span><span class="sxs-lookup"><span data-stu-id="22dd7-216">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="22dd7-217">El ejemplo siguiente muestra cómo usar el proveedor en memoria y enlazar a una clase:</span><span class="sxs-lookup"><span data-stu-id="22dd7-217">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

<span data-ttu-id="22dd7-218">[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-218">[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]</span></span>

<span data-ttu-id="22dd7-219">Valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos.</span><span class="sxs-lookup"><span data-stu-id="22dd7-219">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="22dd7-220">Enlace le permite recuperar objetos POCO o gráficos de objetos incluso todo.</span><span class="sxs-lookup"><span data-stu-id="22dd7-220">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="22dd7-221">El ejemplo siguiente muestra cómo enlazar a `MyWindow` y utiliza el patrón de opciones con una aplicación de MVC de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="22dd7-221">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

<span data-ttu-id="22dd7-222">[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-222">[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]</span></span>

<span data-ttu-id="22dd7-223">[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-223">[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]</span></span>

<span data-ttu-id="22dd7-224">Enlazar la clase personalizada de `ConfigureServices` al crear el host:</span><span class="sxs-lookup"><span data-stu-id="22dd7-224">Bind the custom class in `ConfigureServices` when building the host:</span></span>

<span data-ttu-id="22dd7-225">[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-225">[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]</span></span>

<span data-ttu-id="22dd7-226">Mostrar la configuración de la `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="22dd7-226">Display the settings from the `HomeController`:</span></span>

<span data-ttu-id="22dd7-227">[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-227">[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]</span></span>

### <a name="getvalue"></a><span data-ttu-id="22dd7-228">GetValue</span><span class="sxs-lookup"><span data-stu-id="22dd7-228">GetValue</span></span>

<span data-ttu-id="22dd7-229">El ejemplo siguiente se muestra la [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensión:</span><span class="sxs-lookup"><span data-stu-id="22dd7-229">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

<span data-ttu-id="22dd7-230">[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-230">[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]</span></span>

<span data-ttu-id="22dd7-231">El ConfigurationBinder `GetValue<T>` método le permite especificar un valor predeterminado (80 en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="22dd7-231">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="22dd7-232">`GetValue<T>`es para escenarios sencillos y no se enlaza con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="22dd7-232">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="22dd7-233">`GetValue<T>`Obtiene los valores escalares de `GetSection(key).Value` convertir a un tipo específico.</span><span class="sxs-lookup"><span data-stu-id="22dd7-233">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="22dd7-234">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="22dd7-234">Binding to an object graph</span></span>

<span data-ttu-id="22dd7-235">Puede enlazar de forma recursiva para cada objeto en una clase.</span><span class="sxs-lookup"><span data-stu-id="22dd7-235">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="22dd7-236">Tenga en cuenta la siguiente `AppOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="22dd7-236">Consider the following `AppOptions` class:</span></span>

<span data-ttu-id="22dd7-237">[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-237">[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]</span></span>

<span data-ttu-id="22dd7-238">El ejemplo siguiente se enlaza a la `AppOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="22dd7-238">The following sample binds to the `AppOptions` class:</span></span>

<span data-ttu-id="22dd7-239">[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-239">[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]</span></span>

<span data-ttu-id="22dd7-240">**Núcleo de ASP.NET 1.1** y versiones posteriores pueden usar `Get<T>`, que funciona con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="22dd7-240">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="22dd7-241">`Get<T>`puede ser más convienent que el uso de `Bind`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-241">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="22dd7-242">El código siguiente muestra cómo utilizar `Get<T>` con el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="22dd7-242">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="22dd7-243">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-243">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="22dd7-244">[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-244">[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]</span></span>

<span data-ttu-id="22dd7-245">El programa mostrará `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-245">The program displays `Height 11`.</span></span>

<span data-ttu-id="22dd7-246">El código siguiente puede utilizarse para la unidad de la configuración de pruebas:</span><span class="sxs-lookup"><span data-stu-id="22dd7-246">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="22dd7-247">Ejemplo básico de proveedor personalizado de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="22dd7-247">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="22dd7-248">En esta sección, se crea un proveedor de configuración básica que lee los pares nombre / valor de una base de datos usan EF.</span><span class="sxs-lookup"><span data-stu-id="22dd7-248">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="22dd7-249">Definir una `ConfigurationValue` entidad para almacenar los valores de configuración en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="22dd7-249">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

<span data-ttu-id="22dd7-250">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-250">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]</span></span>

<span data-ttu-id="22dd7-251">Agregar un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:</span><span class="sxs-lookup"><span data-stu-id="22dd7-251">Add a `ConfigurationContext` to store and access the configured values:</span></span>

<span data-ttu-id="22dd7-252">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-252">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span></span>

<span data-ttu-id="22dd7-253">Cree una clase que implemente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="22dd7-253">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

<span data-ttu-id="22dd7-254">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-254">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span></span>

<span data-ttu-id="22dd7-255">Crear el proveedor de configuración personalizados heredando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="22dd7-255">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="22dd7-256">El proveedor de configuración inicializa la base de datos cuando está vacía:</span><span class="sxs-lookup"><span data-stu-id="22dd7-256">The configuration provider initializes the database when it's empty:</span></span>

<span data-ttu-id="22dd7-257">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-257">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span></span>

<span data-ttu-id="22dd7-258">Cuando se ejecuta el ejemplo, se muestran los valores de resaltado de la base de datos ("value_from_ef_1" y "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="22dd7-258">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="22dd7-259">Puede agregar un `EFConfigSource` método de extensión para agregar el origen de configuración:</span><span class="sxs-lookup"><span data-stu-id="22dd7-259">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

<span data-ttu-id="22dd7-260">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-260">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span></span>

<span data-ttu-id="22dd7-261">El código siguiente muestra cómo utilizar la opción de instalación `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="22dd7-261">The following code shows how to use the custom `EFConfigProvider`:</span></span>

<span data-ttu-id="22dd7-262">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-262">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]</span></span>

<span data-ttu-id="22dd7-263">Tenga en cuenta el ejemplo agrega personalizado `EFConfigProvider` una vez que el proveedor JSON, por lo que cualquier configuración de la base de datos invalidará la configuración de la *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="22dd7-263">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="22dd7-264">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-264">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="22dd7-265">[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-265">[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]</span></span>

<span data-ttu-id="22dd7-266">Se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22dd7-266">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="22dd7-267">Proveedor de configuración de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="22dd7-267">CommandLine configuration provider</span></span>

<span data-ttu-id="22dd7-268">El ejemplo siguiente habilita al proveedor de configuración de la línea de comandos de última:</span><span class="sxs-lookup"><span data-stu-id="22dd7-268">The following sample enables the CommandLine configuration provider last:</span></span>

<span data-ttu-id="22dd7-269">[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="22dd7-269">[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]</span></span>

<span data-ttu-id="22dd7-270">Para pasar valores de configuración, utilice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="22dd7-270">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="22dd7-271">Que muestra:</span><span class="sxs-lookup"><span data-stu-id="22dd7-271">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="22dd7-272">El `GetSwitchMappings` método le permite usar `-` en lugar de `/` y elimina los prefijos de subclaves iniciales.</span><span class="sxs-lookup"><span data-stu-id="22dd7-272">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="22dd7-273">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-273">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="22dd7-274">Muestra:</span><span class="sxs-lookup"><span data-stu-id="22dd7-274">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="22dd7-275">Argumentos de línea de comandos deben incluir un valor (puede ser null).</span><span class="sxs-lookup"><span data-stu-id="22dd7-275">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="22dd7-276">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="22dd7-276">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="22dd7-277">Es correcto, pero</span><span class="sxs-lookup"><span data-stu-id="22dd7-277">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="22dd7-278">se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="22dd7-278">results in an exception.</span></span> <span data-ttu-id="22dd7-279">Si especifica un prefijo de modificador de línea de comandos de - o bien para que no hay ninguna asignación de conmutador correspondiente, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="22dd7-279">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="22dd7-280">El archivo web.config</span><span class="sxs-lookup"><span data-stu-id="22dd7-280">The web.config file</span></span>

<span data-ttu-id="22dd7-281">A *web.config* archivo es necesario cuando se hospeda la aplicación en IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="22dd7-281">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="22dd7-282">*Web.config* activa el AspNetCoreModule en IIS para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="22dd7-282">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="22dd7-283">Configuración de *web.config* habilitar la AspNetCoreModule en IIS para que inicie la aplicación y configurar otros módulos y la configuración de IIS.</span><span class="sxs-lookup"><span data-stu-id="22dd7-283">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="22dd7-284">Si usa Visual Studio y eliminar *web.config*, Visual Studio creará uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="22dd7-284">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="22dd7-285">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="22dd7-285">Additional notes</span></span>

* <span data-ttu-id="22dd7-286">Inyección de dependencia (DI) no se establece hasta después de `ConfigureServices` se invoca.</span><span class="sxs-lookup"><span data-stu-id="22dd7-286">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="22dd7-287">El sistema de configuración no es compatible con DI.</span><span class="sxs-lookup"><span data-stu-id="22dd7-287">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="22dd7-288">`IConfiguration`tiene dos especializaciones:</span><span class="sxs-lookup"><span data-stu-id="22dd7-288">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="22dd7-289">`IConfigurationRoot`Se usa para el nodo raíz.</span><span class="sxs-lookup"><span data-stu-id="22dd7-289">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="22dd7-290">Puede desencadenar una recarga.</span><span class="sxs-lookup"><span data-stu-id="22dd7-290">Can trigger a reload.</span></span>
  * <span data-ttu-id="22dd7-291">`IConfigurationSection`Representa una sección de valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="22dd7-291">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="22dd7-292">El `GetSection` y `GetChildren` métodos devuelven un `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="22dd7-292">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="22dd7-293">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="22dd7-293">Additional resources</span></span>

* [<span data-ttu-id="22dd7-294">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="22dd7-294">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="22dd7-295">Ubicación de almacenamiento segura de secretos de aplicación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="22dd7-295">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="22dd7-296">Inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="22dd7-296">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="22dd7-297">Proveedor de configuración de almacén de claves de Azure</span><span class="sxs-lookup"><span data-stu-id="22dd7-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
