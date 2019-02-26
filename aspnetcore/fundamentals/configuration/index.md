---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 01/25/2019
uid: fundamentals/configuration/index
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="bc916-103">Configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bc916-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="bc916-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bc916-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="bc916-105">La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*.</span><span class="sxs-lookup"><span data-stu-id="bc916-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="bc916-106">Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="bc916-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bc916-107">Azure Key Vault</span></span>
* <span data-ttu-id="bc916-108">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-108">Command-line arguments</span></span>
* <span data-ttu-id="bc916-109">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="bc916-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="bc916-110">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="bc916-110">Directory files</span></span>
* <span data-ttu-id="bc916-111">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-111">Environment variables</span></span>
* <span data-ttu-id="bc916-112">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-112">In-memory .NET objects</span></span>
* <span data-ttu-id="bc916-113">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="bc916-113">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

* <span data-ttu-id="bc916-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bc916-114">Azure Key Vault</span></span>
* <span data-ttu-id="bc916-115">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-115">Command-line arguments</span></span>
* <span data-ttu-id="bc916-116">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="bc916-116">Custom providers (installed or created)</span></span>
* <span data-ttu-id="bc916-117">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-117">Environment variables</span></span>
* <span data-ttu-id="bc916-118">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-118">In-memory .NET objects</span></span>
* <span data-ttu-id="bc916-119">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="bc916-119">Settings files</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.0"

* <span data-ttu-id="bc916-120">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-120">Command-line arguments</span></span>
* <span data-ttu-id="bc916-121">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="bc916-121">Custom providers (installed or created)</span></span>
* <span data-ttu-id="bc916-122">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-122">Environment variables</span></span>
* <span data-ttu-id="bc916-123">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-123">In-memory .NET objects</span></span>
* <span data-ttu-id="bc916-124">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="bc916-124">Settings files</span></span>

::: moniker-end

<span data-ttu-id="bc916-125">El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="bc916-125">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="bc916-126">Las opciones usan clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="bc916-126">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="bc916-127">Para más información sobre cómo usar el patrón de opciones, consulte <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="bc916-127">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="bc916-128">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bc916-128">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-129">Estos tres paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-129">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc916-130">Estos tres paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="bc916-130">These three packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

## <a name="host-vs-app-configuration"></a><span data-ttu-id="bc916-131">Configuración de host frente a configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="bc916-131">Host vs. app configuration</span></span>

<span data-ttu-id="bc916-132">Antes de configurar e iniciar la aplicación, se configura e inicia un *host*.</span><span class="sxs-lookup"><span data-stu-id="bc916-132">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="bc916-133">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-133">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="bc916-134">Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="bc916-134">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="bc916-135">Los pares clave-valor de la configuración de host se vuelven parte de la configuración global de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-135">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="bc916-136">Para obtener más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte [El host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="bc916-136">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="bc916-137">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="bc916-137">Default configuration</span></span>

<span data-ttu-id="bc916-138">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="bc916-138">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="bc916-139">`CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-139">`CreateDefaultBuilder` provides default configuration for the app.</span></span>

* <span data-ttu-id="bc916-140">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="bc916-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="bc916-141">Variables de entorno prefijadas con `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bc916-141">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="bc916-142">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bc916-142">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="bc916-143">La configuración de la aplicación la proporcionan los siguientes elementos en el orden establecido:</span><span class="sxs-lookup"><span data-stu-id="bc916-143">App configuration is provided from (in the following order):</span></span>
  * <span data-ttu-id="bc916-144">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bc916-144">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="bc916-145">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bc916-145">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="bc916-146">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="bc916-146">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="bc916-147">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bc916-147">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="bc916-148">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="bc916-148">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="bc916-149">Los proveedores de configuración se explican posteriormente en este tema.</span><span class="sxs-lookup"><span data-stu-id="bc916-149">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="bc916-150">Para obtener más información sobre el host y `CreateDefaultBuilder`, vea <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="bc916-150">For more information on the host and `CreateDefaultBuilder`, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="bc916-151">Seguridad</span><span class="sxs-lookup"><span data-stu-id="bc916-151">Security</span></span>

<span data-ttu-id="bc916-152">Adopte estos procedimientos recomendados:</span><span class="sxs-lookup"><span data-stu-id="bc916-152">Adopt the following best practices:</span></span>

* <span data-ttu-id="bc916-153">Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="bc916-153">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="bc916-154">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="bc916-154">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="bc916-155">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="bc916-155">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="bc916-156">Obtenga más información sobre [cómo usar varios entornos](xref:fundamentals/environments) y cómo administrar el [almacenamiento seguro de secretos de aplicación en el desarrollo con el Administrador de secretos](xref:security/app-secrets) (incluye consejos sobre el uso de variables de entorno para almacenar información confidencial).</span><span class="sxs-lookup"><span data-stu-id="bc916-156">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="bc916-157">El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local.</span><span class="sxs-lookup"><span data-stu-id="bc916-157">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="bc916-158">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="bc916-158">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="bc916-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) es una opción para el almacenamiento seguro de los secretos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-159">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="bc916-160">Para obtener más información, vea <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="bc916-160">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="bc916-161">Datos de configuración jerárquica</span><span class="sxs-lookup"><span data-stu-id="bc916-161">Hierarchical configuration data</span></span>

<span data-ttu-id="bc916-162">La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-162">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="bc916-163">En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:</span><span class="sxs-lookup"><span data-stu-id="bc916-163">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="bc916-164">Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-164">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="bc916-165">Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:</span><span class="sxs-lookup"><span data-stu-id="bc916-165">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="bc916-166">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-166">section0:key0</span></span>
* <span data-ttu-id="bc916-167">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-167">section0:key1</span></span>
* <span data-ttu-id="bc916-168">section1:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-168">section1:key0</span></span>
* <span data-ttu-id="bc916-169">section1:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-169">section1:key1</span></span>

<span data-ttu-id="bc916-170">Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-170"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="bc916-171">Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="bc916-171">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="bc916-172">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-172">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="bc916-173">Convenciones</span><span class="sxs-lookup"><span data-stu-id="bc916-173">Conventions</span></span>

<span data-ttu-id="bc916-174">Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-174">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="bc916-175">Los proveedores de configuración de archivo tienen la capacidad de recargar la configuración cuando se modifica un archivo de configuración subyacente después del inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-175">File Configuration Providers have the ability to reload configuration when an underlying settings file is changed after app startup.</span></span> <span data-ttu-id="bc916-176">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="bc916-176">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="bc916-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor [Inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-177"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [Dependency Injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="bc916-178">Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.</span><span class="sxs-lookup"><span data-stu-id="bc916-178">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="bc916-179">Las claves de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="bc916-179">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="bc916-180">Las claves no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="bc916-180">Keys are case-insensitive.</span></span> <span data-ttu-id="bc916-181">Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.</span><span class="sxs-lookup"><span data-stu-id="bc916-181">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="bc916-182">Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.</span><span class="sxs-lookup"><span data-stu-id="bc916-182">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="bc916-183">Claves jerárquicas</span><span class="sxs-lookup"><span data-stu-id="bc916-183">Hierarchical keys</span></span>
  * <span data-ttu-id="bc916-184">Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="bc916-184">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="bc916-185">En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="bc916-185">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="bc916-186">Un guion bajo doble (`__`) es compatible con todas las plataformas y se convierte en un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="bc916-186">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="bc916-187">En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="bc916-187">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="bc916-188">Debe proporcionar código para reemplazar los guiones por dos puntos cuando los secretos se cargan en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-188">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="bc916-189"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-189">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="bc916-190">El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="bc916-190">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="bc916-191">Los valores de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="bc916-191">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="bc916-192">Los valores son cadenas.</span><span class="sxs-lookup"><span data-stu-id="bc916-192">Values are strings.</span></span>
* <span data-ttu-id="bc916-193">Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.</span><span class="sxs-lookup"><span data-stu-id="bc916-193">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="bc916-194">Proveedores</span><span class="sxs-lookup"><span data-stu-id="bc916-194">Providers</span></span>

<span data-ttu-id="bc916-195">La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bc916-195">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

::: moniker range=">= aspnetcore-2.1"

| <span data-ttu-id="bc916-196">Proveedor</span><span class="sxs-lookup"><span data-stu-id="bc916-196">Provider</span></span> | <span data-ttu-id="bc916-197">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="bc916-197">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="bc916-198">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="bc916-198">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="bc916-199">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bc916-199">Azure Key Vault</span></span> |
| [<span data-ttu-id="bc916-200">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-200">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="bc916-201">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-201">Command-line parameters</span></span> |
| [<span data-ttu-id="bc916-202">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="bc916-202">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="bc916-203">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="bc916-203">Custom source</span></span> |
| [<span data-ttu-id="bc916-204">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-204">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="bc916-205">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-205">Environment variables</span></span> |
| [<span data-ttu-id="bc916-206">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="bc916-206">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="bc916-207">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="bc916-207">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="bc916-208">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="bc916-208">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="bc916-209">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="bc916-209">Directory files</span></span> |
| [<span data-ttu-id="bc916-210">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-210">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="bc916-211">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-211">In-memory collections</span></span> |
| <span data-ttu-id="bc916-212">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="bc916-212">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="bc916-213">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="bc916-213">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-1.1"

| <span data-ttu-id="bc916-214">Proveedor</span><span class="sxs-lookup"><span data-stu-id="bc916-214">Provider</span></span> | <span data-ttu-id="bc916-215">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="bc916-215">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="bc916-216">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="bc916-216">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="bc916-217">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bc916-217">Azure Key Vault</span></span> |
| [<span data-ttu-id="bc916-218">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="bc916-219">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-219">Command-line parameters</span></span> |
| [<span data-ttu-id="bc916-220">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="bc916-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="bc916-221">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="bc916-221">Custom source</span></span> |
| [<span data-ttu-id="bc916-222">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="bc916-223">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-223">Environment variables</span></span> |
| [<span data-ttu-id="bc916-224">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="bc916-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="bc916-225">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="bc916-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="bc916-226">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-226">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="bc916-227">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-227">In-memory collections</span></span> |
| <span data-ttu-id="bc916-228">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="bc916-228">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="bc916-229">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="bc916-229">File in the user profile directory</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-1.0"

| <span data-ttu-id="bc916-230">Proveedor</span><span class="sxs-lookup"><span data-stu-id="bc916-230">Provider</span></span> | <span data-ttu-id="bc916-231">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="bc916-231">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| [<span data-ttu-id="bc916-232">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-232">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="bc916-233">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-233">Command-line parameters</span></span> |
| [<span data-ttu-id="bc916-234">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="bc916-234">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="bc916-235">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="bc916-235">Custom source</span></span> |
| [<span data-ttu-id="bc916-236">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-236">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="bc916-237">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-237">Environment variables</span></span> |
| [<span data-ttu-id="bc916-238">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="bc916-238">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="bc916-239">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="bc916-239">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="bc916-240">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-240">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="bc916-241">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-241">In-memory collections</span></span> |
| <span data-ttu-id="bc916-242">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="bc916-242">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="bc916-243">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="bc916-243">File in the user profile directory</span></span> |

::: moniker-end

<span data-ttu-id="bc916-244">Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="bc916-244">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="bc916-245">En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en que el código podría organizarlos.</span><span class="sxs-lookup"><span data-stu-id="bc916-245">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="bc916-246">Ordene los proveedores de configuración en el código para cumplir con sus prioridades relacionadas con los orígenes de configuración subyacentes.</span><span class="sxs-lookup"><span data-stu-id="bc916-246">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="bc916-247">Esta es una secuencia típica de proveedores de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-247">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="bc916-248">Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)</span><span class="sxs-lookup"><span data-stu-id="bc916-248">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="bc916-249">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="bc916-249">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="bc916-250">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (solo en el entorno de desarrollo)</span><span class="sxs-lookup"><span data-stu-id="bc916-250">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="bc916-251">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-251">Environment variables</span></span>
1. <span data-ttu-id="bc916-252">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-252">Command-line arguments</span></span>

<span data-ttu-id="bc916-253">Una práctica común es colocar el proveedor de configuración de línea de comandos al final de en una serie de proveedores para permitir que los argumentos de la línea de comandos invaliden la configuración que establecieron los demás proveedores.</span><span class="sxs-lookup"><span data-stu-id="bc916-253">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-254">Esta secuencia de los proveedores se aplica al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-254">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="bc916-255">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bc916-255">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-256">Esta secuencia de proveedores se puede crear para la aplicación (no para el host) con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> y una llamada a su método <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> en `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bc916-256">This sequence of providers can be created for the app (not the host) with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> and a call to its <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder.Build*> method in `Startup`:</span></span>

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

<span data-ttu-id="bc916-257">En el ejemplo anterior, <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> proporciona el nombre del entorno (`env.EnvironmentName`) y el nombre del ensamblado de la aplicación (`env.ApplicationName`).</span><span class="sxs-lookup"><span data-stu-id="bc916-257">In the preceding example, the environment name (`env.EnvironmentName`) and app assembly name (`env.ApplicationName`) are provided by the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment>.</span></span> <span data-ttu-id="bc916-258">Para obtener más información, vea <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="bc916-258">For more information, see <xref:fundamentals/environments>.</span></span> <span data-ttu-id="bc916-259">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-259">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-260">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-260">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>
<span data-ttu-id="bc916-261">.</span><span class="sxs-lookup"><span data-stu-id="bc916-261">.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="configureappconfiguration"></a><span data-ttu-id="bc916-262">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="bc916-262">ConfigureAppConfiguration</span></span>

<span data-ttu-id="bc916-263">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> agrega automáticamente:</span><span class="sxs-lookup"><span data-stu-id="bc916-263">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

::: moniker-end

## <a name="command-line-configuration-provider"></a><span data-ttu-id="bc916-264">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="bc916-264">Command-line Configuration Provider</span></span>

<span data-ttu-id="bc916-265"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="bc916-265">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="bc916-266">Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-266">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-267">`AddCommandLine` se llama automáticamente cuando se inicializa un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-267">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="bc916-268">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bc916-268">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="bc916-269">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="bc916-269">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="bc916-270">Configuración opcional de *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="bc916-270">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="bc916-271">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="bc916-271">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="bc916-272">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="bc916-272">Environment variables.</span></span>

<span data-ttu-id="bc916-273">`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final.</span><span class="sxs-lookup"><span data-stu-id="bc916-273">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="bc916-274">Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="bc916-274">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="bc916-275">`CreateDefaultBuilder` actúa cuando se construye el host.</span><span class="sxs-lookup"><span data-stu-id="bc916-275">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="bc916-276">Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.</span><span class="sxs-lookup"><span data-stu-id="bc916-276">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-277">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-277">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="bc916-278">Ya se ha llamado a `AddCommandLine` por parte de `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc916-278">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="bc916-279">Si tiene que proporcionar la configuración de la aplicación y mantener la posibilidad de invalidar dicha configuración con argumentos de línea de comandos, llame a los proveedores adicionales de la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> y llame por último a `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="bc916-279">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call other providers here and call AddCommandLine last.
                config.AddCommandLine(args);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-280">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-280">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc916-281">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-281">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="bc916-282">Ya se ha llamado a `AddCommandLine` por parte de `CreateDefaultBuilder` cuando `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="bc916-282">`AddCommandLine` has already been called by `CreateDefaultBuilder` when `UseConfiguration` is called.</span></span> <span data-ttu-id="bc916-283">Si tiene que proporcionar la configuración de la aplicación y mantener la posibilidad de invalidar dicha configuración con argumentos de línea de comandos, llame a los proveedores adicionales de la aplicación en `ConfigurationBuilder` y llame por último a `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="bc916-283">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers on a `ConfigurationBuilder` and call `AddCommandLine` last.</span></span>

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
            // Call other providers here and call AddCommandLine last.
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="bc916-284">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-284">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-285">Para activar la configuración de línea de comandos, llame al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-285">To activate command-line configuration, call the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bc916-286">Llame en último lugar al proveedor para permitir que los argumentos de la línea de comandos que se pasan en tiempo de ejecución invaliden la configuración establecida por los otros proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-286">Call the provider last to allow the command-line arguments passed at runtime to override configuration set by other configuration providers.</span></span>

<span data-ttu-id="bc916-287">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bc916-287">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

::: moniker-end

```csharp
var config = new ConfigurationBuilder()
    // Call additional providers here as needed.
    // Call AddCommandLine last to allow arguments to override other configuration.
    .AddCommandLine(args)
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="bc916-288">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="bc916-288">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-289">La aplicación de ejemplo 2.x aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-289">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-290">La aplicación de ejemplo 1.x llama a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-290">The 1.x sample app calls <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> on a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

::: moniker-end

1. <span data-ttu-id="bc916-291">Abra un símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bc916-291">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="bc916-292">Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="bc916-292">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="bc916-293">Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bc916-293">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="bc916-294">Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="bc916-294">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="bc916-295">Argumentos</span><span class="sxs-lookup"><span data-stu-id="bc916-295">Arguments</span></span>

<span data-ttu-id="bc916-296">El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio.</span><span class="sxs-lookup"><span data-stu-id="bc916-296">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="bc916-297">El valor puede ser NULL si se usa un signo igual (por ejemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="bc916-297">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="bc916-298">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="bc916-298">Key prefix</span></span>               | <span data-ttu-id="bc916-299">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="bc916-299">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="bc916-300">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="bc916-300">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="bc916-301">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="bc916-301">Two dashes (`--`)</span></span>        | <span data-ttu-id="bc916-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="bc916-302">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="bc916-303">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="bc916-303">Forward slash (`/`)</span></span>      | <span data-ttu-id="bc916-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="bc916-304">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="bc916-305">Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.</span><span class="sxs-lookup"><span data-stu-id="bc916-305">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="bc916-306">Comandos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bc916-306">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="bc916-307">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="bc916-307">Switch mappings</span></span>

<span data-ttu-id="bc916-308">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="bc916-308">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="bc916-309">Cuando crea manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, puede proporcionar un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-309">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="bc916-310">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="bc916-310">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="bc916-311">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-311">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="bc916-312">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="bc916-312">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="bc916-313">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="bc916-313">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="bc916-314">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="bc916-314">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="bc916-315">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="bc916-315">The switch mappings dictionary must not contain duplicate keys.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-316">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bc916-316">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _switchMappings = 
        new Dictionary<string, string>
        {
            { "-CLKey1", "CommandLineKey1" },
            { "-CLKey2", "CommandLineKey2" }
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    // Do not pass the args to CreateDefaultBuilder
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder()
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddCommandLine(args, _switchMappings);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-317">Como se muestra en el ejemplo anterior, la llamada a `CreateDefaultBuilder` no debería pasar argumentos cuando se usan las asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="bc916-317">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="bc916-318">La llamada de `AddCommandLine` del método `CreateDefaultBuilder` no incluye modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc916-318">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="bc916-319">Si los argumentos incluyen un conmutador asignado y se pasan a `CreateDefaultBuilder`, el proveedor de `AddCommandLine` no se puede inicializar con una <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="bc916-319">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="bc916-320">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="bc916-320">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

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

        // Do not pass the args to CreateDefaultBuilder
        return WebHost.CreateDefaultBuilder()
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="bc916-321">Como se muestra en el ejemplo anterior, la llamada a `CreateDefaultBuilder` no debería pasar argumentos cuando se usan las asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="bc916-321">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="bc916-322">La llamada de `AddCommandLine` del método `CreateDefaultBuilder` no incluye modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc916-322">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="bc916-323">Si los argumentos incluyen un conmutador asignado y se pasan a `CreateDefaultBuilder`, el proveedor de `AddCommandLine` no se puede inicializar con una <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="bc916-323">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="bc916-324">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="bc916-324">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

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

<span data-ttu-id="bc916-325">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="bc916-325">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="bc916-326">Key</span><span class="sxs-lookup"><span data-stu-id="bc916-326">Key</span></span>       | <span data-ttu-id="bc916-327">Valor</span><span class="sxs-lookup"><span data-stu-id="bc916-327">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="bc916-328">Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:</span><span class="sxs-lookup"><span data-stu-id="bc916-328">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="bc916-329">Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="bc916-329">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="bc916-330">Key</span><span class="sxs-lookup"><span data-stu-id="bc916-330">Key</span></span>               | <span data-ttu-id="bc916-331">Valor</span><span class="sxs-lookup"><span data-stu-id="bc916-331">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="bc916-332">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-332">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="bc916-333"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="bc916-333">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="bc916-334">Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-334">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bc916-335">Al trabajar con claves jerárquicas en variables de entorno, es posible que un separador de dos puntos (`:`) no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="bc916-335">When working with hierarchical keys in environment variables, a colon separator (`:`) may not work on all platforms.</span></span> <span data-ttu-id="bc916-336">Un guion bajo doble (`__`) es compatible con todas las plataformas y lo reemplaza un separador de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="bc916-336">A double underscore (`__`) is supported by all platforms and is replaced by a colon.</span></span>

<span data-ttu-id="bc916-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer las variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="bc916-337">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="bc916-338">Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="bc916-338">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-339">Se llama automáticamente a `AddEnvironmentVariables` para las variables de entorno con el prefijo `ASPNETCORE_` al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-339">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="bc916-340">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bc916-340">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="bc916-341">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="bc916-341">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="bc916-342">Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.</span><span class="sxs-lookup"><span data-stu-id="bc916-342">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="bc916-343">Configuración opcional de *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="bc916-343">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="bc916-344">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="bc916-344">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="bc916-345">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="bc916-345">Command-line arguments.</span></span>

<span data-ttu-id="bc916-346">El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="bc916-346">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="bc916-347">Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="bc916-347">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-348">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-348">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="bc916-349">Si tiene que proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> y llame a `AddEnvironmentVariables` con el prefijo.</span><span class="sxs-lookup"><span data-stu-id="bc916-349">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                // Call additional providers here as needed.
                // Call AddEnvironmentVariables last if you need to allow environment
                // variables to override values from other providers.
                config.AddEnvironmentVariables(prefix: "PREFIX_");
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-350">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-350">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc916-351">Llame al método de extensión `AddEnvironmentVariables` en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-351">Call the `AddEnvironmentVariables` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="bc916-352">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-352">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method.</span></span>

<span data-ttu-id="bc916-353">Si tiene que proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> y llame a `AddEnvironmentVariables` con el prefijo.</span><span class="sxs-lookup"><span data-stu-id="bc916-353">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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
            // Call additional providers here as needed.
            // Call AddEnvironmentVariables last if you need to allow environment
            // variables to override values from other providers.
            .AddEnvironmentVariables(prefix: "PREFIX_")
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseConfiguration(config)
            .UseStartup<Startup>();
    }
}
```

<span data-ttu-id="bc916-354">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-354">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-355">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bc916-355">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="bc916-356">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="bc916-356">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-357">La aplicación de ejemplo 2.x aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="bc916-357">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-358">La aplicación de ejemplo 1.x llama a `AddEnvironmentVariables` en `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc916-358">The 1.x sample app calls `AddEnvironmentVariables` on a `ConfigurationBuilder`.</span></span>

::: moniker-end

1. <span data-ttu-id="bc916-359">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="bc916-359">Run the sample app.</span></span> <span data-ttu-id="bc916-360">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bc916-360">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="bc916-361">Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="bc916-361">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="bc916-362">El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.</span><span class="sxs-lookup"><span data-stu-id="bc916-362">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="bc916-363">Para que la lista de variables de entorno que muestra la aplicación sea breve, la aplicación filtra las variables de entorno que comienzan con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc916-363">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="bc916-364">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="bc916-364">ASPNETCORE_</span></span>
* <span data-ttu-id="bc916-365">urls</span><span class="sxs-lookup"><span data-stu-id="bc916-365">urls</span></span>
* <span data-ttu-id="bc916-366">Registro</span><span class="sxs-lookup"><span data-stu-id="bc916-366">Logging</span></span>
* <span data-ttu-id="bc916-367">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="bc916-367">ENVIRONMENT</span></span>
* <span data-ttu-id="bc916-368">contentRoot</span><span class="sxs-lookup"><span data-stu-id="bc916-368">contentRoot</span></span>
* <span data-ttu-id="bc916-369">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="bc916-369">AllowedHosts</span></span>
* <span data-ttu-id="bc916-370">applicationName</span><span class="sxs-lookup"><span data-stu-id="bc916-370">applicationName</span></span>
* <span data-ttu-id="bc916-371">CommandLine</span><span class="sxs-lookup"><span data-stu-id="bc916-371">CommandLine</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-372">Si quiere exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc916-372">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-373">Si quiere exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Controllers/HomeController.cs* a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bc916-373">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Controllers/HomeController.cs* to the following:</span></span>

::: moniker-end

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="bc916-374">Prefijos</span><span class="sxs-lookup"><span data-stu-id="bc916-374">Prefixes</span></span>

<span data-ttu-id="bc916-375">Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="bc916-375">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="bc916-376">Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-376">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="bc916-377">El prefijo se quita cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-377">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-378">El método de conveniencia estático `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> para establecer el host de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-378">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="bc916-379">Cuando se crea `WebHostBuilder`, encuentra su configuración de host en variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="bc916-379">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

::: moniker-end

<span data-ttu-id="bc916-380">**Prefijos de cadena de conexión**</span><span class="sxs-lookup"><span data-stu-id="bc916-380">**Connection string prefixes**</span></span>

<span data-ttu-id="bc916-381">La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-381">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="bc916-382">Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="bc916-382">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="bc916-383">Prefijo de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="bc916-383">Connection string prefix</span></span> | <span data-ttu-id="bc916-384">Proveedor</span><span class="sxs-lookup"><span data-stu-id="bc916-384">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="bc916-385">Proveedor personalizado</span><span class="sxs-lookup"><span data-stu-id="bc916-385">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="bc916-386">MySQL</span><span class="sxs-lookup"><span data-stu-id="bc916-386">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="bc916-387">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="bc916-387">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="bc916-388">SQL Server</span><span class="sxs-lookup"><span data-stu-id="bc916-388">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="bc916-389">Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:</span><span class="sxs-lookup"><span data-stu-id="bc916-389">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="bc916-390">La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="bc916-390">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="bc916-391">Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).</span><span class="sxs-lookup"><span data-stu-id="bc916-391">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="bc916-392">Clave de variable de entorno</span><span class="sxs-lookup"><span data-stu-id="bc916-392">Environment variable key</span></span> | <span data-ttu-id="bc916-393">Clave de configuración convertida</span><span class="sxs-lookup"><span data-stu-id="bc916-393">Converted configuration key</span></span> | <span data-ttu-id="bc916-394">Entrada de configuración del proveedor</span><span class="sxs-lookup"><span data-stu-id="bc916-394">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bc916-395">Entrada de configuración no creada.</span><span class="sxs-lookup"><span data-stu-id="bc916-395">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bc916-396">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="bc916-396">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="bc916-397">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="bc916-397">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bc916-398">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="bc916-398">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="bc916-399">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="bc916-399">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="bc916-400">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="bc916-400">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="bc916-401">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="bc916-401">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="bc916-402">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="bc916-402">File Configuration Provider</span></span>

<span data-ttu-id="bc916-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="bc916-403"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="bc916-404">Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:</span><span class="sxs-lookup"><span data-stu-id="bc916-404">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="bc916-405">Proveedor de configuración INI</span><span class="sxs-lookup"><span data-stu-id="bc916-405">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="bc916-406">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="bc916-406">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="bc916-407">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="bc916-407">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="bc916-408">Proveedor de configuración de INI</span><span class="sxs-lookup"><span data-stu-id="bc916-408">INI Configuration Provider</span></span>

<span data-ttu-id="bc916-409"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="bc916-409">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="bc916-410">Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-410">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bc916-411">Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.</span><span class="sxs-lookup"><span data-stu-id="bc916-411">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="bc916-412">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="bc916-412">Overloads permit specifying:</span></span>

* <span data-ttu-id="bc916-413">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="bc916-413">Whether the file is optional.</span></span>
* <span data-ttu-id="bc916-414">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="bc916-414">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="bc916-415"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="bc916-415">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-416">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bc916-416">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddIniFile("config.ini", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-417">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-417">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-418">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-418">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-419">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-419">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc916-420">Al llamar a `CreateDefaultBuilder`, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-420">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bc916-421">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-421">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-422">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-422">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-423">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-423">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-424">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bc916-424">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="bc916-425">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-425">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-426">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-426">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-427">Un ejemplo genérico de un archivo de configuración INI:</span><span class="sxs-lookup"><span data-stu-id="bc916-427">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="bc916-428">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="bc916-428">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bc916-429">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-429">section0:key0</span></span>
* <span data-ttu-id="bc916-430">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-430">section0:key1</span></span>
* <span data-ttu-id="bc916-431">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="bc916-431">section1:subsection:key</span></span>
* <span data-ttu-id="bc916-432">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="bc916-432">section2:subsection0:key</span></span>
* <span data-ttu-id="bc916-433">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="bc916-433">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="bc916-434">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="bc916-434">JSON Configuration Provider</span></span>

<span data-ttu-id="bc916-435"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="bc916-435">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="bc916-436">Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-436">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bc916-437">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="bc916-437">Overloads permit specifying:</span></span>

* <span data-ttu-id="bc916-438">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="bc916-438">Whether the file is optional.</span></span>
* <span data-ttu-id="bc916-439">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="bc916-439">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="bc916-440"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="bc916-440">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-441">`AddJsonFile` se llama automáticamente dos veces al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-441">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="bc916-442">Se llama al método para cargar la configuración desde:</span><span class="sxs-lookup"><span data-stu-id="bc916-442">The method is called to load configuration from:</span></span>

* <span data-ttu-id="bc916-443">*appsettings.json* &ndash; Este archivo se lee primero.</span><span class="sxs-lookup"><span data-stu-id="bc916-443">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="bc916-444">La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="bc916-444">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="bc916-445">*appsettings.{Environment}.json* &ndash; La versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="bc916-445">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="bc916-446">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="bc916-446">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="bc916-447">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="bc916-447">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="bc916-448">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="bc916-448">Environment variables.</span></span>
* <span data-ttu-id="bc916-449">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="bc916-449">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="bc916-450">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="bc916-450">Command-line arguments.</span></span>

<span data-ttu-id="bc916-451">El proveedor de configuración de JSON se establece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="bc916-451">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="bc916-452">Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="bc916-452">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-453">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="bc916-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddJsonFile("config.json", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-454">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-455">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-456">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc916-457">Puede llamar directamente al método de extensión `AddJsonFile` en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-457">You can also directly call the `AddJsonFile` extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bc916-458">Al llamar a `CreateDefaultBuilder`, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-458">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bc916-459">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-459">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-460">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-460">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-461">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-461">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-462">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bc916-462">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="bc916-463">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-463">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-464">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-464">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-465">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="bc916-465">**Example**</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-466">La aplicación de ejemplo 2.x aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye dos llamadas a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="bc916-466">The 2.x sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="bc916-467">La configuración se carga desde *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="bc916-467">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-468">La aplicación de ejemplo 1.x llama a `AddJsonFile` dos veces en un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc916-468">The 1.x sample app calls `AddJsonFile` twice on a `ConfigurationBuilder`.</span></span> <span data-ttu-id="bc916-469">La configuración se carga desde *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="bc916-469">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

::: moniker-end

1. <span data-ttu-id="bc916-470">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="bc916-470">Run the sample app.</span></span> <span data-ttu-id="bc916-471">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bc916-471">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="bc916-472">Observe que la salida contiene los pares clave-valor para la configuración que se muestra en la tabla según el entorno.</span><span class="sxs-lookup"><span data-stu-id="bc916-472">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="bc916-473">Las claves de configuración de registro usan dos puntos (`:`) como separador jerárquico.</span><span class="sxs-lookup"><span data-stu-id="bc916-473">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="bc916-474">Key</span><span class="sxs-lookup"><span data-stu-id="bc916-474">Key</span></span>                        | <span data-ttu-id="bc916-475">Valor de desarrollo</span><span class="sxs-lookup"><span data-stu-id="bc916-475">Development Value</span></span> | <span data-ttu-id="bc916-476">Valor de producción</span><span class="sxs-lookup"><span data-stu-id="bc916-476">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="bc916-477">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="bc916-477">Logging:LogLevel:System</span></span>    | <span data-ttu-id="bc916-478">Información</span><span class="sxs-lookup"><span data-stu-id="bc916-478">Information</span></span>       | <span data-ttu-id="bc916-479">Información</span><span class="sxs-lookup"><span data-stu-id="bc916-479">Information</span></span>      |
| <span data-ttu-id="bc916-480">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="bc916-480">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="bc916-481">Información</span><span class="sxs-lookup"><span data-stu-id="bc916-481">Information</span></span>       | <span data-ttu-id="bc916-482">Información</span><span class="sxs-lookup"><span data-stu-id="bc916-482">Information</span></span>      |
| <span data-ttu-id="bc916-483">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="bc916-483">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="bc916-484">Depuración</span><span class="sxs-lookup"><span data-stu-id="bc916-484">Debug</span></span>             | <span data-ttu-id="bc916-485">Error</span><span class="sxs-lookup"><span data-stu-id="bc916-485">Error</span></span>            |
| <span data-ttu-id="bc916-486">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="bc916-486">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="bc916-487">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="bc916-487">XML Configuration Provider</span></span>

<span data-ttu-id="bc916-488"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="bc916-488">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="bc916-489">Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-489">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bc916-490">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="bc916-490">Overloads permit specifying:</span></span>

* <span data-ttu-id="bc916-491">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="bc916-491">Whether the file is optional.</span></span>
* <span data-ttu-id="bc916-492">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="bc916-492">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="bc916-493"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="bc916-493">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="bc916-494">El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-494">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="bc916-495">No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.</span><span class="sxs-lookup"><span data-stu-id="bc916-495">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-496">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bc916-496">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                config.AddXmlFile("config.xml", optional: true, reloadOnChange: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-497">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-497">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-498">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-498">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-499">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-499">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc916-500">Al llamar a `CreateDefaultBuilder`, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-500">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bc916-501">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-501">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-502">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-502">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-503">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-503">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-504">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bc916-504">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

<span data-ttu-id="bc916-505">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-505">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-506">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-506">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-507">Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:</span><span class="sxs-lookup"><span data-stu-id="bc916-507">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="bc916-508">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="bc916-508">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bc916-509">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-509">section0:key0</span></span>
* <span data-ttu-id="bc916-510">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-510">section0:key1</span></span>
* <span data-ttu-id="bc916-511">section1:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-511">section1:key0</span></span>
* <span data-ttu-id="bc916-512">section1:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-512">section1:key1</span></span>

<span data-ttu-id="bc916-513">Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:</span><span class="sxs-lookup"><span data-stu-id="bc916-513">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="bc916-514">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="bc916-514">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bc916-515">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-515">section:section0:key:key0</span></span>
* <span data-ttu-id="bc916-516">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-516">section:section0:key:key1</span></span>
* <span data-ttu-id="bc916-517">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-517">section:section1:key:key0</span></span>
* <span data-ttu-id="bc916-518">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-518">section:section1:key:key1</span></span>

<span data-ttu-id="bc916-519">Los atributos se pueden usar para suministrar valores:</span><span class="sxs-lookup"><span data-stu-id="bc916-519">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="bc916-520">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="bc916-520">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="bc916-521">key:attribute</span><span class="sxs-lookup"><span data-stu-id="bc916-521">key:attribute</span></span>
* <span data-ttu-id="bc916-522">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="bc916-522">section:key:attribute</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="bc916-523">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="bc916-523">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="bc916-524"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-524">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="bc916-525">La clave es el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="bc916-525">The key is the file name.</span></span> <span data-ttu-id="bc916-526">El valor contiene el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="bc916-526">The value contains the file's contents.</span></span> <span data-ttu-id="bc916-527">El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.</span><span class="sxs-lookup"><span data-stu-id="bc916-527">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="bc916-528">Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-528">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="bc916-529">La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="bc916-529">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="bc916-530">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="bc916-530">Overloads permit specifying:</span></span>

* <span data-ttu-id="bc916-531">Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.</span><span class="sxs-lookup"><span data-stu-id="bc916-531">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="bc916-532">Si el directorio es opcional y la ruta de acceso al directorio.</span><span class="sxs-lookup"><span data-stu-id="bc916-532">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="bc916-533">El carácter de subrayado doble (`__`) se usa como delimitador de claves de configuración en los nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="bc916-533">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="bc916-534">Por ejemplo, el nombre de archivo `Logging__LogLevel__System` genera la clave de configuración `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="bc916-534">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="bc916-535">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bc916-535">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.SetBasePath(Directory.GetCurrentDirectory());
                var path = Path.Combine(Directory.GetCurrentDirectory(), "path/to/files");
                config.AddKeyPerFile(directoryPath: path, optional: true);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-536">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="bc916-536">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="bc916-537">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-537">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-538">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-538">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="bc916-539">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="bc916-539">Memory Configuration Provider</span></span>

<span data-ttu-id="bc916-540"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-540">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="bc916-541">Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="bc916-541">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="bc916-542">El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="bc916-542">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="bc916-543">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bc916-543">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

```csharp
public class Program
{
    public static readonly Dictionary<string, string> _dict = 
        new Dictionary<string, string>
        {
            {"MemoryCollectionKey1", "value1"},
            {"MemoryCollectionKey2", "value2"}
        };

    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                config.AddInMemoryCollection(_dict);
            })
            .UseStartup<Startup>();
}
```

<span data-ttu-id="bc916-544">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-544">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="bc916-545">Al llamar a `CreateDefaultBuilder`, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-545">When calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="bc916-546">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-546">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-547">Aplique la configuración a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con el método <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bc916-547">Apply the configuration to <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with the <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> method:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="bc916-548">GetValue</span><span class="sxs-lookup"><span data-stu-id="bc916-548">GetValue</span></span>

<span data-ttu-id="bc916-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un valor de la configuración con una clave especificada y lo convierte al tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="bc916-549">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="bc916-550">Una sobrecarga permite proporcionar un valor predeterminado si no se encuentra la clave.</span><span class="sxs-lookup"><span data-stu-id="bc916-550">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="bc916-551">El ejemplo siguiente extrae el valor de cadena desde la configuración con la clave `NumberKey`, escribe el valor como `int` y almacena el valor en la variable `intValue`.</span><span class="sxs-lookup"><span data-stu-id="bc916-551">The following example extracts the string value from configuration with the key `NumberKey`, types the value as an `int`, and stores the value in the variable `intValue`.</span></span> <span data-ttu-id="bc916-552">Si `NumberKey` no se encuentra en las claves de configuración, `intValue` recibe el valor predeterminado de `99`:</span><span class="sxs-lookup"><span data-stu-id="bc916-552">If `NumberKey` isn't found in the configuration keys, `intValue` receives the default value of `99`:</span></span>

```csharp
var intValue = config.GetValue<int>("NumberKey", 99);
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="bc916-553">GetSection, GetChildren y Exists</span><span class="sxs-lookup"><span data-stu-id="bc916-553">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="bc916-554">Para los ejemplos siguientes, considere el siguiente archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="bc916-554">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="bc916-555">Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:</span><span class="sxs-lookup"><span data-stu-id="bc916-555">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="bc916-556">Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-556">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="bc916-557">section0:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-557">section0:key0</span></span>
* <span data-ttu-id="bc916-558">section0:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-558">section0:key1</span></span>
* <span data-ttu-id="bc916-559">section1:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-559">section1:key0</span></span>
* <span data-ttu-id="bc916-560">section1:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-560">section1:key1</span></span>
* <span data-ttu-id="bc916-561">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-561">section2:subsection0:key0</span></span>
* <span data-ttu-id="bc916-562">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-562">section2:subsection0:key1</span></span>
* <span data-ttu-id="bc916-563">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="bc916-563">section2:subsection1:key0</span></span>
* <span data-ttu-id="bc916-564">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="bc916-564">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="bc916-565">GetSection</span><span class="sxs-lookup"><span data-stu-id="bc916-565">GetSection</span></span>

<span data-ttu-id="bc916-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.</span><span class="sxs-lookup"><span data-stu-id="bc916-566">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="bc916-567">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-567">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-568">Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:</span><span class="sxs-lookup"><span data-stu-id="bc916-568">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="bc916-569">`configSection` no tiene ningún valor, solo una clave y una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="bc916-569">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="bc916-570">De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:</span><span class="sxs-lookup"><span data-stu-id="bc916-570">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="bc916-571">`GetSection` nunca devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="bc916-571">`GetSection` never returns `null`.</span></span> <span data-ttu-id="bc916-572">Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.</span><span class="sxs-lookup"><span data-stu-id="bc916-572">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="bc916-573">Cuando `GetSection` devuelve una sección coincidente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> no se rellena.</span><span class="sxs-lookup"><span data-stu-id="bc916-573">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="bc916-574">Se devuelven <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> y <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> si la sección existe.</span><span class="sxs-lookup"><span data-stu-id="bc916-574">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="bc916-575">GetChildren</span><span class="sxs-lookup"><span data-stu-id="bc916-575">GetChildren</span></span>

<span data-ttu-id="bc916-576">Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:</span><span class="sxs-lookup"><span data-stu-id="bc916-576">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

::: moniker range=">= aspnetcore-2.0"

### <a name="exists"></a><span data-ttu-id="bc916-577">Existe</span><span class="sxs-lookup"><span data-stu-id="bc916-577">Exists</span></span>

<span data-ttu-id="bc916-578">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-578">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="bc916-579">Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-579">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

::: moniker-end

## <a name="bind-to-a-class"></a><span data-ttu-id="bc916-580">Enlace a una clase</span><span class="sxs-lookup"><span data-stu-id="bc916-580">Bind to a class</span></span>

<span data-ttu-id="bc916-581">La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="bc916-581">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="bc916-582">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="bc916-582">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="bc916-583">Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="bc916-583">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="bc916-584">`Bind` está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-584">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-585">La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="bc916-585">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-586">La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-586">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="bc916-587">Se crean los siguientes pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-587">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="bc916-588">Key</span><span class="sxs-lookup"><span data-stu-id="bc916-588">Key</span></span>                   | <span data-ttu-id="bc916-589">Valor</span><span class="sxs-lookup"><span data-stu-id="bc916-589">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="bc916-590">starship:name</span><span class="sxs-lookup"><span data-stu-id="bc916-590">starship:name</span></span>         | <span data-ttu-id="bc916-591">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="bc916-591">USS Enterprise</span></span>                                    |
| <span data-ttu-id="bc916-592">starship:registry</span><span class="sxs-lookup"><span data-stu-id="bc916-592">starship:registry</span></span>     | <span data-ttu-id="bc916-593">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="bc916-593">NCC-1701</span></span>                                          |
| <span data-ttu-id="bc916-594">starship:class</span><span class="sxs-lookup"><span data-stu-id="bc916-594">starship:class</span></span>        | <span data-ttu-id="bc916-595">Constitution</span><span class="sxs-lookup"><span data-stu-id="bc916-595">Constitution</span></span>                                      |
| <span data-ttu-id="bc916-596">starship:length</span><span class="sxs-lookup"><span data-stu-id="bc916-596">starship:length</span></span>       | <span data-ttu-id="bc916-597">304.8</span><span class="sxs-lookup"><span data-stu-id="bc916-597">304.8</span></span>                                             |
| <span data-ttu-id="bc916-598">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="bc916-598">starship:commissioned</span></span> | <span data-ttu-id="bc916-599">False</span><span class="sxs-lookup"><span data-stu-id="bc916-599">False</span></span>                                             |
| <span data-ttu-id="bc916-600">trademark</span><span class="sxs-lookup"><span data-stu-id="bc916-600">trademark</span></span>             | <span data-ttu-id="bc916-601">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="bc916-601">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="bc916-602">La aplicación de ejemplo llama a `GetSection` con la clave `starship`.</span><span class="sxs-lookup"><span data-stu-id="bc916-602">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="bc916-603">Los pares clave-valor `starship` están aislados.</span><span class="sxs-lookup"><span data-stu-id="bc916-603">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="bc916-604">El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`.</span><span class="sxs-lookup"><span data-stu-id="bc916-604">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="bc916-605">Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="bc916-605">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_starship)]

::: moniker-end

<span data-ttu-id="bc916-606">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-606">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="bc916-607">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="bc916-607">Bind to an object graph</span></span>

<span data-ttu-id="bc916-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO.</span><span class="sxs-lookup"><span data-stu-id="bc916-608"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="bc916-609">`Bind` está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-609">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="bc916-610">El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="bc916-610">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-611">La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-611">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-xml[](index/samples/1.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="bc916-612">Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="bc916-612">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="bc916-613">La instancia enlazada se asigna a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="bc916-613">The bound instance is assigned to a property for rendering:</span></span>

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

<span data-ttu-id="bc916-614">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="bc916-614">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="bc916-615">`Get<T>` es más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="bc916-615">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="bc916-616">El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:</span><span class="sxs-lookup"><span data-stu-id="bc916-616">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_tvshow)]

::: moniker-end

<span data-ttu-id="bc916-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-617"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="bc916-618">`Get<T>` está disponible en ASP.NET Core 1.1 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="bc916-618">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="bc916-619">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-619">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="bc916-620">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="bc916-620">Bind an array to a class</span></span>

<span data-ttu-id="bc916-621">*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*</span><span class="sxs-lookup"><span data-stu-id="bc916-621">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="bc916-622"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-622">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="bc916-623">Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.</span><span class="sxs-lookup"><span data-stu-id="bc916-623">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="bc916-624">"Bind" está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-624">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="bc916-625">El enlace se proporciona por convención.</span><span class="sxs-lookup"><span data-stu-id="bc916-625">Binding is provided by convention.</span></span> <span data-ttu-id="bc916-626">Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.</span><span class="sxs-lookup"><span data-stu-id="bc916-626">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="bc916-627">**Procesamiento de matriz en memoria**</span><span class="sxs-lookup"><span data-stu-id="bc916-627">**In-memory array processing**</span></span>

<span data-ttu-id="bc916-628">Considere los valores y las claves de configuración que se muestran en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="bc916-628">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="bc916-629">Key</span><span class="sxs-lookup"><span data-stu-id="bc916-629">Key</span></span>             | <span data-ttu-id="bc916-630">Valor</span><span class="sxs-lookup"><span data-stu-id="bc916-630">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="bc916-631">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="bc916-631">array:entries:0</span></span> | <span data-ttu-id="bc916-632">value0</span><span class="sxs-lookup"><span data-stu-id="bc916-632">value0</span></span> |
| <span data-ttu-id="bc916-633">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="bc916-633">array:entries:1</span></span> | <span data-ttu-id="bc916-634">value1</span><span class="sxs-lookup"><span data-stu-id="bc916-634">value1</span></span> |
| <span data-ttu-id="bc916-635">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="bc916-635">array:entries:2</span></span> | <span data-ttu-id="bc916-636">value2</span><span class="sxs-lookup"><span data-stu-id="bc916-636">value2</span></span> |
| <span data-ttu-id="bc916-637">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="bc916-637">array:entries:4</span></span> | <span data-ttu-id="bc916-638">value4</span><span class="sxs-lookup"><span data-stu-id="bc916-638">value4</span></span> |
| <span data-ttu-id="bc916-639">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="bc916-639">array:entries:5</span></span> | <span data-ttu-id="bc916-640">value5</span><span class="sxs-lookup"><span data-stu-id="bc916-640">value5</span></span> |

<span data-ttu-id="bc916-641">Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:</span><span class="sxs-lookup"><span data-stu-id="bc916-641">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=5-12,16)]

::: moniker-end

<span data-ttu-id="bc916-642">La matriz omite un valor de índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="bc916-642">The array skips a value for index &num;3.</span></span> <span data-ttu-id="bc916-643">El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.</span><span class="sxs-lookup"><span data-stu-id="bc916-643">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="bc916-644">En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:</span><span class="sxs-lookup"><span data-stu-id="bc916-644">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-645">Los datos de configuración están enlazados al objeto:</span><span class="sxs-lookup"><span data-stu-id="bc916-645">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="bc916-646">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="bc916-646">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker range=">= aspnetcore-1.1"

<span data-ttu-id="bc916-647">También se puede usar la sintaxis de [ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:</span><span class="sxs-lookup"><span data-stu-id="bc916-647">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="= aspnetcore-1.1"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Controllers/HomeController.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="bc916-648">El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-648">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="bc916-649">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="bc916-649">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="bc916-650">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="bc916-650">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="bc916-651">0</span><span class="sxs-lookup"><span data-stu-id="bc916-651">0</span></span>                            | <span data-ttu-id="bc916-652">value0</span><span class="sxs-lookup"><span data-stu-id="bc916-652">value0</span></span>                       |
| <span data-ttu-id="bc916-653">1</span><span class="sxs-lookup"><span data-stu-id="bc916-653">1</span></span>                            | <span data-ttu-id="bc916-654">value1</span><span class="sxs-lookup"><span data-stu-id="bc916-654">value1</span></span>                       |
| <span data-ttu-id="bc916-655">2</span><span class="sxs-lookup"><span data-stu-id="bc916-655">2</span></span>                            | <span data-ttu-id="bc916-656">value2</span><span class="sxs-lookup"><span data-stu-id="bc916-656">value2</span></span>                       |
| <span data-ttu-id="bc916-657">3</span><span class="sxs-lookup"><span data-stu-id="bc916-657">3</span></span>                            | <span data-ttu-id="bc916-658">value4</span><span class="sxs-lookup"><span data-stu-id="bc916-658">value4</span></span>                       |
| <span data-ttu-id="bc916-659">4</span><span class="sxs-lookup"><span data-stu-id="bc916-659">4</span></span>                            | <span data-ttu-id="bc916-660">value5</span><span class="sxs-lookup"><span data-stu-id="bc916-660">value5</span></span>                       |

<span data-ttu-id="bc916-661">El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="bc916-661">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="bc916-662">Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto.</span><span class="sxs-lookup"><span data-stu-id="bc916-662">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="bc916-663">No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.</span><span class="sxs-lookup"><span data-stu-id="bc916-663">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="bc916-664">El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExample` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-664">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="bc916-665">Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExample.Entries` coincide con la matriz de configuración completa:</span><span class="sxs-lookup"><span data-stu-id="bc916-665">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="bc916-666">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="bc916-666">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bc916-667">En <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="bc916-667">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="bc916-668">En el constructor `Startup`:</span><span class="sxs-lookup"><span data-stu-id="bc916-668">In the `Startup` constructor:</span></span>

```csharp
.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

::: moniker-end

<span data-ttu-id="bc916-669">El par clave-valor que se muestra en la tabla se carga en la configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-669">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="bc916-670">Key</span><span class="sxs-lookup"><span data-stu-id="bc916-670">Key</span></span>             | <span data-ttu-id="bc916-671">Valor</span><span class="sxs-lookup"><span data-stu-id="bc916-671">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="bc916-672">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="bc916-672">array:entries:3</span></span> | <span data-ttu-id="bc916-673">value3</span><span class="sxs-lookup"><span data-stu-id="bc916-673">value3</span></span> |

<span data-ttu-id="bc916-674">Si la instancia de clase `ArrayExample` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExample.Entries` incluye el valor.</span><span class="sxs-lookup"><span data-stu-id="bc916-674">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="bc916-675">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="bc916-675">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="bc916-676">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="bc916-676">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="bc916-677">0</span><span class="sxs-lookup"><span data-stu-id="bc916-677">0</span></span>                            | <span data-ttu-id="bc916-678">value0</span><span class="sxs-lookup"><span data-stu-id="bc916-678">value0</span></span>                       |
| <span data-ttu-id="bc916-679">1</span><span class="sxs-lookup"><span data-stu-id="bc916-679">1</span></span>                            | <span data-ttu-id="bc916-680">value1</span><span class="sxs-lookup"><span data-stu-id="bc916-680">value1</span></span>                       |
| <span data-ttu-id="bc916-681">2</span><span class="sxs-lookup"><span data-stu-id="bc916-681">2</span></span>                            | <span data-ttu-id="bc916-682">value2</span><span class="sxs-lookup"><span data-stu-id="bc916-682">value2</span></span>                       |
| <span data-ttu-id="bc916-683">3</span><span class="sxs-lookup"><span data-stu-id="bc916-683">3</span></span>                            | <span data-ttu-id="bc916-684">value3</span><span class="sxs-lookup"><span data-stu-id="bc916-684">value3</span></span>                       |
| <span data-ttu-id="bc916-685">4</span><span class="sxs-lookup"><span data-stu-id="bc916-685">4</span></span>                            | <span data-ttu-id="bc916-686">value4</span><span class="sxs-lookup"><span data-stu-id="bc916-686">value4</span></span>                       |
| <span data-ttu-id="bc916-687">5</span><span class="sxs-lookup"><span data-stu-id="bc916-687">5</span></span>                            | <span data-ttu-id="bc916-688">value5</span><span class="sxs-lookup"><span data-stu-id="bc916-688">value5</span></span>                       |

<span data-ttu-id="bc916-689">**Procesamiento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="bc916-689">**JSON array processing**</span></span>

<span data-ttu-id="bc916-690">Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero.</span><span class="sxs-lookup"><span data-stu-id="bc916-690">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="bc916-691">En el siguiente archivo de configuración, `subsection` es una matriz:</span><span class="sxs-lookup"><span data-stu-id="bc916-691">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-json[](index/samples/1.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="bc916-692">El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="bc916-692">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="bc916-693">Key</span><span class="sxs-lookup"><span data-stu-id="bc916-693">Key</span></span>                     | <span data-ttu-id="bc916-694">Valor</span><span class="sxs-lookup"><span data-stu-id="bc916-694">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="bc916-695">json_array:key</span><span class="sxs-lookup"><span data-stu-id="bc916-695">json_array:key</span></span>          | <span data-ttu-id="bc916-696">valueA</span><span class="sxs-lookup"><span data-stu-id="bc916-696">valueA</span></span> |
| <span data-ttu-id="bc916-697">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="bc916-697">json_array:subsection:0</span></span> | <span data-ttu-id="bc916-698">valueB</span><span class="sxs-lookup"><span data-stu-id="bc916-698">valueB</span></span> |
| <span data-ttu-id="bc916-699">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="bc916-699">json_array:subsection:1</span></span> | <span data-ttu-id="bc916-700">valueC</span><span class="sxs-lookup"><span data-stu-id="bc916-700">valueC</span></span> |
| <span data-ttu-id="bc916-701">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="bc916-701">json_array:subsection:2</span></span> | <span data-ttu-id="bc916-702">valueD</span><span class="sxs-lookup"><span data-stu-id="bc916-702">valueD</span></span> |

<span data-ttu-id="bc916-703">En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="bc916-703">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-704">Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="bc916-704">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="bc916-705">Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="bc916-705">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="bc916-706">Índice de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="bc916-706">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="bc916-707">Valor de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="bc916-707">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="bc916-708">0</span><span class="sxs-lookup"><span data-stu-id="bc916-708">0</span></span>                                   | <span data-ttu-id="bc916-709">valueB</span><span class="sxs-lookup"><span data-stu-id="bc916-709">valueB</span></span>                              |
| <span data-ttu-id="bc916-710">1</span><span class="sxs-lookup"><span data-stu-id="bc916-710">1</span></span>                                   | <span data-ttu-id="bc916-711">valueC</span><span class="sxs-lookup"><span data-stu-id="bc916-711">valueC</span></span>                              |
| <span data-ttu-id="bc916-712">2</span><span class="sxs-lookup"><span data-stu-id="bc916-712">2</span></span>                                   | <span data-ttu-id="bc916-713">valueD</span><span class="sxs-lookup"><span data-stu-id="bc916-713">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="bc916-714">Proveedores de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="bc916-714">Custom configuration provider</span></span>

<span data-ttu-id="bc916-715">La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="bc916-715">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="bc916-716">El proveedor tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="bc916-716">The provider has the following characteristics:</span></span>

* <span data-ttu-id="bc916-717">La base de datos en memoria de EF se usa para fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="bc916-717">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="bc916-718">Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="bc916-718">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="bc916-719">El proveedor lee una tabla de base de datos en la configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="bc916-719">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="bc916-720">El proveedor no consulta la base de datos por clave.</span><span class="sxs-lookup"><span data-stu-id="bc916-720">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="bc916-721">La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bc916-721">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="bc916-722">Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bc916-722">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="bc916-723">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc916-723">*Models/EFConfigurationValue.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-724">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="bc916-724">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="bc916-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc916-725">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-726">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="bc916-726">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="bc916-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc916-727">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-728">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="bc916-728">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="bc916-729">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="bc916-729">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="bc916-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc916-730">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-731">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="bc916-731">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="bc916-732">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc916-732">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="bc916-733">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bc916-733">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](index/samples/1.x/ConfigurationSample/Startup.cs?name=snippet_Startup&highlight=24)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="bc916-734">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="bc916-734">Access configuration during startup</span></span>

<span data-ttu-id="bc916-735">Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bc916-735">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="bc916-736">Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:</span><span class="sxs-lookup"><span data-stu-id="bc916-736">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="bc916-737">Para ver un ejemplo de cómo acceder a la configuración mediante métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods)</span><span class="sxs-lookup"><span data-stu-id="bc916-737">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="bc916-738">Acceso a la configuración en una página de Razor Pages o en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="bc916-738">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="bc916-739">Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.</span><span class="sxs-lookup"><span data-stu-id="bc916-739">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="bc916-740">En una página de las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="bc916-740">In a Razor Pages page:</span></span>

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

<span data-ttu-id="bc916-741">En una vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="bc916-741">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="bc916-742">Agregar configuración a partir de un ensamblado externo</span><span class="sxs-lookup"><span data-stu-id="bc916-742">Add configuration from an external assembly</span></span>

<span data-ttu-id="bc916-743">Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="bc916-743">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="bc916-744">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="bc916-744">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bc916-745">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bc916-745">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="bc916-746">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Profundización en la configuración de Microsoft)</span><span class="sxs-lookup"><span data-stu-id="bc916-746">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
