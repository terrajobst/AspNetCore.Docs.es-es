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
ms.openlocfilehash: dae7ac6e377d2c17bc8f86e5b6da98107366cc73
ms.sourcegitcommit: 418e6aa4ab79474ecc4d0a6af573a3759b113fe4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a><span data-ttu-id="05c21-104">Configuración de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05c21-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="05c21-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), y [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="05c21-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](http://ardalis.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="05c21-106">La API de configuración proporciona una manera de configurar una aplicación basada en una lista de pares nombre / valor.</span><span class="sxs-lookup"><span data-stu-id="05c21-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="05c21-107">Se lee la configuración en tiempo de ejecución de varios orígenes.</span><span class="sxs-lookup"><span data-stu-id="05c21-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="05c21-108">Los pares de nombre-valor se pueden agrupar en una jerarquía multinivel.</span><span class="sxs-lookup"><span data-stu-id="05c21-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="05c21-109">Existen proveedores de configuración para:</span><span class="sxs-lookup"><span data-stu-id="05c21-109">There are configuration providers for:</span></span>

* <span data-ttu-id="05c21-110">Formatos de archivo (INI, JSON y XML)</span><span class="sxs-lookup"><span data-stu-id="05c21-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="05c21-111">Argumentos de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="05c21-111">Command-line arguments</span></span>
* <span data-ttu-id="05c21-112">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="05c21-112">Environment variables</span></span>
* <span data-ttu-id="05c21-113">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="05c21-113">In-memory .NET objects</span></span>
* <span data-ttu-id="05c21-114">Un almacén de usuario cifrado</span><span class="sxs-lookup"><span data-stu-id="05c21-114">An encrypted user store</span></span>
* [<span data-ttu-id="05c21-115">Almacén de claves de Azure</span><span class="sxs-lookup"><span data-stu-id="05c21-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="05c21-116">Proveedores personalizados, que se instale o cree</span><span class="sxs-lookup"><span data-stu-id="05c21-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="05c21-117">Cada valor de configuración se asigna a una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="05c21-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="05c21-118">No hay compatibilidad de enlace integrado para deserializar la configuración en un personalizado [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (una clase .NET simple con propiedades).</span><span class="sxs-lookup"><span data-stu-id="05c21-118">There's built-in binding support to deserialize settings into a custom [POCO](https://en.wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="05c21-119">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="05c21-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="05c21-120">Muestra una configuración sencilla</span><span class="sxs-lookup"><span data-stu-id="05c21-120">Simple configuration</span></span>

<span data-ttu-id="05c21-121">La siguiente aplicación de consola utiliza el proveedor de configuración de JSON:</span><span class="sxs-lookup"><span data-stu-id="05c21-121">The following console app uses the JSON configuration provider:</span></span>

<span data-ttu-id="05c21-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-122">[!code-csharp[Main](configuration/sample/src/ConfigJson/Program.cs)]</span></span>

<span data-ttu-id="05c21-123">La aplicación lee y muestra los valores de configuración siguientes:</span><span class="sxs-lookup"><span data-stu-id="05c21-123">The app reads and displays the following configuration settings:</span></span>

<span data-ttu-id="05c21-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="05c21-124">[!code-json[Main](configuration/sample/src/ConfigJson/appsettings.json)]</span></span>

<span data-ttu-id="05c21-125">Configuración consta de una lista jerárquica de los pares de nombre / valor en el que los nodos están separados por un coma.</span><span class="sxs-lookup"><span data-stu-id="05c21-125">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="05c21-126">Para recuperar un valor, tener acceso a la `Configuration` indizador con la correspondiente clave del elemento:</span><span class="sxs-lookup"><span data-stu-id="05c21-126">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="05c21-127">Para trabajar con matrices en orígenes de configuración con formato JSON, usar un índice de matriz como parte de la cadena separada por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="05c21-127">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="05c21-128">En el ejemplo siguiente se obtiene el nombre del primer elemento en el anterior `wizards` matriz:</span><span class="sxs-lookup"><span data-stu-id="05c21-128">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="05c21-129">Pares de nombre / valor que se escriben en integrado en `Configuration` proveedores son **no** es persistente, sin embargo, puede crear un proveedor personalizado que guarda los valores.</span><span class="sxs-lookup"><span data-stu-id="05c21-129">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="05c21-130">Vea [proveedor de configuración personalizada](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="05c21-130">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="05c21-131">En el ejemplo anterior utiliza el indizador de configuración para leer los valores.</span><span class="sxs-lookup"><span data-stu-id="05c21-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="05c21-132">Acceso a la configuración fuera de `Startup`, use la [patrón opciones](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="05c21-132">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="05c21-133">El *patrón opciones* se muestra más adelante en este artículo.</span><span class="sxs-lookup"><span data-stu-id="05c21-133">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="05c21-134">Es habitual tener distintos valores de configuración para los entornos diferentes, por ejemplo, desarrollo, prueba y producción.</span><span class="sxs-lookup"><span data-stu-id="05c21-134">It's typical to have different configuration settings for different environments, for example, development, test and production.</span></span> <span data-ttu-id="05c21-135">El código resaltado siguiente agrega dos proveedores de configuración a tres orígenes:</span><span class="sxs-lookup"><span data-stu-id="05c21-135">The following highlighted code adds two configuration providers to three sources:</span></span>

1. <span data-ttu-id="05c21-136">Proveedor JSON, leer *appSettings.JSON que se*</span><span class="sxs-lookup"><span data-stu-id="05c21-136">JSON provider, reading *appsettings.json*</span></span>
2. <span data-ttu-id="05c21-137">Proveedor JSON, leer *appsettings.\< EnvironmentName > .json*</span><span class="sxs-lookup"><span data-stu-id="05c21-137">JSON provider, reading *appsettings.\<EnvironmentName>.json*</span></span>
3. <span data-ttu-id="05c21-138">Proveedor de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="05c21-138">Environment variables provider</span></span>

<span data-ttu-id="05c21-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span><span class="sxs-lookup"><span data-stu-id="05c21-139">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet2&highlight=7-9)]</span></span>

<span data-ttu-id="05c21-140">Vea [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="05c21-140">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="05c21-141">`reloadOnChange`solo se admite en ASP.NET Core 1.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="05c21-141">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="05c21-142">Orígenes de configuración se leen en el orden en que se especifican.</span><span class="sxs-lookup"><span data-stu-id="05c21-142">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="05c21-143">En el código anterior, las variables de entorno se leen de la última.</span><span class="sxs-lookup"><span data-stu-id="05c21-143">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="05c21-144">Los valores de configuración establecido a través del entorno podría reemplazarlas que se establecen en los dos proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="05c21-144">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="05c21-145">El entorno se establece normalmente en uno de `Development`, `Staging`, o `Production`.</span><span class="sxs-lookup"><span data-stu-id="05c21-145">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="05c21-146">Vea [trabajar con varios entornos](environments.md) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="05c21-146">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="05c21-147">Consideraciones sobre la configuración:</span><span class="sxs-lookup"><span data-stu-id="05c21-147">Configuration considerations:</span></span>

* <span data-ttu-id="05c21-148">`IOptionsSnapshot`puede volver a cargar los datos de configuración, cuando el estado cambia.</span><span class="sxs-lookup"><span data-stu-id="05c21-148">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="05c21-149">Use `IOptionsSnapshot` si tiene que volver a cargar los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="05c21-149">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="05c21-150">Vea [IOptionsSnapshot](#ioptionssnapshot) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="05c21-150">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="05c21-151">Claves de configuración que distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="05c21-151">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="05c21-152">Una práctica recomendada es especificar variables de entorno por último, para que el entorno local puedan reemplazar todo lo establecido en los archivos de configuración implementada.</span><span class="sxs-lookup"><span data-stu-id="05c21-152">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="05c21-153">**Nunca** almacenar contraseñas u otros datos confidenciales en el código de proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="05c21-153">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="05c21-154">No utilice secretos de producción en el desarrollo o entornos de prueba.</span><span class="sxs-lookup"><span data-stu-id="05c21-154">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="05c21-155">En su lugar, especifique secretos fuera del árbol del proyecto, por lo que no se pueden confirmar accidentalmente en el repositorio.</span><span class="sxs-lookup"><span data-stu-id="05c21-155">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="05c21-156">Obtenga más información sobre [trabajar con varios entornos](environments.md) y administrar [almacenamiento seguro para la ejecución de secretos de aplicación durante el desarrollo](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="05c21-156">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="05c21-157">Si `:` no se puede utilizar en variables de entorno en el sistema, reemplace `:` con `__` (subrayado doble).</span><span class="sxs-lookup"><span data-stu-id="05c21-157">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="05c21-158">Uso de opciones y objetos de configuración</span><span class="sxs-lookup"><span data-stu-id="05c21-158">Using Options and configuration objects</span></span>

<span data-ttu-id="05c21-159">El patrón de opciones usa las clases de opciones personalizadas para representar un grupo de valores relacionados.</span><span class="sxs-lookup"><span data-stu-id="05c21-159">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="05c21-160">Se recomienda crear clases desacopladas para cada característica dentro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05c21-160">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="05c21-161">Clases desacopladas siguen:</span><span class="sxs-lookup"><span data-stu-id="05c21-161">Decoupled classes follow:</span></span>

* <span data-ttu-id="05c21-162">El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/) : clases dependen únicamente de los valores de configuración que se usan.</span><span class="sxs-lookup"><span data-stu-id="05c21-162">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="05c21-163">[Separación de intereses](http://deviq.com/separation-of-concerns/) : configuración para las distintas partes de la aplicación no es dependientes o acoplamiento entre sí.</span><span class="sxs-lookup"><span data-stu-id="05c21-163">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="05c21-164">La clase de opciones debe ser no abstracto con un constructor público sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="05c21-164">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="05c21-165">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="05c21-165">For example:</span></span>

<span data-ttu-id="05c21-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-166">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MyOptions.cs)]</span></span>

<a name=options-example></a>

<span data-ttu-id="05c21-167">En el código siguiente, se habilita el proveedor de configuración de JSON.</span><span class="sxs-lookup"><span data-stu-id="05c21-167">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="05c21-168">La `MyOptions` clase se agrega al contenedor de servicios y se enlaza a la configuración.</span><span class="sxs-lookup"><span data-stu-id="05c21-168">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

<span data-ttu-id="05c21-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span><span class="sxs-lookup"><span data-stu-id="05c21-169">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-22)]</span></span>

<span data-ttu-id="05c21-170">El siguiente [controlador](../mvc/controllers/index.md) utiliza [constructor inyección de dependencia](xref:fundamentals/dependency-injection#what-is-dependency-injection) en [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) acceso a la configuración:</span><span class="sxs-lookup"><span data-stu-id="05c21-170">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

<span data-ttu-id="05c21-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="05c21-171">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="05c21-172">Con el siguiente *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="05c21-172">With the following *appsettings.json* file:</span></span>

<span data-ttu-id="05c21-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span><span class="sxs-lookup"><span data-stu-id="05c21-173">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings1.json)]</span></span>

<span data-ttu-id="05c21-174">El `HomeController.Index` método `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="05c21-174">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="05c21-175">Las aplicaciones típicas no enlazar toda la configuración a un archivo de opciones de inicio único.</span><span class="sxs-lookup"><span data-stu-id="05c21-175">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="05c21-176">Más adelante voy a explicar cómo usar `GetSection` para enlazar a una sección.</span><span class="sxs-lookup"><span data-stu-id="05c21-176">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="05c21-177">En el código siguiente, un segundo `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="05c21-177">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="05c21-178">Usa un delegado para configurar el enlace con `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="05c21-178">It uses a delegate to configure the binding with `MyOptions`.</span></span>

<span data-ttu-id="05c21-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="05c21-179">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span></span>

<span data-ttu-id="05c21-180">Puede agregar varios proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="05c21-180">You can add multiple configuration providers.</span></span> <span data-ttu-id="05c21-181">Proveedores de configuración están disponibles en paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="05c21-181">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="05c21-182">Se aplican en orden que están registrados.</span><span class="sxs-lookup"><span data-stu-id="05c21-182">They are applied in order they are registered.</span></span>

<span data-ttu-id="05c21-183">Cada llamada a `Configure<TOptions>` agrega un `IConfigureOptions<TOptions>` servicio al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="05c21-183">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="05c21-184">En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.JSON que se* --pero el valor de `Option1` se reemplaza por el delegado configurado.</span><span class="sxs-lookup"><span data-stu-id="05c21-184">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="05c21-185">Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificado "gana" (que establece el valor de configuración).</span><span class="sxs-lookup"><span data-stu-id="05c21-185">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="05c21-186">En el código anterior, el `HomeController.Index` método `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="05c21-186">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="05c21-187">Al enlazar opciones de configuración, cada propiedad en el tipo de opciones está enlazado a una clave de configuración del formulario `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="05c21-187">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="05c21-188">Por ejemplo, el `MyOptions.Option1` propiedad se enlaza a la clave `Option1`, que se lee desde el `option1` propiedad en *appSettings.JSON que se*.</span><span class="sxs-lookup"><span data-stu-id="05c21-188">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="05c21-189">Más adelante en este artículo, se muestra un ejemplo de la subpropiedad.</span><span class="sxs-lookup"><span data-stu-id="05c21-189">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="05c21-190">En el código siguiente, una tercera `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="05c21-190">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="05c21-191">Enlaza `MySubOptions` a la sección `subsection` de la *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="05c21-191">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

<span data-ttu-id="05c21-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span><span class="sxs-lookup"><span data-stu-id="05c21-192">[!code-csharp[Main](configuration/sample/src/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span></span>

<span data-ttu-id="05c21-193">Nota: Este método de extensión requiere el `Microsoft.Extensions.Options.ConfigurationExtensions` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="05c21-193">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="05c21-194">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="05c21-194">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="05c21-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="05c21-195">[!code-json[Main](configuration/sample/src/UsingOptions/appsettings.json)]</span></span>

<span data-ttu-id="05c21-196">La `MySubOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="05c21-196">The `MySubOptions` class:</span></span>

<span data-ttu-id="05c21-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-197">[!code-csharp[Main](configuration/sample/src/UsingOptions/Models/MySubOptions.cs)]</span></span>

<span data-ttu-id="05c21-198">Con los siguientes `Controller`:</span><span class="sxs-lookup"><span data-stu-id="05c21-198">With the following `Controller`:</span></span>

<span data-ttu-id="05c21-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="05c21-199">[!code-csharp[Main](configuration/sample/src/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span></span>

<span data-ttu-id="05c21-200">`subOption1 = subvalue1_from_json, subOption2 = 200`se devuelve.</span><span class="sxs-lookup"><span data-stu-id="05c21-200">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="05c21-201">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="05c21-201">IOptionsSnapshot</span></span>

<span data-ttu-id="05c21-202">*Requiere ASP.NET Core 1.1 o posterior.*</span><span class="sxs-lookup"><span data-stu-id="05c21-202">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="05c21-203">`IOptionsSnapshot`permite volver a cargar los datos de configuración cuando ha cambiado el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="05c21-203">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="05c21-204">Tiene una sobrecarga mínima.</span><span class="sxs-lookup"><span data-stu-id="05c21-204">It has minimal overhead.</span></span> <span data-ttu-id="05c21-205">Usar `IOptionsSnapshot` con `reloadOnChange: true`, las opciones están enlazadas a `IConfiguration` y volver a cargar cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="05c21-205">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="05c21-206">El siguiente ejemplo se muestra cómo un nuevo `IOptionsSnapshot` se crea después *config.json* cambios.</span><span class="sxs-lookup"><span data-stu-id="05c21-206">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="05c21-207">Las solicitudes realizadas al servidor devolverán la misma hora *config.json* tiene **no** cambiado.</span><span class="sxs-lookup"><span data-stu-id="05c21-207">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="05c21-208">La primera solicitud después de *config.json* cambios mostrará una nueva hora.</span><span class="sxs-lookup"><span data-stu-id="05c21-208">The first request after *config.json* changes will show a new time.</span></span>

<span data-ttu-id="05c21-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span><span class="sxs-lookup"><span data-stu-id="05c21-209">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span></span>

<span data-ttu-id="05c21-210">La siguiente imagen muestra la salida de servidor:</span><span class="sxs-lookup"><span data-stu-id="05c21-210">The following image shows the server output:</span></span>

![que muestra de imagen de explorador "última actualización: 11/22/2016 4:43 PM"](configuration/_static/first.png)

<span data-ttu-id="05c21-212">Actualizar el explorador no cambia el valor de mensaje o el tiempo que se muestra (cuando *config.json* no ha cambiado).</span><span class="sxs-lookup"><span data-stu-id="05c21-212">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="05c21-213">Cambiar y guardar la *config.json* y, a continuación, actualice el explorador:</span><span class="sxs-lookup"><span data-stu-id="05c21-213">Change and save the  *config.json* and then refresh the browser:</span></span>

![que muestra de imagen de explorador "se actualizó por última vez a e: 11/22/2016 4:53 P.M."](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="05c21-215">Proveedor en memoria y el enlace a una clase POCO</span><span class="sxs-lookup"><span data-stu-id="05c21-215">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="05c21-216">El ejemplo siguiente muestra cómo usar el proveedor en memoria y enlazar a una clase:</span><span class="sxs-lookup"><span data-stu-id="05c21-216">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

<span data-ttu-id="05c21-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-217">[!code-csharp[Main](configuration/sample/src/InMemory/Program.cs)]</span></span>

<span data-ttu-id="05c21-218">Valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos.</span><span class="sxs-lookup"><span data-stu-id="05c21-218">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="05c21-219">Enlace le permite recuperar objetos POCO o gráficos de objetos incluso todo.</span><span class="sxs-lookup"><span data-stu-id="05c21-219">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="05c21-220">El ejemplo siguiente muestra cómo enlazar a `MyWindow` y utiliza el patrón de opciones con una aplicación de MVC de ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="05c21-220">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

<span data-ttu-id="05c21-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-221">[!code-csharp[Main](configuration/sample/src/WebConfigBind/MyWindow.cs)]</span></span>

<span data-ttu-id="05c21-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="05c21-222">[!code-json[Main](configuration/sample/src/WebConfigBind/appsettings.json)]</span></span>

<span data-ttu-id="05c21-223">Enlazar la clase personalizada de `ConfigureServices` en la `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="05c21-223">Bind the custom class in `ConfigureServices` in the `Startup` class:</span></span>

<span data-ttu-id="05c21-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span><span class="sxs-lookup"><span data-stu-id="05c21-224">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Startup.cs?name=snippet1&highlight=3,4)]</span></span>

<span data-ttu-id="05c21-225">Mostrar la configuración de la `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="05c21-225">Display the settings from the `HomeController`:</span></span>

<span data-ttu-id="05c21-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-226">[!code-csharp[Main](configuration/sample/src/WebConfigBind/Controllers/HomeController.cs)]</span></span>

### <a name="getvalue"></a><span data-ttu-id="05c21-227">GetValue</span><span class="sxs-lookup"><span data-stu-id="05c21-227">GetValue</span></span>

<span data-ttu-id="05c21-228">El ejemplo siguiente se muestra la [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensión:</span><span class="sxs-lookup"><span data-stu-id="05c21-228">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

<span data-ttu-id="05c21-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span><span class="sxs-lookup"><span data-stu-id="05c21-229">[!code-csharp[Main](configuration/sample/src/InMemoryGetValue/Program.cs?highlight=27-29)]</span></span>

<span data-ttu-id="05c21-230">El ConfigurationBinder `GetValue<T>` método le permite especificar un valor predeterminado (80 en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="05c21-230">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="05c21-231">`GetValue<T>`es para escenarios sencillos y no se enlaza con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="05c21-231">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="05c21-232">`GetValue<T>`Obtiene los valores escalares de `GetSection(key).Value` convertir a un tipo específico.</span><span class="sxs-lookup"><span data-stu-id="05c21-232">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="05c21-233">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="05c21-233">Binding to an object graph</span></span>

<span data-ttu-id="05c21-234">Puede enlazar de forma recursiva para cada objeto en una clase.</span><span class="sxs-lookup"><span data-stu-id="05c21-234">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="05c21-235">Tenga en cuenta la siguiente `AppOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="05c21-235">Consider the following `AppOptions` class:</span></span>

<span data-ttu-id="05c21-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-236">[!code-csharp[Main](configuration/sample/src/ObjectGraph/AppOptions.cs)]</span></span>

<span data-ttu-id="05c21-237">El ejemplo siguiente se enlaza a la `AppOptions` clase:</span><span class="sxs-lookup"><span data-stu-id="05c21-237">The following sample binds to the `AppOptions` class:</span></span>

<span data-ttu-id="05c21-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="05c21-238">[!code-csharp[Main](configuration/sample/src/ObjectGraph/Program.cs?highlight=15-16)]</span></span>

<span data-ttu-id="05c21-239">**Núcleo de ASP.NET 1.1** y versiones posteriores pueden usar `Get<T>`, que funciona con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="05c21-239">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="05c21-240">`Get<T>`puede ser más convienent que el uso de `Bind`.</span><span class="sxs-lookup"><span data-stu-id="05c21-240">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="05c21-241">El código siguiente muestra cómo utilizar `Get<T>` con el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="05c21-241">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="05c21-242">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="05c21-242">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="05c21-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="05c21-243">[!code-json[Main](configuration/sample/src/ObjectGraph/appsettings.json)]</span></span>

<span data-ttu-id="05c21-244">El programa mostrará `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="05c21-244">The program displays `Height 11`.</span></span>

<span data-ttu-id="05c21-245">El código siguiente puede utilizarse para la unidad de la configuración de pruebas:</span><span class="sxs-lookup"><span data-stu-id="05c21-245">The following code can be used to unit test the configuration:</span></span>

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

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="05c21-246">Ejemplo básico de proveedor personalizado de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="05c21-246">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="05c21-247">En esta sección, se crea un proveedor de configuración básica que lee los pares nombre / valor de una base de datos usan EF.</span><span class="sxs-lookup"><span data-stu-id="05c21-247">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="05c21-248">Definir una `ConfigurationValue` entidad para almacenar los valores de configuración en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="05c21-248">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

<span data-ttu-id="05c21-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-249">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationValue.cs)]</span></span>

<span data-ttu-id="05c21-250">Agregar un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:</span><span class="sxs-lookup"><span data-stu-id="05c21-250">Add a `ConfigurationContext` to store and access the configured values:</span></span>

<span data-ttu-id="05c21-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="05c21-251">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span></span>

<span data-ttu-id="05c21-252">Cree una clase que implemente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="05c21-252">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

<span data-ttu-id="05c21-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="05c21-253">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span></span>

<span data-ttu-id="05c21-254">Crear el proveedor de configuración personalizados heredando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="05c21-254">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="05c21-255">El proveedor de configuración inicializa la base de datos cuando está vacía:</span><span class="sxs-lookup"><span data-stu-id="05c21-255">The configuration provider initializes the database when it's empty:</span></span>

<span data-ttu-id="05c21-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span><span class="sxs-lookup"><span data-stu-id="05c21-256">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span></span>

<span data-ttu-id="05c21-257">Cuando se ejecuta el ejemplo, se muestran los valores de resaltado de la base de datos ("value_from_ef_1" y "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="05c21-257">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="05c21-258">Puede agregar un `EFConfigSource` método de extensión para agregar el origen de configuración:</span><span class="sxs-lookup"><span data-stu-id="05c21-258">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

<span data-ttu-id="05c21-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="05c21-259">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span></span>

<span data-ttu-id="05c21-260">El código siguiente muestra cómo utilizar la opción de instalación `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="05c21-260">The following code shows how to use the custom `EFConfigProvider`:</span></span>

<span data-ttu-id="05c21-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span><span class="sxs-lookup"><span data-stu-id="05c21-261">[!code-csharp[Main](configuration/sample/src/CustomConfigurationProvider/Program.cs?highlight=20-25)]</span></span>

<span data-ttu-id="05c21-262">Tenga en cuenta el ejemplo agrega personalizado `EFConfigProvider` una vez que el proveedor JSON, por lo que cualquier configuración de la base de datos invalidará la configuración de la *appSettings.JSON que se* archivo.</span><span class="sxs-lookup"><span data-stu-id="05c21-262">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="05c21-263">Con los siguientes *appSettings.JSON que se* archivo:</span><span class="sxs-lookup"><span data-stu-id="05c21-263">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="05c21-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="05c21-264">[!code-json[Main](configuration/sample/src/CustomConfigurationProvider/appsettings.json)]</span></span>

<span data-ttu-id="05c21-265">Se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="05c21-265">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="05c21-266">Proveedor de configuración de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="05c21-266">CommandLine configuration provider</span></span>

<span data-ttu-id="05c21-267">El ejemplo siguiente habilita al proveedor de configuración de la línea de comandos de última:</span><span class="sxs-lookup"><span data-stu-id="05c21-267">The following sample enables the CommandLine configuration provider last:</span></span>

<span data-ttu-id="05c21-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="05c21-268">[!code-csharp[Main](configuration/sample/src/CommandLine/Program.cs)]</span></span>

<span data-ttu-id="05c21-269">Para pasar valores de configuración, utilice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="05c21-269">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="05c21-270">Que muestra:</span><span class="sxs-lookup"><span data-stu-id="05c21-270">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="05c21-271">El `GetSwitchMappings` método le permite usar `-` en lugar de `/` y elimina los prefijos de subclaves iniciales.</span><span class="sxs-lookup"><span data-stu-id="05c21-271">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="05c21-272">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="05c21-272">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="05c21-273">Muestra:</span><span class="sxs-lookup"><span data-stu-id="05c21-273">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="05c21-274">Argumentos de línea de comandos deben incluir un valor (puede ser null).</span><span class="sxs-lookup"><span data-stu-id="05c21-274">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="05c21-275">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="05c21-275">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="05c21-276">Es correcto, pero</span><span class="sxs-lookup"><span data-stu-id="05c21-276">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="05c21-277">se produce una excepción.</span><span class="sxs-lookup"><span data-stu-id="05c21-277">results in an exception.</span></span> <span data-ttu-id="05c21-278">Si especifica un prefijo de modificador de línea de comandos de - o bien para que no hay ninguna asignación de conmutador correspondiente, se producirá una excepción.</span><span class="sxs-lookup"><span data-stu-id="05c21-278">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="05c21-279">El archivo web.config</span><span class="sxs-lookup"><span data-stu-id="05c21-279">The web.config file</span></span>

<span data-ttu-id="05c21-280">A *web.config* archivo es necesario cuando se hospeda la aplicación en IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="05c21-280">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="05c21-281">*Web.config* activa el AspNetCoreModule en IIS para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="05c21-281">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="05c21-282">Configuración de *web.config* habilitar la AspNetCoreModule en IIS para que inicie la aplicación y configurar otros módulos y la configuración de IIS.</span><span class="sxs-lookup"><span data-stu-id="05c21-282">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="05c21-283">Si usa Visual Studio y eliminar *web.config*, Visual Studio creará uno nuevo.</span><span class="sxs-lookup"><span data-stu-id="05c21-283">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="05c21-284">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="05c21-284">Additional notes</span></span>

* <span data-ttu-id="05c21-285">Inyección de dependencia (DI) no se establece hasta después de `ConfigureServices` se invoca.</span><span class="sxs-lookup"><span data-stu-id="05c21-285">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="05c21-286">El sistema de configuración no es compatible con DI.</span><span class="sxs-lookup"><span data-stu-id="05c21-286">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="05c21-287">`IConfiguration`tiene dos especializaciones:</span><span class="sxs-lookup"><span data-stu-id="05c21-287">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="05c21-288">`IConfigurationRoot`Se usa para el nodo raíz.</span><span class="sxs-lookup"><span data-stu-id="05c21-288">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="05c21-289">Puede desencadenar una recarga.</span><span class="sxs-lookup"><span data-stu-id="05c21-289">Can trigger a reload.</span></span>
  * <span data-ttu-id="05c21-290">`IConfigurationSection`Representa una sección de valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="05c21-290">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="05c21-291">El `GetSection` y `GetChildren` métodos devuelven un `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="05c21-291">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="05c21-292">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="05c21-292">Additional resources</span></span>

* [<span data-ttu-id="05c21-293">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="05c21-293">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="05c21-294">Ubicación de almacenamiento segura de secretos de aplicación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="05c21-294">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="05c21-295">Inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="05c21-295">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="05c21-296">Proveedor de configuración de almacén de claves de Azure</span><span class="sxs-lookup"><span data-stu-id="05c21-296">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
