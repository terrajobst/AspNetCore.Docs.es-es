---
title: "Configuración en ASP.NET Core"
author: rick-anderson
description: "Use la API de configuración para configurar una aplicación ASP.NET Core mediante varios métodos."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 7c41621db835b452c9aad9463a9ffccdf0c06484
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="24261-103">Configurar una aplicación ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24261-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="24261-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) y [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="24261-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="24261-105">La API de configuración proporciona una manera de configurar una aplicación web ASP.NET Core según una lista de pares de nombre y valor.</span><span class="sxs-lookup"><span data-stu-id="24261-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="24261-106">La configuración se lee en tiempo de ejecución desde varios orígenes.</span><span class="sxs-lookup"><span data-stu-id="24261-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="24261-107">Puede agrupar estos pares nombre-valor en una jerarquía multinivel.</span><span class="sxs-lookup"><span data-stu-id="24261-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="24261-108">Existen proveedores de configuración para:</span><span class="sxs-lookup"><span data-stu-id="24261-108">There are configuration providers for:</span></span>

* <span data-ttu-id="24261-109">Formatos de archivo (INI, JSON y XML).</span><span class="sxs-lookup"><span data-stu-id="24261-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="24261-110">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="24261-110">Command-line arguments.</span></span>
* <span data-ttu-id="24261-111">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="24261-111">Environment variables.</span></span>
* <span data-ttu-id="24261-112">Objetos de .NET en memoria.</span><span class="sxs-lookup"><span data-stu-id="24261-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="24261-113">El almacenamiento de [administrador secreto](xref:security/app-secrets) sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="24261-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="24261-114">Un almacén de usuario cifrado, como [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="24261-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="24261-115">Proveedores personalizados (instalados o creados).</span><span class="sxs-lookup"><span data-stu-id="24261-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="24261-116">Cada valor de configuración se asigna a una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="24261-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="24261-117">Hay compatibilidad de enlace integrada para deserializar la configuración en un objeto [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) personalizado (una clase simple de .NET con propiedades).</span><span class="sxs-lookup"><span data-stu-id="24261-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="24261-118">El patrón de opciones usa las clases de opciones para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="24261-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="24261-119">Para más información sobre cómo usar el patrón de opciones, vea el tema [Opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="24261-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="24261-120">[Vea o descargue el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="24261-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="24261-121">Configuración de JSON</span><span class="sxs-lookup"><span data-stu-id="24261-121">JSON configuration</span></span>

<span data-ttu-id="24261-122">En la aplicación de consola siguiente se usa el proveedor de configuración de JSON:</span><span class="sxs-lookup"><span data-stu-id="24261-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="24261-123">La aplicación lee y muestra los valores de configuración siguientes:</span><span class="sxs-lookup"><span data-stu-id="24261-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="24261-124">La configuración consta de una lista jerárquica de pares de nombre y valor en los que los nodos se separan con dos puntos.</span><span class="sxs-lookup"><span data-stu-id="24261-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="24261-125">Para recuperar un valor, obtenga acceso al indizador `Configuration` con la clave del elemento correspondiente:</span><span class="sxs-lookup"><span data-stu-id="24261-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="24261-126">Para trabajar con matrices en orígenes de configuración con formato JSON, use un índice de matriz como parte de la cadena separada por dos puntos.</span><span class="sxs-lookup"><span data-stu-id="24261-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="24261-127">En el ejemplo siguiente se obtiene el nombre del primer elemento de la matriz `wizards` anterior:</span><span class="sxs-lookup"><span data-stu-id="24261-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="24261-128">Los pares de nombre y valor que se escriben en los proveedores de [Configuración](/dotnet/api/microsoft.extensions.configuration) integrados **no** se conservan,</span><span class="sxs-lookup"><span data-stu-id="24261-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="24261-129">pero se puede crear un proveedor personalizado que guarde los valores.</span><span class="sxs-lookup"><span data-stu-id="24261-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="24261-130">Vea [Proveedor de configuración personalizado](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="24261-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="24261-131">En el ejemplo anterior se usa el indizador de configuración para leer los valores.</span><span class="sxs-lookup"><span data-stu-id="24261-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="24261-132">Para obtener acceso a la configuración fuera de `Startup`, use el *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="24261-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="24261-133">Para más información, vea el tema [Opciones](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="24261-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="24261-134">Configuración XML</span><span class="sxs-lookup"><span data-stu-id="24261-134">XML configuration</span></span>

<span data-ttu-id="24261-135">Para trabajar con matrices en orígenes de configuración con formato XML, proporcione un índice `name` a cada elemento.</span><span class="sxs-lookup"><span data-stu-id="24261-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="24261-136">Use el índice para acceder a los valores:</span><span class="sxs-lookup"><span data-stu-id="24261-136">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="24261-137">Configuración de entorno</span><span class="sxs-lookup"><span data-stu-id="24261-137">Configuration by environment</span></span>

<span data-ttu-id="24261-138">Es habitual tener distintos valores de configuración para entornos diferentes, por ejemplo para desarrollo, pruebas y producción.</span><span class="sxs-lookup"><span data-stu-id="24261-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="24261-139">El método de extensión `CreateDefaultBuilder` en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y orígenes de configuración del sistema:</span><span class="sxs-lookup"><span data-stu-id="24261-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="24261-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="24261-140">*appsettings.json*</span></span>
* <span data-ttu-id="24261-141">*appsettings.\<NombreDelEntorno>.json*</span><span class="sxs-lookup"><span data-stu-id="24261-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="24261-142">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="24261-142">Environment variables</span></span>

<span data-ttu-id="24261-143">Las aplicaciones ASP.NET Core 1.x deben llamar a `AddJsonFile` y [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span><span class="sxs-lookup"><span data-stu-id="24261-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="24261-144">Vea [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="24261-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="24261-145">`reloadOnChange` solo se admite en ASP.NET Core 1.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="24261-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="24261-146">Los orígenes de configuración se leen en el orden en que se especifican.</span><span class="sxs-lookup"><span data-stu-id="24261-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="24261-147">En el código anterior, las variables de entorno se leen en último lugar.</span><span class="sxs-lookup"><span data-stu-id="24261-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="24261-148">Cualquier valor de configuración establecido mediante el entorno reemplaza a los establecidos en los dos proveedores anteriores.</span><span class="sxs-lookup"><span data-stu-id="24261-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="24261-149">Tenga en cuenta el siguiente archivo *appsettings.Staging.json*:</span><span class="sxs-lookup"><span data-stu-id="24261-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="24261-150">Cuando el entorno se configura en `Staging`, el siguiente método `Configure` lee el valor de `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="24261-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="24261-151">El entorno se establece normalmente en `Development`, `Staging` o `Production`.</span><span class="sxs-lookup"><span data-stu-id="24261-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="24261-152">Para obtener más información, consulte [Working with multiple environments](xref:fundamentals/environments) (Trabajo con varios entornos).</span><span class="sxs-lookup"><span data-stu-id="24261-152">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="24261-153">Consideraciones de configuración:</span><span class="sxs-lookup"><span data-stu-id="24261-153">Configuration considerations:</span></span>

* <span data-ttu-id="24261-154">`IOptionsSnapshot` puede volver a cargar los datos de configuración cuando cambia.</span><span class="sxs-lookup"><span data-stu-id="24261-154">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="24261-155">Para obtener más información, consulte [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="24261-155">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="24261-156">En las claves de configuraciones **no** se distingue entre mayúsculas y minúsculas.</span><span class="sxs-lookup"><span data-stu-id="24261-156">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="24261-157">**Nunca** almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="24261-157">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="24261-158">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="24261-158">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="24261-159">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="24261-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="24261-160">Obtenga más información sobre [trabajar con varios entornos](xref:fundamentals/environments) y administrar el [almacenamiento seguro de los secretos de aplicación durante el desarrollo](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="24261-160">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="24261-161">Si no se puede usar un signo de dos puntos (`:`) en las variables de entorno del sistema, reemplace los dos puntos (`:`) por un carácter de subrayado doble (`__`).</span><span class="sxs-lookup"><span data-stu-id="24261-161">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="24261-162">Proveedor en memoria y enlace a una clase POCO</span><span class="sxs-lookup"><span data-stu-id="24261-162">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="24261-163">En el ejemplo siguiente se muestra cómo usar el proveedor en memoria y enlazar a una clase:</span><span class="sxs-lookup"><span data-stu-id="24261-163">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="24261-164">Los valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos.</span><span class="sxs-lookup"><span data-stu-id="24261-164">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="24261-165">El enlace permite recuperar objetos POCO o incluso gráficos de objetos completos.</span><span class="sxs-lookup"><span data-stu-id="24261-165">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="24261-166">GetValue</span><span class="sxs-lookup"><span data-stu-id="24261-166">GetValue</span></span>

<span data-ttu-id="24261-167">En el ejemplo siguiente se muestra el método de extensión [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_):</span><span class="sxs-lookup"><span data-stu-id="24261-167">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="24261-168">El método `GetValue<T>` de ConfigurationBinder permite especificar un valor predeterminado (80 en el ejemplo).</span><span class="sxs-lookup"><span data-stu-id="24261-168">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="24261-169">`GetValue<T>` es para escenarios sencillos y no se enlaza con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="24261-169">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="24261-170">`GetValue<T>` obtiene los valores escalares de `GetSection(key).Value` convertidos a un tipo específico.</span><span class="sxs-lookup"><span data-stu-id="24261-170">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="24261-171">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="24261-171">Bind to an object graph</span></span>

<span data-ttu-id="24261-172">Cada objeto de una clase se puede enlazar de forma recursiva.</span><span class="sxs-lookup"><span data-stu-id="24261-172">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="24261-173">Observe la clase `AppSettings` siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-173">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="24261-174">El ejemplo siguiente se enlaza a la clase `AppSettings`:</span><span class="sxs-lookup"><span data-stu-id="24261-174">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="24261-175">En **ASP.NET Core 1.1** y versiones posteriores se puede usar `Get<T>`, que funciona con secciones completas.</span><span class="sxs-lookup"><span data-stu-id="24261-175">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="24261-176">`Get<T>` puede ser más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="24261-176">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="24261-177">En el código siguiente se muestra cómo usar `Get<T>` con el ejemplo anterior:</span><span class="sxs-lookup"><span data-stu-id="24261-177">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="24261-178">Con el siguiente archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="24261-178">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="24261-179">El programa muestra `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="24261-179">The program displays `Height 11`.</span></span>

<span data-ttu-id="24261-180">El código siguiente se puede usar para pruebas unitarias de la configuración:</span><span class="sxs-lookup"><span data-stu-id="24261-180">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="24261-181">Crear un proveedor personalizado de Entity Framework</span><span class="sxs-lookup"><span data-stu-id="24261-181">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="24261-182">En esta sección, se crea un proveedor de configuración básico que lee pares de nombre y valor de una base de datos que se crea con EF.</span><span class="sxs-lookup"><span data-stu-id="24261-182">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="24261-183">Defina una entidad `ConfigurationValue` para almacenar los valores de configuración en la base de datos:</span><span class="sxs-lookup"><span data-stu-id="24261-183">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="24261-184">Agregue un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:</span><span class="sxs-lookup"><span data-stu-id="24261-184">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="24261-185">Cree una clase que implemente [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span><span class="sxs-lookup"><span data-stu-id="24261-185">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="24261-186">Cree el proveedor de configuración personalizado heredando de [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="24261-186">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="24261-187">El proveedor de configuración inicializa la base de datos cuando está vacía:</span><span class="sxs-lookup"><span data-stu-id="24261-187">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="24261-188">Cuando se ejecuta el ejemplo, se muestran los valores resaltados de la base de datos ("value_from_ef_1" y "value_from_ef_2").</span><span class="sxs-lookup"><span data-stu-id="24261-188">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="24261-189">Se puede usar un método de extensión `EFConfigSource` para agregar el origen de configuración:</span><span class="sxs-lookup"><span data-stu-id="24261-189">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="24261-190">En el código siguiente se muestra cómo puede usar el `EFConfigProvider` personalizado:</span><span class="sxs-lookup"><span data-stu-id="24261-190">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="24261-191">Tenga en cuenta que el ejemplo agrega el `EFConfigProvider` personalizado después del proveedor de JSON, por lo que cualquier configuración de la base de datos invalidará la configuración del archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="24261-191">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="24261-192">Con el siguiente archivo *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="24261-192">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="24261-193">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="24261-193">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="24261-194">Proveedor de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="24261-194">CommandLine configuration provider</span></span>

<span data-ttu-id="24261-195">El [proveedor de configuración CommandLine](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) recibe pares de clave y valor de argumento de línea de comandos para la configuración en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="24261-195">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="24261-196">Ver o descargar el ejemplo de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="24261-196">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="24261-197">Configurar y usar el proveedor de configuración CommandLine</span><span class="sxs-lookup"><span data-stu-id="24261-197">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="24261-198">Configuración básica</span><span class="sxs-lookup"><span data-stu-id="24261-198">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="24261-199">Para activar la configuración de línea de comandos, llame al método de extensión `AddCommandLine` en una instancia de [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="24261-199">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="24261-200">Al ejecutar el código, se muestra la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-200">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="24261-201">Pasar pares de clave y valor de argumento en la línea de comandos cambia los valores de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="24261-201">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="24261-202">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-202">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="24261-203">Para invalidar la configuración proporcionada por otros proveedores de configuración con la configuración de línea de comandos, llame a `AddCommandLine` en último lugar en `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="24261-203">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="24261-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="24261-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="24261-205">Las aplicaciones típicas de ASP.NET Core 2.x usan el método estático útil `CreateDefaultBuilder` para crear el host:</span><span class="sxs-lookup"><span data-stu-id="24261-205">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="24261-206">`CreateDefaultBuilder` carga la configuración opcional de *appsettings.json*, *appsettings.{Entorno}.json*, [secretos del usuario](xref:security/app-secrets) (en el entorno `Development`), variables de entorno y argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="24261-206">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="24261-207">El proveedor de configuración CommandLine se llama en último lugar.</span><span class="sxs-lookup"><span data-stu-id="24261-207">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="24261-208">Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="24261-208">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="24261-209">Para archivos *appsettings* en los que:</span><span class="sxs-lookup"><span data-stu-id="24261-209">For *appsettings* files where:</span></span>

* <span data-ttu-id="24261-210">`reloadOnChange` se ha habilitado.</span><span class="sxs-lookup"><span data-stu-id="24261-210">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="24261-211">Contiene la misma configuración en los argumentos de línea de comandos y en un archivo *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="24261-211">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="24261-212">El archivo *appsettings* que contiene el argumento de línea de comandos coincidente se cambia después de iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="24261-212">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="24261-213">Si se cumplen todas las condiciones anteriores, se reemplazan los argumentos de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="24261-213">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="24261-214">La aplicación de ASP.NET Core 2.x puede usar [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) en lugar de `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="24261-214">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="24261-215">Si usa `WebHostBuilder`, realice la configuración manualmente con [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span><span class="sxs-lookup"><span data-stu-id="24261-215">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="24261-216">Vea la pestaña ASP.NET Core 1.x para más información.</span><span class="sxs-lookup"><span data-stu-id="24261-216">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="24261-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="24261-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="24261-218">Cree un [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) y llame al método `AddCommandLine` para usar el proveedor de configuración CommandLine.</span><span class="sxs-lookup"><span data-stu-id="24261-218">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="24261-219">Llamar al proveedor en último lugar permite que los argumentos de línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración que se llamaron anteriormente.</span><span class="sxs-lookup"><span data-stu-id="24261-219">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="24261-220">Aplique la configuración de [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="24261-220">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="24261-221">Argumentos</span><span class="sxs-lookup"><span data-stu-id="24261-221">Arguments</span></span>

<span data-ttu-id="24261-222">Los argumentos que se pasan en la línea de comandos deben ajustarse a uno de los dos formatos que se muestran en la tabla siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-222">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="24261-223">Formato de argumento</span><span class="sxs-lookup"><span data-stu-id="24261-223">Argument format</span></span>                                                     | <span data-ttu-id="24261-224">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="24261-224">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="24261-225">Un solo argumento: un par de clave y valor separado por un signo igual (`=`)</span><span class="sxs-lookup"><span data-stu-id="24261-225">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="24261-226">Secuencia de dos argumentos: un par de clave y valor separado por un espacio</span><span class="sxs-lookup"><span data-stu-id="24261-226">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="24261-227">**Un solo argumento**</span><span class="sxs-lookup"><span data-stu-id="24261-227">**Single argument**</span></span>

<span data-ttu-id="24261-228">El valor debe seguir a un signo igual (`=`).</span><span class="sxs-lookup"><span data-stu-id="24261-228">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="24261-229">El valor puede ser NULL (por ejemplo, `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="24261-229">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="24261-230">La clave puede tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="24261-230">The key may have a prefix.</span></span>

| <span data-ttu-id="24261-231">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="24261-231">Key prefix</span></span>               | <span data-ttu-id="24261-232">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="24261-232">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="24261-233">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="24261-233">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="24261-234">Un solo guion (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="24261-234">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="24261-235">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="24261-235">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="24261-236">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="24261-236">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="24261-237">&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="24261-237">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="24261-238">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="24261-238">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="24261-239">Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="24261-239">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="24261-240">**Secuencia de dos argumentos**</span><span class="sxs-lookup"><span data-stu-id="24261-240">**Sequence of two arguments**</span></span>

<span data-ttu-id="24261-241">El valor no puede ser NULL y debe seguir a la clave separado por un espacio.</span><span class="sxs-lookup"><span data-stu-id="24261-241">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="24261-242">La clave debe tener un prefijo.</span><span class="sxs-lookup"><span data-stu-id="24261-242">The key must have a prefix.</span></span>

| <span data-ttu-id="24261-243">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="24261-243">Key prefix</span></span>               | <span data-ttu-id="24261-244">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="24261-244">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="24261-245">Un solo guion (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="24261-245">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="24261-246">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="24261-246">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="24261-247">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="24261-247">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="24261-248">&#8224;En las [asignaciones de modificador](#switch-mappings) que se describen a continuación debe proporcionarse una clave con un prefijo de un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="24261-248">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="24261-249">Comando de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="24261-249">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="24261-250">Nota: Si `-key1` no está presente en las [asignaciones de modificador](#switch-mappings) que se proporcionan al proveedor de configuración, se produce una excepción `FormatException`.</span><span class="sxs-lookup"><span data-stu-id="24261-250">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="24261-251">Claves duplicadas</span><span class="sxs-lookup"><span data-stu-id="24261-251">Duplicate keys</span></span>

<span data-ttu-id="24261-252">Si se proporcionan claves duplicadas, se usa el último par de clave y valor.</span><span class="sxs-lookup"><span data-stu-id="24261-252">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="24261-253">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="24261-253">Switch mappings</span></span>

<span data-ttu-id="24261-254">Si realiza la configuración de compilación manualmente con `ConfigurationBuilder`, se puede agregar un diccionario de asignaciones de modificador al método `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="24261-254">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="24261-255">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="24261-255">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="24261-256">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="24261-256">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="24261-257">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer la configuración.</span><span class="sxs-lookup"><span data-stu-id="24261-257">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="24261-258">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="24261-258">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="24261-259">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="24261-259">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="24261-260">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="24261-260">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="24261-261">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="24261-261">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="24261-262">En el ejemplo siguiente, el método `GetSwitchMappings` permite que los argumentos de línea de comandos usen un prefijo de clave de un solo guión (`-`) y evita prefijos de subclave iniciales.</span><span class="sxs-lookup"><span data-stu-id="24261-262">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="24261-263">Sin proporcionar argumentos de línea de comandos, el diccionario proporcionado para `AddInMemoryCollection` establece los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="24261-263">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="24261-264">Ejecute la aplicación con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-264">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="24261-265">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-265">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="24261-266">Use lo siguiente para pasar valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="24261-266">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="24261-267">En la ventana de consola se muestra lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-267">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="24261-268">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-268">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="24261-269">Key</span><span class="sxs-lookup"><span data-stu-id="24261-269">Key</span></span>            | <span data-ttu-id="24261-270">Valor</span><span class="sxs-lookup"><span data-stu-id="24261-270">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="24261-271">Para mostrar la conmutación de claves mediante el diccionario, ejecute el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="24261-271">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="24261-272">Se intercambian las claves de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="24261-272">The command-line keys are swapped.</span></span> <span data-ttu-id="24261-273">En la ventana de consola se muestran los valores de configuración de `Profile:MachineName` y `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="24261-273">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="24261-274">Archivo web.config</span><span class="sxs-lookup"><span data-stu-id="24261-274">web.config file</span></span>

<span data-ttu-id="24261-275">Un archivo *web.config* es necesario cuando la aplicación se hospeda en IIS o IIS Express.</span><span class="sxs-lookup"><span data-stu-id="24261-275">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="24261-276">La configuración de *web.config* habilita el [módulo ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) para que inicie la aplicación y configure otros módulos y valores de configuración de IIS.</span><span class="sxs-lookup"><span data-stu-id="24261-276">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="24261-277">Si el archivo *web.config* no está presente y el archivo de proyecto incluye `<Project Sdk="Microsoft.NET.Sdk.Web">`, al publicar el proyecto se crea un archivo *web.config* en la salida publicada (la carpeta de *publicación*).</span><span class="sxs-lookup"><span data-stu-id="24261-277">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="24261-278">Para más información, vea [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file) (Hospedar ASP.NET Core en Windows con IIS).</span><span class="sxs-lookup"><span data-stu-id="24261-278">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="24261-279">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="24261-279">Accessing configuration during startup</span></span>

<span data-ttu-id="24261-280">Para acceder a la configuración en `ConfigureServices` o `Configure` durante el inicio, vea los ejemplos del tema [Inicio de la aplicación](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="24261-280">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="24261-281">Notas adicionales</span><span class="sxs-lookup"><span data-stu-id="24261-281">Additional notes</span></span>

* <span data-ttu-id="24261-282">La inserción de dependencias (DI) no se establece hasta que se invoca `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="24261-282">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="24261-283">El sistema de configuración no es compatible con DI.</span><span class="sxs-lookup"><span data-stu-id="24261-283">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="24261-284">`IConfiguration` tiene dos especializaciones:</span><span class="sxs-lookup"><span data-stu-id="24261-284">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="24261-285">`IConfigurationRoot` Se usa para el nodo raíz.</span><span class="sxs-lookup"><span data-stu-id="24261-285">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="24261-286">Puede desencadenar una recarga.</span><span class="sxs-lookup"><span data-stu-id="24261-286">Can trigger a reload.</span></span>
  * <span data-ttu-id="24261-287">`IConfigurationSection` Representa una sección de valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="24261-287">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="24261-288">Los métodos `GetSection` y `GetChildren` devuelven un elemento `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="24261-288">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="24261-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) al recargar la configuración o para acceder a todos los proveedores.</span><span class="sxs-lookup"><span data-stu-id="24261-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="24261-290">Ninguna de estas situaciones son comunes.</span><span class="sxs-lookup"><span data-stu-id="24261-290">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="24261-291">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="24261-291">Additional resources</span></span>

* [<span data-ttu-id="24261-292">Opciones</span><span class="sxs-lookup"><span data-stu-id="24261-292">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="24261-293">Trabajar con varios entornos</span><span class="sxs-lookup"><span data-stu-id="24261-293">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="24261-294">Almacenamiento seguro de secretos de aplicación durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="24261-294">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="24261-295">Hospedar en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24261-295">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="24261-296">Inserción de dependencias</span><span class="sxs-lookup"><span data-stu-id="24261-296">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="24261-297">Proveedor de configuración de Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="24261-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
