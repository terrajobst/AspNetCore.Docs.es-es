---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 288f8ba5b45cdecd8c9eae060fee2c2c25dec7f9
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893251"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="6485e-103">Configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6485e-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="6485e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="6485e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6485e-105">La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*.</span><span class="sxs-lookup"><span data-stu-id="6485e-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="6485e-106">Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="6485e-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="6485e-107">Azure Key Vault</span></span>
* <span data-ttu-id="6485e-108">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-108">Command-line arguments</span></span>
* <span data-ttu-id="6485e-109">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="6485e-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="6485e-110">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="6485e-110">Directory files</span></span>
* <span data-ttu-id="6485e-111">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-111">Environment variables</span></span>
* <span data-ttu-id="6485e-112">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-112">In-memory .NET objects</span></span>
* <span data-ttu-id="6485e-113">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="6485e-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="6485e-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="6485e-114">Azure Key Vault</span></span>
* <span data-ttu-id="6485e-115">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-115">Command-line arguments</span></span>
* <span data-ttu-id="6485e-116">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="6485e-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="6485e-117">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-117">Environment variables</span></span>
* <span data-ttu-id="6485e-118">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-118">In-memory .NET objects</span></span>
* <span data-ttu-id="6485e-119">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="6485e-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="6485e-120">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-120">Command-line arguments</span></span>
* <span data-ttu-id="6485e-121">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="6485e-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="6485e-122">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-122">Environment variables</span></span>
* <span data-ttu-id="6485e-123">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-123">In-memory .NET objects</span></span>
* <span data-ttu-id="6485e-124">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="6485e-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="6485e-125">El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="6485e-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="6485e-126">Las opciones usan clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="6485e-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="6485e-127">Para más información sobre cómo usar el patrón de opciones, consulte <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="6485e-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="6485e-128">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6485e-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="6485e-129">Los ejemplos proporcionados en este tema dependen de lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6485e-129">The examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="6485e-130">Establecer la ruta base de la aplicación con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-130">Setting the base path of the app with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="6485e-131">`SetBasePath` está disponible para una aplicación mediante una referencia al paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/).</span><span class="sxs-lookup"><span data-stu-id="6485e-131">`SetBasePath` is made available to an app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="6485e-132">Resolver las secciones de los archivos de configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-132">Resolving sections of configuration files with <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*>.</span></span> <span data-ttu-id="6485e-133">`GetSection` está disponible para una aplicación mediante una referencia al paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/).</span><span class="sxs-lookup"><span data-stu-id="6485e-133">`GetSection` is made available to an app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="6485e-134">Enlazar la configuración a clases .NET con <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> y [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span><span class="sxs-lookup"><span data-stu-id="6485e-134">Binding configuration to .NET classes with <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> and [Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*).</span></span> <span data-ttu-id="6485e-135">`Bind` y `Get<T>` están disponibles para una aplicación mediante una referencia al paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/).</span><span class="sxs-lookup"><span data-stu-id="6485e-135">`Bind` and `Get<T>` are made available to an app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span> <span data-ttu-id="6485e-136">`Get<T>` está disponible en ASP.NET Core 1.1 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="6485e-136">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="6485e-137">Estos tres paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6485e-137">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="6485e-138">Estos tres paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="6485e-138">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="6485e-139">Configuración de host frente a configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="6485e-139">Host vs. app configuration</span></span>

<span data-ttu-id="6485e-140">Antes de configurar e iniciar la aplicación, se configura e inicia un *host*.</span><span class="sxs-lookup"><span data-stu-id="6485e-140">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="6485e-141">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-141">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="6485e-142">Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="6485e-142">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="6485e-143">Los pares clave-valor de la configuración de host se vuelven parte de la configuración global de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-143">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="6485e-144">Para más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte <xref:fundamentals/host/index>.</span><span class="sxs-lookup"><span data-stu-id="6485e-144">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/host/index>.</span></span>

## <a name="security"></a><span data-ttu-id="6485e-145">Seguridad</span><span class="sxs-lookup"><span data-stu-id="6485e-145">Security</span></span>

<span data-ttu-id="6485e-146">Adopte estos procedimientos recomendados:</span><span class="sxs-lookup"><span data-stu-id="6485e-146">Adopt the following best practices:</span></span>

* <span data-ttu-id="6485e-147">Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="6485e-147">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="6485e-148">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="6485e-148">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="6485e-149">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="6485e-149">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="6485e-150">Obtenga más información sobre [cómo usar varios entornos](xref:fundamentals/environments) y cómo administrar el [almacenamiento seguro de secretos de aplicación en el desarrollo con el Administrador de secretos](xref:security/app-secrets) (incluye consejos sobre el uso de variables de entorno para almacenar información confidencial).</span><span class="sxs-lookup"><span data-stu-id="6485e-150">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="6485e-151">El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local.</span><span class="sxs-lookup"><span data-stu-id="6485e-151">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="6485e-152">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="6485e-152">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="6485e-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) es una opción para el almacenamiento seguro de los secretos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-153">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="6485e-154">Para obtener más información, vea <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="6485e-154">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="6485e-155">Datos de configuración jerárquica</span><span class="sxs-lookup"><span data-stu-id="6485e-155">Hierarchical configuration data</span></span>

<span data-ttu-id="6485e-156">La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-156">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="6485e-157">En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:</span><span class="sxs-lookup"><span data-stu-id="6485e-157">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  }
}
```

<span data-ttu-id="6485e-158">Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-158">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="6485e-159">Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:</span><span class="sxs-lookup"><span data-stu-id="6485e-159">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="6485e-160">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-160">section0:key0</span></span>
* <span data-ttu-id="6485e-161">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-161">section0:key1</span></span>
* <span data-ttu-id="6485e-162">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-162">section1:key0</span></span>
* <span data-ttu-id="6485e-163">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-163">section1:key1</span></span>

<span data-ttu-id="6485e-164">Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-164"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="6485e-165">Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="6485e-165">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="6485e-166">Convenciones</span><span class="sxs-lookup"><span data-stu-id="6485e-166">Conventions</span></span>

<span data-ttu-id="6485e-167">Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-167">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="6485e-168">Los proveedores de configuración de archivo tienen la capacidad de recargar la configuración cuando se modifica un archivo de configuración subyacente después del inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-168">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="6485e-169">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="6485e-169">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="6485e-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor [Inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-170"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="6485e-171">Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.</span><span class="sxs-lookup"><span data-stu-id="6485e-171">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="6485e-172">Las claves de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="6485e-172">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="6485e-173">Las claves no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="6485e-173">Keys are case-insensitive.</span></span> <span data-ttu-id="6485e-174">Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.</span><span class="sxs-lookup"><span data-stu-id="6485e-174">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="6485e-175">Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.</span><span class="sxs-lookup"><span data-stu-id="6485e-175">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="6485e-176">Claves jerárquicas</span><span class="sxs-lookup"><span data-stu-id="6485e-176">Hierarchical keys</span></span>
  * <span data-ttu-id="6485e-177">Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="6485e-177">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="6485e-178">En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="6485e-178">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="6485e-179">Un guion bajo doble (`__`) es compatible con todas las plataformas y se convierte en un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="6485e-179">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="6485e-180">En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="6485e-180">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="6485e-181">Debe proporcionar código para reemplazar los guiones por dos puntos cuando los secretos se cargan en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-181">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="6485e-182"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-182">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="6485e-183">El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="6485e-183">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="6485e-184">Los valores de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="6485e-184">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="6485e-185">Los valores son cadenas.</span><span class="sxs-lookup"><span data-stu-id="6485e-185">Values are strings.</span></span>
* <span data-ttu-id="6485e-186">Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.</span><span class="sxs-lookup"><span data-stu-id="6485e-186">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="6485e-187">Proveedores</span><span class="sxs-lookup"><span data-stu-id="6485e-187">Providers</span></span>

<span data-ttu-id="6485e-188">La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6485e-188">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="6485e-189">Proveedor</span><span class="sxs-lookup"><span data-stu-id="6485e-189">Provider</span></span> | <span data-ttu-id="6485e-190">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="6485e-190">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="6485e-191">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="6485e-191">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="6485e-192">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="6485e-192">Azure Key Vault</span></span> |
| [<span data-ttu-id="6485e-193">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-193">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="6485e-194">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-194">Command-line parameters</span></span> |
| [<span data-ttu-id="6485e-195">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="6485e-195">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="6485e-196">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="6485e-196">Custom source</span></span> |
| [<span data-ttu-id="6485e-197">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-197">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="6485e-198">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-198">Environment variables</span></span> |
| [<span data-ttu-id="6485e-199">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="6485e-199">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="6485e-200">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="6485e-200">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="6485e-201">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="6485e-201">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="6485e-202">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="6485e-202">Directory files</span></span> |
| [<span data-ttu-id="6485e-203">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-203">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="6485e-204">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-204">In-memory collections</span></span> |
| <span data-ttu-id="6485e-205">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="6485e-205">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="6485e-206">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="6485e-206">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="6485e-207">Proveedor</span><span class="sxs-lookup"><span data-stu-id="6485e-207">Provider</span></span> | <span data-ttu-id="6485e-208">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="6485e-208">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="6485e-209">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="6485e-209">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="6485e-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="6485e-210">Azure Key Vault</span></span> |
| [<span data-ttu-id="6485e-211">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="6485e-212">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-212">Command-line parameters</span></span> |
| [<span data-ttu-id="6485e-213">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="6485e-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="6485e-214">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="6485e-214">Custom source</span></span> |
| [<span data-ttu-id="6485e-215">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-215">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="6485e-216">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-216">Environment variables</span></span> |
| [<span data-ttu-id="6485e-217">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="6485e-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="6485e-218">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="6485e-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="6485e-219">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-219">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="6485e-220">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-220">In-memory collections</span></span> |
| <span data-ttu-id="6485e-221">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="6485e-221">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="6485e-222">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="6485e-222">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="6485e-223">Proveedor</span><span class="sxs-lookup"><span data-stu-id="6485e-223">Provider</span></span> | <span data-ttu-id="6485e-224">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="6485e-224">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="6485e-225">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-225">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="6485e-226">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-226">Command-line parameters</span></span> |
| [<span data-ttu-id="6485e-227">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="6485e-227">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="6485e-228">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="6485e-228">Custom source</span></span> |
| [<span data-ttu-id="6485e-229">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-229">Environment variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="6485e-230">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-230">Environment variables</span></span> |
| [<span data-ttu-id="6485e-231">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="6485e-231">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="6485e-232">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="6485e-232">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="6485e-233">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-233">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="6485e-234">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-234">In-memory collections</span></span> |
| <span data-ttu-id="6485e-235">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="6485e-235">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="6485e-236">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="6485e-236">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="6485e-237">Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="6485e-237">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="6485e-238">En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en que el código podría organizarlos.</span><span class="sxs-lookup"><span data-stu-id="6485e-238">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="6485e-239">Ordene los proveedores de configuración en el código para cumplir con sus prioridades relacionadas con los orígenes de configuración subyacentes.</span><span class="sxs-lookup"><span data-stu-id="6485e-239">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="6485e-240">Esta es una secuencia típica de proveedores de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-240">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="6485e-241">Archivos (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, donde `<Environment>` es el entorno de hospedaje actual de la aplicación)</span><span class="sxs-lookup"><span data-stu-id="6485e-241">Files (*appsettings.json*, *appsettings.&lt;Environment&gt;.json*, where `<Environment>` is the app's current hosting environment)</span></span>
1. <span data-ttu-id="6485e-242">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (solo en el entorno de desarrollo)</span><span class="sxs-lookup"><span data-stu-id="6485e-242">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="6485e-243">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-243">Environment variables</span></span>
1. <span data-ttu-id="6485e-244">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-244">Command-line arguments</span></span>

<span data-ttu-id="6485e-245">Una práctica común es colocar el proveedor de configuración de línea de comandos al final de en una serie de proveedores para permitir que los argumentos de la línea de comandos invaliden la configuración que establecieron los demás proveedores.</span><span class="sxs-lookup"><span data-stu-id="6485e-245">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-246">Esta secuencia de los proveedores se aplica al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-246">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6485e-247">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6485e-247">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6485e-248">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host web para especificar los proveedores de configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="6485e-248">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the Web Host to specify the app's configuration providers:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="6485e-249">`ConfigureAppConfiguration` *está disponible en ASP.NET Core 2.1 o versiones posteriores.*</span><span class="sxs-lookup"><span data-stu-id="6485e-249">`ConfigureAppConfiguration` *is available in ASP.NET Core 2.1 or later.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-250">Esta secuencia de proveedores se puede crear para la aplicación (no para el host) con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> y una llamada a su método <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6485e-250">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

```csharp
public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(Directory.GetCurrentDirectory())
        .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, 
            reloadOnChange: true);

    var appAssembly = Assembly.Load(new AssemblyName(env.ApplicationName));

    if (appAssembly != null)
    {
        builder.AddUserSecrets(appAssembly, optional: true);
    }

    builder.AddEnvironmentVariables();

    Configuration = builder.Build();
}

public IConfiguration Configuration { get; }

public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton<IConfiguration>(Configuration);
}
```

<span data-ttu-id="6485e-251">En el ejemplo anterior, <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> proporciona el nombre del entorno (`env.EnvironmentName`) y el nombre del ensamblado de la aplicación (`env.ApplicationName`).</span><span class="sxs-lookup"><span data-stu-id="6485e-251">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="6485e-252">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="6485e-252">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="6485e-253">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="6485e-253">Command-line Configuration Provider</span></span>

<span data-ttu-id="6485e-254"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="6485e-254">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-255">Para activar la configuración de línea de comandos, llame al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-255">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-256">`AddCommandLine` se llama automáticamente cuando se inicializa un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-256">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6485e-257">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6485e-257">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6485e-258">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="6485e-258">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6485e-259">Configuración opcional de *appsettings.json* y *appsettings.&lt; Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="6485e-259">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="6485e-260">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="6485e-260">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6485e-261">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="6485e-261">Environment variables.</span></span>

<span data-ttu-id="6485e-262">`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final.</span><span class="sxs-lookup"><span data-stu-id="6485e-262">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="6485e-263">Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="6485e-263">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="6485e-264">`CreateDefaultBuilder` actúa cuando se construye el host.</span><span class="sxs-lookup"><span data-stu-id="6485e-264">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="6485e-265">Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.</span><span class="sxs-lookup"><span data-stu-id="6485e-265">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="6485e-266">Al crear el host manualmente y no mediante una llamada a `CreateDefaultBuilder`, llame al método de extensión `AddCommandLine` en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="6485e-266">When building the host manually and not calling `CreateDefaultBuilder`, call the `AddCommandLine` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-267">Para activar la configuración de línea de comandos, llame al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-267">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-268">Llame en último lugar al proveedor para permitir que los argumentos de la línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-268">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="6485e-269">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="6485e-269">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6485e-270">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="6485e-270">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-271">La aplicación de ejemplo 2.x aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-271">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-272">La aplicación de ejemplo 1.x llama a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-272">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="6485e-273">Abra un símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="6485e-273">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="6485e-274">Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="6485e-274">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="6485e-275">Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6485e-275">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6485e-276">Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6485e-276">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="6485e-277">Argumentos</span><span class="sxs-lookup"><span data-stu-id="6485e-277">Arguments</span></span>

<span data-ttu-id="6485e-278">El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio.</span><span class="sxs-lookup"><span data-stu-id="6485e-278">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="6485e-279">El valor puede ser NULL si se usa un signo igual (por ejemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="6485e-279">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="6485e-280">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="6485e-280">Key prefix</span></span>               | <span data-ttu-id="6485e-281">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="6485e-281">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="6485e-282">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="6485e-282">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="6485e-283">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="6485e-283">Two dashes (`--`)</span></span>        | <span data-ttu-id="6485e-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="6485e-284">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="6485e-285">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="6485e-285">Forward slash (`/`)</span></span>      | <span data-ttu-id="6485e-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="6485e-286">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="6485e-287">Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.</span><span class="sxs-lookup"><span data-stu-id="6485e-287">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="6485e-288">Comandos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="6485e-288">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value --CommandLineKey2=value /CommandLineKey2=value
dotnet run --CommandLineKey1 value /CommandLineKey2 value
dotnet run CommandLineKey1= CommandLineKey2=value
```

### <a name="switch-mappings"></a><span data-ttu-id="6485e-289">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="6485e-289">Switch mappings</span></span>

<span data-ttu-id="6485e-290">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="6485e-290">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="6485e-291">Cuando crea manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, puede proporcionar un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-291">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="6485e-292">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="6485e-292">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="6485e-293">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-293">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="6485e-294">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="6485e-294">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="6485e-295">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="6485e-295">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="6485e-296">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="6485e-296">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="6485e-297">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="6485e-297">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var switchMappings = new Dictionary<string, string>
            {
                { "-CLKey1", "CommandLineKey1" },
                { "-CLKey2", "CommandLineKey2" }
            };

        var config = new ConfigurationBuilder()
            .AddCommandLine(args, switchMappings)
            .Build();

        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6485e-298">Como se muestra en el ejemplo anterior, la llamada a `CreateDefaultBuilder` no debería pasar argumentos cuando se usan las asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="6485e-298">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="6485e-299">La llamada de `AddCommandLine` del método `CreateDefaultBuilder` no incluye modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6485e-299">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="6485e-300">Si los argumentos incluyen un conmutador asignado y se pasan a `CreateDefaultBuilder`, el proveedor de `AddCommandLine` no se puede inicializar con una <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="6485e-300">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="6485e-301">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="6485e-301">The solution is not to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public static void Main(string[] args)
{
    var switchMappings = new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    var config = new ConfigurationBuilder()
        .AddCommandLine(args, switchMappings)
        .Build();

    var host = new WebHostBuilder()
        .UseConfiguration(config)
        .UseKestrel()
        .UseStartup<Startup>()
        .Start();

    using (host)
    {
        Console.ReadLine();
    }
}
```

::: moniker-end

<span data-ttu-id="6485e-302">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="6485e-302">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="6485e-303">Key</span><span class="sxs-lookup"><span data-stu-id="6485e-303">Key</span></span>       | <span data-ttu-id="6485e-304">Valor</span><span class="sxs-lookup"><span data-stu-id="6485e-304">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="6485e-305">Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:</span><span class="sxs-lookup"><span data-stu-id="6485e-305">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="6485e-306">Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="6485e-306">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="6485e-307">Key</span><span class="sxs-lookup"><span data-stu-id="6485e-307">Key</span></span>               | <span data-ttu-id="6485e-308">Valor</span><span class="sxs-lookup"><span data-stu-id="6485e-308">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="6485e-309">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-309">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="6485e-310"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="6485e-310">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="6485e-311">Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-311">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-312">Al trabajar con claves jerárquicas en variables de entorno, es posible que un separador de dos puntos (`:`) no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="6485e-312">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="6485e-313">Un guion bajo doble (`__`) es compatible con todas las plataformas y lo reemplaza un separador de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="6485e-313">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="6485e-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer las variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="6485e-314">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="6485e-315">Para más información, consulte el artículo sobre [Azure Apps: Invalidación de la configuración de la aplicación con Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="6485e-315">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-316">`AddEnvironmentVariables` se llama automáticamente cuando se inicializa un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-316">`AddEnvironmentVariables` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6485e-317">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6485e-317">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6485e-318">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="6485e-318">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6485e-319">Configuración opcional de *appsettings.json* y *appsettings.&lt; Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="6485e-319">Optional configuration from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>
* <span data-ttu-id="6485e-320">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="6485e-320">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6485e-321">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="6485e-321">Command-line arguments.</span></span>

<span data-ttu-id="6485e-322">El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="6485e-322">The Environment Variable Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="6485e-323">Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="6485e-323">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="6485e-324">Puede llamar directamente al método de extensión `AddEnvironmentVariables` en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span><span class="sxs-lookup"><span data-stu-id="6485e-324">You can also directly call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-325">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="6485e-325">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6485e-326">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="6485e-326">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-327">La aplicación de ejemplo 2.x aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6485e-327">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-328">La aplicación de ejemplo 1.x llama a `AddEnvironmentVariables` en `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6485e-328">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="6485e-329">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="6485e-329">Run the sample app.</span></span> <span data-ttu-id="6485e-330">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6485e-330">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6485e-331">Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="6485e-331">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="6485e-332">El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.</span><span class="sxs-lookup"><span data-stu-id="6485e-332">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="6485e-333">Para que la lista de variables de entorno que muestra la aplicación sea breve, la aplicación filtra las variables de entorno que comienzan con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6485e-333">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="6485e-334">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="6485e-334">ASPNETCORE_</span></span>
* <span data-ttu-id="6485e-335">urls</span><span class="sxs-lookup"><span data-stu-id="6485e-335">urls</span></span>
* <span data-ttu-id="6485e-336">Registro</span><span class="sxs-lookup"><span data-stu-id="6485e-336">Logging</span></span>
* <span data-ttu-id="6485e-337">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="6485e-337">ENVIRONMENT</span></span>
* <span data-ttu-id="6485e-338">contentRoot</span><span class="sxs-lookup"><span data-stu-id="6485e-338">contentRoot</span></span>
* <span data-ttu-id="6485e-339">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="6485e-339">AllowedHosts</span></span>
* <span data-ttu-id="6485e-340">applicationName</span><span class="sxs-lookup"><span data-stu-id="6485e-340">applicationName</span></span>
* <span data-ttu-id="6485e-341">CommandLine</span><span class="sxs-lookup"><span data-stu-id="6485e-341">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-342">Si quiere exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6485e-342">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-343">Si quiere exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Controllers/HomeController.cs* a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6485e-343">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="6485e-344">Prefijos</span><span class="sxs-lookup"><span data-stu-id="6485e-344">Prefixes</span></span>

<span data-ttu-id="6485e-345">Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6485e-345">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="6485e-346">Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-346">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="6485e-347">El prefijo se quita cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-347">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-348">El método de conveniencia estático `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> para establecer el host de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-348">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="6485e-349">Cuando se crea `WebHostBuilder`, encuentra su configuración de host en variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="6485e-349">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="6485e-350">**Prefijos de cadena de conexión**</span><span class="sxs-lookup"><span data-stu-id="6485e-350">**Connection string prefixes**</span></span>

<span data-ttu-id="6485e-351">La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-351">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="6485e-352">Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="6485e-352">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="6485e-353">Prefijo de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="6485e-353">Connection string prefix</span></span> | <span data-ttu-id="6485e-354">Proveedor</span><span class="sxs-lookup"><span data-stu-id="6485e-354">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="6485e-355">Proveedor personalizado</span><span class="sxs-lookup"><span data-stu-id="6485e-355">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="6485e-356">MySQL</span><span class="sxs-lookup"><span data-stu-id="6485e-356">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="6485e-357">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="6485e-357">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="6485e-358">SQL Server</span><span class="sxs-lookup"><span data-stu-id="6485e-358">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="6485e-359">Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:</span><span class="sxs-lookup"><span data-stu-id="6485e-359">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="6485e-360">La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="6485e-360">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="6485e-361">Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).</span><span class="sxs-lookup"><span data-stu-id="6485e-361">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="6485e-362">Clave de variable de entorno</span><span class="sxs-lookup"><span data-stu-id="6485e-362">Environment variable key</span></span> | <span data-ttu-id="6485e-363">Clave de configuración convertida</span><span class="sxs-lookup"><span data-stu-id="6485e-363">Converted configuration key</span></span> | <span data-ttu-id="6485e-364">Entrada de configuración del proveedor</span><span class="sxs-lookup"><span data-stu-id="6485e-364">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6485e-365">Entrada de configuración no creada.</span><span class="sxs-lookup"><span data-stu-id="6485e-365">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6485e-366">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6485e-366">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6485e-367">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="6485e-367">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6485e-368">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6485e-368">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6485e-369">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="6485e-369">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="6485e-370">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="6485e-370">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="6485e-371">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="6485e-371">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="6485e-372">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="6485e-372">File Configuration Provider</span></span>

<span data-ttu-id="6485e-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="6485e-373"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="6485e-374">Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:</span><span class="sxs-lookup"><span data-stu-id="6485e-374">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="6485e-375">Proveedor de configuración INI</span><span class="sxs-lookup"><span data-stu-id="6485e-375">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="6485e-376">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="6485e-376">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="6485e-377">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="6485e-377">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="6485e-378">Proveedor de configuración de INI</span><span class="sxs-lookup"><span data-stu-id="6485e-378">INI Configuration Provider</span></span>

<span data-ttu-id="6485e-379"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="6485e-379">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="6485e-380">Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-380">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-381">Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.</span><span class="sxs-lookup"><span data-stu-id="6485e-381">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="6485e-382">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="6485e-382">Overloads permit specifying:</span></span>

* <span data-ttu-id="6485e-383">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="6485e-383">Whether the file is optional.</span></span>
* <span data-ttu-id="6485e-384">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="6485e-384">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6485e-385"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="6485e-385">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-386">Al llamar a `CreateDefaultBuilder`, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-386">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddIniFile("config.ini", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6485e-387">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-388">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="6485e-388">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddIniFile("config.ini", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6485e-389">Un ejemplo genérico de un archivo de configuración INI:</span><span class="sxs-lookup"><span data-stu-id="6485e-389">A generic example of an INI configuration file:</span></span>

```ini
[section0]
key0=value
key1=value

[section1]
subsection:key=value

[section2:subsection0]
key=value

[section2:subsection1]
key=value
```

<span data-ttu-id="6485e-390">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="6485e-390">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6485e-391">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-391">section0:key0</span></span>
* <span data-ttu-id="6485e-392">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-392">section0:key1</span></span>
* <span data-ttu-id="6485e-393">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="6485e-393">section1:subsection:key</span></span>
* <span data-ttu-id="6485e-394">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="6485e-394">section2:subsection0:key</span></span>
* <span data-ttu-id="6485e-395">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="6485e-395">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="6485e-396">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="6485e-396">JSON Configuration Provider</span></span>

<span data-ttu-id="6485e-397"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="6485e-397">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="6485e-398">Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-398">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-399">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="6485e-399">Overloads permit specifying:</span></span>

* <span data-ttu-id="6485e-400">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="6485e-400">Whether the file is optional.</span></span>
* <span data-ttu-id="6485e-401">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="6485e-401">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6485e-402"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="6485e-402">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-403">`AddJsonFile` se llama automáticamente dos veces al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="6485e-403">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="6485e-404">Se llama al método para cargar la configuración desde:</span><span class="sxs-lookup"><span data-stu-id="6485e-404">The method is called to load configuration from:</span></span>

* <span data-ttu-id="6485e-405">*appsettings.json* &ndash; Este archivo se lee primero.</span><span class="sxs-lookup"><span data-stu-id="6485e-405">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="6485e-406">La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="6485e-406">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="6485e-407">*appsettings.&lt;Environment&gt;.json* &ndash; La versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="6485e-407">*appsettings.&lt;Environment&gt;.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="6485e-408">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="6485e-408">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="6485e-409">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="6485e-409">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="6485e-410">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="6485e-410">Environment variables.</span></span>
* <span data-ttu-id="6485e-411">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="6485e-411">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="6485e-412">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="6485e-412">Command-line arguments.</span></span>

<span data-ttu-id="6485e-413">El proveedor de configuración de JSON se establece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="6485e-413">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="6485e-414">Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="6485e-414">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="6485e-415">Puede llamar directamente al método de extensión `AddJsonFile` en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-415">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-416">Al llamar a `CreateDefaultBuilder`, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-416">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("config.json", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6485e-417">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-417">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-418">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="6485e-418">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("config.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6485e-419">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="6485e-419">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-420">La aplicación de ejemplo 2.x aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye dos llamadas a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="6485e-420">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="6485e-421">La configuración se carga desde *appsettings.json* y *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="6485e-421">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-422">La aplicación de ejemplo 1.x llama a `AddJsonFile` dos veces en un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6485e-422">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="6485e-423">La configuración se carga desde *appsettings.json* y *appsettings.&lt;Environment&gt;.json*.</span><span class="sxs-lookup"><span data-stu-id="6485e-423">Configuration is loaded from *appsettings.json* and *appsettings.&lt;Environment&gt;.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="6485e-424">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="6485e-424">Run the sample app.</span></span> <span data-ttu-id="6485e-425">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6485e-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="6485e-426">Observe que la salida contiene los pares clave-valor para la configuración que se muestra en la tabla según el entorno.</span><span class="sxs-lookup"><span data-stu-id="6485e-426">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="6485e-427">Las claves de configuración de registro usan dos puntos (`:`) como separador jerárquico.</span><span class="sxs-lookup"><span data-stu-id="6485e-427">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="6485e-428">Key</span><span class="sxs-lookup"><span data-stu-id="6485e-428">Key</span></span>                        | <span data-ttu-id="6485e-429">Valor de desarrollo</span><span class="sxs-lookup"><span data-stu-id="6485e-429">Development Value</span></span> | <span data-ttu-id="6485e-430">Valor de producción</span><span class="sxs-lookup"><span data-stu-id="6485e-430">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="6485e-431">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="6485e-431">Logging:LogLevel:System</span></span>    | <span data-ttu-id="6485e-432">Información</span><span class="sxs-lookup"><span data-stu-id="6485e-432">Information</span></span>       | <span data-ttu-id="6485e-433">Información</span><span class="sxs-lookup"><span data-stu-id="6485e-433">Information</span></span>      |
| <span data-ttu-id="6485e-434">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="6485e-434">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="6485e-435">Información</span><span class="sxs-lookup"><span data-stu-id="6485e-435">Information</span></span>       | <span data-ttu-id="6485e-436">Información</span><span class="sxs-lookup"><span data-stu-id="6485e-436">Information</span></span>      |
| <span data-ttu-id="6485e-437">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="6485e-437">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="6485e-438">Depuración</span><span class="sxs-lookup"><span data-stu-id="6485e-438">Debug</span></span>             | <span data-ttu-id="6485e-439">Error</span><span class="sxs-lookup"><span data-stu-id="6485e-439">Error</span></span>            |
| <span data-ttu-id="6485e-440">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="6485e-440">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="6485e-441">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="6485e-441">XML Configuration Provider</span></span>

<span data-ttu-id="6485e-442"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="6485e-442">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="6485e-443">Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-443">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-444">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="6485e-444">Overloads permit specifying:</span></span>

* <span data-ttu-id="6485e-445">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="6485e-445">Whether the file is optional.</span></span>
* <span data-ttu-id="6485e-446">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="6485e-446">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="6485e-447"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="6485e-447">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="6485e-448">El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-448">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="6485e-449">No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.</span><span class="sxs-lookup"><span data-stu-id="6485e-449">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-450">Al llamar a `CreateDefaultBuilder`, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-450">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6485e-451">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-451">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-452">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="6485e-452">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddXmlFile("config.xml", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="6485e-453">Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:</span><span class="sxs-lookup"><span data-stu-id="6485e-453">XML configuration files can use distinct element names for repeating sections:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section0>
    <key0>value</key0>
    <key1>value</key1>
  </section0>
  <section1>
    <key0>value</key0>
    <key1>value</key1>
  </section1>
</configuration>
```

<span data-ttu-id="6485e-454">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="6485e-454">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6485e-455">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-455">section0:key0</span></span>
* <span data-ttu-id="6485e-456">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-456">section0:key1</span></span>
* <span data-ttu-id="6485e-457">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-457">section1:key0</span></span>
* <span data-ttu-id="6485e-458">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-458">section1:key1</span></span>

<span data-ttu-id="6485e-459">Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:</span><span class="sxs-lookup"><span data-stu-id="6485e-459">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <section name="section0">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
  <section name="section1">
    <key name="key0">value</key>
    <key name="key1">value</key>
  </section>
</configuration>
```

<span data-ttu-id="6485e-460">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="6485e-460">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6485e-461">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-461">section:section0:key:key0</span></span>
* <span data-ttu-id="6485e-462">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-462">section:section0:key:key1</span></span>
* <span data-ttu-id="6485e-463">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-463">section:section1:key:key0</span></span>
* <span data-ttu-id="6485e-464">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-464">section:section1:key:key1</span></span>

<span data-ttu-id="6485e-465">Los atributos se pueden usar para suministrar valores:</span><span class="sxs-lookup"><span data-stu-id="6485e-465">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="6485e-466">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="6485e-466">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="6485e-467">key:attribute</span><span class="sxs-lookup"><span data-stu-id="6485e-467">key:attribute</span></span>
* <span data-ttu-id="6485e-468">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="6485e-468">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="6485e-469">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="6485e-469">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="6485e-470"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-470">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="6485e-471">La clave es el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="6485e-471">The key is the file name.</span></span> <span data-ttu-id="6485e-472">El valor contiene el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="6485e-472">The value contains the file's contents.</span></span> <span data-ttu-id="6485e-473">El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.</span><span class="sxs-lookup"><span data-stu-id="6485e-473">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="6485e-474">Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-474">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="6485e-475">La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="6485e-475">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="6485e-476">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="6485e-476">Overloads permit specifying:</span></span>

* <span data-ttu-id="6485e-477">Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.</span><span class="sxs-lookup"><span data-stu-id="6485e-477">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="6485e-478">Si el directorio es opcional y la ruta de acceso al directorio.</span><span class="sxs-lookup"><span data-stu-id="6485e-478">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="6485e-479">Al llamar a `CreateDefaultBuilder`, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-479">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
        var config = new ConfigurationBuilder()
            .AddKeyPerFile(directoryPath: path, optional: true)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6485e-480">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-480">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

```csharp
var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
var config = new ConfigurationBuilder()
    .AddKeyPerFile(directoryPath: path, optional: true)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

::: moniker-end

## <a name="memory-configuration-provider"></a><span data-ttu-id="6485e-481">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="6485e-481">Memory Configuration Provider</span></span>

<span data-ttu-id="6485e-482"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-482">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="6485e-483">Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="6485e-483">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="6485e-484">El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="6485e-484">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-485">Al llamar a `CreateDefaultBuilder`, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-485">When calling `CreateDefaultBuilder`, call `UseConfiguration` with the configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

        var config = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="6485e-486">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a `UseConfiguration` con la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-486">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call `UseConfiguration` with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-487">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método `UseConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="6485e-487">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the `UseConfiguration` method:</span></span>

::: moniker-end

```csharp
var dict = new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };

var config = new ConfigurationBuilder()
    .AddInMemoryCollection(dict)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

## <a name="getvalue"></a><span data-ttu-id="6485e-488">GetValue</span><span class="sxs-lookup"><span data-stu-id="6485e-488">GetValue</span></span>

<span data-ttu-id="6485e-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un valor de la configuración con una clave especificada y lo convierte al tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="6485e-489">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="6485e-490">Una sobrecarga permite proporcionar un valor predeterminado si no se encuentra la clave.</span><span class="sxs-lookup"><span data-stu-id="6485e-490">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="6485e-491">El ejemplo siguiente extrae el valor de cadena desde la configuración con la clave `NumberKey`, escribe el valor como `int` y almacena el valor en la variable `intValue`.</span><span class="sxs-lookup"><span data-stu-id="6485e-491">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="6485e-492">Si `NumberKey` no se encuentra en las claves de configuración, `intValue` recibe el valor predeterminado de `99`:</span><span class="sxs-lookup"><span data-stu-id="6485e-492">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="6485e-493">GetSection, GetChildren y Exists</span><span class="sxs-lookup"><span data-stu-id="6485e-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="6485e-494">Para los ejemplos siguientes, considere el siguiente archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="6485e-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="6485e-495">Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:</span><span class="sxs-lookup"><span data-stu-id="6485e-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

```json
{
  "section0": {
    "key0": "value",
    "key1": "value"
  },
  "section1": {
    "key0": "value",
    "key1": "value"
  },
  "section2": {
    "subsection0" : {
      "key0": "value",
      "key1": "value"
    },
    "subsection1" : {
      "key0": "value",
      "key1": "value"
    }
  }
}
```

<span data-ttu-id="6485e-496">Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="6485e-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-497">section0:key0</span></span>
* <span data-ttu-id="6485e-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-498">section0:key1</span></span>
* <span data-ttu-id="6485e-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-499">section1:key0</span></span>
* <span data-ttu-id="6485e-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-500">section1:key1</span></span>
* <span data-ttu-id="6485e-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="6485e-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="6485e-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="6485e-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="6485e-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="6485e-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="6485e-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="6485e-505">GetSection</span></span>

<span data-ttu-id="6485e-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.</span><span class="sxs-lookup"><span data-stu-id="6485e-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="6485e-507">Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:</span><span class="sxs-lookup"><span data-stu-id="6485e-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="6485e-508">De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:</span><span class="sxs-lookup"><span data-stu-id="6485e-508">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="6485e-509">`GetSection` nunca devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="6485e-509">`GetSection` never returns `null`.</span></span> <span data-ttu-id="6485e-510">Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.</span><span class="sxs-lookup"><span data-stu-id="6485e-510">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

### <a name="getchildren"></a><span data-ttu-id="6485e-511">GetChildren</span><span class="sxs-lookup"><span data-stu-id="6485e-511">GetChildren</span></span>

<span data-ttu-id="6485e-512">Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:</span><span class="sxs-lookup"><span data-stu-id="6485e-512">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="6485e-513">Existe</span><span class="sxs-lookup"><span data-stu-id="6485e-513">Exists</span></span>

<span data-ttu-id="6485e-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-514">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="6485e-515">Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-515">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="6485e-516">Enlace a una clase</span><span class="sxs-lookup"><span data-stu-id="6485e-516">Bind to a class</span></span>

<span data-ttu-id="6485e-517">La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="6485e-517">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="6485e-518">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="6485e-518">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="6485e-519">Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="6485e-519">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span>

<span data-ttu-id="6485e-520">La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="6485e-520">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-521">La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-521">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="6485e-522">Se crean los siguientes pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-522">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="6485e-523">Key</span><span class="sxs-lookup"><span data-stu-id="6485e-523">Key</span></span>                   | <span data-ttu-id="6485e-524">Valor</span><span class="sxs-lookup"><span data-stu-id="6485e-524">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="6485e-525">starship:name</span><span class="sxs-lookup"><span data-stu-id="6485e-525">starship:name</span></span>         | <span data-ttu-id="6485e-526">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="6485e-526">USS Enterprise</span></span>                                    |
| <span data-ttu-id="6485e-527">starship:registry</span><span class="sxs-lookup"><span data-stu-id="6485e-527">starship:registry</span></span>     | <span data-ttu-id="6485e-528">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="6485e-528">NCC-1701</span></span>                                          |
| <span data-ttu-id="6485e-529">starship:class</span><span class="sxs-lookup"><span data-stu-id="6485e-529">starship:class</span></span>        | <span data-ttu-id="6485e-530">Constitution</span><span class="sxs-lookup"><span data-stu-id="6485e-530">Constitution</span></span>                                      |
| <span data-ttu-id="6485e-531">starship:length</span><span class="sxs-lookup"><span data-stu-id="6485e-531">starship:length</span></span>       | <span data-ttu-id="6485e-532">304.8</span><span class="sxs-lookup"><span data-stu-id="6485e-532">304.8</span></span>                                             |
| <span data-ttu-id="6485e-533">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="6485e-533">starship:commissioned</span></span> | <span data-ttu-id="6485e-534">False</span><span class="sxs-lookup"><span data-stu-id="6485e-534">False</span></span>                                             |
| <span data-ttu-id="6485e-535">trademark</span><span class="sxs-lookup"><span data-stu-id="6485e-535">trademark</span></span>             | <span data-ttu-id="6485e-536">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="6485e-536">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="6485e-537">La aplicación de ejemplo llama a `GetSection` con la clave `starship`.</span><span class="sxs-lookup"><span data-stu-id="6485e-537">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="6485e-538">Los pares clave-valor `starship` están aislados.</span><span class="sxs-lookup"><span data-stu-id="6485e-538">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="6485e-539">El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`.</span><span class="sxs-lookup"><span data-stu-id="6485e-539">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="6485e-540">Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="6485e-540">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="6485e-541">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="6485e-541">Bind to an object graph</span></span>

<span data-ttu-id="6485e-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO.</span><span class="sxs-lookup"><span data-stu-id="6485e-542"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span>

<span data-ttu-id="6485e-543">El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="6485e-543">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-544">La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-544">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="6485e-545">Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="6485e-545">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="6485e-546">La instancia enlazada se asigna a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="6485e-546">The bound instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
viewModel.TvShow = tvShow;
```

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="6485e-547">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="6485e-547">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="6485e-548">`Get<T>` es más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="6485e-548">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="6485e-549">El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:</span><span class="sxs-lookup"><span data-stu-id="6485e-549">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="6485e-550">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="6485e-550">Bind an array to a class</span></span>

<span data-ttu-id="6485e-551">*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*</span><span class="sxs-lookup"><span data-stu-id="6485e-551">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="6485e-552"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-552">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="6485e-553">Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.</span><span class="sxs-lookup"><span data-stu-id="6485e-553">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="6485e-554">El enlace se proporciona por convención.</span><span class="sxs-lookup"><span data-stu-id="6485e-554">Binding is provided by convention.</span></span> <span data-ttu-id="6485e-555">Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.</span><span class="sxs-lookup"><span data-stu-id="6485e-555">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="6485e-556">**Procesamiento de matriz en memoria**</span><span class="sxs-lookup"><span data-stu-id="6485e-556">**In-memory array processing**</span></span>

<span data-ttu-id="6485e-557">Considere los valores y las claves de configuración que se muestran en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="6485e-557">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="6485e-558">Key</span><span class="sxs-lookup"><span data-stu-id="6485e-558">Key</span></span>     | <span data-ttu-id="6485e-559">Valor</span><span class="sxs-lookup"><span data-stu-id="6485e-559">Value</span></span>  |
| :-----: | :----: |
| <span data-ttu-id="6485e-560">array:0</span><span class="sxs-lookup"><span data-stu-id="6485e-560">array:0</span></span> | <span data-ttu-id="6485e-561">value0</span><span class="sxs-lookup"><span data-stu-id="6485e-561">value0</span></span> |
| <span data-ttu-id="6485e-562">array:1</span><span class="sxs-lookup"><span data-stu-id="6485e-562">array:1</span></span> | <span data-ttu-id="6485e-563">value1</span><span class="sxs-lookup"><span data-stu-id="6485e-563">value1</span></span> |
| <span data-ttu-id="6485e-564">array:2</span><span class="sxs-lookup"><span data-stu-id="6485e-564">array:2</span></span> | <span data-ttu-id="6485e-565">value2</span><span class="sxs-lookup"><span data-stu-id="6485e-565">value2</span></span> |
| <span data-ttu-id="6485e-566">array:4</span><span class="sxs-lookup"><span data-stu-id="6485e-566">array:4</span></span> | <span data-ttu-id="6485e-567">value4</span><span class="sxs-lookup"><span data-stu-id="6485e-567">value4</span></span> |
| <span data-ttu-id="6485e-568">array:5</span><span class="sxs-lookup"><span data-stu-id="6485e-568">array:5</span></span> | <span data-ttu-id="6485e-569">value5</span><span class="sxs-lookup"><span data-stu-id="6485e-569">value5</span></span> |

<span data-ttu-id="6485e-570">Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:</span><span class="sxs-lookup"><span data-stu-id="6485e-570">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="6485e-571">La matriz omite un valor de índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="6485e-571">The array skips a value for index &num;3.</span></span> <span data-ttu-id="6485e-572">El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.</span><span class="sxs-lookup"><span data-stu-id="6485e-572">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="6485e-573">En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:</span><span class="sxs-lookup"><span data-stu-id="6485e-573">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-574">Los datos de configuración están enlazados al objeto:</span><span class="sxs-lookup"><span data-stu-id="6485e-574">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="6485e-575">También se puede usar la sintaxis de [ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:</span><span class="sxs-lookup"><span data-stu-id="6485e-575">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="6485e-576">El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-576">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="6485e-577">Índice de `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="6485e-577">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="6485e-578">Valor de `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="6485e-578">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="6485e-579">0</span><span class="sxs-lookup"><span data-stu-id="6485e-579">0</span></span>                             | <span data-ttu-id="6485e-580">value0</span><span class="sxs-lookup"><span data-stu-id="6485e-580">value0</span></span>                        |
| <span data-ttu-id="6485e-581">1</span><span class="sxs-lookup"><span data-stu-id="6485e-581">1</span></span>                             | <span data-ttu-id="6485e-582">value1</span><span class="sxs-lookup"><span data-stu-id="6485e-582">value1</span></span>                        |
| <span data-ttu-id="6485e-583">2</span><span class="sxs-lookup"><span data-stu-id="6485e-583">2</span></span>                             | <span data-ttu-id="6485e-584">value2</span><span class="sxs-lookup"><span data-stu-id="6485e-584">value2</span></span>                        |
| <span data-ttu-id="6485e-585">3</span><span class="sxs-lookup"><span data-stu-id="6485e-585">3</span></span>                             | <span data-ttu-id="6485e-586">value4</span><span class="sxs-lookup"><span data-stu-id="6485e-586">value4</span></span>                        |
| <span data-ttu-id="6485e-587">4</span><span class="sxs-lookup"><span data-stu-id="6485e-587">4</span></span>                             | <span data-ttu-id="6485e-588">value5</span><span class="sxs-lookup"><span data-stu-id="6485e-588">value5</span></span>                        |

<span data-ttu-id="6485e-589">El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="6485e-589">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="6485e-590">Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto.</span><span class="sxs-lookup"><span data-stu-id="6485e-590">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="6485e-591">No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.</span><span class="sxs-lookup"><span data-stu-id="6485e-591">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="6485e-592">El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExamples` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-592">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExamples` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="6485e-593">Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExamples.Entries` coincide con la matriz de configuración completa:</span><span class="sxs-lookup"><span data-stu-id="6485e-593">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExamples.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="6485e-594">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="6485e-594">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="6485e-595">En `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="6485e-595">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="6485e-596">En el constructor `Startup`:</span><span class="sxs-lookup"><span data-stu-id="6485e-596">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="6485e-597">El par clave-valor que se muestra en la tabla se carga en la configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-597">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="6485e-598">Key</span><span class="sxs-lookup"><span data-stu-id="6485e-598">Key</span></span>             | <span data-ttu-id="6485e-599">Valor</span><span class="sxs-lookup"><span data-stu-id="6485e-599">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="6485e-600">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="6485e-600">array:entries:3</span></span> | <span data-ttu-id="6485e-601">value3</span><span class="sxs-lookup"><span data-stu-id="6485e-601">value3</span></span> |

<span data-ttu-id="6485e-602">Si la instancia de clase `ArrayExamples` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExamples.Entries` incluye el valor.</span><span class="sxs-lookup"><span data-stu-id="6485e-602">If the `ArrayExamples` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExamples.Entries` array includes the value.</span></span>

| <span data-ttu-id="6485e-603">Índice de `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="6485e-603">`ArrayExamples.Entries` Index</span></span> | <span data-ttu-id="6485e-604">Valor de `ArrayExamples.Entries`</span><span class="sxs-lookup"><span data-stu-id="6485e-604">`ArrayExamples.Entries` Value</span></span> |
| :---------------------------: | :---------------------------: |
| <span data-ttu-id="6485e-605">0</span><span class="sxs-lookup"><span data-stu-id="6485e-605">0</span></span>                             | <span data-ttu-id="6485e-606">value0</span><span class="sxs-lookup"><span data-stu-id="6485e-606">value0</span></span>                        |
| <span data-ttu-id="6485e-607">1</span><span class="sxs-lookup"><span data-stu-id="6485e-607">1</span></span>                             | <span data-ttu-id="6485e-608">value1</span><span class="sxs-lookup"><span data-stu-id="6485e-608">value1</span></span>                        |
| <span data-ttu-id="6485e-609">2</span><span class="sxs-lookup"><span data-stu-id="6485e-609">2</span></span>                             | <span data-ttu-id="6485e-610">value2</span><span class="sxs-lookup"><span data-stu-id="6485e-610">value2</span></span>                        |
| <span data-ttu-id="6485e-611">3</span><span class="sxs-lookup"><span data-stu-id="6485e-611">3</span></span>                             | <span data-ttu-id="6485e-612">value3</span><span class="sxs-lookup"><span data-stu-id="6485e-612">value3</span></span>                        |
| <span data-ttu-id="6485e-613">4</span><span class="sxs-lookup"><span data-stu-id="6485e-613">4</span></span>                             | <span data-ttu-id="6485e-614">value4</span><span class="sxs-lookup"><span data-stu-id="6485e-614">value4</span></span>                        |
| <span data-ttu-id="6485e-615">5</span><span class="sxs-lookup"><span data-stu-id="6485e-615">5</span></span>                             | <span data-ttu-id="6485e-616">value5</span><span class="sxs-lookup"><span data-stu-id="6485e-616">value5</span></span>                        |

<span data-ttu-id="6485e-617">**Procesamiento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="6485e-617">**JSON array processing**</span></span>

<span data-ttu-id="6485e-618">Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero.</span><span class="sxs-lookup"><span data-stu-id="6485e-618">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="6485e-619">En el siguiente archivo de configuración, `subsection` es una matriz:</span><span class="sxs-lookup"><span data-stu-id="6485e-619">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="6485e-620">El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="6485e-620">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="6485e-621">Key</span><span class="sxs-lookup"><span data-stu-id="6485e-621">Key</span></span>                     | <span data-ttu-id="6485e-622">Valor</span><span class="sxs-lookup"><span data-stu-id="6485e-622">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="6485e-623">json_array:key</span><span class="sxs-lookup"><span data-stu-id="6485e-623">json_array:key</span></span>          | <span data-ttu-id="6485e-624">valueA</span><span class="sxs-lookup"><span data-stu-id="6485e-624">valueA</span></span> |
| <span data-ttu-id="6485e-625">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="6485e-625">json_array:subsection:0</span></span> | <span data-ttu-id="6485e-626">valueB</span><span class="sxs-lookup"><span data-stu-id="6485e-626">valueB</span></span> |
| <span data-ttu-id="6485e-627">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="6485e-627">json_array:subsection:1</span></span> | <span data-ttu-id="6485e-628">valueC</span><span class="sxs-lookup"><span data-stu-id="6485e-628">valueC</span></span> |
| <span data-ttu-id="6485e-629">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="6485e-629">json_array:subsection:2</span></span> | <span data-ttu-id="6485e-630">valueD</span><span class="sxs-lookup"><span data-stu-id="6485e-630">valueD</span></span> |

<span data-ttu-id="6485e-631">En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="6485e-631">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-632">Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="6485e-632">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="6485e-633">Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="6485e-633">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="6485e-634">Índice de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="6485e-634">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="6485e-635">Valor de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="6485e-635">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="6485e-636">0</span><span class="sxs-lookup"><span data-stu-id="6485e-636">0</span></span>                                   | <span data-ttu-id="6485e-637">valueB</span><span class="sxs-lookup"><span data-stu-id="6485e-637">valueB</span></span>                              |
| <span data-ttu-id="6485e-638">1</span><span class="sxs-lookup"><span data-stu-id="6485e-638">1</span></span>                                   | <span data-ttu-id="6485e-639">valueC</span><span class="sxs-lookup"><span data-stu-id="6485e-639">valueC</span></span>                              |
| <span data-ttu-id="6485e-640">2</span><span class="sxs-lookup"><span data-stu-id="6485e-640">2</span></span>                                   | <span data-ttu-id="6485e-641">valueD</span><span class="sxs-lookup"><span data-stu-id="6485e-641">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="6485e-642">Proveedores de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="6485e-642">Custom configuration provider</span></span>

<span data-ttu-id="6485e-643">La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="6485e-643">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="6485e-644">El proveedor tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="6485e-644">The provider has the following characteristics:</span></span>

* <span data-ttu-id="6485e-645">La base de datos en memoria de EF se usa para fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="6485e-645">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="6485e-646">Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="6485e-646">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="6485e-647">El proveedor lee una tabla de base de datos en la configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="6485e-647">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="6485e-648">El proveedor no consulta la base de datos por clave.</span><span class="sxs-lookup"><span data-stu-id="6485e-648">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="6485e-649">La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6485e-649">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="6485e-650">Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="6485e-650">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="6485e-651">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="6485e-651">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-652">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="6485e-652">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="6485e-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="6485e-653">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-654">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="6485e-654">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="6485e-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="6485e-655">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-656">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="6485e-656">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="6485e-657">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="6485e-657">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="6485e-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="6485e-658">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-659">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="6485e-659">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="6485e-660">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="6485e-660">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="6485e-661">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="6485e-661">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="6485e-662">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="6485e-662">Access configuration during startup</span></span>

<span data-ttu-id="6485e-663">Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="6485e-663">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="6485e-664">Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:</span><span class="sxs-lookup"><span data-stu-id="6485e-664">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

```csharp
public class Startup
{
    private readonly IConfiguration _config;

    public Startup(IConfiguration config)
    {
        _config = config;
    }

    public void ConfigureServices(IServiceCollection services)
    {
        var value = _config["key"];
    }

    public void Configure(IApplicationBuilder app, IConfiguration config)
    {
        var value = config["key"];
    }
}
```

<span data-ttu-id="6485e-665">Para un ejemplo de cómo acceder a la configuración a través de métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods).</span><span class="sxs-lookup"><span data-stu-id="6485e-665">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="6485e-666">Acceso a la configuración en una página de Razor Pages o en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="6485e-666">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="6485e-667">Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.</span><span class="sxs-lookup"><span data-stu-id="6485e-667">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="6485e-668">En una página de las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="6485e-668">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="6485e-669">En una vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="6485e-669">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration value for 'key': @Configuration["key"]</p>
</body>
</html>
```

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="6485e-670">Agregar configuración a partir de un ensamblado externo</span><span class="sxs-lookup"><span data-stu-id="6485e-670">Add configuration from an external assembly</span></span>

<span data-ttu-id="6485e-671">Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="6485e-671">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="6485e-672">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="6485e-672">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6485e-673">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="6485e-673">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="6485e-674">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Profundización en la configuración de Microsoft)</span><span class="sxs-lookup"><span data-stu-id="6485e-674">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
