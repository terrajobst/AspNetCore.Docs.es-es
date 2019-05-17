---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2019
uid: fundamentals/configuration/index
ms.openlocfilehash: 63a876c09f952537d790f2a5df4b8672df49d015
ms.sourcegitcommit: 3376f224b47a89acf329b2d2f9260046a372f924
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517021"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="b58c7-103">Configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b58c7-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="b58c7-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b58c7-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b58c7-105">La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="b58c7-106">Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="b58c7-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b58c7-107">Azure Key Vault</span></span>
* <span data-ttu-id="b58c7-108">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="b58c7-108">Command-line arguments</span></span>
* <span data-ttu-id="b58c7-109">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="b58c7-109">Custom providers (installed or created)</span></span>
* <span data-ttu-id="b58c7-110">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="b58c7-110">Directory files</span></span>
* <span data-ttu-id="b58c7-111">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="b58c7-111">Environment variables</span></span>
* <span data-ttu-id="b58c7-112">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="b58c7-112">In-memory .NET objects</span></span>
* <span data-ttu-id="b58c7-113">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="b58c7-113">Settings files</span></span>

<span data-ttu-id="b58c7-114">El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="b58c7-114">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="b58c7-115">Las opciones usan clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="b58c7-115">Options uses classes to represent groups of related settings.</span></span> <span data-ttu-id="b58c7-116">Para más información sobre cómo usar el patrón de opciones, consulte <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-116">For more information on using the options pattern, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b58c7-117">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b58c7-117">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b58c7-118">Estos tres paquetes están incluidos en el metapaquete [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-118">These three packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="host-vs-app-configuration"></a><span data-ttu-id="b58c7-119">Configuración de host frente a configuración de aplicación</span><span class="sxs-lookup"><span data-stu-id="b58c7-119">Host vs. app configuration</span></span>

<span data-ttu-id="b58c7-120">Antes de configurar e iniciar la aplicación, se configura e inicia un *host*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-120">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="b58c7-121">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-121">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="b58c7-122">Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="b58c7-122">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="b58c7-123">Los pares clave-valor de la configuración de host se vuelven parte de la configuración global de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-123">Host configuration key-value pairs become part of the app's global configuration.</span></span> <span data-ttu-id="b58c7-124">Para obtener más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte [El host](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="b58c7-124">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see [The host](xref:fundamentals/index#host).</span></span>

## <a name="default-configuration"></a><span data-ttu-id="b58c7-125">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="b58c7-125">Default configuration</span></span>

<span data-ttu-id="b58c7-126">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="b58c7-126">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="b58c7-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> proporciona la configuración predeterminada para la aplicación en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="b58c7-127"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> provides default configuration for the app in the following order:</span></span>

* <span data-ttu-id="b58c7-128">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="b58c7-128">Host configuration is provided from:</span></span>
  * <span data-ttu-id="b58c7-129">Variables de entorno prefijadas con `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b58c7-129">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="b58c7-130">El prefijo (`ASPNETCORE_`) se quita cuando se cargan los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-130">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="b58c7-131">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b58c7-131">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="b58c7-132">La configuración de la aplicación la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="b58c7-132">App configuration is provided from:</span></span>
  * <span data-ttu-id="b58c7-133">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b58c7-133">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b58c7-134">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b58c7-134">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="b58c7-135">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="b58c7-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="b58c7-136">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b58c7-136">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="b58c7-137">Si se usa un prefijo personalizado (por ejemplo, `PREFIX_` con `.AddEnvironmentVariables(prefix: "PREFIX_")`), el prefijo se quita cuando se cargan los pares clave y valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-137">If a custom prefix is used (for example, `PREFIX_` with `.AddEnvironmentVariables(prefix: "PREFIX_")`), the prefix is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="b58c7-138">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="b58c7-138">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="b58c7-139">Los proveedores de configuración se explican posteriormente en este tema.</span><span class="sxs-lookup"><span data-stu-id="b58c7-139">The configuration providers are explained later in this topic.</span></span> <span data-ttu-id="b58c7-140">Para obtener más información sobre el host y <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, vea <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-140">For more information on the host and <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, see <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="security"></a><span data-ttu-id="b58c7-141">Seguridad</span><span class="sxs-lookup"><span data-stu-id="b58c7-141">Security</span></span>

<span data-ttu-id="b58c7-142">Adopte estos procedimientos recomendados:</span><span class="sxs-lookup"><span data-stu-id="b58c7-142">Adopt the following best practices:</span></span>

* <span data-ttu-id="b58c7-143">Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="b58c7-143">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="b58c7-144">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="b58c7-144">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="b58c7-145">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="b58c7-145">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="b58c7-146">Obtenga más información sobre [cómo usar varios entornos](xref:fundamentals/environments) y cómo administrar el [almacenamiento seguro de secretos de aplicación en el desarrollo con el Administrador de secretos](xref:security/app-secrets) (incluye consejos sobre el uso de variables de entorno para almacenar información confidencial).</span><span class="sxs-lookup"><span data-stu-id="b58c7-146">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing the [safe storage of app secrets in development with the Secret Manager](xref:security/app-secrets) (includes advice on using environment variables to store sensitive data).</span></span> <span data-ttu-id="b58c7-147">El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local.</span><span class="sxs-lookup"><span data-stu-id="b58c7-147">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="b58c7-148">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="b58c7-148">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="b58c7-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) es una opción para el almacenamiento seguro de los secretos de aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-149">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) is one option for the safe storage of app secrets.</span></span> <span data-ttu-id="b58c7-150">Para obtener más información, vea <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-150">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="b58c7-151">Datos de configuración jerárquica</span><span class="sxs-lookup"><span data-stu-id="b58c7-151">Hierarchical configuration data</span></span>

<span data-ttu-id="b58c7-152">La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-152">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="b58c7-153">En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:</span><span class="sxs-lookup"><span data-stu-id="b58c7-153">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="b58c7-154">Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-154">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="b58c7-155">Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:</span><span class="sxs-lookup"><span data-stu-id="b58c7-155">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="b58c7-156">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-156">section0:key0</span></span>
* <span data-ttu-id="b58c7-157">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-157">section0:key1</span></span>
* <span data-ttu-id="b58c7-158">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-158">section1:key0</span></span>
* <span data-ttu-id="b58c7-159">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-159">section1:key1</span></span>

<span data-ttu-id="b58c7-160">Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-160"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="b58c7-161">Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="b58c7-161">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span> <span data-ttu-id="b58c7-162">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-162">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="conventions"></a><span data-ttu-id="b58c7-163">Convenciones</span><span class="sxs-lookup"><span data-stu-id="b58c7-163">Conventions</span></span>

<span data-ttu-id="b58c7-164">Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-164">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="b58c7-165">Los proveedores de configuración que implementan la detección de cambios tienen la capacidad de volver a cargar la configuración cuando se cambia una configuración subyacente.</span><span class="sxs-lookup"><span data-stu-id="b58c7-165">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="b58c7-166">Por ejemplo, el proveedor de configuración de archivos (descrito más adelante en este tema) y el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) implementan la detección de cambios.</span><span class="sxs-lookup"><span data-stu-id="b58c7-166">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="b58c7-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-167"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="b58c7-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages para obtener la configuración de la clase:</span><span class="sxs-lookup"><span data-stu-id="b58c7-168"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> to obtain configuration for the class:</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    // The _config local variable is used to obtain configuration 
    // throughout the class.
}
```

<span data-ttu-id="b58c7-169">Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.</span><span class="sxs-lookup"><span data-stu-id="b58c7-169">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

<span data-ttu-id="b58c7-170">Las claves de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b58c7-170">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="b58c7-171">Las claves no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="b58c7-171">Keys are case-insensitive.</span></span> <span data-ttu-id="b58c7-172">Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.</span><span class="sxs-lookup"><span data-stu-id="b58c7-172">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="b58c7-173">Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.</span><span class="sxs-lookup"><span data-stu-id="b58c7-173">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="b58c7-174">Claves jerárquicas</span><span class="sxs-lookup"><span data-stu-id="b58c7-174">Hierarchical keys</span></span>
  * <span data-ttu-id="b58c7-175">Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="b58c7-175">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="b58c7-176">En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="b58c7-176">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="b58c7-177">Un guion bajo doble (`__`) es compatible con todas las plataformas y se convierte en un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-177">A double underscore (`__`) is supported by all platforms and is converted to a colon.</span></span>
  * <span data-ttu-id="b58c7-178">En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="b58c7-178">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="b58c7-179">Debe proporcionar código para reemplazar los guiones por dos puntos cuando los secretos se cargan en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-179">You must provide code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="b58c7-180"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-180">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b58c7-181">El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="b58c7-181">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

<span data-ttu-id="b58c7-182">Los valores de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="b58c7-182">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="b58c7-183">Los valores son cadenas.</span><span class="sxs-lookup"><span data-stu-id="b58c7-183">Values are strings.</span></span>
* <span data-ttu-id="b58c7-184">Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-184">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="b58c7-185">Proveedores</span><span class="sxs-lookup"><span data-stu-id="b58c7-185">Providers</span></span>

<span data-ttu-id="b58c7-186">La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b58c7-186">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="b58c7-187">Proveedor</span><span class="sxs-lookup"><span data-stu-id="b58c7-187">Provider</span></span> | <span data-ttu-id="b58c7-188">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="b58c7-188">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="b58c7-189">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="b58c7-189">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="b58c7-190">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b58c7-190">Azure Key Vault</span></span> |
| [<span data-ttu-id="b58c7-191">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="b58c7-191">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="b58c7-192">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="b58c7-192">Command-line parameters</span></span> |
| [<span data-ttu-id="b58c7-193">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="b58c7-193">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="b58c7-194">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="b58c7-194">Custom source</span></span> |
| [<span data-ttu-id="b58c7-195">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="b58c7-195">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="b58c7-196">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="b58c7-196">Environment variables</span></span> |
| [<span data-ttu-id="b58c7-197">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="b58c7-197">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="b58c7-198">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="b58c7-198">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="b58c7-199">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="b58c7-199">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="b58c7-200">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="b58c7-200">Directory files</span></span> |
| [<span data-ttu-id="b58c7-201">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="b58c7-201">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="b58c7-202">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="b58c7-202">In-memory collections</span></span> |
| <span data-ttu-id="b58c7-203">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="b58c7-203">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="b58c7-204">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="b58c7-204">File in the user profile directory</span></span> |

<span data-ttu-id="b58c7-205">Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="b58c7-205">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="b58c7-206">En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en que el código podría organizarlos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-206">The configuration providers described in this topic are described in alphabetical order, not in the order that your code may arrange them.</span></span> <span data-ttu-id="b58c7-207">Ordene los proveedores de configuración en el código para cumplir con sus prioridades relacionadas con los orígenes de configuración subyacentes.</span><span class="sxs-lookup"><span data-stu-id="b58c7-207">Order configuration providers in your code to suit your priorities for the underlying configuration sources.</span></span>

<span data-ttu-id="b58c7-208">Esta es una secuencia típica de proveedores de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-208">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="b58c7-209">Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)</span><span class="sxs-lookup"><span data-stu-id="b58c7-209">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="b58c7-210">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="b58c7-210">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="b58c7-211">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (solo en el entorno de desarrollo)</span><span class="sxs-lookup"><span data-stu-id="b58c7-211">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment only)</span></span>
1. <span data-ttu-id="b58c7-212">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="b58c7-212">Environment variables</span></span>
1. <span data-ttu-id="b58c7-213">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="b58c7-213">Command-line arguments</span></span>

<span data-ttu-id="b58c7-214">Una práctica común es colocar el proveedor de configuración de línea de comandos al final de en una serie de proveedores para permitir que los argumentos de la línea de comandos invaliden la configuración que establecieron los demás proveedores.</span><span class="sxs-lookup"><span data-stu-id="b58c7-214">It's a common practice to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="b58c7-215">Esta secuencia de los proveedores se aplica al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-215">This sequence of providers is put into place when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b58c7-216">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b58c7-216">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

## <a name="configureappconfiguration"></a><span data-ttu-id="b58c7-217">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="b58c7-217">ConfigureAppConfiguration</span></span>

<span data-ttu-id="b58c7-218">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> agrega automáticamente:</span><span class="sxs-lookup"><span data-stu-id="b58c7-218">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration providers in addition to those added automatically by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=19)]

<span data-ttu-id="b58c7-219">La configuración proporcionada a la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-219">Configuration supplied to the app in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b58c7-220">Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="b58c7-220">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="b58c7-221">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="b58c7-221">Command-line Configuration Provider</span></span>

<span data-ttu-id="b58c7-222"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b58c7-222">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="b58c7-223">Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-223">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b58c7-224">`AddCommandLine` se llama automáticamente cuando se inicializa un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-224">`AddCommandLine` is automatically called when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b58c7-225">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b58c7-225">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b58c7-226">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="b58c7-226">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b58c7-227">Configuración opcional de *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-227">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="b58c7-228">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="b58c7-228">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b58c7-229">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="b58c7-229">Environment variables.</span></span>

<span data-ttu-id="b58c7-230">`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final.</span><span class="sxs-lookup"><span data-stu-id="b58c7-230">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="b58c7-231">Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="b58c7-231">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="b58c7-232">`CreateDefaultBuilder` actúa cuando se construye el host.</span><span class="sxs-lookup"><span data-stu-id="b58c7-232">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="b58c7-233">Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.</span><span class="sxs-lookup"><span data-stu-id="b58c7-233">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="b58c7-234">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-234">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b58c7-235">Ya se ha llamado a `AddCommandLine` por parte de `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-235">`AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b58c7-236">Si tiene que proporcionar la configuración de la aplicación y mantener la posibilidad de invalidar dicha configuración con argumentos de línea de comandos, llame a los proveedores adicionales de la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> y llame por último a `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-236">If you need to provide app configuration and still be able to override that configuration with command-line arguments, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddCommandLine` last.</span></span>

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

<span data-ttu-id="b58c7-237">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-237">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b58c7-238">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="b58c7-238">**Example**</span></span>

<span data-ttu-id="b58c7-239">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-239">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="b58c7-240">Abra un símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="b58c7-240">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="b58c7-241">Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-241">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="b58c7-242">Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-242">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b58c7-243">Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-243">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="b58c7-244">Argumentos</span><span class="sxs-lookup"><span data-stu-id="b58c7-244">Arguments</span></span>

<span data-ttu-id="b58c7-245">El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio.</span><span class="sxs-lookup"><span data-stu-id="b58c7-245">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="b58c7-246">El valor puede ser NULL si se usa un signo igual (por ejemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="b58c7-246">The value can be null if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="b58c7-247">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="b58c7-247">Key prefix</span></span>               | <span data-ttu-id="b58c7-248">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="b58c7-248">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="b58c7-249">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="b58c7-249">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="b58c7-250">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="b58c7-250">Two dashes (`--`)</span></span>        | <span data-ttu-id="b58c7-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="b58c7-251">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="b58c7-252">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="b58c7-252">Forward slash (`/`)</span></span>      | <span data-ttu-id="b58c7-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="b58c7-253">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="b58c7-254">Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.</span><span class="sxs-lookup"><span data-stu-id="b58c7-254">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="b58c7-255">Comandos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="b58c7-255">Example commands:</span></span>

```console
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="b58c7-256">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="b58c7-256">Switch mappings</span></span>

<span data-ttu-id="b58c7-257">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="b58c7-257">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="b58c7-258">Cuando crea manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, puede proporcionar un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-258">When you manually build configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, you can provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="b58c7-259">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-259">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="b58c7-260">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-260">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="b58c7-261">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="b58c7-261">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="b58c7-262">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="b58c7-262">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="b58c7-263">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="b58c7-263">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="b58c7-264">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="b58c7-264">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="b58c7-265">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-265">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b58c7-266">Como se muestra en el ejemplo anterior, la llamada a `CreateDefaultBuilder` no debería pasar argumentos cuando se usan las asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="b58c7-266">As shown in the preceding example, the call to `CreateDefaultBuilder` shouldn't pass arguments when switch mappings are used.</span></span> <span data-ttu-id="b58c7-267">La llamada de `AddCommandLine` del método `CreateDefaultBuilder` no incluye modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-267">`CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="b58c7-268">Si los argumentos incluyen un conmutador asignado y se pasan a `CreateDefaultBuilder`, el proveedor de `AddCommandLine` no se puede inicializar con una <xref:System.FormatException>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-268">If the arguments include a mapped switch and are passed to `CreateDefaultBuilder`, its `AddCommandLine` provider fails to initialize with a <xref:System.FormatException>.</span></span> <span data-ttu-id="b58c7-269">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="b58c7-269">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="b58c7-270">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="b58c7-270">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="b58c7-271">Key</span><span class="sxs-lookup"><span data-stu-id="b58c7-271">Key</span></span>       | <span data-ttu-id="b58c7-272">Valor</span><span class="sxs-lookup"><span data-stu-id="b58c7-272">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="b58c7-273">Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:</span><span class="sxs-lookup"><span data-stu-id="b58c7-273">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```console
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="b58c7-274">Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="b58c7-274">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="b58c7-275">Key</span><span class="sxs-lookup"><span data-stu-id="b58c7-275">Key</span></span>               | <span data-ttu-id="b58c7-276">Valor</span><span class="sxs-lookup"><span data-stu-id="b58c7-276">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="b58c7-277">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="b58c7-277">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="b58c7-278"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b58c7-278">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="b58c7-279">Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-279">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="b58c7-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer las variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="b58c7-280">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits you to set environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="b58c7-281">Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="b58c7-281">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="b58c7-282">Se llama automáticamente a `AddEnvironmentVariables` para las variables de entorno con el prefijo `ASPNETCORE_` al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-282">`AddEnvironmentVariables` is automatically called for environment variables prefixed with `ASPNETCORE_` when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>.</span></span> <span data-ttu-id="b58c7-283">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b58c7-283">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b58c7-284">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="b58c7-284">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b58c7-285">Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-285">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="b58c7-286">Configuración opcional de *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-286">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>
* <span data-ttu-id="b58c7-287">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="b58c7-287">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b58c7-288">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-288">Command-line arguments.</span></span>

<span data-ttu-id="b58c7-289">El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-289">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="b58c7-290">Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-290">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="b58c7-291">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-291">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="b58c7-292">Si tiene que proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> y llame a `AddEnvironmentVariables` con el prefijo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-292">If you need to provide app configuration from additional environment variables, call the app's additional providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> and call `AddEnvironmentVariables` with the prefix.</span></span>

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

<span data-ttu-id="b58c7-293">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-293">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables()
    .Build();

var host = new WebHostBuilder()
    .UseConfiguration(config)
    .UseKestrel()
    .UseStartup<Startup>();
```

<span data-ttu-id="b58c7-294">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="b58c7-294">**Example**</span></span>

<span data-ttu-id="b58c7-295">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-295">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="b58c7-296">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-296">Run the sample app.</span></span> <span data-ttu-id="b58c7-297">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-297">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b58c7-298">Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-298">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="b58c7-299">El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.</span><span class="sxs-lookup"><span data-stu-id="b58c7-299">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="b58c7-300">Para que la lista de variables de entorno que muestra la aplicación sea breve, la aplicación filtra las variables de entorno que comienzan con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b58c7-300">To keep the list of environment variables rendered by the app short, the app filters environment variables to those that start with the following:</span></span>

* <span data-ttu-id="b58c7-301">ASPNETCORE_</span><span class="sxs-lookup"><span data-stu-id="b58c7-301">ASPNETCORE_</span></span>
* <span data-ttu-id="b58c7-302">urls</span><span class="sxs-lookup"><span data-stu-id="b58c7-302">urls</span></span>
* <span data-ttu-id="b58c7-303">Registro</span><span class="sxs-lookup"><span data-stu-id="b58c7-303">Logging</span></span>
* <span data-ttu-id="b58c7-304">ENVIRONMENT</span><span class="sxs-lookup"><span data-stu-id="b58c7-304">ENVIRONMENT</span></span>
* <span data-ttu-id="b58c7-305">contentRoot</span><span class="sxs-lookup"><span data-stu-id="b58c7-305">contentRoot</span></span>
* <span data-ttu-id="b58c7-306">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="b58c7-306">AllowedHosts</span></span>
* <span data-ttu-id="b58c7-307">applicationName</span><span class="sxs-lookup"><span data-stu-id="b58c7-307">applicationName</span></span>
* <span data-ttu-id="b58c7-308">CommandLine</span><span class="sxs-lookup"><span data-stu-id="b58c7-308">CommandLine</span></span>

<span data-ttu-id="b58c7-309">Si quiere exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b58c7-309">If you wish to expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="b58c7-310">Prefijos</span><span class="sxs-lookup"><span data-stu-id="b58c7-310">Prefixes</span></span>

<span data-ttu-id="b58c7-311">Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-311">Environment variables loaded into the app's configuration are filtered when you supply a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="b58c7-312">Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-312">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="b58c7-313">El prefijo se quita cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-313">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="b58c7-314">El método de conveniencia estático `CreateDefaultBuilder` crea un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> para establecer el host de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-314">The static convenience method `CreateDefaultBuilder` creates a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> to establish the app's host.</span></span> <span data-ttu-id="b58c7-315">Cuando se crea `WebHostBuilder`, encuentra su configuración de host en variables de entorno con el prefijo `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-315">When `WebHostBuilder` is created, it finds its host configuration in environment variables prefixed with `ASPNETCORE_`.</span></span>

<span data-ttu-id="b58c7-316">**Prefijos de cadena de conexión**</span><span class="sxs-lookup"><span data-stu-id="b58c7-316">**Connection string prefixes**</span></span>

<span data-ttu-id="b58c7-317">La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-317">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="b58c7-318">Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-318">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="b58c7-319">Prefijo de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="b58c7-319">Connection string prefix</span></span> | <span data-ttu-id="b58c7-320">Proveedor</span><span class="sxs-lookup"><span data-stu-id="b58c7-320">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="b58c7-321">Proveedor personalizado</span><span class="sxs-lookup"><span data-stu-id="b58c7-321">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="b58c7-322">MySQL</span><span class="sxs-lookup"><span data-stu-id="b58c7-322">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="b58c7-323">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b58c7-323">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="b58c7-324">SQL Server</span><span class="sxs-lookup"><span data-stu-id="b58c7-324">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="b58c7-325">Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:</span><span class="sxs-lookup"><span data-stu-id="b58c7-325">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="b58c7-326">La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="b58c7-326">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="b58c7-327">Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).</span><span class="sxs-lookup"><span data-stu-id="b58c7-327">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="b58c7-328">Clave de variable de entorno</span><span class="sxs-lookup"><span data-stu-id="b58c7-328">Environment variable key</span></span> | <span data-ttu-id="b58c7-329">Clave de configuración convertida</span><span class="sxs-lookup"><span data-stu-id="b58c7-329">Converted configuration key</span></span> | <span data-ttu-id="b58c7-330">Entrada de configuración del proveedor</span><span class="sxs-lookup"><span data-stu-id="b58c7-330">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b58c7-331">Entrada de configuración no creada.</span><span class="sxs-lookup"><span data-stu-id="b58c7-331">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b58c7-332">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b58c7-332">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b58c7-333">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="b58c7-333">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b58c7-334">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b58c7-334">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b58c7-335">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b58c7-335">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="b58c7-336">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="b58c7-336">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="b58c7-337">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="b58c7-337">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="b58c7-338">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="b58c7-338">File Configuration Provider</span></span>

<span data-ttu-id="b58c7-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-339"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="b58c7-340">Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:</span><span class="sxs-lookup"><span data-stu-id="b58c7-340">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="b58c7-341">Proveedor de configuración INI</span><span class="sxs-lookup"><span data-stu-id="b58c7-341">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="b58c7-342">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="b58c7-342">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="b58c7-343">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="b58c7-343">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="b58c7-344">Proveedor de configuración de INI</span><span class="sxs-lookup"><span data-stu-id="b58c7-344">INI Configuration Provider</span></span>

<span data-ttu-id="b58c7-345"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b58c7-345">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="b58c7-346">Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-346">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b58c7-347">Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.</span><span class="sxs-lookup"><span data-stu-id="b58c7-347">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="b58c7-348">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="b58c7-348">Overloads permit specifying:</span></span>

* <span data-ttu-id="b58c7-349">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="b58c7-349">Whether the file is optional.</span></span>
* <span data-ttu-id="b58c7-350">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="b58c7-350">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b58c7-351"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-351">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b58c7-352">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-352">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b58c7-353">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-353">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b58c7-354">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-354">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-355">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-355">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b58c7-356">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-356">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b58c7-357">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-357">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-358">Un ejemplo genérico de un archivo de configuración INI:</span><span class="sxs-lookup"><span data-stu-id="b58c7-358">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="b58c7-359">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="b58c7-359">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b58c7-360">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-360">section0:key0</span></span>
* <span data-ttu-id="b58c7-361">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-361">section0:key1</span></span>
* <span data-ttu-id="b58c7-362">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="b58c7-362">section1:subsection:key</span></span>
* <span data-ttu-id="b58c7-363">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="b58c7-363">section2:subsection0:key</span></span>
* <span data-ttu-id="b58c7-364">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="b58c7-364">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="b58c7-365">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="b58c7-365">JSON Configuration Provider</span></span>

<span data-ttu-id="b58c7-366"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b58c7-366">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="b58c7-367">Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-367">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b58c7-368">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="b58c7-368">Overloads permit specifying:</span></span>

* <span data-ttu-id="b58c7-369">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="b58c7-369">Whether the file is optional.</span></span>
* <span data-ttu-id="b58c7-370">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="b58c7-370">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b58c7-371"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-371">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b58c7-372">`AddJsonFile` se llama automáticamente dos veces al inicializar un nuevo <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> con <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-372">`AddJsonFile` is automatically called twice when you initialize a new <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> with <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span> <span data-ttu-id="b58c7-373">Se llama al método para cargar la configuración desde:</span><span class="sxs-lookup"><span data-stu-id="b58c7-373">The method is called to load configuration from:</span></span>

* <span data-ttu-id="b58c7-374">*appsettings.json* &ndash; Este archivo se lee primero.</span><span class="sxs-lookup"><span data-stu-id="b58c7-374">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="b58c7-375">La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-375">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="b58c7-376">*appsettings.{Environment}.json* &ndash; La versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="b58c7-376">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="b58c7-377">Para más información, consulte [Host web: Configuración de un host](xref:fundamentals/host/web-host#set-up-a-host).</span><span class="sxs-lookup"><span data-stu-id="b58c7-377">For more information, see [Web Host: Set up a host](xref:fundamentals/host/web-host#set-up-a-host).</span></span>

<span data-ttu-id="b58c7-378">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="b58c7-378">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="b58c7-379">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="b58c7-379">Environment variables.</span></span>
* <span data-ttu-id="b58c7-380">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (en el entorno de desarrollo).</span><span class="sxs-lookup"><span data-stu-id="b58c7-380">[User secrets (Secret Manager)](xref:security/app-secrets) (in the Development environment).</span></span>
* <span data-ttu-id="b58c7-381">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-381">Command-line arguments.</span></span>

<span data-ttu-id="b58c7-382">El proveedor de configuración de JSON se establece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="b58c7-382">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="b58c7-383">Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-383">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="b58c7-384">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-384">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

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

<span data-ttu-id="b58c7-385">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-385">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b58c7-386">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-386">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-387">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-387">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b58c7-388">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-388">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b58c7-389">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-389">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-390">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="b58c7-390">**Example**</span></span>

<span data-ttu-id="b58c7-391">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-391">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`.</span></span> <span data-ttu-id="b58c7-392">La configuración se carga desde *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-392">Configuration is loaded from *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

1. <span data-ttu-id="b58c7-393">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-393">Run the sample app.</span></span> <span data-ttu-id="b58c7-394">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-394">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="b58c7-395">Observe que la salida contiene los pares clave-valor para la configuración que se muestra en la tabla según el entorno.</span><span class="sxs-lookup"><span data-stu-id="b58c7-395">Observe that the output contains key-value pairs for the configuration shown in the table depending on the environment.</span></span> <span data-ttu-id="b58c7-396">Las claves de configuración de registro usan dos puntos (`:`) como separador jerárquico.</span><span class="sxs-lookup"><span data-stu-id="b58c7-396">Logging configuration keys use the colon (`:`) as a hierarchical separator.</span></span>

| <span data-ttu-id="b58c7-397">Key</span><span class="sxs-lookup"><span data-stu-id="b58c7-397">Key</span></span>                        | <span data-ttu-id="b58c7-398">Valor de desarrollo</span><span class="sxs-lookup"><span data-stu-id="b58c7-398">Development Value</span></span> | <span data-ttu-id="b58c7-399">Valor de producción</span><span class="sxs-lookup"><span data-stu-id="b58c7-399">Production Value</span></span> |
| -------------------------- | :---------------: | :--------------: |
| <span data-ttu-id="b58c7-400">Logging:LogLevel:System</span><span class="sxs-lookup"><span data-stu-id="b58c7-400">Logging:LogLevel:System</span></span>    | <span data-ttu-id="b58c7-401">Información</span><span class="sxs-lookup"><span data-stu-id="b58c7-401">Information</span></span>       | <span data-ttu-id="b58c7-402">Información</span><span class="sxs-lookup"><span data-stu-id="b58c7-402">Information</span></span>      |
| <span data-ttu-id="b58c7-403">Logging:LogLevel:Microsoft</span><span class="sxs-lookup"><span data-stu-id="b58c7-403">Logging:LogLevel:Microsoft</span></span> | <span data-ttu-id="b58c7-404">Información</span><span class="sxs-lookup"><span data-stu-id="b58c7-404">Information</span></span>       | <span data-ttu-id="b58c7-405">Información</span><span class="sxs-lookup"><span data-stu-id="b58c7-405">Information</span></span>      |
| <span data-ttu-id="b58c7-406">Logging:LogLevel:Default</span><span class="sxs-lookup"><span data-stu-id="b58c7-406">Logging:LogLevel:Default</span></span>   | <span data-ttu-id="b58c7-407">Depuración</span><span class="sxs-lookup"><span data-stu-id="b58c7-407">Debug</span></span>             | <span data-ttu-id="b58c7-408">Error</span><span class="sxs-lookup"><span data-stu-id="b58c7-408">Error</span></span>            |
| <span data-ttu-id="b58c7-409">AllowedHosts</span><span class="sxs-lookup"><span data-stu-id="b58c7-409">AllowedHosts</span></span>               | *                 | *                |

### <a name="xml-configuration-provider"></a><span data-ttu-id="b58c7-410">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="b58c7-410">XML Configuration Provider</span></span>

<span data-ttu-id="b58c7-411"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="b58c7-411">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="b58c7-412">Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-412">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b58c7-413">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="b58c7-413">Overloads permit specifying:</span></span>

* <span data-ttu-id="b58c7-414">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="b58c7-414">Whether the file is optional.</span></span>
* <span data-ttu-id="b58c7-415">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="b58c7-415">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="b58c7-416"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-416">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="b58c7-417">El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-417">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="b58c7-418">No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-418">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="b58c7-419">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-419">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b58c7-420">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-420">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b58c7-421">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-421">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-422">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-422">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

<span data-ttu-id="b58c7-423">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-423">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b58c7-424">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-424">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-425">Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:</span><span class="sxs-lookup"><span data-stu-id="b58c7-425">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="b58c7-426">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="b58c7-426">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b58c7-427">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-427">section0:key0</span></span>
* <span data-ttu-id="b58c7-428">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-428">section0:key1</span></span>
* <span data-ttu-id="b58c7-429">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-429">section1:key0</span></span>
* <span data-ttu-id="b58c7-430">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-430">section1:key1</span></span>

<span data-ttu-id="b58c7-431">Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:</span><span class="sxs-lookup"><span data-stu-id="b58c7-431">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="b58c7-432">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="b58c7-432">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b58c7-433">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-433">section:section0:key:key0</span></span>
* <span data-ttu-id="b58c7-434">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-434">section:section0:key:key1</span></span>
* <span data-ttu-id="b58c7-435">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-435">section:section1:key:key0</span></span>
* <span data-ttu-id="b58c7-436">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-436">section:section1:key:key1</span></span>

<span data-ttu-id="b58c7-437">Los atributos se pueden usar para suministrar valores:</span><span class="sxs-lookup"><span data-stu-id="b58c7-437">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="b58c7-438">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="b58c7-438">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="b58c7-439">key:attribute</span><span class="sxs-lookup"><span data-stu-id="b58c7-439">key:attribute</span></span>
* <span data-ttu-id="b58c7-440">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="b58c7-440">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="b58c7-441">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="b58c7-441">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="b58c7-442"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-442">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="b58c7-443">La clave es el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-443">The key is the file name.</span></span> <span data-ttu-id="b58c7-444">El valor contiene el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-444">The value contains the file's contents.</span></span> <span data-ttu-id="b58c7-445">El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.</span><span class="sxs-lookup"><span data-stu-id="b58c7-445">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="b58c7-446">Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-446">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="b58c7-447">La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="b58c7-447">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="b58c7-448">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="b58c7-448">Overloads permit specifying:</span></span>

* <span data-ttu-id="b58c7-449">Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.</span><span class="sxs-lookup"><span data-stu-id="b58c7-449">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="b58c7-450">Si el directorio es opcional y la ruta de acceso al directorio.</span><span class="sxs-lookup"><span data-stu-id="b58c7-450">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="b58c7-451">El carácter de subrayado doble (`__`) se usa como delimitador de claves de configuración en los nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="b58c7-451">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="b58c7-452">Por ejemplo, el nombre de archivo `Logging__LogLevel__System` genera la clave de configuración `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-452">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="b58c7-453">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-453">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b58c7-454">Se establece la ruta de acceso base con <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-454">The base path is set with <xref:Microsoft.Extensions.Configuration.FileConfigurationExtensions.SetBasePath*>.</span></span> <span data-ttu-id="b58c7-455">`SetBasePath` está en el paquete [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-455">`SetBasePath` is in the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-456">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-456">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="memory-configuration-provider"></a><span data-ttu-id="b58c7-457">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="b58c7-457">Memory Configuration Provider</span></span>

<span data-ttu-id="b58c7-458"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-458">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="b58c7-459">Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-459">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="b58c7-460">El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-460">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="b58c7-461">Llame a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-461">Call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> when building the host to specify the app's configuration:</span></span>

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

<span data-ttu-id="b58c7-462">Al crear directamente un <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, llame a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> con la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-462">When creating a <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> directly, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> with the configuration:</span></span>

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

## <a name="getvalue"></a><span data-ttu-id="b58c7-463">GetValue</span><span class="sxs-lookup"><span data-stu-id="b58c7-463">GetValue</span></span>

<span data-ttu-id="b58c7-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un valor de la configuración con una clave especificada y lo convierte al tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="b58c7-464">[ConfigurationBinder.GetValue&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a value from configuration with a specified key and converts it to the specified type.</span></span> <span data-ttu-id="b58c7-465">Una sobrecarga permite proporcionar un valor predeterminado si no se encuentra la clave.</span><span class="sxs-lookup"><span data-stu-id="b58c7-465">An overload permits you to provide a default value if the key isn't found.</span></span>

<span data-ttu-id="b58c7-466">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="b58c7-466">The following example:</span></span>

* <span data-ttu-id="b58c7-467">Se extrae el valor de cadena de la configuración con la clave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-467">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="b58c7-468">Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-468">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="b58c7-469">Se escribe el valor como `int`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-469">Types the value as an `int`.</span></span>
* <span data-ttu-id="b58c7-470">Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="b58c7-470">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
// using Microsoft.Extensions.Configuration;

public class IndexModel : PageModel
{
    public IndexModel(IConfiguration config)
    {
        _config = config;
    }

    public int NumberConfig { get; private set; }

    public void OnGet()
    {
        NumberConfig = _config.GetValue<int>("NumberKey", 99);
    }
}
```

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="b58c7-471">GetSection, GetChildren y Exists</span><span class="sxs-lookup"><span data-stu-id="b58c7-471">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="b58c7-472">Para los ejemplos siguientes, considere el siguiente archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="b58c7-472">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="b58c7-473">Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:</span><span class="sxs-lookup"><span data-stu-id="b58c7-473">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="b58c7-474">Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-474">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="b58c7-475">section0:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-475">section0:key0</span></span>
* <span data-ttu-id="b58c7-476">section0:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-476">section0:key1</span></span>
* <span data-ttu-id="b58c7-477">section1:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-477">section1:key0</span></span>
* <span data-ttu-id="b58c7-478">section1:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-478">section1:key1</span></span>
* <span data-ttu-id="b58c7-479">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-479">section2:subsection0:key0</span></span>
* <span data-ttu-id="b58c7-480">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-480">section2:subsection0:key1</span></span>
* <span data-ttu-id="b58c7-481">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="b58c7-481">section2:subsection1:key0</span></span>
* <span data-ttu-id="b58c7-482">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="b58c7-482">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="b58c7-483">GetSection</span><span class="sxs-lookup"><span data-stu-id="b58c7-483">GetSection</span></span>

<span data-ttu-id="b58c7-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.</span><span class="sxs-lookup"><span data-stu-id="b58c7-484">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span> <span data-ttu-id="b58c7-485">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-485">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-486">Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:</span><span class="sxs-lookup"><span data-stu-id="b58c7-486">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="b58c7-487">`configSection` no tiene ningún valor, solo una clave y una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="b58c7-487">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="b58c7-488">De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:</span><span class="sxs-lookup"><span data-stu-id="b58c7-488">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="b58c7-489">`GetSection` nunca devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-489">`GetSection` never returns `null`.</span></span> <span data-ttu-id="b58c7-490">Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.</span><span class="sxs-lookup"><span data-stu-id="b58c7-490">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="b58c7-491">Cuando `GetSection` devuelve una sección coincidente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> no se rellena.</span><span class="sxs-lookup"><span data-stu-id="b58c7-491">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="b58c7-492">Se devuelven <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> y <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> si la sección existe.</span><span class="sxs-lookup"><span data-stu-id="b58c7-492">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="b58c7-493">GetChildren</span><span class="sxs-lookup"><span data-stu-id="b58c7-493">GetChildren</span></span>

<span data-ttu-id="b58c7-494">Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:</span><span class="sxs-lookup"><span data-stu-id="b58c7-494">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="b58c7-495">Existe</span><span class="sxs-lookup"><span data-stu-id="b58c7-495">Exists</span></span>

<span data-ttu-id="b58c7-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-496">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="b58c7-497">Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-497">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="b58c7-498">Enlace a una clase</span><span class="sxs-lookup"><span data-stu-id="b58c7-498">Bind to a class</span></span>

<span data-ttu-id="b58c7-499">La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="b58c7-499">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="b58c7-500">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-500">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="b58c7-501">Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="b58c7-501">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="b58c7-502">`Bind` está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-502">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-503">La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="b58c7-503">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="b58c7-504">La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-504">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="b58c7-505">Se crean los siguientes pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-505">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="b58c7-506">Key</span><span class="sxs-lookup"><span data-stu-id="b58c7-506">Key</span></span>                   | <span data-ttu-id="b58c7-507">Valor</span><span class="sxs-lookup"><span data-stu-id="b58c7-507">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="b58c7-508">starship:name</span><span class="sxs-lookup"><span data-stu-id="b58c7-508">starship:name</span></span>         | <span data-ttu-id="b58c7-509">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="b58c7-509">USS Enterprise</span></span>                                    |
| <span data-ttu-id="b58c7-510">starship:registry</span><span class="sxs-lookup"><span data-stu-id="b58c7-510">starship:registry</span></span>     | <span data-ttu-id="b58c7-511">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="b58c7-511">NCC-1701</span></span>                                          |
| <span data-ttu-id="b58c7-512">starship:class</span><span class="sxs-lookup"><span data-stu-id="b58c7-512">starship:class</span></span>        | <span data-ttu-id="b58c7-513">Constitution</span><span class="sxs-lookup"><span data-stu-id="b58c7-513">Constitution</span></span>                                      |
| <span data-ttu-id="b58c7-514">starship:length</span><span class="sxs-lookup"><span data-stu-id="b58c7-514">starship:length</span></span>       | <span data-ttu-id="b58c7-515">304.8</span><span class="sxs-lookup"><span data-stu-id="b58c7-515">304.8</span></span>                                             |
| <span data-ttu-id="b58c7-516">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="b58c7-516">starship:commissioned</span></span> | <span data-ttu-id="b58c7-517">False</span><span class="sxs-lookup"><span data-stu-id="b58c7-517">False</span></span>                                             |
| <span data-ttu-id="b58c7-518">trademark</span><span class="sxs-lookup"><span data-stu-id="b58c7-518">trademark</span></span>             | <span data-ttu-id="b58c7-519">Paramount Pictures Corp. http://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="b58c7-519">Paramount Pictures Corp. http://www.paramount.com</span></span> |

<span data-ttu-id="b58c7-520">La aplicación de ejemplo llama a `GetSection` con la clave `starship`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-520">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="b58c7-521">Los pares clave-valor `starship` están aislados.</span><span class="sxs-lookup"><span data-stu-id="b58c7-521">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="b58c7-522">El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-522">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="b58c7-523">Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-523">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

<span data-ttu-id="b58c7-524">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-524">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="b58c7-525">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="b58c7-525">Bind to an object graph</span></span>

<span data-ttu-id="b58c7-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO.</span><span class="sxs-lookup"><span data-stu-id="b58c7-526"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="b58c7-527">`Bind` está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-527">`Bind` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-528">El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="b58c7-528">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="b58c7-529">La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-529">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="b58c7-530">Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-530">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="b58c7-531">La instancia enlazada se asigna a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-531">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="b58c7-532">[ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="b58c7-532">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="b58c7-533">`Get<T>` es más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-533">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="b58c7-534">El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:</span><span class="sxs-lookup"><span data-stu-id="b58c7-534">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

<span data-ttu-id="b58c7-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-535"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*> is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b58c7-536">`Get<T>` está disponible en ASP.NET Core 1.1 o versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="b58c7-536">`Get<T>` is available in ASP.NET Core 1.1 or later.</span></span> <span data-ttu-id="b58c7-537">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-537">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="b58c7-538">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="b58c7-538">Bind an array to a class</span></span>

<span data-ttu-id="b58c7-539">*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*</span><span class="sxs-lookup"><span data-stu-id="b58c7-539">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="b58c7-540"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-540">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="b58c7-541">Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.</span><span class="sxs-lookup"><span data-stu-id="b58c7-541">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span> <span data-ttu-id="b58c7-542">"Bind" está en el paquete [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-542">\`Bind\`\` is in the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

> [!NOTE]
> <span data-ttu-id="b58c7-543">El enlace se proporciona por convención.</span><span class="sxs-lookup"><span data-stu-id="b58c7-543">Binding is provided by convention.</span></span> <span data-ttu-id="b58c7-544">Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.</span><span class="sxs-lookup"><span data-stu-id="b58c7-544">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="b58c7-545">**Procesamiento de matriz en memoria**</span><span class="sxs-lookup"><span data-stu-id="b58c7-545">**In-memory array processing**</span></span>

<span data-ttu-id="b58c7-546">Considere los valores y las claves de configuración que se muestran en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="b58c7-546">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="b58c7-547">Key</span><span class="sxs-lookup"><span data-stu-id="b58c7-547">Key</span></span>             | <span data-ttu-id="b58c7-548">Valor</span><span class="sxs-lookup"><span data-stu-id="b58c7-548">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b58c7-549">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="b58c7-549">array:entries:0</span></span> | <span data-ttu-id="b58c7-550">value0</span><span class="sxs-lookup"><span data-stu-id="b58c7-550">value0</span></span> |
| <span data-ttu-id="b58c7-551">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="b58c7-551">array:entries:1</span></span> | <span data-ttu-id="b58c7-552">value1</span><span class="sxs-lookup"><span data-stu-id="b58c7-552">value1</span></span> |
| <span data-ttu-id="b58c7-553">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="b58c7-553">array:entries:2</span></span> | <span data-ttu-id="b58c7-554">value2</span><span class="sxs-lookup"><span data-stu-id="b58c7-554">value2</span></span> |
| <span data-ttu-id="b58c7-555">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="b58c7-555">array:entries:4</span></span> | <span data-ttu-id="b58c7-556">value4</span><span class="sxs-lookup"><span data-stu-id="b58c7-556">value4</span></span> |
| <span data-ttu-id="b58c7-557">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="b58c7-557">array:entries:5</span></span> | <span data-ttu-id="b58c7-558">value5</span><span class="sxs-lookup"><span data-stu-id="b58c7-558">value5</span></span> |

<span data-ttu-id="b58c7-559">Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:</span><span class="sxs-lookup"><span data-stu-id="b58c7-559">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=3-10,22)]

<span data-ttu-id="b58c7-560">La matriz omite un valor de índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="b58c7-560">The array skips a value for index &num;3.</span></span> <span data-ttu-id="b58c7-561">El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.</span><span class="sxs-lookup"><span data-stu-id="b58c7-561">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="b58c7-562">En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:</span><span class="sxs-lookup"><span data-stu-id="b58c7-562">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="b58c7-563">Los datos de configuración están enlazados al objeto:</span><span class="sxs-lookup"><span data-stu-id="b58c7-563">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="b58c7-564">`GetSection` está en el paquete [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/), que está en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b58c7-564">`GetSection` is in the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b58c7-565">También se puede usar la sintaxis de [ConfigurationBinder.Get&lt;T&gt; ](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:</span><span class="sxs-lookup"><span data-stu-id="b58c7-565">[ConfigurationBinder.Get&lt;T&gt;](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="b58c7-566">El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-566">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="b58c7-567">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b58c7-567">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b58c7-568">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b58c7-568">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b58c7-569">0</span><span class="sxs-lookup"><span data-stu-id="b58c7-569">0</span></span>                            | <span data-ttu-id="b58c7-570">value0</span><span class="sxs-lookup"><span data-stu-id="b58c7-570">value0</span></span>                       |
| <span data-ttu-id="b58c7-571">1</span><span class="sxs-lookup"><span data-stu-id="b58c7-571">1</span></span>                            | <span data-ttu-id="b58c7-572">value1</span><span class="sxs-lookup"><span data-stu-id="b58c7-572">value1</span></span>                       |
| <span data-ttu-id="b58c7-573">2</span><span class="sxs-lookup"><span data-stu-id="b58c7-573">2</span></span>                            | <span data-ttu-id="b58c7-574">value2</span><span class="sxs-lookup"><span data-stu-id="b58c7-574">value2</span></span>                       |
| <span data-ttu-id="b58c7-575">3</span><span class="sxs-lookup"><span data-stu-id="b58c7-575">3</span></span>                            | <span data-ttu-id="b58c7-576">value4</span><span class="sxs-lookup"><span data-stu-id="b58c7-576">value4</span></span>                       |
| <span data-ttu-id="b58c7-577">4</span><span class="sxs-lookup"><span data-stu-id="b58c7-577">4</span></span>                            | <span data-ttu-id="b58c7-578">value5</span><span class="sxs-lookup"><span data-stu-id="b58c7-578">value5</span></span>                       |

<span data-ttu-id="b58c7-579">El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-579">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="b58c7-580">Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto.</span><span class="sxs-lookup"><span data-stu-id="b58c7-580">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="b58c7-581">No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.</span><span class="sxs-lookup"><span data-stu-id="b58c7-581">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="b58c7-582">El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExample` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-582">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="b58c7-583">Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExample.Entries` coincide con la matriz de configuración completa:</span><span class="sxs-lookup"><span data-stu-id="b58c7-583">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="b58c7-584">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-584">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="b58c7-585">En <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="b58c7-585">In <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

```csharp
config.AddJsonFile("missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="b58c7-586">El par clave-valor que se muestra en la tabla se carga en la configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-586">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="b58c7-587">Key</span><span class="sxs-lookup"><span data-stu-id="b58c7-587">Key</span></span>             | <span data-ttu-id="b58c7-588">Valor</span><span class="sxs-lookup"><span data-stu-id="b58c7-588">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="b58c7-589">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="b58c7-589">array:entries:3</span></span> | <span data-ttu-id="b58c7-590">value3</span><span class="sxs-lookup"><span data-stu-id="b58c7-590">value3</span></span> |

<span data-ttu-id="b58c7-591">Si la instancia de clase `ArrayExample` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExample.Entries` incluye el valor.</span><span class="sxs-lookup"><span data-stu-id="b58c7-591">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="b58c7-592">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b58c7-592">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="b58c7-593">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="b58c7-593">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="b58c7-594">0</span><span class="sxs-lookup"><span data-stu-id="b58c7-594">0</span></span>                            | <span data-ttu-id="b58c7-595">value0</span><span class="sxs-lookup"><span data-stu-id="b58c7-595">value0</span></span>                       |
| <span data-ttu-id="b58c7-596">1</span><span class="sxs-lookup"><span data-stu-id="b58c7-596">1</span></span>                            | <span data-ttu-id="b58c7-597">value1</span><span class="sxs-lookup"><span data-stu-id="b58c7-597">value1</span></span>                       |
| <span data-ttu-id="b58c7-598">2</span><span class="sxs-lookup"><span data-stu-id="b58c7-598">2</span></span>                            | <span data-ttu-id="b58c7-599">value2</span><span class="sxs-lookup"><span data-stu-id="b58c7-599">value2</span></span>                       |
| <span data-ttu-id="b58c7-600">3</span><span class="sxs-lookup"><span data-stu-id="b58c7-600">3</span></span>                            | <span data-ttu-id="b58c7-601">value3</span><span class="sxs-lookup"><span data-stu-id="b58c7-601">value3</span></span>                       |
| <span data-ttu-id="b58c7-602">4</span><span class="sxs-lookup"><span data-stu-id="b58c7-602">4</span></span>                            | <span data-ttu-id="b58c7-603">value4</span><span class="sxs-lookup"><span data-stu-id="b58c7-603">value4</span></span>                       |
| <span data-ttu-id="b58c7-604">5</span><span class="sxs-lookup"><span data-stu-id="b58c7-604">5</span></span>                            | <span data-ttu-id="b58c7-605">value5</span><span class="sxs-lookup"><span data-stu-id="b58c7-605">value5</span></span>                       |

<span data-ttu-id="b58c7-606">**Procesamiento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="b58c7-606">**JSON array processing**</span></span>

<span data-ttu-id="b58c7-607">Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero.</span><span class="sxs-lookup"><span data-stu-id="b58c7-607">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="b58c7-608">En el siguiente archivo de configuración, `subsection` es una matriz:</span><span class="sxs-lookup"><span data-stu-id="b58c7-608">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="b58c7-609">El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="b58c7-609">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="b58c7-610">Key</span><span class="sxs-lookup"><span data-stu-id="b58c7-610">Key</span></span>                     | <span data-ttu-id="b58c7-611">Valor</span><span class="sxs-lookup"><span data-stu-id="b58c7-611">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="b58c7-612">json_array:key</span><span class="sxs-lookup"><span data-stu-id="b58c7-612">json_array:key</span></span>          | <span data-ttu-id="b58c7-613">valueA</span><span class="sxs-lookup"><span data-stu-id="b58c7-613">valueA</span></span> |
| <span data-ttu-id="b58c7-614">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="b58c7-614">json_array:subsection:0</span></span> | <span data-ttu-id="b58c7-615">valueB</span><span class="sxs-lookup"><span data-stu-id="b58c7-615">valueB</span></span> |
| <span data-ttu-id="b58c7-616">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="b58c7-616">json_array:subsection:1</span></span> | <span data-ttu-id="b58c7-617">valueC</span><span class="sxs-lookup"><span data-stu-id="b58c7-617">valueC</span></span> |
| <span data-ttu-id="b58c7-618">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="b58c7-618">json_array:subsection:2</span></span> | <span data-ttu-id="b58c7-619">valueD</span><span class="sxs-lookup"><span data-stu-id="b58c7-619">valueD</span></span> |

<span data-ttu-id="b58c7-620">En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="b58c7-620">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="b58c7-621">Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-621">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="b58c7-622">Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-622">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="b58c7-623">Índice de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="b58c7-623">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="b58c7-624">Valor de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="b58c7-624">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="b58c7-625">0</span><span class="sxs-lookup"><span data-stu-id="b58c7-625">0</span></span>                                   | <span data-ttu-id="b58c7-626">valueB</span><span class="sxs-lookup"><span data-stu-id="b58c7-626">valueB</span></span>                              |
| <span data-ttu-id="b58c7-627">1</span><span class="sxs-lookup"><span data-stu-id="b58c7-627">1</span></span>                                   | <span data-ttu-id="b58c7-628">valueC</span><span class="sxs-lookup"><span data-stu-id="b58c7-628">valueC</span></span>                              |
| <span data-ttu-id="b58c7-629">2</span><span class="sxs-lookup"><span data-stu-id="b58c7-629">2</span></span>                                   | <span data-ttu-id="b58c7-630">valueD</span><span class="sxs-lookup"><span data-stu-id="b58c7-630">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="b58c7-631">Proveedores de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="b58c7-631">Custom configuration provider</span></span>

<span data-ttu-id="b58c7-632">La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="b58c7-632">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="b58c7-633">El proveedor tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="b58c7-633">The provider has the following characteristics:</span></span>

* <span data-ttu-id="b58c7-634">La base de datos en memoria de EF se usa para fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-634">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="b58c7-635">Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="b58c7-635">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="b58c7-636">El proveedor lee una tabla de base de datos en la configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="b58c7-636">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="b58c7-637">El proveedor no consulta la base de datos por clave.</span><span class="sxs-lookup"><span data-stu-id="b58c7-637">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="b58c7-638">La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b58c7-638">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="b58c7-639">Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="b58c7-639">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="b58c7-640">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-640">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="b58c7-641">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="b58c7-641">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="b58c7-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-642">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="b58c7-643">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-643">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="b58c7-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-644">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="b58c7-645">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-645">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="b58c7-646">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="b58c7-646">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="b58c7-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-647">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="b58c7-648">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-648">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="b58c7-649">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-649">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="b58c7-650">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="b58c7-650">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=26)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="b58c7-651">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="b58c7-651">Access configuration during startup</span></span>

<span data-ttu-id="b58c7-652">Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b58c7-652">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b58c7-653">Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:</span><span class="sxs-lookup"><span data-stu-id="b58c7-653">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="b58c7-654">Para ver un ejemplo de cómo acceder a la configuración mediante métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods)</span><span class="sxs-lookup"><span data-stu-id="b58c7-654">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="b58c7-655">Acceso a la configuración en una página de Razor Pages o en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="b58c7-655">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="b58c7-656">Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.</span><span class="sxs-lookup"><span data-stu-id="b58c7-656">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="b58c7-657">En una página de las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="b58c7-657">In a Razor Pages page:</span></span>

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

<span data-ttu-id="b58c7-658">En una vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="b58c7-658">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="b58c7-659">Agregar configuración a partir de un ensamblado externo</span><span class="sxs-lookup"><span data-stu-id="b58c7-659">Add configuration from an external assembly</span></span>

<span data-ttu-id="b58c7-660">Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="b58c7-660">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="b58c7-661">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="b58c7-661">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b58c7-662">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="b58c7-662">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
* <span data-ttu-id="b58c7-663">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/) (Profundización en la configuración de Microsoft)</span><span class="sxs-lookup"><span data-stu-id="b58c7-663">[Deep Dive into Microsoft Configuration](https://www.paraesthesia.com/archive/2018/06/20/microsoft-extensions-configuration-deep-dive/)</span></span>
