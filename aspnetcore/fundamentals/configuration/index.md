---
title: "Configuración en ASP.NET Core"
author: rick-anderson
description: "Use la API de configuración para configurar una aplicación ASP.NET Core mediante varios métodos."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: ee9bdc66d0bfa6433736fbc55126bdd37ba9d080
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="d9eb4-103">Configurar una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9eb4-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="d9eb4-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d9eb4-105">La API de configuración proporciona una manera de configurar una aplicación web ASP.NET Core según una lista de pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="d9eb4-106">La configuración se lee en tiempo de ejecución desde varios orígenes.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="d9eb4-107">Puede agrupar estos pares de nombre y valor en una jerarquía multinivel.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-107">You can group these name-value pairs into a multi-level hierarchy.</span></span>

<span data-ttu-id="d9eb4-108">Existen proveedores de configuración para:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-108">There are configuration providers for:</span></span>

* <span data-ttu-id="d9eb4-109">Formatos de archivo (INI, JSON y XML)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="d9eb4-110">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="d9eb4-110">Command-line arguments</span></span>
* <span data-ttu-id="d9eb4-111">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="d9eb4-111">Environment variables</span></span>
* <span data-ttu-id="d9eb4-112">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="d9eb4-112">In-memory .NET objects</span></span>
* <span data-ttu-id="d9eb4-113">Un almacén de usuario cifrado</span><span class="sxs-lookup"><span data-stu-id="d9eb4-113">An encrypted user store</span></span>
* [<span data-ttu-id="d9eb4-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d9eb4-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="d9eb4-115">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="d9eb4-116">Cada valor de configuración se asigna a una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="d9eb4-117">Hay compatibilidad de enlace integrada para deserializar la configuración en un objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (una clase simple de .NET con propiedades).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="d9eb4-118">El patrón de opciones usa las clases de opciones para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="d9eb4-119">Para más información sobre cómo usar el patrón de opciones, vea el tema [Opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="d9eb4-120">[Vea o descargue el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d9eb4-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="d9eb4-121">Configuración de JSON</span><span class="sxs-lookup"><span data-stu-id="d9eb4-121">JSON configuration</span></span>

<span data-ttu-id="d9eb4-122">En la aplicación de consola siguiente se usa el proveedor de configuración de JSON:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="d9eb4-123">La aplicación lee y muestra los valores de configuración siguientes:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="d9eb4-124">La configuración consta de una lista jerárquica de pares de nombre y valor en los que los nodos se separan con dos puntos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="d9eb4-125">Para recuperar un valor, obtenga acceso al indizador `Configuration` con la clave del elemento correspondiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="d9eb4-126">Para trabajar con matrices en orígenes de configuración con formato JSON, use un índice de matriz como parte de la cadena separada por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="d9eb4-127">En el ejemplo siguiente se obtiene el nombre del primer elemento de la matriz `wizards` anterior:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="d9eb4-128">Los pares de nombre y valor que se escriben en los proveedores de [Configuración](/dotnet/api/microsoft.extensions.configuration) integrados **no** se conservan,</span><span class="sxs-lookup"><span data-stu-id="d9eb4-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="d9eb4-129">pero se puede crear un proveedor personalizado que guarde los valores.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="d9eb4-130">Vea [Proveedor de configuración personalizado](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="d9eb4-131">En el ejemplo anterior se usa el indizador de configuración para leer los valores.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="d9eb4-132">Para obtener acceso a la configuración fuera de `Startup`, use el *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="d9eb4-133">Para más información, vea el tema [Opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="d9eb4-134">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="d9eb4-134">Configuration by environment</span></span>

<span data-ttu-id="d9eb4-135">Es habitual tener distintos valores de configuración para entornos diferentes, por ejemplo para desarrollo, pruebas y producción.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="d9eb4-136">El método de extensión `CreateDefaultBuilder` en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y orígenes de configuración del sistema:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="d9eb4-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="d9eb4-137">*appsettings.json*</span></span>
* <span data-ttu-id="d9eb4-138">*appsettings.\<NombreDelEntorno>.json*</span><span class="sxs-lookup"><span data-stu-id="d9eb4-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="d9eb4-139">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="d9eb4-139">Environment variables</span></span>

<span data-ttu-id="d9eb4-140">Las aplicaciones ASP.NET Core 1.x deben llamar a `AddJsonFile` y [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="d9eb4-141">Vea [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="d9eb4-142">`reloadOnChange` solo se admite en ASP.NET Core 1.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="d9eb4-143">Los orígenes de configuración se leen en el orden en que se especifican.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="d9eb4-144">En el código anterior, las variables de entorno se leen en último lugar.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="d9eb4-145">Cualquier valor de configuración establecido mediante el entorno reemplaza a los establecidos en los dos proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="d9eb4-146">Tenga en cuenta el siguiente archivo *appsettings.Staging.json*:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="d9eb4-147">Cuando el entorno se configura en `Staging`, el siguiente método `Configure` lee el valor de `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="d9eb4-148">El entorno se establece normalmente en `Development`, `Staging` o `Production`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="d9eb4-149">Para obtener más información, consulte [Working with multiple environments](xref:fundamentals/environments) (Trabajo con varios entornos).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="d9eb4-150">Consideraciones de configuración:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-150">Configuration considerations:</span></span>

* <span data-ttu-id="d9eb4-151">`IOptionsSnapshot` puede volver a cargar los datos de configuración cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="d9eb4-152">Para obtener más información, consulte [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="d9eb4-153">En las claves de configuraciones **no** se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="d9eb4-154">**Nunca** almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="d9eb4-155">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-155">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="d9eb4-156">Especifique los secretos fuera del proyecto para que no se confirmen en el repositorio de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-156">Specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="d9eb4-157">Obtenga más información sobre [trabajar con varios entornos](xref:fundamentals/environments) y administrar el [almacenamiento seguro de los secretos de aplicación durante el desarrollo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="d9eb4-158">Si no se puede usar un signo de dos puntos (`:`) en las variables de entorno del sistema, reemplace los dos puntos (`:`) por un carácter de subrayado doble (`__`).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-158">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="d9eb4-159">Proveedor en memoria y enlace a una clase POCO</span><span class="sxs-lookup"><span data-stu-id="d9eb4-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="d9eb4-160">En el ejemplo siguiente se muestra cómo usar el proveedor en memoria y enlazar a una clase:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="d9eb4-161">Los valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="d9eb4-162">El enlace permite recuperar objetos POCO o incluso gráficos de objetos completos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-162">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="d9eb4-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="d9eb4-163">GetValue</span></span>

<span data-ttu-id="d9eb4-164">En el ejemplo siguiente se muestra el método de extensión [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="d9eb4-164">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="d9eb4-165">El método `GetValue<T>` de ConfigurationBinder permite especificar un valor predeterminado (80 en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-165">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="d9eb4-166">`GetValue<T>` es para escenarios sencillos y no se enlaza con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-166">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="d9eb4-167">`GetValue<T>` obtiene los valores escalares de `GetSection(key).Value` convertidos a un tipo específico.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-167">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="d9eb4-168">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="d9eb4-168">Bind to an object graph</span></span>

<span data-ttu-id="d9eb4-169">Puede enlazar de forma recursiva a cada objeto de una clase.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-169">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="d9eb4-170">Observe la clase `AppSettings` siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="d9eb4-171">El ejemplo siguiente se enlaza a la clase `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="d9eb4-172">En **ASP.NET Core 1.1** y versiones posteriores se puede usar `Get<T>`, que funciona con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="d9eb4-173">`Get<T>` puede ser más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="d9eb4-174">En el código siguiente se muestra cómo usar `Get<T>` con el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="d9eb4-175">Con el siguiente archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="d9eb4-176">El programa muestra `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="d9eb4-177">El código siguiente se puede usar para pruebas unitarias de la configuración:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-177">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="d9eb4-178">Crear un proveedor personalizado de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="d9eb4-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="d9eb4-179">En esta sección, se crea un proveedor de configuración básico que lee pares de nombre y valor de una base de datos que se crea con EF.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="d9eb4-180">Defina una entidad `ConfigurationValue` para almacenar los valores de configuración en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="d9eb4-181">Agregue un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="d9eb4-182">Cree una clase que implemente [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="d9eb4-182">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="d9eb4-183">Cree el proveedor de configuración personalizado heredando de [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="d9eb4-184">El proveedor de configuración inicializa la base de datos cuando está vacía:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="d9eb4-185">Cuando se ejecuta el ejemplo, se muestran los valores resaltados de la base de datos ("value_from_ef_1" y "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="d9eb4-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="d9eb4-186">Puede agregar un método de extensión `EFConfigSource` para agregar el origen de configuración:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-186">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="d9eb4-187">En el código siguiente se muestra cómo puede usar el `EFConfigProvider` personalizado:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="d9eb4-188">Tenga en cuenta que el ejemplo agrega el `EFConfigProvider` personalizado después del proveedor de JSON, por lo que cualquier configuración de la base de datos invalidará la configuración del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="d9eb4-189">Con el siguiente archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="d9eb4-190">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="d9eb4-191">Proveedor de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="d9eb4-191">CommandLine configuration provider</span></span>

<span data-ttu-id="d9eb4-192">El [proveedor de configuración CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recibe pares de clave y valor de argumento de línea de comandos para la configuración en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="d9eb4-193">Ver o descargar el ejemplo de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="d9eb4-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="d9eb4-194">Configurar y usar el proveedor de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="d9eb4-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="d9eb4-195">Configuración básica</span><span class="sxs-lookup"><span data-stu-id="d9eb4-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="d9eb4-196">Para activar la configuración de línea de comandos, llame al método de extensión `AddCommandLine` en una instancia de [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="d9eb4-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="d9eb4-197">Al ejecutar el código, se muestra la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="d9eb4-198">Pasar pares de clave y valor de argumento en la línea de comandos cambia los valores de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="d9eb4-199">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="d9eb4-200">Para invalidar la configuración proporcionada por otros proveedores de configuración con la configuración de línea de comandos, llame a `AddCommandLine` en último lugar en `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d9eb4-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d9eb4-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d9eb4-202">Las aplicaciones típicas de ASP.NET Core 2.x usan el método estático útil `CreateDefaultBuilder` para crear el host:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="d9eb4-203">`CreateDefaultBuilder` carga la configuración opcional de *appsettings.json*, *appsettings.{Entorno}.json*, [secretos del usuario](xref:security/app-secrets) (en el entorno `Development`), variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="d9eb4-204">El proveedor de configuración CommandLine se llama en último lugar.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="d9eb4-205">Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="d9eb4-206">Para archivos *appsettings* en los que:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="d9eb4-207">`reloadOnChange` se ha habilitado.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="d9eb4-208">Contiene la misma configuración en los argumentos de línea de comandos y en un archivo *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="d9eb4-209">El archivo *appsettings* que contiene el argumento de línea de comandos coincidente se cambia después de iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="d9eb4-210">Si se cumplen todas las condiciones anteriores, se reemplazan los argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="d9eb4-211">La aplicación ASP.NET Core 2.x puede usar WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) en lugar de \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, con la configuración establecida manualmente mediante [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-211">ASP.NET Core 2.x app can use WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="d9eb4-212">Vea la pestaña ASP.NET Core 1.x para más información.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-212">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d9eb4-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d9eb4-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d9eb4-214">Cree un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) y llame al método `AddCommandLine` para usar el proveedor de configuración CommandLine.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-214">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="d9eb4-215">Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-215">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="d9eb4-216">Aplique la configuración de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-216">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="d9eb4-217">Argumentos</span><span class="sxs-lookup"><span data-stu-id="d9eb4-217">Arguments</span></span>

<span data-ttu-id="d9eb4-218">Los argumentos que se pasan en la línea de comandos deben ajustarse a uno de los dos formatos que se muestran en la tabla siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-218">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="d9eb4-219">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="d9eb4-219">Argument format</span></span>                                                     | <span data-ttu-id="d9eb4-220">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9eb4-220">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="d9eb4-221">Un solo argumento: un par de clave y valor separado por un signo igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-221">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="d9eb4-222">Secuencia de dos argumentos: un par de clave y valor separado por un espacio</span><span class="sxs-lookup"><span data-stu-id="d9eb4-222">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="d9eb4-223">**Un solo argumento**</span><span class="sxs-lookup"><span data-stu-id="d9eb4-223">**Single argument**</span></span>

<span data-ttu-id="d9eb4-224">El valor debe seguir a un signo igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-224">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="d9eb4-225">El valor puede ser NULL (por ejemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-225">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="d9eb4-226">La clave puede tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-226">The key may have a prefix.</span></span>

| <span data-ttu-id="d9eb4-227">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="d9eb4-227">Key prefix</span></span>               | <span data-ttu-id="d9eb4-228">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9eb4-228">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="d9eb4-229">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="d9eb4-229">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="d9eb4-230">Un solo guion (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="d9eb4-230">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="d9eb4-231">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-231">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="d9eb4-232">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-232">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="d9eb4-233">&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-233">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="d9eb4-234">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-234">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="d9eb4-235">Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-235">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="d9eb4-236">**Secuencia de dos argumentos**</span><span class="sxs-lookup"><span data-stu-id="d9eb4-236">**Sequence of two arguments**</span></span>

<span data-ttu-id="d9eb4-237">El valor no puede ser NULL y debe seguir a la clave separado por un espacio.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-237">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="d9eb4-238">La clave debe tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-238">The key must have a prefix.</span></span>

| <span data-ttu-id="d9eb4-239">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="d9eb4-239">Key prefix</span></span>               | <span data-ttu-id="d9eb4-240">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="d9eb4-240">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="d9eb4-241">Un solo guion (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="d9eb4-241">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="d9eb4-242">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-242">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="d9eb4-243">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="d9eb4-243">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="d9eb4-244">&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-244">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="d9eb4-245">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-245">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="d9eb4-246">Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-246">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="d9eb4-247">Claves duplicadas</span><span class="sxs-lookup"><span data-stu-id="d9eb4-247">Duplicate keys</span></span>

<span data-ttu-id="d9eb4-248">Si se proporcionan claves duplicadas, se usa el último par de clave y valor.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-248">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="d9eb4-249">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="d9eb4-249">Switch mappings</span></span>

<span data-ttu-id="d9eb4-250">Cuando la configuración se compila de forma manual con `ConfigurationBuilder`, opcionalmente se puede proporcionar un diccionario de asignaciones de modificador para el método `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-250">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="d9eb4-251">Las asignaciones de modificador permiten proporcionar lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-251">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="d9eb4-252">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-252">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="d9eb4-253">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer la configuración.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-253">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="d9eb4-254">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-254">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="d9eb4-255">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-255">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="d9eb4-256">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-256">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="d9eb4-257">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-257">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="d9eb4-258">En el ejemplo siguiente, el método `GetSwitchMappings` permite que los argumentos de línea de comandos usen un prefijo de clave de un solo guion (`-`) y evita prefijos de subclave iniciales.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-258">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="d9eb4-259">Sin proporcionar argumentos de línea de comandos, el diccionario proporcionado para `AddInMemoryCollection` establece los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-259">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="d9eb4-260">Ejecute la aplicación con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-260">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="d9eb4-261">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-261">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="d9eb4-262">Use lo siguiente para pasar valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-262">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="d9eb4-263">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-263">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="d9eb4-264">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-264">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="d9eb4-265">Key</span><span class="sxs-lookup"><span data-stu-id="d9eb4-265">Key</span></span>            | <span data-ttu-id="d9eb4-266">Valor</span><span class="sxs-lookup"><span data-stu-id="d9eb4-266">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="d9eb4-267">Para mostrar la conmutación de claves mediante el diccionario, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-267">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="d9eb4-268">Se intercambian las claves de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-268">The command-line keys are swapped.</span></span> <span data-ttu-id="d9eb4-269">En la ventana de consola se muestran los valores de configuración de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-269">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="d9eb4-270">El archivo web.config</span><span class="sxs-lookup"><span data-stu-id="d9eb4-270">The web.config file</span></span>

<span data-ttu-id="d9eb4-271">Un archivo *web.config* es necesario cuando la aplicación se hospeda en IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-271">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="d9eb4-272">La configuración de *web.config* habilita el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para que inicie la aplicación y configure otros módulos y valores de configuración de IIS.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-272">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="d9eb4-273">Si el archivo *web.config* no está presente y el archivo de proyecto incluye `<Project Sdk="Microsoft.NET.Sdk.Web">`, al publicar el proyecto se crea un archivo *web.config* en la salida publicada (la carpeta de *publicación*).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-273">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="d9eb4-274">Para más información, vea [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig) (Hospedar ASP.NET Core en Windows con IIS).</span><span class="sxs-lookup"><span data-stu-id="d9eb4-274">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="additional-notes"></a><span data-ttu-id="d9eb4-275">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="d9eb4-275">Additional notes</span></span>

* <span data-ttu-id="d9eb4-276">La inserción de dependencias (DI) no se establece hasta que se invoca `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-276">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="d9eb4-277">El sistema de configuración no es compatible con DI.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-277">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="d9eb4-278">`IConfiguration` tiene dos especializaciones:</span><span class="sxs-lookup"><span data-stu-id="d9eb4-278">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="d9eb4-279">`IConfigurationRoot` Se usa para el nodo raíz.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-279">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="d9eb4-280">Puede desencadenar una recarga.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-280">Can trigger a reload.</span></span>
  * <span data-ttu-id="d9eb4-281">`IConfigurationSection` Representa una sección de valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-281">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="d9eb4-282">Los métodos `GetSection` y `GetChildren` devuelven un elemento `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-282">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="d9eb4-283">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) al volver a cargar configuración o si necesita acceder a todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-283">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or need access to each provider.</span></span> <span data-ttu-id="d9eb4-284">Ninguna de estas situaciones son comunes.</span><span class="sxs-lookup"><span data-stu-id="d9eb4-284">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d9eb4-285">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d9eb4-285">Additional resources</span></span>

* [<span data-ttu-id="d9eb4-286">Opciones</span><span class="sxs-lookup"><span data-stu-id="d9eb4-286">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="d9eb4-287">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="d9eb4-287">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="d9eb4-288">Almacenamiento seguro de secretos de aplicación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="d9eb4-288">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="d9eb4-289">Hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9eb4-289">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="d9eb4-290">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="d9eb4-290">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="d9eb4-291">Proveedor de configuración de Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="d9eb4-291">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
