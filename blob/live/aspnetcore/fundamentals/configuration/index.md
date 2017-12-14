---
title: "Configuración en ASP.NET Core"
author: rick-anderson
description: "Use la API de configuración para configurar una aplicación ASP.NET Core mediante varios métodos."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 6281d6ba254670b111964715410fc0694ae4d149
ms.sourcegitcommit: 216dfac27542f10a79274a9ce60dc449e888ed20
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2017
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="08930-103">Configurar una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08930-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="08930-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="08930-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="08930-105">La API de configuración proporciona una manera de configurar una aplicación web ASP.NET Core según una lista de pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="08930-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="08930-106">La configuración se lee en tiempo de ejecución desde varios orígenes.</span><span class="sxs-lookup"><span data-stu-id="08930-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="08930-107">Puede agrupar estos pares de nombre y valor en una jerarquía multinivel.</span><span class="sxs-lookup"><span data-stu-id="08930-107">You can group these name-value pairs into a multi-level hierarchy.</span></span> 

<span data-ttu-id="08930-108">Existen proveedores de configuración para:</span><span class="sxs-lookup"><span data-stu-id="08930-108">There are configuration providers for:</span></span>

* <span data-ttu-id="08930-109">Formatos de archivo (INI, JSON y XML)</span><span class="sxs-lookup"><span data-stu-id="08930-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="08930-110">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="08930-110">Command-line arguments</span></span>
* <span data-ttu-id="08930-111">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="08930-111">Environment variables</span></span>
* <span data-ttu-id="08930-112">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="08930-112">In-memory .NET objects</span></span>
* <span data-ttu-id="08930-113">Un almacén de usuario cifrado</span><span class="sxs-lookup"><span data-stu-id="08930-113">An encrypted user store</span></span>
* [<span data-ttu-id="08930-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="08930-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="08930-115">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="08930-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="08930-116">Cada valor de configuración se asigna a una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="08930-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="08930-117">Hay compatibilidad de enlace integrada para deserializar la configuración en un objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (una clase simple de .NET con propiedades).</span><span class="sxs-lookup"><span data-stu-id="08930-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="08930-118">El patrón de opciones usa las clases de opciones para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="08930-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="08930-119">Para más información sobre cómo usar el patrón de opciones, vea el tema [Opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="08930-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="08930-120">[Vea o descargue el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="08930-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="08930-121">Configuración de JSON</span><span class="sxs-lookup"><span data-stu-id="08930-121">JSON configuration</span></span>

<span data-ttu-id="08930-122">En la aplicación de consola siguiente se usa el proveedor de configuración de JSON:</span><span class="sxs-lookup"><span data-stu-id="08930-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="08930-123">La aplicación lee y muestra los valores de configuración siguientes:</span><span class="sxs-lookup"><span data-stu-id="08930-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="08930-124">La configuración consta de una lista jerárquica de pares de nombre y valor en los que los nodos se separan con dos puntos.</span><span class="sxs-lookup"><span data-stu-id="08930-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="08930-125">Para recuperar un valor, obtenga acceso al indizador `Configuration` con la clave del elemento correspondiente:</span><span class="sxs-lookup"><span data-stu-id="08930-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="08930-126">Para trabajar con matrices en orígenes de configuración con formato JSON, use un índice de matriz como parte de la cadena separada por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="08930-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="08930-127">En el ejemplo siguiente se obtiene el nombre del primer elemento de la matriz `wizards` anterior:</span><span class="sxs-lookup"><span data-stu-id="08930-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="08930-128">Los pares de nombre y valor que se escriben en los proveedores de `Configuration` integrados **no** se conservan,</span><span class="sxs-lookup"><span data-stu-id="08930-128">Name-value pairs written to the built-in `Configuration` providers are **not** persisted.</span></span> <span data-ttu-id="08930-129">pero se puede crear un proveedor personalizado que guarde los valores.</span><span class="sxs-lookup"><span data-stu-id="08930-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="08930-130">Vea [Proveedor de configuración personalizado](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="08930-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="08930-131">En el ejemplo anterior se usa el indizador de configuración para leer los valores.</span><span class="sxs-lookup"><span data-stu-id="08930-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="08930-132">Para obtener acceso a la configuración fuera de `Startup`, use el *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="08930-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="08930-133">Para más información, vea el tema [Opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="08930-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="08930-134">Es habitual tener distintos valores de configuración para entornos diferentes, por ejemplo para desarrollo, pruebas y producción.</span><span class="sxs-lookup"><span data-stu-id="08930-134">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="08930-135">El método de extensión `CreateDefaultBuilder` en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y orígenes de configuración del sistema:</span><span class="sxs-lookup"><span data-stu-id="08930-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="08930-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="08930-136">*appsettings.json*</span></span>
* <span data-ttu-id="08930-137">*appsettings.\<NombreDelEntorno>.json*</span><span class="sxs-lookup"><span data-stu-id="08930-137">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="08930-138">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="08930-138">Environment variables</span></span>

<span data-ttu-id="08930-139">Vea [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="08930-139">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="08930-140">`reloadOnChange` solo se admite en ASP.NET Core 1.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="08930-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span> 

<span data-ttu-id="08930-141">Los orígenes de configuración se leen en el orden en que se especifican.</span><span class="sxs-lookup"><span data-stu-id="08930-141">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="08930-142">En el código anterior, las variables de entorno se leen en último lugar.</span><span class="sxs-lookup"><span data-stu-id="08930-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="08930-143">Cualquier valor de configuración establecido mediante el entorno reemplaza a los establecidos en los dos proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="08930-143">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="08930-144">El entorno se establece normalmente en `Development`, `Staging` o `Production`.</span><span class="sxs-lookup"><span data-stu-id="08930-144">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="08930-145">Para más información, vea [Trabajar con varios entornos](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="08930-145">See [Working with multiple environments](xref:fundamentals/environments) for more information.</span></span>

<span data-ttu-id="08930-146">Consideraciones de configuración:</span><span class="sxs-lookup"><span data-stu-id="08930-146">Configuration considerations:</span></span>

* <span data-ttu-id="08930-147">`IOptionsSnapshot` puede volver a cargar los datos de configuración cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="08930-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="08930-148">Vea [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) para más información.</span><span class="sxs-lookup"><span data-stu-id="08930-148">See [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="08930-149">Las claves de configuración no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="08930-149">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="08930-150">Especifique las variables de entorno en último lugar para que el entorno local pueda invalidar la configuración en los archivos de configuración implementados.</span><span class="sxs-lookup"><span data-stu-id="08930-150">Specify environment variables last so that the local environment can override settings in deployed configuration files.</span></span>
* <span data-ttu-id="08930-151">**Nunca** almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="08930-151">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="08930-152">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="08930-152">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="08930-153">En su lugar, especifique los secretos fuera del proyecto para que no se confirmen en el repositorio de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="08930-153">Instead, specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="08930-154">Obtenga más información sobre [trabajar con varios entornos](xref:fundamentals/environments) y administrar el [almacenamiento seguro de los secretos de aplicación durante el desarrollo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="08930-154">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="08930-155">Si no se puede usar un signo de dos puntos (`:`) en las variables de entorno del sistema, reemplace los dos puntos (`:`) por un carácter de subrayado doble (`__`).</span><span class="sxs-lookup"><span data-stu-id="08930-155">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="08930-156">Proveedor en memoria y enlace a una clase POCO</span><span class="sxs-lookup"><span data-stu-id="08930-156">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="08930-157">En el ejemplo siguiente se muestra cómo usar el proveedor en memoria y enlazar a una clase:</span><span class="sxs-lookup"><span data-stu-id="08930-157">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="08930-158">Los valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos.</span><span class="sxs-lookup"><span data-stu-id="08930-158">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="08930-159">El enlace permite recuperar objetos POCO o incluso gráficos de objetos completos.</span><span class="sxs-lookup"><span data-stu-id="08930-159">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="08930-160">GetValue</span><span class="sxs-lookup"><span data-stu-id="08930-160">GetValue</span></span>

<span data-ttu-id="08930-161">En el ejemplo siguiente se muestra el método de extensión [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_):</span><span class="sxs-lookup"><span data-stu-id="08930-161">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="08930-162">El método `GetValue<T>` de ConfigurationBinder permite especificar un valor predeterminado (80 en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="08930-162">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="08930-163">`GetValue<T>` es para escenarios sencillos y no se enlaza con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="08930-163">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="08930-164">`GetValue<T>` obtiene los valores escalares de `GetSection(key).Value` convertidos a un tipo específico.</span><span class="sxs-lookup"><span data-stu-id="08930-164">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="08930-165">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="08930-165">Bind to an object graph</span></span>

<span data-ttu-id="08930-166">Puede enlazar de forma recursiva a cada objeto de una clase.</span><span class="sxs-lookup"><span data-stu-id="08930-166">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="08930-167">Observe la clase `AppSettings` siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-167">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="08930-168">El ejemplo siguiente se enlaza a la clase `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="08930-168">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="08930-169">En **ASP.NET Core 1.1** y versiones posteriores se puede usar `Get<T>`, que funciona con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="08930-169">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="08930-170">`Get<T>` puede ser más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="08930-170">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="08930-171">En el código siguiente se muestra cómo usar `Get<T>` con el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="08930-171">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="08930-172">Con el siguiente archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="08930-172">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="08930-173">El programa muestra `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="08930-173">The program displays `Height 11`.</span></span>

<span data-ttu-id="08930-174">El código siguiente se puede usar para pruebas unitarias de la configuración:</span><span class="sxs-lookup"><span data-stu-id="08930-174">The following code can be used to unit test the configuration:</span></span>

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

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="08930-175">Crear un proveedor personalizado de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="08930-175">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="08930-176">En esta sección, se crea un proveedor de configuración básico que lee pares de nombre y valor de una base de datos que se crea con EF.</span><span class="sxs-lookup"><span data-stu-id="08930-176">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="08930-177">Defina una entidad `ConfigurationValue` para almacenar los valores de configuración en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="08930-177">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="08930-178">Agregue un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:</span><span class="sxs-lookup"><span data-stu-id="08930-178">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="08930-179">Cree una clase que implemente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="08930-179">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="08930-180">Cree el proveedor de configuración personalizado heredando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="08930-180">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="08930-181">El proveedor de configuración inicializa la base de datos cuando está vacía:</span><span class="sxs-lookup"><span data-stu-id="08930-181">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="08930-182">Cuando se ejecuta el ejemplo, se muestran los valores resaltados de la base de datos ("value_from_ef_1" y "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="08930-182">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="08930-183">Puede agregar un método de extensión `EFConfigSource` para agregar el origen de configuración:</span><span class="sxs-lookup"><span data-stu-id="08930-183">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="08930-184">En el código siguiente se muestra cómo puede usar el `EFConfigProvider` personalizado:</span><span class="sxs-lookup"><span data-stu-id="08930-184">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="08930-185">Tenga en cuenta que el ejemplo agrega el `EFConfigProvider` personalizado después del proveedor de JSON, por lo que cualquier configuración de la base de datos invalidará la configuración del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="08930-185">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="08930-186">Con el siguiente archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="08930-186">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="08930-187">Se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-187">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="08930-188">Proveedor de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="08930-188">CommandLine configuration provider</span></span>

<span data-ttu-id="08930-189">El [proveedor de configuración CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recibe pares de clave y valor de argumento de línea de comandos para la configuración en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="08930-189">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="08930-190">Ver o descargar el ejemplo de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="08930-190">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="08930-191">Configurar y usar el proveedor de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="08930-191">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="08930-192">Configuración básica</span><span class="sxs-lookup"><span data-stu-id="08930-192">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="08930-193">Para activar la configuración de línea de comandos, llame al método de extensión `AddCommandLine` en una instancia de [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="08930-193">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="08930-194">Al ejecutar el código, se muestra la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-194">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="08930-195">Pasar pares de clave y valor de argumento en la línea de comandos cambia los valores de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="08930-195">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="08930-196">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-196">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="08930-197">Para invalidar la configuración proporcionada por otros proveedores de configuración con la configuración de línea de comandos, llame a `AddCommandLine` en último lugar en `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="08930-197">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="08930-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="08930-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="08930-199">Las aplicaciones típicas de ASP.NET Core 2.x usan el método estático útil `CreateDefaultBuilder` para crear el host:</span><span class="sxs-lookup"><span data-stu-id="08930-199">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="08930-200">`CreateDefaultBuilder` carga la configuración opcional de *appsettings.json*, *appsettings.{Entorno}.json*, [secretos del usuario](xref:security/app-secrets) (en el entorno `Development`), variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="08930-200">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="08930-201">El proveedor de configuración CommandLine se llama en último lugar.</span><span class="sxs-lookup"><span data-stu-id="08930-201">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="08930-202">Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="08930-202">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="08930-203">Tenga en cuenta que para los archivos *appsettings* se habilita `reloadOnChange`.</span><span class="sxs-lookup"><span data-stu-id="08930-203">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="08930-204">Los argumentos de línea de comandos se invalidan si un valor de configuración coincidente en un archivo *appsettings* se cambia después de iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08930-204">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="08930-205">Como alternativa al uso del método `CreateDefaultBuilder`, en ASP.NET Core 2.x se admite crear un host con [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) y crear manualmente la configuración con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="08930-205">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="08930-206">Vea la pestaña ASP.NET Core 1.x para más información.</span><span class="sxs-lookup"><span data-stu-id="08930-206">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="08930-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="08930-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="08930-208">Cree un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) y llame al método `AddCommandLine` para usar el proveedor de configuración CommandLine.</span><span class="sxs-lookup"><span data-stu-id="08930-208">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="08930-209">Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="08930-209">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="08930-210">Aplique la configuración de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="08930-210">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="08930-211">Argumentos</span><span class="sxs-lookup"><span data-stu-id="08930-211">Arguments</span></span>

<span data-ttu-id="08930-212">Los argumentos que se pasan en la línea de comandos deben ajustarse a uno de los dos formatos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="08930-212">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="08930-213">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="08930-213">Argument format</span></span>                                                     | <span data-ttu-id="08930-214">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="08930-214">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="08930-215">Un solo argumento: un par de clave y valor separado por un signo igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="08930-215">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="08930-216">Secuencia de dos argumentos: un par de clave y valor separado por un espacio</span><span class="sxs-lookup"><span data-stu-id="08930-216">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="08930-217">**Un solo argumento**</span><span class="sxs-lookup"><span data-stu-id="08930-217">**Single argument**</span></span>

<span data-ttu-id="08930-218">El valor debe seguir a un signo igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="08930-218">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="08930-219">El valor puede ser NULL (por ejemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="08930-219">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="08930-220">La clave puede tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="08930-220">The key may have a prefix.</span></span>

| <span data-ttu-id="08930-221">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="08930-221">Key prefix</span></span>               | <span data-ttu-id="08930-222">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="08930-222">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="08930-223">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="08930-223">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="08930-224">Un solo guion (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="08930-224">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="08930-225">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="08930-225">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="08930-226">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="08930-226">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="08930-227">&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="08930-227">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="08930-228">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="08930-228">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="08930-229">Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="08930-229">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="08930-230">**Secuencia de dos argumentos**</span><span class="sxs-lookup"><span data-stu-id="08930-230">**Sequence of two arguments**</span></span>

<span data-ttu-id="08930-231">El valor no puede ser NULL y debe seguir a la clave separado por un espacio.</span><span class="sxs-lookup"><span data-stu-id="08930-231">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="08930-232">La clave debe tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="08930-232">The key must have a prefix.</span></span>

| <span data-ttu-id="08930-233">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="08930-233">Key prefix</span></span>               | <span data-ttu-id="08930-234">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="08930-234">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="08930-235">Un solo guion (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="08930-235">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="08930-236">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="08930-236">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="08930-237">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="08930-237">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="08930-238">&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="08930-238">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="08930-239">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="08930-239">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="08930-240">Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="08930-240">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="08930-241">Claves duplicadas</span><span class="sxs-lookup"><span data-stu-id="08930-241">Duplicate keys</span></span>

<span data-ttu-id="08930-242">Si se proporcionan claves duplicadas, se usa el último par de clave y valor.</span><span class="sxs-lookup"><span data-stu-id="08930-242">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="08930-243">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="08930-243">Switch mappings</span></span>

<span data-ttu-id="08930-244">Cuando la configuración se compila de forma manual con `ConfigurationBuilder`, opcionalmente se puede proporcionar un diccionario de asignaciones de modificador para el método `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="08930-244">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="08930-245">Las asignaciones de modificador permiten proporcionar lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="08930-245">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="08930-246">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="08930-246">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="08930-247">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer la configuración.</span><span class="sxs-lookup"><span data-stu-id="08930-247">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="08930-248">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="08930-248">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="08930-249">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="08930-249">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="08930-250">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="08930-250">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="08930-251">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="08930-251">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="08930-252">En el ejemplo siguiente, el método `GetSwitchMappings` permite que los argumentos de línea de comandos usen un prefijo de clave de un solo guion (`-`) y evita prefijos de subclave iniciales.</span><span class="sxs-lookup"><span data-stu-id="08930-252">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="08930-253">Sin proporcionar argumentos de línea de comandos, el diccionario proporcionado para `AddInMemoryCollection` establece los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="08930-253">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="08930-254">Ejecute la aplicación con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-254">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="08930-255">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-255">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="08930-256">Use lo siguiente para pasar valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="08930-256">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="08930-257">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-257">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="08930-258">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="08930-258">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="08930-259">Key</span><span class="sxs-lookup"><span data-stu-id="08930-259">Key</span></span>            | <span data-ttu-id="08930-260">Valor</span><span class="sxs-lookup"><span data-stu-id="08930-260">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="08930-261">Para mostrar la conmutación de claves mediante el diccionario, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="08930-261">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="08930-262">Se intercambian las claves de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="08930-262">The command-line keys are swapped.</span></span> <span data-ttu-id="08930-263">En la ventana de consola se muestran los valores de configuración de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="08930-263">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="08930-264">El archivo web.config</span><span class="sxs-lookup"><span data-stu-id="08930-264">The web.config file</span></span>

<span data-ttu-id="08930-265">Un archivo *web.config* es necesario cuando la aplicación se hospeda en IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="08930-265">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="08930-266">*web.config* activa AspNetCoreModule en IIS para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="08930-266">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="08930-267">La configuración de *web.config* habilita AspNetCoreModule en IIS para que inicie la aplicación y configure otros módulos y valores de configuración de IIS.</span><span class="sxs-lookup"><span data-stu-id="08930-267">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="08930-268">Si usa Visual Studio y elimina *web.config*, Visual Studio creará un archivo nuevo.</span><span class="sxs-lookup"><span data-stu-id="08930-268">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="08930-269">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="08930-269">Additional notes</span></span>

* <span data-ttu-id="08930-270">La inserción de dependencias (DI) no se establece hasta que se invoca `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="08930-270">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="08930-271">El sistema de configuración no es compatible con DI.</span><span class="sxs-lookup"><span data-stu-id="08930-271">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="08930-272">`IConfiguration` tiene dos especializaciones:</span><span class="sxs-lookup"><span data-stu-id="08930-272">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="08930-273">`IConfigurationRoot` Se usa para el nodo raíz.</span><span class="sxs-lookup"><span data-stu-id="08930-273">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="08930-274">Puede desencadenar una recarga.</span><span class="sxs-lookup"><span data-stu-id="08930-274">Can trigger a reload.</span></span>
  * <span data-ttu-id="08930-275">`IConfigurationSection` Representa una sección de valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="08930-275">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="08930-276">Los métodos `GetSection` y `GetChildren` devuelven un elemento `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="08930-276">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08930-277">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="08930-277">Additional resources</span></span>

* [<span data-ttu-id="08930-278">Opciones</span><span class="sxs-lookup"><span data-stu-id="08930-278">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="08930-279">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="08930-279">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="08930-280">Almacenamiento seguro de secretos de aplicación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="08930-280">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="08930-281">Hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08930-281">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="08930-282">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="08930-282">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="08930-283">Proveedor de configuración de Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="08930-283">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
