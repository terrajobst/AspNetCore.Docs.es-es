---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/10/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 3dcabae3f76d81e641057c346dbb9097c2da42c7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78644105"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="87977-103">Configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="87977-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="87977-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="87977-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="87977-105">La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*.</span><span class="sxs-lookup"><span data-stu-id="87977-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="87977-106">Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="87977-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="87977-107">Azure Key Vault</span></span>
* <span data-ttu-id="87977-108">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="87977-108">Azure App Configuration</span></span>
* <span data-ttu-id="87977-109">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-109">Command-line arguments</span></span>
* <span data-ttu-id="87977-110">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="87977-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="87977-111">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="87977-111">Directory files</span></span>
* <span data-ttu-id="87977-112">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-112">Environment variables</span></span>
* <span data-ttu-id="87977-113">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="87977-113">In-memory .NET objects</span></span>
* <span data-ttu-id="87977-114">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="87977-114">Settings files</span></span>

<span data-ttu-id="87977-115">Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen implícitamente en el marco.</span><span class="sxs-lookup"><span data-stu-id="87977-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

<span data-ttu-id="87977-116">Los ejemplos de código que siguen y en la aplicación de ejemplo utilizan el espacio de nombres <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="87977-116">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="87977-117">El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="87977-117">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="87977-118">Las opciones usan clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="87977-118">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="87977-119">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="87977-119">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="87977-120">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="87977-120">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="87977-121">Configuración de host y de aplicación</span><span class="sxs-lookup"><span data-stu-id="87977-121">Host versus app configuration</span></span>

<span data-ttu-id="87977-122">Antes de configurar e iniciar la aplicación, se configura e inicia un *host*.</span><span class="sxs-lookup"><span data-stu-id="87977-122">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="87977-123">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-123">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="87977-124">Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="87977-124">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="87977-125">Los pares clave-valor de configuración del host también se incluyen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-125">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="87977-126">Para más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="87977-126">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="87977-127">Otra configuración</span><span class="sxs-lookup"><span data-stu-id="87977-127">Other configuration</span></span>

<span data-ttu-id="87977-128">Este tema solo concierne a la *configuración de aplicaciones*.</span><span class="sxs-lookup"><span data-stu-id="87977-128">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="87977-129">Otros aspectos de la ejecución y el hospedaje de aplicaciones ASP.NET Core se configuran mediante archivos de configuración que no se describen en este tema:</span><span class="sxs-lookup"><span data-stu-id="87977-129">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="87977-130">*launch.json*/*launchSettings.json* son archivos de configuración de herramientas para el entorno de desarrollo, que se describe a continuación:</span><span class="sxs-lookup"><span data-stu-id="87977-130">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="87977-131">En <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="87977-131">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="87977-132">En el conjunto de documentación en el que se usan los archivos para configurar aplicaciones ASP.NET Core para escenarios de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-132">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="87977-133">*web.config* es un archivo de configuración del servidor, que se describe en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-133">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="87977-134">Para más información sobre cómo migrar la configuración de aplicaciones de versiones anteriores de ASP.NET, consulte <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="87977-134">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="87977-135">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="87977-135">Default configuration</span></span>

<span data-ttu-id="87977-136">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="87977-136">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="87977-137">`CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-137">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="87977-138">El contenido siguiente es válido para las aplicaciones que usen el [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="87977-138">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="87977-139">Para obtener más información sobre la configuración predeterminada al usar el [host de web](xref:fundamentals/host/web-host), vea la [versión de este tema para ASP.NET Core 2.2](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="87977-139">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="87977-140">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="87977-140">Host configuration is provided from:</span></span>
  * <span data-ttu-id="87977-141">Variables de entorno prefijadas con `DOTNET_` (por ejemplo, `DOTNET_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-141">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="87977-142">El prefijo (`DOTNET_`) se quita cuando se cargan los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-142">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="87977-143">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-143">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="87977-144">La configuración predeterminada del host de web se ha establecido (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="87977-144">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="87977-145">Kestrel se usa como el servidor web y se ha configurado mediante los proveedores de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-145">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="87977-146">Agregar el middleware de filtrado de host</span><span class="sxs-lookup"><span data-stu-id="87977-146">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="87977-147">Agregue el middleware de encabezados reenviados si la variable de entorno `ASPNETCORE_FORWARDEDHEADERS_ENABLED` se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="87977-147">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="87977-148">Habilite la integración con IIS.</span><span class="sxs-lookup"><span data-stu-id="87977-148">Enable IIS integration.</span></span>
* <span data-ttu-id="87977-149">La configuración de la aplicación la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="87977-149">App configuration is provided from:</span></span>
  * <span data-ttu-id="87977-150">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-150">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="87977-151">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-151">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="87977-152">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="87977-152">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="87977-153">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-153">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="87977-154">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-154">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="87977-155">Seguridad</span><span class="sxs-lookup"><span data-stu-id="87977-155">Security</span></span>

<span data-ttu-id="87977-156">Adopte los procedimientos siguientes para proteger los datos de configuración confidenciales:</span><span class="sxs-lookup"><span data-stu-id="87977-156">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="87977-157">Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="87977-157">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="87977-158">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="87977-158">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="87977-159">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="87977-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="87977-160">Para obtener más información, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-160">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="87977-161"><xref:security/app-secrets> &ndash; Se incluyen recomendaciones sobre el uso de variables de entorno para almacenar información confidencial.</span><span class="sxs-lookup"><span data-stu-id="87977-161"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="87977-162">El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local.</span><span class="sxs-lookup"><span data-stu-id="87977-162">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="87977-163">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="87977-163">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="87977-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) almacena de forma segura secretos de aplicación para aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87977-164">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="87977-165">Para obtener más información, vea <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="87977-165">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="87977-166">Datos de configuración jerárquica</span><span class="sxs-lookup"><span data-stu-id="87977-166">Hierarchical configuration data</span></span>

<span data-ttu-id="87977-167">La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-167">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="87977-168">En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:</span><span class="sxs-lookup"><span data-stu-id="87977-168">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="87977-169">Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-169">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="87977-170">Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:</span><span class="sxs-lookup"><span data-stu-id="87977-170">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="87977-171">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-171">section0:key0</span></span>
* <span data-ttu-id="87977-172">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-172">section0:key1</span></span>
* <span data-ttu-id="87977-173">section1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-173">section1:key0</span></span>
* <span data-ttu-id="87977-174">section1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-174">section1:key1</span></span>

<span data-ttu-id="87977-175">Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-175"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="87977-176">Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="87977-176">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="87977-177">Convenciones</span><span class="sxs-lookup"><span data-stu-id="87977-177">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="87977-178">Orígenes y proveedores</span><span class="sxs-lookup"><span data-stu-id="87977-178">Sources and providers</span></span>

<span data-ttu-id="87977-179">Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-179">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="87977-180">Los proveedores de configuración que implementan la detección de cambios tienen la capacidad de volver a cargar la configuración cuando se cambia una configuración subyacente.</span><span class="sxs-lookup"><span data-stu-id="87977-180">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="87977-181">Por ejemplo, el proveedor de configuración de archivos (descrito más adelante en este tema) y el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) implementan la detección de cambios.</span><span class="sxs-lookup"><span data-stu-id="87977-181">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="87977-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-182"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="87977-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages o <xref:Microsoft.AspNetCore.Mvc.Controller> de MVC para obtener la configuración de la clase.</span><span class="sxs-lookup"><span data-stu-id="87977-183"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="87977-184">En los ejemplos siguientes, se usa el campo `_config` para tener acceso a los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-184">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="87977-185">Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.</span><span class="sxs-lookup"><span data-stu-id="87977-185">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="87977-186">Teclas</span><span class="sxs-lookup"><span data-stu-id="87977-186">Keys</span></span>

<span data-ttu-id="87977-187">Las claves de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-187">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="87977-188">Las claves no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="87977-188">Keys are case-insensitive.</span></span> <span data-ttu-id="87977-189">Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.</span><span class="sxs-lookup"><span data-stu-id="87977-189">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="87977-190">Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.</span><span class="sxs-lookup"><span data-stu-id="87977-190">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="87977-191">Claves jerárquicas</span><span class="sxs-lookup"><span data-stu-id="87977-191">Hierarchical keys</span></span>
  * <span data-ttu-id="87977-192">Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="87977-192">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="87977-193">En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="87977-193">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="87977-194">Todas las plataformas admiten un carácter de subrayado doble (`__`), que se convierte automáticamente en un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="87977-194">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="87977-195">En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="87977-195">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="87977-196">Escriba código para reemplazar los guiones por dos puntos cuando los secretos se carguen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-196">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="87977-197"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-197">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="87977-198">El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="87977-198">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="87977-199">Valores</span><span class="sxs-lookup"><span data-stu-id="87977-199">Values</span></span>

<span data-ttu-id="87977-200">Los valores de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-200">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="87977-201">Los valores son cadenas.</span><span class="sxs-lookup"><span data-stu-id="87977-201">Values are strings.</span></span>
* <span data-ttu-id="87977-202">Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.</span><span class="sxs-lookup"><span data-stu-id="87977-202">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="87977-203">Proveedores</span><span class="sxs-lookup"><span data-stu-id="87977-203">Providers</span></span>

<span data-ttu-id="87977-204">La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87977-204">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="87977-205">Proveedor</span><span class="sxs-lookup"><span data-stu-id="87977-205">Provider</span></span> | <span data-ttu-id="87977-206">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="87977-206">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="87977-207">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="87977-207">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="87977-208">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="87977-208">Azure Key Vault</span></span> |
| <span data-ttu-id="87977-209">[Proveedor de Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentación de Azure)</span><span class="sxs-lookup"><span data-stu-id="87977-209">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="87977-210">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="87977-210">Azure App Configuration</span></span> |
| [<span data-ttu-id="87977-211">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-211">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="87977-212">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-212">Command-line parameters</span></span> |
| [<span data-ttu-id="87977-213">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="87977-213">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="87977-214">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="87977-214">Custom source</span></span> |
| [<span data-ttu-id="87977-215">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-215">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="87977-216">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-216">Environment variables</span></span> |
| [<span data-ttu-id="87977-217">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="87977-217">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="87977-218">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="87977-218">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="87977-219">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="87977-219">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="87977-220">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="87977-220">Directory files</span></span> |
| [<span data-ttu-id="87977-221">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="87977-221">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="87977-222">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="87977-222">In-memory collections</span></span> |
| <span data-ttu-id="87977-223">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="87977-223">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="87977-224">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="87977-224">File in the user profile directory</span></span> |

<span data-ttu-id="87977-225">Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="87977-225">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="87977-226">En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en el que el código los organiza.</span><span class="sxs-lookup"><span data-stu-id="87977-226">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="87977-227">Ordene los proveedores de configuración en el código para cumplir con las prioridades relacionadas con los orígenes de configuración subyacentes que la aplicación necesita.</span><span class="sxs-lookup"><span data-stu-id="87977-227">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="87977-228">Esta es una secuencia típica de proveedores de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-228">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="87977-229">Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)</span><span class="sxs-lookup"><span data-stu-id="87977-229">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="87977-230">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="87977-230">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="87977-231">[Secretos de usuario (administrador de secretos)](xref:security/app-secrets) (solo para entornos de desarrollo)</span><span class="sxs-lookup"><span data-stu-id="87977-231">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="87977-232">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-232">Environment variables</span></span>
1. <span data-ttu-id="87977-233">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-233">Command-line arguments</span></span>

<span data-ttu-id="87977-234">Una práctica común es colocar el proveedor de configuración de línea de comandos al final en una serie de proveedores para permitir que los argumentos de la línea de comandos reemplacen la configuración establecida por el resto de los proveedores.</span><span class="sxs-lookup"><span data-stu-id="87977-234">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="87977-235">La secuencia de proveedores anterior se usa cuando se inicializa un nuevo generador de host con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-235">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-236">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-236">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="87977-237">Configurar el generador de host con ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="87977-237">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="87977-238">Para configurar el generador de host, realice una llamada a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> y proporcione la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-238">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="87977-239">`ConfigureHostConfiguration` se usa para inicializar <xref:Microsoft.Extensions.Hosting.IHostEnvironment> para su uso posterior en el proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="87977-239">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="87977-240">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="87977-240">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureHostConfiguration(config =>
        {
            var dict = new Dictionary<string, string>
            {
                {"MemoryCollectionKey1", "value1"},
                {"MemoryCollectionKey2", "value2"}
            };

            config.AddInMemoryCollection(dict);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

## <a name="configureappconfiguration"></a><span data-ttu-id="87977-241">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="87977-241">ConfigureAppConfiguration</span></span>

<span data-ttu-id="87977-242">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que `CreateDefaultBuilder` agrega automáticamente:</span><span class="sxs-lookup"><span data-stu-id="87977-242">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="87977-243">Reemplazar la configuración anterior con argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-243">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="87977-244">Para proporcionar configuración de aplicaciones que se pueda reemplazar por argumentos de la línea de comandos, llame a `AddCommandLine` en último lugar:</span><span class="sxs-lookup"><span data-stu-id="87977-244">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="87977-245">Quitar proveedores agregados por CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="87977-245">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="87977-246">Para quitar los proveedores agregados por `CreateDefaultBuilder`, llame primero a [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) en [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="87977-246">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="87977-247">Usar la configuración durante el inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="87977-247">Consume configuration during app startup</span></span>

<span data-ttu-id="87977-248">La configuración proporcionada a la aplicación en `ConfigureAppConfiguration` está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="87977-248">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="87977-249">Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="87977-249">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="87977-250">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-250">Command-line Configuration Provider</span></span>

<span data-ttu-id="87977-251"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-251">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-252">Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-252">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-253">`AddCommandLine` se llama automáticamente al llamar a `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="87977-253">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="87977-254">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-254">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-255">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="87977-255">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="87977-256">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-256">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="87977-257">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-257">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="87977-258">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-258">Environment variables.</span></span>

<span data-ttu-id="87977-259">`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final.</span><span class="sxs-lookup"><span data-stu-id="87977-259">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="87977-260">Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="87977-260">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="87977-261">`CreateDefaultBuilder` actúa cuando se construye el host.</span><span class="sxs-lookup"><span data-stu-id="87977-261">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="87977-262">Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.</span><span class="sxs-lookup"><span data-stu-id="87977-262">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="87977-263">Para las aplicaciones basadas en plantillas de ASP.NET Core, `CreateDefaultBuilder` ya realiza una llamada a `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="87977-263">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-264">Para agregar proveedores de configuración adicionales y mantener la capacidad de reemplazar la configuración de sus proveedores con argumentos de la línea de comandos, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddCommandLine` en último lugar.</span><span class="sxs-lookup"><span data-stu-id="87977-264">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="87977-265">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-265">**Example**</span></span>

<span data-ttu-id="87977-266">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="87977-266">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="87977-267">Abra un símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="87977-267">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="87977-268">Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="87977-268">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="87977-269">Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="87977-269">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="87977-270">Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="87977-270">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="87977-271">Argumentos</span><span class="sxs-lookup"><span data-stu-id="87977-271">Arguments</span></span>

<span data-ttu-id="87977-272">El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio.</span><span class="sxs-lookup"><span data-stu-id="87977-272">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="87977-273">El valor no es obligatorio si se usa un signo igual (por ejemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="87977-273">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="87977-274">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="87977-274">Key prefix</span></span>               | <span data-ttu-id="87977-275">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="87977-275">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="87977-276">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="87977-276">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="87977-277">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="87977-277">Two dashes (`--`)</span></span>        | <span data-ttu-id="87977-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="87977-278">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="87977-279">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="87977-279">Forward slash (`/`)</span></span>      | <span data-ttu-id="87977-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="87977-280">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="87977-281">Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.</span><span class="sxs-lookup"><span data-stu-id="87977-281">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="87977-282">Comandos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="87977-282">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="87977-283">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="87977-283">Switch mappings</span></span>

<span data-ttu-id="87977-284">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="87977-284">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="87977-285">Al crear manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, proporcione un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="87977-285">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="87977-286">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-286">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="87977-287">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-287">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="87977-288">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="87977-288">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="87977-289">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="87977-289">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="87977-290">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="87977-290">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="87977-291">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="87977-291">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="87977-292">Cree un diccionario de asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="87977-292">Create a switch mappings dictionary.</span></span> <span data-ttu-id="87977-293">En el ejemplo siguiente, se crean dos asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="87977-293">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="87977-294">Al compilar el host, llame a `AddCommandLine` con el diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="87977-294">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="87977-295">En el caso de las aplicaciones que usen las asignaciones de modificador, la llamada a `CreateDefaultBuilder` no tiene que pasar argumentos.</span><span class="sxs-lookup"><span data-stu-id="87977-295">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="87977-296">En la llamada de `AddCommandLine` del método `CreateDefaultBuilder`, no se incluyen modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-296">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-297">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="87977-297">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="87977-298">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="87977-298">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="87977-299">Key</span><span class="sxs-lookup"><span data-stu-id="87977-299">Key</span></span>       | <span data-ttu-id="87977-300">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-300">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="87977-301">Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:</span><span class="sxs-lookup"><span data-stu-id="87977-301">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="87977-302">Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="87977-302">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="87977-303">Key</span><span class="sxs-lookup"><span data-stu-id="87977-303">Key</span></span>               | <span data-ttu-id="87977-304">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-304">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="87977-305">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-305">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="87977-306"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-306">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-307">Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-307">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="87977-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-308">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="87977-309">Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="87977-309">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="87977-310">`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `DOTNET_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host genérico](xref:fundamentals/host/generic-host) y al llamar a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-310">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="87977-311">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-311">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-312">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="87977-312">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="87977-313">Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.</span><span class="sxs-lookup"><span data-stu-id="87977-313">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="87977-314">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-314">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="87977-315">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-315">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="87977-316">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-316">Command-line arguments.</span></span>

<span data-ttu-id="87977-317">El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="87977-317">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="87977-318">Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="87977-318">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="87977-319">Para proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddEnvironmentVariables` con el prefijo:</span><span class="sxs-lookup"><span data-stu-id="87977-319">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="87977-320">Llame a `AddEnvironmentVariables` en último lugar para permitir que las variables de entorno con el prefijo especificado invaliden los valores de otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="87977-320">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="87977-321">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-321">**Example**</span></span>

<span data-ttu-id="87977-322">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="87977-322">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="87977-323">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="87977-323">Run the sample app.</span></span> <span data-ttu-id="87977-324">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="87977-324">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="87977-325">Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="87977-325">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="87977-326">El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.</span><span class="sxs-lookup"><span data-stu-id="87977-326">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="87977-327">Para mantener un número adecuado de variables de entorno en la lista representada por la aplicación, la aplicación filtrará las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-327">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="87977-328">Vea el archivo *Pages/Index.cshtml.cs* de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="87977-328">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="87977-329">Para exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-329">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="87977-330">Prefijos</span><span class="sxs-lookup"><span data-stu-id="87977-330">Prefixes</span></span>

<span data-ttu-id="87977-331">Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="87977-331">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="87977-332">Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-332">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="87977-333">El prefijo se quita cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-333">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="87977-334">Al crear el generador de host, las variables de entorno proporcionan la configuración del host.</span><span class="sxs-lookup"><span data-stu-id="87977-334">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="87977-335">Para obtener más información sobre el prefijo usado para estas variables de entorno, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-335">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-336">**Prefijos de cadena de conexión**</span><span class="sxs-lookup"><span data-stu-id="87977-336">**Connection string prefixes**</span></span>

<span data-ttu-id="87977-337">La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-337">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="87977-338">Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="87977-338">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="87977-339">Prefijo de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="87977-339">Connection string prefix</span></span> | <span data-ttu-id="87977-340">Proveedor</span><span class="sxs-lookup"><span data-stu-id="87977-340">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="87977-341">Proveedor personalizado</span><span class="sxs-lookup"><span data-stu-id="87977-341">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="87977-342">MySQL</span><span class="sxs-lookup"><span data-stu-id="87977-342">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="87977-343">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="87977-343">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="87977-344">SQL Server</span><span class="sxs-lookup"><span data-stu-id="87977-344">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="87977-345">Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:</span><span class="sxs-lookup"><span data-stu-id="87977-345">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="87977-346">La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="87977-346">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="87977-347">Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).</span><span class="sxs-lookup"><span data-stu-id="87977-347">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="87977-348">Clave de variable de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-348">Environment variable key</span></span> | <span data-ttu-id="87977-349">Clave de configuración convertida</span><span class="sxs-lookup"><span data-stu-id="87977-349">Converted configuration key</span></span> | <span data-ttu-id="87977-350">Entrada de configuración del proveedor</span><span class="sxs-lookup"><span data-stu-id="87977-350">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-351">Entrada de configuración no creada.</span><span class="sxs-lookup"><span data-stu-id="87977-351">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-352">Clave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="87977-352">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="87977-353">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="87977-353">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-354">Clave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="87977-354">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="87977-355">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="87977-355">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-356">Clave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="87977-356">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="87977-357">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="87977-357">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="87977-358">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-358">**Example**</span></span>

<span data-ttu-id="87977-359">Se crea una variable de entorno de una cadena de conexión personalizada en el servidor:</span><span class="sxs-lookup"><span data-stu-id="87977-359">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="87977-360">Nombre: `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="87977-360">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="87977-361">Valor: `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="87977-361">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="87977-362">Si `IConfiguration` se inserta y se asigna a un campo denominado `_config`, se lee el valor siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-362">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="87977-363">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="87977-363">File Configuration Provider</span></span>

<span data-ttu-id="87977-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="87977-364"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="87977-365">Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:</span><span class="sxs-lookup"><span data-stu-id="87977-365">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="87977-366">Proveedor de configuración INI</span><span class="sxs-lookup"><span data-stu-id="87977-366">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="87977-367">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="87977-367">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="87977-368">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="87977-368">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="87977-369">Proveedor de configuración de INI</span><span class="sxs-lookup"><span data-stu-id="87977-369">INI Configuration Provider</span></span>

<span data-ttu-id="87977-370"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-370">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-371">Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-371">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-372">Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.</span><span class="sxs-lookup"><span data-stu-id="87977-372">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="87977-373">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-373">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-374">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="87977-374">Whether the file is optional.</span></span>
* <span data-ttu-id="87977-375">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="87977-375">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="87977-376"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-376">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="87977-377">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="87977-377">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="87977-378">Un ejemplo genérico de un archivo de configuración INI:</span><span class="sxs-lookup"><span data-stu-id="87977-378">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="87977-379">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-379">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-380">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-380">section0:key0</span></span>
* <span data-ttu-id="87977-381">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-381">section0:key1</span></span>
* <span data-ttu-id="87977-382">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="87977-382">section1:subsection:key</span></span>
* <span data-ttu-id="87977-383">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="87977-383">section2:subsection0:key</span></span>
* <span data-ttu-id="87977-384">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="87977-384">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="87977-385">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="87977-385">JSON Configuration Provider</span></span>

<span data-ttu-id="87977-386"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-386">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="87977-387">Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-387">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-388">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-388">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-389">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="87977-389">Whether the file is optional.</span></span>
* <span data-ttu-id="87977-390">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="87977-390">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="87977-391"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-391">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="87977-392">Se llama automáticamente a `AddJsonFile` dos veces cuando un nuevo generador de host se inicializa con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-392">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-393">Se llama al método para cargar la configuración desde:</span><span class="sxs-lookup"><span data-stu-id="87977-393">The method is called to load configuration from:</span></span>

* <span data-ttu-id="87977-394">*appsettings.json*: este archivo se lee primero.</span><span class="sxs-lookup"><span data-stu-id="87977-394">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="87977-395">La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-395">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="87977-396">*appsettings.{Environment}.json*: la versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="87977-396">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="87977-397">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-397">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-398">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="87977-398">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="87977-399">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-399">Environment variables.</span></span>
* <span data-ttu-id="87977-400">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-400">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="87977-401">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-401">Command-line arguments.</span></span>

<span data-ttu-id="87977-402">El proveedor de configuración de JSON se establece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="87977-402">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="87977-403">Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="87977-403">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="87977-404">Llame a `ConfigureAppConfiguration` al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="87977-404">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="87977-405">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-405">**Example**</span></span>

<span data-ttu-id="87977-406">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="87977-406">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="87977-407">La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="87977-407">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="87977-408">La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-408">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="87977-409">Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:</span><span class="sxs-lookup"><span data-stu-id="87977-409">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="87977-410">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="87977-410">Run the sample app.</span></span> <span data-ttu-id="87977-411">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="87977-411">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="87977-412">La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-412">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="87977-413">El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-413">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="87977-414">Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:</span><span class="sxs-lookup"><span data-stu-id="87977-414">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="87977-415">Abra el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-415">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="87977-416">En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.</span><span class="sxs-lookup"><span data-stu-id="87977-416">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="87977-417">Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-417">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="87977-418">La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-418">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="87977-419">El nivel de registro de la clave `Logging:LogLevel:Default` es `Information`.</span><span class="sxs-lookup"><span data-stu-id="87977-419">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="87977-420">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="87977-420">XML Configuration Provider</span></span>

<span data-ttu-id="87977-421"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-421">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-422">Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-422">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-423">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-423">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-424">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="87977-424">Whether the file is optional.</span></span>
* <span data-ttu-id="87977-425">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="87977-425">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="87977-426"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-426">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="87977-427">El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-427">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="87977-428">No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-428">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="87977-429">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="87977-429">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="87977-430">Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:</span><span class="sxs-lookup"><span data-stu-id="87977-430">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="87977-431">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-431">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-432">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-432">section0:key0</span></span>
* <span data-ttu-id="87977-433">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-433">section0:key1</span></span>
* <span data-ttu-id="87977-434">section1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-434">section1:key0</span></span>
* <span data-ttu-id="87977-435">section1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-435">section1:key1</span></span>

<span data-ttu-id="87977-436">Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:</span><span class="sxs-lookup"><span data-stu-id="87977-436">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="87977-437">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-437">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-438">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="87977-438">section:section0:key:key0</span></span>
* <span data-ttu-id="87977-439">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="87977-439">section:section0:key:key1</span></span>
* <span data-ttu-id="87977-440">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="87977-440">section:section1:key:key0</span></span>
* <span data-ttu-id="87977-441">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="87977-441">section:section1:key:key1</span></span>

<span data-ttu-id="87977-442">Los atributos se pueden usar para suministrar valores:</span><span class="sxs-lookup"><span data-stu-id="87977-442">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="87977-443">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-443">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-444">key:attribute</span><span class="sxs-lookup"><span data-stu-id="87977-444">key:attribute</span></span>
* <span data-ttu-id="87977-445">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="87977-445">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="87977-446">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="87977-446">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="87977-447"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-447">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="87977-448">La clave es el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-448">The key is the file name.</span></span> <span data-ttu-id="87977-449">El valor contiene el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-449">The value contains the file's contents.</span></span> <span data-ttu-id="87977-450">El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.</span><span class="sxs-lookup"><span data-stu-id="87977-450">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="87977-451">Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-451">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="87977-452">La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="87977-452">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="87977-453">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-453">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-454">Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.</span><span class="sxs-lookup"><span data-stu-id="87977-454">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="87977-455">Si el directorio es opcional y la ruta de acceso al directorio.</span><span class="sxs-lookup"><span data-stu-id="87977-455">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="87977-456">El carácter de subrayado doble (`__`) se usa como delimitador de claves de configuración en los nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-456">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="87977-457">Por ejemplo, el nombre de archivo `Logging__LogLevel__System` genera la clave de configuración `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="87977-457">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="87977-458">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="87977-458">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="87977-459">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="87977-459">Memory Configuration Provider</span></span>

<span data-ttu-id="87977-460"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-460">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="87977-461">Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-461">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-462">El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="87977-462">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="87977-463">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-463">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="87977-464">En el ejemplo siguiente, se crea un diccionario de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-464">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="87977-465">El diccionario se usa con una llamada a `AddInMemoryCollection` para proporcionar la configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-465">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="87977-466">GetValue</span><span class="sxs-lookup"><span data-stu-id="87977-466">GetValue</span></span>

<span data-ttu-id="87977-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un único valor de la configuración con una clave especificada y lo convierte al tipo especificado que no es de colección.</span><span class="sxs-lookup"><span data-stu-id="87977-467">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="87977-468">Una sobrecarga acepta un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="87977-468">An overload accepts a default value.</span></span>

<span data-ttu-id="87977-469">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-469">The following example:</span></span>

* <span data-ttu-id="87977-470">Se extrae el valor de cadena de la configuración con la clave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="87977-470">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="87977-471">Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.</span><span class="sxs-lookup"><span data-stu-id="87977-471">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="87977-472">Se escribe el valor como `int`.</span><span class="sxs-lookup"><span data-stu-id="87977-472">Types the value as an `int`.</span></span>
* <span data-ttu-id="87977-473">Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="87977-473">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="87977-474">GetSection, GetChildren y Exists</span><span class="sxs-lookup"><span data-stu-id="87977-474">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="87977-475">Para los ejemplos siguientes, considere el siguiente archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="87977-475">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="87977-476">Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:</span><span class="sxs-lookup"><span data-stu-id="87977-476">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="87977-477">Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-477">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="87977-478">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-478">section0:key0</span></span>
* <span data-ttu-id="87977-479">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-479">section0:key1</span></span>
* <span data-ttu-id="87977-480">section1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-480">section1:key0</span></span>
* <span data-ttu-id="87977-481">section1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-481">section1:key1</span></span>
* <span data-ttu-id="87977-482">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-482">section2:subsection0:key0</span></span>
* <span data-ttu-id="87977-483">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-483">section2:subsection0:key1</span></span>
* <span data-ttu-id="87977-484">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-484">section2:subsection1:key0</span></span>
* <span data-ttu-id="87977-485">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-485">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="87977-486">GetSection</span><span class="sxs-lookup"><span data-stu-id="87977-486">GetSection</span></span>

<span data-ttu-id="87977-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.</span><span class="sxs-lookup"><span data-stu-id="87977-487">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="87977-488">Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:</span><span class="sxs-lookup"><span data-stu-id="87977-488">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="87977-489">`configSection` no tiene ningún valor, solo una clave y una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="87977-489">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="87977-490">De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:</span><span class="sxs-lookup"><span data-stu-id="87977-490">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="87977-491">`GetSection` nunca devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="87977-491">`GetSection` never returns `null`.</span></span> <span data-ttu-id="87977-492">Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.</span><span class="sxs-lookup"><span data-stu-id="87977-492">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="87977-493">Cuando `GetSection` devuelve una sección coincidente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> no se rellena.</span><span class="sxs-lookup"><span data-stu-id="87977-493">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="87977-494">Se devuelven <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> y <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> si la sección existe.</span><span class="sxs-lookup"><span data-stu-id="87977-494">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="87977-495">GetChildren</span><span class="sxs-lookup"><span data-stu-id="87977-495">GetChildren</span></span>

<span data-ttu-id="87977-496">Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:</span><span class="sxs-lookup"><span data-stu-id="87977-496">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="87977-497">Existe</span><span class="sxs-lookup"><span data-stu-id="87977-497">Exists</span></span>

<span data-ttu-id="87977-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-498">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="87977-499">Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-499">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="87977-500">Enlace a una clase</span><span class="sxs-lookup"><span data-stu-id="87977-500">Bind to a class</span></span>

<span data-ttu-id="87977-501">La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="87977-501">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="87977-502">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="87977-502">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="87977-503">Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="87977-503">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="87977-504">El enlazador enlaza los valores con todas las propiedades de lectura o escritura públicas del tipo proporcionado.</span><span class="sxs-lookup"><span data-stu-id="87977-504">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="87977-505">Los campos **no** se enlazan.</span><span class="sxs-lookup"><span data-stu-id="87977-505">Fields are **not** bound.</span></span>

<span data-ttu-id="87977-506">La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="87977-506">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="87977-507">La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-507">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

<span data-ttu-id="87977-508">Se crean los siguientes pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-508">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="87977-509">Key</span><span class="sxs-lookup"><span data-stu-id="87977-509">Key</span></span>                   | <span data-ttu-id="87977-510">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-510">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="87977-511">starship:name</span><span class="sxs-lookup"><span data-stu-id="87977-511">starship:name</span></span>         | <span data-ttu-id="87977-512">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="87977-512">USS Enterprise</span></span>                                    |
| <span data-ttu-id="87977-513">starship:registry</span><span class="sxs-lookup"><span data-stu-id="87977-513">starship:registry</span></span>     | <span data-ttu-id="87977-514">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="87977-514">NCC-1701</span></span>                                          |
| <span data-ttu-id="87977-515">starship:class</span><span class="sxs-lookup"><span data-stu-id="87977-515">starship:class</span></span>        | <span data-ttu-id="87977-516">Constitution</span><span class="sxs-lookup"><span data-stu-id="87977-516">Constitution</span></span>                                      |
| <span data-ttu-id="87977-517">starship:length</span><span class="sxs-lookup"><span data-stu-id="87977-517">starship:length</span></span>       | <span data-ttu-id="87977-518">304.8</span><span class="sxs-lookup"><span data-stu-id="87977-518">304.8</span></span>                                             |
| <span data-ttu-id="87977-519">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="87977-519">starship:commissioned</span></span> | <span data-ttu-id="87977-520">False</span><span class="sxs-lookup"><span data-stu-id="87977-520">False</span></span>                                             |
| <span data-ttu-id="87977-521">trademark</span><span class="sxs-lookup"><span data-stu-id="87977-521">trademark</span></span>             | <span data-ttu-id="87977-522">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="87977-522">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="87977-523">La aplicación de ejemplo llama a `GetSection` con la clave `starship`.</span><span class="sxs-lookup"><span data-stu-id="87977-523">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="87977-524">Los pares clave-valor `starship` están aislados.</span><span class="sxs-lookup"><span data-stu-id="87977-524">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="87977-525">El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`.</span><span class="sxs-lookup"><span data-stu-id="87977-525">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="87977-526">Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="87977-526">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="87977-527">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="87977-527">Bind to an object graph</span></span>

<span data-ttu-id="87977-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO.</span><span class="sxs-lookup"><span data-stu-id="87977-528"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="87977-529">Al igual que ocurre con el enlace de un objeto simple, solo se enlazan las propiedades públicas de lectura o escritura.</span><span class="sxs-lookup"><span data-stu-id="87977-529">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="87977-530">El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="87977-530">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="87977-531">La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-531">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="87977-532">Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="87977-532">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="87977-533">La instancia enlazada se asigna a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="87977-533">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="87977-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="87977-534">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="87977-535">`Get<T>` es más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="87977-535">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="87977-536">El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:</span><span class="sxs-lookup"><span data-stu-id="87977-536">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="87977-537">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="87977-537">Bind an array to a class</span></span>

<span data-ttu-id="87977-538">*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*</span><span class="sxs-lookup"><span data-stu-id="87977-538">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="87977-539"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-539">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="87977-540">Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.</span><span class="sxs-lookup"><span data-stu-id="87977-540">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="87977-541">El enlace se proporciona por convención.</span><span class="sxs-lookup"><span data-stu-id="87977-541">Binding is provided by convention.</span></span> <span data-ttu-id="87977-542">Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.</span><span class="sxs-lookup"><span data-stu-id="87977-542">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="87977-543">**Procesamiento de matriz en memoria**</span><span class="sxs-lookup"><span data-stu-id="87977-543">**In-memory array processing**</span></span>

<span data-ttu-id="87977-544">Considere los valores y las claves de configuración que se muestran en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="87977-544">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="87977-545">Key</span><span class="sxs-lookup"><span data-stu-id="87977-545">Key</span></span>             | <span data-ttu-id="87977-546">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-546">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="87977-547">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="87977-547">array:entries:0</span></span> | <span data-ttu-id="87977-548">value0</span><span class="sxs-lookup"><span data-stu-id="87977-548">value0</span></span> |
| <span data-ttu-id="87977-549">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="87977-549">array:entries:1</span></span> | <span data-ttu-id="87977-550">value1</span><span class="sxs-lookup"><span data-stu-id="87977-550">value1</span></span> |
| <span data-ttu-id="87977-551">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="87977-551">array:entries:2</span></span> | <span data-ttu-id="87977-552">value2</span><span class="sxs-lookup"><span data-stu-id="87977-552">value2</span></span> |
| <span data-ttu-id="87977-553">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="87977-553">array:entries:4</span></span> | <span data-ttu-id="87977-554">value4</span><span class="sxs-lookup"><span data-stu-id="87977-554">value4</span></span> |
| <span data-ttu-id="87977-555">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="87977-555">array:entries:5</span></span> | <span data-ttu-id="87977-556">value5</span><span class="sxs-lookup"><span data-stu-id="87977-556">value5</span></span> |

<span data-ttu-id="87977-557">Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:</span><span class="sxs-lookup"><span data-stu-id="87977-557">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="87977-558">La matriz omite un valor de índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="87977-558">The array skips a value for index &num;3.</span></span> <span data-ttu-id="87977-559">El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.</span><span class="sxs-lookup"><span data-stu-id="87977-559">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="87977-560">En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:</span><span class="sxs-lookup"><span data-stu-id="87977-560">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="87977-561">Los datos de configuración están enlazados al objeto:</span><span class="sxs-lookup"><span data-stu-id="87977-561">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="87977-562">También se puede usar la sintaxis de [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:</span><span class="sxs-lookup"><span data-stu-id="87977-562">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="87977-563">El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-563">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="87977-564">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-564">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="87977-565">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-565">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="87977-566">0</span><span class="sxs-lookup"><span data-stu-id="87977-566">0</span></span>                            | <span data-ttu-id="87977-567">value0</span><span class="sxs-lookup"><span data-stu-id="87977-567">value0</span></span>                       |
| <span data-ttu-id="87977-568">1</span><span class="sxs-lookup"><span data-stu-id="87977-568">1</span></span>                            | <span data-ttu-id="87977-569">value1</span><span class="sxs-lookup"><span data-stu-id="87977-569">value1</span></span>                       |
| <span data-ttu-id="87977-570">2</span><span class="sxs-lookup"><span data-stu-id="87977-570">2</span></span>                            | <span data-ttu-id="87977-571">value2</span><span class="sxs-lookup"><span data-stu-id="87977-571">value2</span></span>                       |
| <span data-ttu-id="87977-572">3</span><span class="sxs-lookup"><span data-stu-id="87977-572">3</span></span>                            | <span data-ttu-id="87977-573">value4</span><span class="sxs-lookup"><span data-stu-id="87977-573">value4</span></span>                       |
| <span data-ttu-id="87977-574">4</span><span class="sxs-lookup"><span data-stu-id="87977-574">4</span></span>                            | <span data-ttu-id="87977-575">value5</span><span class="sxs-lookup"><span data-stu-id="87977-575">value5</span></span>                       |

<span data-ttu-id="87977-576">El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="87977-576">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="87977-577">Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto.</span><span class="sxs-lookup"><span data-stu-id="87977-577">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="87977-578">No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.</span><span class="sxs-lookup"><span data-stu-id="87977-578">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="87977-579">El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExample` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-579">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="87977-580">Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExample.Entries` coincide con la matriz de configuración completa:</span><span class="sxs-lookup"><span data-stu-id="87977-580">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="87977-581">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="87977-581">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="87977-582">En `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="87977-582">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="87977-583">El par clave-valor que se muestra en la tabla se carga en la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-583">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="87977-584">Key</span><span class="sxs-lookup"><span data-stu-id="87977-584">Key</span></span>             | <span data-ttu-id="87977-585">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-585">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="87977-586">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="87977-586">array:entries:3</span></span> | <span data-ttu-id="87977-587">value3</span><span class="sxs-lookup"><span data-stu-id="87977-587">value3</span></span> |

<span data-ttu-id="87977-588">Si la instancia de clase `ArrayExample` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExample.Entries` incluye el valor.</span><span class="sxs-lookup"><span data-stu-id="87977-588">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="87977-589">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-589">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="87977-590">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-590">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="87977-591">0</span><span class="sxs-lookup"><span data-stu-id="87977-591">0</span></span>                            | <span data-ttu-id="87977-592">value0</span><span class="sxs-lookup"><span data-stu-id="87977-592">value0</span></span>                       |
| <span data-ttu-id="87977-593">1</span><span class="sxs-lookup"><span data-stu-id="87977-593">1</span></span>                            | <span data-ttu-id="87977-594">value1</span><span class="sxs-lookup"><span data-stu-id="87977-594">value1</span></span>                       |
| <span data-ttu-id="87977-595">2</span><span class="sxs-lookup"><span data-stu-id="87977-595">2</span></span>                            | <span data-ttu-id="87977-596">value2</span><span class="sxs-lookup"><span data-stu-id="87977-596">value2</span></span>                       |
| <span data-ttu-id="87977-597">3</span><span class="sxs-lookup"><span data-stu-id="87977-597">3</span></span>                            | <span data-ttu-id="87977-598">value3</span><span class="sxs-lookup"><span data-stu-id="87977-598">value3</span></span>                       |
| <span data-ttu-id="87977-599">4</span><span class="sxs-lookup"><span data-stu-id="87977-599">4</span></span>                            | <span data-ttu-id="87977-600">value4</span><span class="sxs-lookup"><span data-stu-id="87977-600">value4</span></span>                       |
| <span data-ttu-id="87977-601">5</span><span class="sxs-lookup"><span data-stu-id="87977-601">5</span></span>                            | <span data-ttu-id="87977-602">value5</span><span class="sxs-lookup"><span data-stu-id="87977-602">value5</span></span>                       |

<span data-ttu-id="87977-603">**Procesamiento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="87977-603">**JSON array processing**</span></span>

<span data-ttu-id="87977-604">Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero.</span><span class="sxs-lookup"><span data-stu-id="87977-604">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="87977-605">En el siguiente archivo de configuración, `subsection` es una matriz:</span><span class="sxs-lookup"><span data-stu-id="87977-605">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="87977-606">El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="87977-606">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="87977-607">Key</span><span class="sxs-lookup"><span data-stu-id="87977-607">Key</span></span>                     | <span data-ttu-id="87977-608">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-608">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="87977-609">json_array:key</span><span class="sxs-lookup"><span data-stu-id="87977-609">json_array:key</span></span>          | <span data-ttu-id="87977-610">valueA</span><span class="sxs-lookup"><span data-stu-id="87977-610">valueA</span></span> |
| <span data-ttu-id="87977-611">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="87977-611">json_array:subsection:0</span></span> | <span data-ttu-id="87977-612">valueB</span><span class="sxs-lookup"><span data-stu-id="87977-612">valueB</span></span> |
| <span data-ttu-id="87977-613">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="87977-613">json_array:subsection:1</span></span> | <span data-ttu-id="87977-614">valueC</span><span class="sxs-lookup"><span data-stu-id="87977-614">valueC</span></span> |
| <span data-ttu-id="87977-615">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="87977-615">json_array:subsection:2</span></span> | <span data-ttu-id="87977-616">valueD</span><span class="sxs-lookup"><span data-stu-id="87977-616">valueD</span></span> |

<span data-ttu-id="87977-617">En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-617">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="87977-618">Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="87977-618">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="87977-619">Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="87977-619">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="87977-620">Índice de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="87977-620">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="87977-621">Valor de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="87977-621">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="87977-622">0</span><span class="sxs-lookup"><span data-stu-id="87977-622">0</span></span>                                   | <span data-ttu-id="87977-623">valueB</span><span class="sxs-lookup"><span data-stu-id="87977-623">valueB</span></span>                              |
| <span data-ttu-id="87977-624">1</span><span class="sxs-lookup"><span data-stu-id="87977-624">1</span></span>                                   | <span data-ttu-id="87977-625">valueC</span><span class="sxs-lookup"><span data-stu-id="87977-625">valueC</span></span>                              |
| <span data-ttu-id="87977-626">2</span><span class="sxs-lookup"><span data-stu-id="87977-626">2</span></span>                                   | <span data-ttu-id="87977-627">valueD</span><span class="sxs-lookup"><span data-stu-id="87977-627">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="87977-628">Proveedores de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="87977-628">Custom configuration provider</span></span>

<span data-ttu-id="87977-629">La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="87977-629">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="87977-630">El proveedor tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="87977-630">The provider has the following characteristics:</span></span>

* <span data-ttu-id="87977-631">La base de datos en memoria de EF se usa para fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="87977-631">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="87977-632">Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-632">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="87977-633">El proveedor lee una tabla de base de datos en la configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="87977-633">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="87977-634">El proveedor no consulta la base de datos por clave.</span><span class="sxs-lookup"><span data-stu-id="87977-634">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="87977-635">La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-635">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="87977-636">Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="87977-636">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="87977-637">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-637">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="87977-638">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="87977-638">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="87977-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-639">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="87977-640">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="87977-640">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="87977-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-641">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="87977-642">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="87977-642">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="87977-643">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="87977-643">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="87977-644">Puesto que las [claves de configuración no distinguen entre mayúsculas y minúsculas](#keys), el diccionario empleado para iniciar la base de datos se crea con el comparador que tampoco hace tal distinción ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="87977-644">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="87977-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-645">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="87977-646">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-646">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="87977-647">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-647">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="87977-648">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-648">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="87977-649">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="87977-649">Access configuration during startup</span></span>

<span data-ttu-id="87977-650">Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="87977-650">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="87977-651">Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:</span><span class="sxs-lookup"><span data-stu-id="87977-651">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="87977-652">Para ver un ejemplo de cómo acceder a la configuración mediante métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods)</span><span class="sxs-lookup"><span data-stu-id="87977-652">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="87977-653">Acceso a la configuración en una página de Razor Pages o en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="87977-653">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="87977-654">Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.</span><span class="sxs-lookup"><span data-stu-id="87977-654">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="87977-655">En una página de las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="87977-655">In a Razor Pages page:</span></span>

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

<span data-ttu-id="87977-656">En una vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="87977-656">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="87977-657">Agregar configuración a partir de un ensamblado externo</span><span class="sxs-lookup"><span data-stu-id="87977-657">Add configuration from an external assembly</span></span>

<span data-ttu-id="87977-658">Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="87977-658">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="87977-659">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="87977-659">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87977-660">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="87977-660">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="87977-661">La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*.</span><span class="sxs-lookup"><span data-stu-id="87977-661">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="87977-662">Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-662">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="87977-663">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="87977-663">Azure Key Vault</span></span>
* <span data-ttu-id="87977-664">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="87977-664">Azure App Configuration</span></span>
* <span data-ttu-id="87977-665">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-665">Command-line arguments</span></span>
* <span data-ttu-id="87977-666">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="87977-666">Custom providers (installed or created)</span></span>
* <span data-ttu-id="87977-667">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="87977-667">Directory files</span></span>
* <span data-ttu-id="87977-668">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-668">Environment variables</span></span>
* <span data-ttu-id="87977-669">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="87977-669">In-memory .NET objects</span></span>
* <span data-ttu-id="87977-670">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="87977-670">Settings files</span></span>

<span data-ttu-id="87977-671">Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="87977-671">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="87977-672">Los ejemplos de código que siguen y en la aplicación de ejemplo utilizan el espacio de nombres <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="87977-672">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="87977-673">El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="87977-673">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="87977-674">Las opciones usan clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="87977-674">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="87977-675">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="87977-675">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="87977-676">[Vea o descargue el código de ejemplo](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="87977-676">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="87977-677">Configuración de host y de aplicación</span><span class="sxs-lookup"><span data-stu-id="87977-677">Host versus app configuration</span></span>

<span data-ttu-id="87977-678">Antes de configurar e iniciar la aplicación, se configura e inicia un *host*.</span><span class="sxs-lookup"><span data-stu-id="87977-678">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="87977-679">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-679">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="87977-680">Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="87977-680">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="87977-681">Los pares clave-valor de configuración del host también se incluyen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-681">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="87977-682">Para más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="87977-682">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="87977-683">Otra configuración</span><span class="sxs-lookup"><span data-stu-id="87977-683">Other configuration</span></span>

<span data-ttu-id="87977-684">Este tema solo concierne a la *configuración de aplicaciones*.</span><span class="sxs-lookup"><span data-stu-id="87977-684">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="87977-685">Otros aspectos de la ejecución y el hospedaje de aplicaciones ASP.NET Core se configuran mediante archivos de configuración que no se describen en este tema:</span><span class="sxs-lookup"><span data-stu-id="87977-685">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="87977-686">*launch.json*/*launchSettings.json* son archivos de configuración de herramientas para el entorno de desarrollo, que se describe a continuación:</span><span class="sxs-lookup"><span data-stu-id="87977-686">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="87977-687">En <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="87977-687">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="87977-688">En el conjunto de documentación en el que se usan los archivos para configurar aplicaciones ASP.NET Core para escenarios de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-688">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="87977-689">*web.config* es un archivo de configuración del servidor, que se describe en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-689">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="87977-690">Para más información sobre cómo migrar la configuración de aplicaciones de versiones anteriores de ASP.NET, consulte <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="87977-690">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="87977-691">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="87977-691">Default configuration</span></span>

<span data-ttu-id="87977-692">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="87977-692">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="87977-693">`CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-693">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="87977-694">El contenido siguiente es válido para las aplicaciones que usen el [host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="87977-694">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="87977-695">Para obtener más información sobre la configuración predeterminada al usar el [host genérico](xref:fundamentals/host/generic-host), vea la [versión más reciente de ese tema](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="87977-695">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="87977-696">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="87977-696">Host configuration is provided from:</span></span>
  * <span data-ttu-id="87977-697">Variables de entorno prefijadas con `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-697">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="87977-698">El prefijo (`ASPNETCORE_`) se quita cuando se cargan los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-698">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="87977-699">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-699">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="87977-700">La configuración de la aplicación la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="87977-700">App configuration is provided from:</span></span>
  * <span data-ttu-id="87977-701">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-701">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="87977-702">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-702">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="87977-703">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="87977-703">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="87977-704">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-704">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="87977-705">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="87977-705">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

## <a name="security"></a><span data-ttu-id="87977-706">Seguridad</span><span class="sxs-lookup"><span data-stu-id="87977-706">Security</span></span>

<span data-ttu-id="87977-707">Adopte los procedimientos siguientes para proteger los datos de configuración confidenciales:</span><span class="sxs-lookup"><span data-stu-id="87977-707">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="87977-708">Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="87977-708">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="87977-709">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="87977-709">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="87977-710">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="87977-710">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="87977-711">Para obtener más información, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-711">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="87977-712"><xref:security/app-secrets> &ndash; Se incluyen recomendaciones sobre el uso de variables de entorno para almacenar información confidencial.</span><span class="sxs-lookup"><span data-stu-id="87977-712"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="87977-713">El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local.</span><span class="sxs-lookup"><span data-stu-id="87977-713">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="87977-714">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="87977-714">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="87977-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) almacena de forma segura secretos de aplicación para aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87977-715">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="87977-716">Para obtener más información, vea <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="87977-716">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="87977-717">Datos de configuración jerárquica</span><span class="sxs-lookup"><span data-stu-id="87977-717">Hierarchical configuration data</span></span>

<span data-ttu-id="87977-718">La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-718">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="87977-719">En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:</span><span class="sxs-lookup"><span data-stu-id="87977-719">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="87977-720">Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-720">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="87977-721">Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:</span><span class="sxs-lookup"><span data-stu-id="87977-721">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="87977-722">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-722">section0:key0</span></span>
* <span data-ttu-id="87977-723">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-723">section0:key1</span></span>
* <span data-ttu-id="87977-724">section1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-724">section1:key0</span></span>
* <span data-ttu-id="87977-725">section1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-725">section1:key1</span></span>

<span data-ttu-id="87977-726">Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-726"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="87977-727">Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="87977-727">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="87977-728">Convenciones</span><span class="sxs-lookup"><span data-stu-id="87977-728">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="87977-729">Orígenes y proveedores</span><span class="sxs-lookup"><span data-stu-id="87977-729">Sources and providers</span></span>

<span data-ttu-id="87977-730">Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-730">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="87977-731">Los proveedores de configuración que implementan la detección de cambios tienen la capacidad de volver a cargar la configuración cuando se cambia una configuración subyacente.</span><span class="sxs-lookup"><span data-stu-id="87977-731">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="87977-732">Por ejemplo, el proveedor de configuración de archivos (descrito más adelante en este tema) y el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) implementan la detección de cambios.</span><span class="sxs-lookup"><span data-stu-id="87977-732">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="87977-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-733"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="87977-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages o <xref:Microsoft.AspNetCore.Mvc.Controller> de MVC para obtener la configuración de la clase.</span><span class="sxs-lookup"><span data-stu-id="87977-734"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="87977-735">En los ejemplos siguientes, se usa el campo `_config` para tener acceso a los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-735">In the following examples, the `_config` field is used to access configuration values:</span></span>

```csharp
public class IndexModel : PageModel
{
    private readonly IConfiguration _config;

    public IndexModel(IConfiguration config)
    {
        _config = config;
    }
}
```

```csharp
public class HomeController : Controller
{
    private readonly IConfiguration _config;

    public HomeController(IConfiguration config)
    {
        _config = config;
    }
}
```

<span data-ttu-id="87977-736">Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.</span><span class="sxs-lookup"><span data-stu-id="87977-736">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="87977-737">Teclas</span><span class="sxs-lookup"><span data-stu-id="87977-737">Keys</span></span>

<span data-ttu-id="87977-738">Las claves de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-738">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="87977-739">Las claves no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="87977-739">Keys are case-insensitive.</span></span> <span data-ttu-id="87977-740">Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.</span><span class="sxs-lookup"><span data-stu-id="87977-740">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="87977-741">Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.</span><span class="sxs-lookup"><span data-stu-id="87977-741">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="87977-742">Claves jerárquicas</span><span class="sxs-lookup"><span data-stu-id="87977-742">Hierarchical keys</span></span>
  * <span data-ttu-id="87977-743">Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="87977-743">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="87977-744">En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="87977-744">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="87977-745">Todas las plataformas admiten un carácter de subrayado doble (`__`), que se convierte automáticamente en un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="87977-745">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="87977-746">En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="87977-746">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="87977-747">Escriba código para reemplazar los guiones por dos puntos cuando los secretos se carguen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-747">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="87977-748"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-748">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="87977-749">El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="87977-749">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="87977-750">Valores</span><span class="sxs-lookup"><span data-stu-id="87977-750">Values</span></span>

<span data-ttu-id="87977-751">Los valores de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="87977-751">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="87977-752">Los valores son cadenas.</span><span class="sxs-lookup"><span data-stu-id="87977-752">Values are strings.</span></span>
* <span data-ttu-id="87977-753">Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.</span><span class="sxs-lookup"><span data-stu-id="87977-753">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="87977-754">Proveedores</span><span class="sxs-lookup"><span data-stu-id="87977-754">Providers</span></span>

<span data-ttu-id="87977-755">La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="87977-755">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="87977-756">Proveedor</span><span class="sxs-lookup"><span data-stu-id="87977-756">Provider</span></span> | <span data-ttu-id="87977-757">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="87977-757">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="87977-758">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="87977-758">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="87977-759">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="87977-759">Azure Key Vault</span></span> |
| <span data-ttu-id="87977-760">[Proveedor de Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentación de Azure)</span><span class="sxs-lookup"><span data-stu-id="87977-760">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="87977-761">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="87977-761">Azure App Configuration</span></span> |
| [<span data-ttu-id="87977-762">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-762">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="87977-763">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-763">Command-line parameters</span></span> |
| [<span data-ttu-id="87977-764">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="87977-764">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="87977-765">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="87977-765">Custom source</span></span> |
| [<span data-ttu-id="87977-766">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-766">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="87977-767">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-767">Environment variables</span></span> |
| [<span data-ttu-id="87977-768">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="87977-768">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="87977-769">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="87977-769">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="87977-770">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="87977-770">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="87977-771">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="87977-771">Directory files</span></span> |
| [<span data-ttu-id="87977-772">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="87977-772">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="87977-773">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="87977-773">In-memory collections</span></span> |
| <span data-ttu-id="87977-774">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="87977-774">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="87977-775">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="87977-775">File in the user profile directory</span></span> |

<span data-ttu-id="87977-776">Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="87977-776">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="87977-777">En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en el que el código los organiza.</span><span class="sxs-lookup"><span data-stu-id="87977-777">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="87977-778">Ordene los proveedores de configuración en el código para cumplir con las prioridades relacionadas con los orígenes de configuración subyacentes que la aplicación necesita.</span><span class="sxs-lookup"><span data-stu-id="87977-778">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="87977-779">Esta es una secuencia típica de proveedores de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-779">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="87977-780">Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)</span><span class="sxs-lookup"><span data-stu-id="87977-780">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="87977-781">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="87977-781">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="87977-782">[Secretos de usuario (administrador de secretos)](xref:security/app-secrets) (solo para entornos de desarrollo)</span><span class="sxs-lookup"><span data-stu-id="87977-782">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="87977-783">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-783">Environment variables</span></span>
1. <span data-ttu-id="87977-784">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-784">Command-line arguments</span></span>

<span data-ttu-id="87977-785">Una práctica común es colocar el proveedor de configuración de línea de comandos al final en una serie de proveedores para permitir que los argumentos de la línea de comandos reemplacen la configuración establecida por el resto de los proveedores.</span><span class="sxs-lookup"><span data-stu-id="87977-785">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="87977-786">La secuencia de proveedores anterior se usa cuando se inicializa un nuevo generador de host con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-786">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-787">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-787">For more information, see the [Default configuration](#default-configuration) section.</span></span>

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="87977-788">Configurar el generador de host con UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="87977-788">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="87977-789">Para configurar el generador de host, realice una llamada a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> en el generador de host con la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-789">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

```csharp
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
```

## <a name="configureappconfiguration"></a><span data-ttu-id="87977-790">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="87977-790">ConfigureAppConfiguration</span></span>

<span data-ttu-id="87977-791">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que `CreateDefaultBuilder` agrega automáticamente:</span><span class="sxs-lookup"><span data-stu-id="87977-791">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="87977-792">Reemplazar la configuración anterior con argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-792">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="87977-793">Para proporcionar configuración de aplicaciones que se pueda reemplazar por argumentos de la línea de comandos, llame a `AddCommandLine` en último lugar:</span><span class="sxs-lookup"><span data-stu-id="87977-793">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="87977-794">Quitar proveedores agregados por CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="87977-794">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="87977-795">Para quitar los proveedores agregados por `CreateDefaultBuilder`, llame primero a [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) en [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="87977-795">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="87977-796">Usar la configuración durante el inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="87977-796">Consume configuration during app startup</span></span>

<span data-ttu-id="87977-797">La configuración proporcionada a la aplicación en `ConfigureAppConfiguration` está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="87977-797">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="87977-798">Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="87977-798">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="87977-799">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="87977-799">Command-line Configuration Provider</span></span>

<span data-ttu-id="87977-800"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-800">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-801">Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-801">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-802">`AddCommandLine` se llama automáticamente al llamar a `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="87977-802">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="87977-803">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-803">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-804">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="87977-804">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="87977-805">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-805">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="87977-806">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-806">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="87977-807">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-807">Environment variables.</span></span>

<span data-ttu-id="87977-808">`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final.</span><span class="sxs-lookup"><span data-stu-id="87977-808">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="87977-809">Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="87977-809">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="87977-810">`CreateDefaultBuilder` actúa cuando se construye el host.</span><span class="sxs-lookup"><span data-stu-id="87977-810">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="87977-811">Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.</span><span class="sxs-lookup"><span data-stu-id="87977-811">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="87977-812">Para las aplicaciones basadas en plantillas de ASP.NET Core, `CreateDefaultBuilder` ya realiza una llamada a `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="87977-812">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-813">Para agregar proveedores de configuración adicionales y mantener la capacidad de reemplazar la configuración de sus proveedores con argumentos de la línea de comandos, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddCommandLine` en último lugar.</span><span class="sxs-lookup"><span data-stu-id="87977-813">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="87977-814">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-814">**Example**</span></span>

<span data-ttu-id="87977-815">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="87977-815">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="87977-816">Abra un símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="87977-816">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="87977-817">Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="87977-817">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="87977-818">Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="87977-818">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="87977-819">Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="87977-819">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="87977-820">Argumentos</span><span class="sxs-lookup"><span data-stu-id="87977-820">Arguments</span></span>

<span data-ttu-id="87977-821">El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio.</span><span class="sxs-lookup"><span data-stu-id="87977-821">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="87977-822">El valor no es obligatorio si se usa un signo igual (por ejemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="87977-822">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="87977-823">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="87977-823">Key prefix</span></span>               | <span data-ttu-id="87977-824">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="87977-824">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="87977-825">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="87977-825">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="87977-826">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="87977-826">Two dashes (`--`)</span></span>        | <span data-ttu-id="87977-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="87977-827">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="87977-828">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="87977-828">Forward slash (`/`)</span></span>      | <span data-ttu-id="87977-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="87977-829">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="87977-830">Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.</span><span class="sxs-lookup"><span data-stu-id="87977-830">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="87977-831">Comandos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="87977-831">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="87977-832">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="87977-832">Switch mappings</span></span>

<span data-ttu-id="87977-833">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="87977-833">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="87977-834">Al crear manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, proporcione un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="87977-834">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="87977-835">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-835">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="87977-836">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-836">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="87977-837">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="87977-837">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="87977-838">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="87977-838">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="87977-839">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="87977-839">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="87977-840">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="87977-840">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="87977-841">Cree un diccionario de asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="87977-841">Create a switch mappings dictionary.</span></span> <span data-ttu-id="87977-842">En el ejemplo siguiente, se crean dos asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="87977-842">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="87977-843">Al compilar el host, llame a `AddCommandLine` con el diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="87977-843">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="87977-844">En el caso de las aplicaciones que usen las asignaciones de modificador, la llamada a `CreateDefaultBuilder` no tiene que pasar argumentos.</span><span class="sxs-lookup"><span data-stu-id="87977-844">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="87977-845">En la llamada de `AddCommandLine` del método `CreateDefaultBuilder`, no se incluyen modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-845">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-846">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="87977-846">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="87977-847">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="87977-847">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="87977-848">Key</span><span class="sxs-lookup"><span data-stu-id="87977-848">Key</span></span>       | <span data-ttu-id="87977-849">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-849">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="87977-850">Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:</span><span class="sxs-lookup"><span data-stu-id="87977-850">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="87977-851">Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="87977-851">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="87977-852">Key</span><span class="sxs-lookup"><span data-stu-id="87977-852">Key</span></span>               | <span data-ttu-id="87977-853">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-853">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="87977-854">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-854">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="87977-855"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-855">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-856">Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-856">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="87977-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-857">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="87977-858">Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="87977-858">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

<span data-ttu-id="87977-859">`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `ASPNETCORE_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host de web](xref:fundamentals/host/web-host) y al llamar a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-859">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="87977-860">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-860">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-861">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="87977-861">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="87977-862">Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.</span><span class="sxs-lookup"><span data-stu-id="87977-862">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="87977-863">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-863">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="87977-864">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-864">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="87977-865">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-865">Command-line arguments.</span></span>

<span data-ttu-id="87977-866">El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="87977-866">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="87977-867">Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="87977-867">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="87977-868">Para proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddEnvironmentVariables` con el prefijo:</span><span class="sxs-lookup"><span data-stu-id="87977-868">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="87977-869">Llame a `AddEnvironmentVariables` en último lugar para permitir que las variables de entorno con el prefijo especificado invaliden los valores de otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="87977-869">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="87977-870">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-870">**Example**</span></span>

<span data-ttu-id="87977-871">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="87977-871">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="87977-872">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="87977-872">Run the sample app.</span></span> <span data-ttu-id="87977-873">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="87977-873">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="87977-874">Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="87977-874">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="87977-875">El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.</span><span class="sxs-lookup"><span data-stu-id="87977-875">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="87977-876">Para mantener un número adecuado de variables de entorno en la lista representada por la aplicación, la aplicación filtrará las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-876">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="87977-877">Vea el archivo *Pages/Index.cshtml.cs* de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="87977-877">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="87977-878">Para exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-878">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="87977-879">Prefijos</span><span class="sxs-lookup"><span data-stu-id="87977-879">Prefixes</span></span>

<span data-ttu-id="87977-880">Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="87977-880">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="87977-881">Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-881">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="87977-882">El prefijo se quita cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-882">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="87977-883">Al crear el generador de host, las variables de entorno proporcionan la configuración del host.</span><span class="sxs-lookup"><span data-stu-id="87977-883">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="87977-884">Para obtener más información sobre el prefijo usado para estas variables de entorno, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-884">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-885">**Prefijos de cadena de conexión**</span><span class="sxs-lookup"><span data-stu-id="87977-885">**Connection string prefixes**</span></span>

<span data-ttu-id="87977-886">La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-886">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="87977-887">Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="87977-887">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="87977-888">Prefijo de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="87977-888">Connection string prefix</span></span> | <span data-ttu-id="87977-889">Proveedor</span><span class="sxs-lookup"><span data-stu-id="87977-889">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="87977-890">Proveedor personalizado</span><span class="sxs-lookup"><span data-stu-id="87977-890">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="87977-891">MySQL</span><span class="sxs-lookup"><span data-stu-id="87977-891">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="87977-892">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="87977-892">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="87977-893">SQL Server</span><span class="sxs-lookup"><span data-stu-id="87977-893">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="87977-894">Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:</span><span class="sxs-lookup"><span data-stu-id="87977-894">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="87977-895">La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="87977-895">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="87977-896">Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).</span><span class="sxs-lookup"><span data-stu-id="87977-896">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="87977-897">Clave de variable de entorno</span><span class="sxs-lookup"><span data-stu-id="87977-897">Environment variable key</span></span> | <span data-ttu-id="87977-898">Clave de configuración convertida</span><span class="sxs-lookup"><span data-stu-id="87977-898">Converted configuration key</span></span> | <span data-ttu-id="87977-899">Entrada de configuración del proveedor</span><span class="sxs-lookup"><span data-stu-id="87977-899">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_{KEY} `   | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-900">Entrada de configuración no creada.</span><span class="sxs-lookup"><span data-stu-id="87977-900">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-901">Clave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="87977-901">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="87977-902">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="87977-902">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-903">Clave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="87977-903">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="87977-904">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="87977-904">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="87977-905">Clave: `ConnectionStrings:{KEY}_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="87977-905">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="87977-906">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="87977-906">Value: `System.Data.SqlClient`</span></span>  |

<span data-ttu-id="87977-907">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-907">**Example**</span></span>

<span data-ttu-id="87977-908">Se crea una variable de entorno de una cadena de conexión personalizada en el servidor:</span><span class="sxs-lookup"><span data-stu-id="87977-908">A custom connection string environment variable is created on the server:</span></span>

* <span data-ttu-id="87977-909">Nombre: `CUSTOMCONNSTR_ReleaseDB`</span><span class="sxs-lookup"><span data-stu-id="87977-909">Name &ndash; `CUSTOMCONNSTR_ReleaseDB`</span></span>
* <span data-ttu-id="87977-910">Valor: `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span><span class="sxs-lookup"><span data-stu-id="87977-910">Value &ndash; `Data Source=ReleaseSQLServer;Initial Catalog=MyReleaseDB;Integrated Security=True`</span></span>

<span data-ttu-id="87977-911">Si `IConfiguration` se inserta y se asigna a un campo denominado `_config`, se lee el valor siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-911">If `IConfiguration` is injected and assigned to a field named `_config`, read the value:</span></span>

```csharp
_config["ConnectionStrings:ReleaseDB"]
```

## <a name="file-configuration-provider"></a><span data-ttu-id="87977-912">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="87977-912">File Configuration Provider</span></span>

<span data-ttu-id="87977-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="87977-913"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="87977-914">Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:</span><span class="sxs-lookup"><span data-stu-id="87977-914">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="87977-915">Proveedor de configuración INI</span><span class="sxs-lookup"><span data-stu-id="87977-915">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="87977-916">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="87977-916">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="87977-917">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="87977-917">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="87977-918">Proveedor de configuración de INI</span><span class="sxs-lookup"><span data-stu-id="87977-918">INI Configuration Provider</span></span>

<span data-ttu-id="87977-919"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-919">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-920">Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-920">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-921">Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.</span><span class="sxs-lookup"><span data-stu-id="87977-921">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="87977-922">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-922">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-923">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="87977-923">Whether the file is optional.</span></span>
* <span data-ttu-id="87977-924">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="87977-924">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="87977-925"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-925">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="87977-926">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="87977-926">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="87977-927">Un ejemplo genérico de un archivo de configuración INI:</span><span class="sxs-lookup"><span data-stu-id="87977-927">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="87977-928">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-928">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-929">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-929">section0:key0</span></span>
* <span data-ttu-id="87977-930">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-930">section0:key1</span></span>
* <span data-ttu-id="87977-931">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="87977-931">section1:subsection:key</span></span>
* <span data-ttu-id="87977-932">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="87977-932">section2:subsection0:key</span></span>
* <span data-ttu-id="87977-933">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="87977-933">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="87977-934">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="87977-934">JSON Configuration Provider</span></span>

<span data-ttu-id="87977-935"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-935">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="87977-936">Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-936">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-937">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-937">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-938">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="87977-938">Whether the file is optional.</span></span>
* <span data-ttu-id="87977-939">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="87977-939">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="87977-940"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-940">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="87977-941">Se llama automáticamente a `AddJsonFile` dos veces cuando un nuevo generador de host se inicializa con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-941">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="87977-942">Se llama al método para cargar la configuración desde:</span><span class="sxs-lookup"><span data-stu-id="87977-942">The method is called to load configuration from:</span></span>

* <span data-ttu-id="87977-943">*appsettings.json*: este archivo se lee primero.</span><span class="sxs-lookup"><span data-stu-id="87977-943">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="87977-944">La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-944">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="87977-945">*appsettings.{Environment}.json*: la versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="87977-945">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="87977-946">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="87977-946">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="87977-947">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="87977-947">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="87977-948">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="87977-948">Environment variables.</span></span>
* <span data-ttu-id="87977-949">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-949">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="87977-950">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-950">Command-line arguments.</span></span>

<span data-ttu-id="87977-951">El proveedor de configuración de JSON se establece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="87977-951">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="87977-952">Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="87977-952">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="87977-953">Llame a `ConfigureAppConfiguration` al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="87977-953">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="87977-954">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="87977-954">**Example**</span></span>

<span data-ttu-id="87977-955">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="87977-955">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

* <span data-ttu-id="87977-956">La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="87977-956">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="87977-957">La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-957">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="87977-958">Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:</span><span class="sxs-lookup"><span data-stu-id="87977-958">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="87977-959">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="87977-959">Run the sample app.</span></span> <span data-ttu-id="87977-960">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="87977-960">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="87977-961">La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-961">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="87977-962">El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="87977-962">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="87977-963">Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:</span><span class="sxs-lookup"><span data-stu-id="87977-963">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="87977-964">Abra el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-964">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="87977-965">En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.</span><span class="sxs-lookup"><span data-stu-id="87977-965">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="87977-966">Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="87977-966">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="87977-967">La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="87977-967">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="87977-968">El nivel de registro de la clave `Logging:LogLevel:Default` es `Warning`.</span><span class="sxs-lookup"><span data-stu-id="87977-968">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

### <a name="xml-configuration-provider"></a><span data-ttu-id="87977-969">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="87977-969">XML Configuration Provider</span></span>

<span data-ttu-id="87977-970"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="87977-970">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="87977-971">Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-971">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-972">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-972">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-973">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="87977-973">Whether the file is optional.</span></span>
* <span data-ttu-id="87977-974">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="87977-974">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="87977-975"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-975">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="87977-976">El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-976">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="87977-977">No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-977">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="87977-978">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="87977-978">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="87977-979">Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:</span><span class="sxs-lookup"><span data-stu-id="87977-979">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="87977-980">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-980">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-981">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-981">section0:key0</span></span>
* <span data-ttu-id="87977-982">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-982">section0:key1</span></span>
* <span data-ttu-id="87977-983">section1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-983">section1:key0</span></span>
* <span data-ttu-id="87977-984">section1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-984">section1:key1</span></span>

<span data-ttu-id="87977-985">Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:</span><span class="sxs-lookup"><span data-stu-id="87977-985">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="87977-986">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-986">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-987">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="87977-987">section:section0:key:key0</span></span>
* <span data-ttu-id="87977-988">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="87977-988">section:section0:key:key1</span></span>
* <span data-ttu-id="87977-989">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="87977-989">section:section1:key:key0</span></span>
* <span data-ttu-id="87977-990">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="87977-990">section:section1:key:key1</span></span>

<span data-ttu-id="87977-991">Los atributos se pueden usar para suministrar valores:</span><span class="sxs-lookup"><span data-stu-id="87977-991">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="87977-992">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="87977-992">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="87977-993">key:attribute</span><span class="sxs-lookup"><span data-stu-id="87977-993">key:attribute</span></span>
* <span data-ttu-id="87977-994">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="87977-994">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="87977-995">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="87977-995">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="87977-996"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-996">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="87977-997">La clave es el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-997">The key is the file name.</span></span> <span data-ttu-id="87977-998">El valor contiene el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-998">The value contains the file's contents.</span></span> <span data-ttu-id="87977-999">El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.</span><span class="sxs-lookup"><span data-stu-id="87977-999">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="87977-1000">Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-1000">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="87977-1001">La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="87977-1001">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="87977-1002">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="87977-1002">Overloads permit specifying:</span></span>

* <span data-ttu-id="87977-1003">Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.</span><span class="sxs-lookup"><span data-stu-id="87977-1003">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="87977-1004">Si el directorio es opcional y la ruta de acceso al directorio.</span><span class="sxs-lookup"><span data-stu-id="87977-1004">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="87977-1005">El carácter de subrayado doble (`__`) se usa como delimitador de claves de configuración en los nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="87977-1005">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="87977-1006">Por ejemplo, el nombre de archivo `Logging__LogLevel__System` genera la clave de configuración `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="87977-1006">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="87977-1007">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="87977-1007">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="87977-1008">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="87977-1008">Memory Configuration Provider</span></span>

<span data-ttu-id="87977-1009"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-1009">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="87977-1010">Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="87977-1010">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="87977-1011">El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="87977-1011">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="87977-1012">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-1012">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="87977-1013">En el ejemplo siguiente, se crea un diccionario de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1013">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="87977-1014">El diccionario se usa con una llamada a `AddInMemoryCollection` para proporcionar la configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1014">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="87977-1015">GetValue</span><span class="sxs-lookup"><span data-stu-id="87977-1015">GetValue</span></span>

<span data-ttu-id="87977-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un único valor de la configuración con una clave especificada y lo convierte al tipo especificado que no es de colección.</span><span class="sxs-lookup"><span data-stu-id="87977-1016">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="87977-1017">Una sobrecarga acepta un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="87977-1017">An overload accepts a default value.</span></span>

<span data-ttu-id="87977-1018">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="87977-1018">The following example:</span></span>

* <span data-ttu-id="87977-1019">Se extrae el valor de cadena de la configuración con la clave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="87977-1019">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="87977-1020">Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.</span><span class="sxs-lookup"><span data-stu-id="87977-1020">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="87977-1021">Se escribe el valor como `int`.</span><span class="sxs-lookup"><span data-stu-id="87977-1021">Types the value as an `int`.</span></span>
* <span data-ttu-id="87977-1022">Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="87977-1022">Stores the value in the `NumberConfig` property for use by the page.</span></span>

```csharp
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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="87977-1023">GetSection, GetChildren y Exists</span><span class="sxs-lookup"><span data-stu-id="87977-1023">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="87977-1024">Para los ejemplos siguientes, considere el siguiente archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="87977-1024">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="87977-1025">Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:</span><span class="sxs-lookup"><span data-stu-id="87977-1025">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="87977-1026">Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1026">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="87977-1027">section0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-1027">section0:key0</span></span>
* <span data-ttu-id="87977-1028">section0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-1028">section0:key1</span></span>
* <span data-ttu-id="87977-1029">section1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-1029">section1:key0</span></span>
* <span data-ttu-id="87977-1030">section1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-1030">section1:key1</span></span>
* <span data-ttu-id="87977-1031">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="87977-1031">section2:subsection0:key0</span></span>
* <span data-ttu-id="87977-1032">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="87977-1032">section2:subsection0:key1</span></span>
* <span data-ttu-id="87977-1033">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="87977-1033">section2:subsection1:key0</span></span>
* <span data-ttu-id="87977-1034">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="87977-1034">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="87977-1035">GetSection</span><span class="sxs-lookup"><span data-stu-id="87977-1035">GetSection</span></span>

<span data-ttu-id="87977-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.</span><span class="sxs-lookup"><span data-stu-id="87977-1036">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="87977-1037">Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:</span><span class="sxs-lookup"><span data-stu-id="87977-1037">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="87977-1038">`configSection` no tiene ningún valor, solo una clave y una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="87977-1038">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="87977-1039">De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:</span><span class="sxs-lookup"><span data-stu-id="87977-1039">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="87977-1040">`GetSection` nunca devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="87977-1040">`GetSection` never returns `null`.</span></span> <span data-ttu-id="87977-1041">Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.</span><span class="sxs-lookup"><span data-stu-id="87977-1041">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="87977-1042">Cuando `GetSection` devuelve una sección coincidente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> no se rellena.</span><span class="sxs-lookup"><span data-stu-id="87977-1042">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="87977-1043">Se devuelven <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> y <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> si la sección existe.</span><span class="sxs-lookup"><span data-stu-id="87977-1043">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="87977-1044">GetChildren</span><span class="sxs-lookup"><span data-stu-id="87977-1044">GetChildren</span></span>

<span data-ttu-id="87977-1045">Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:</span><span class="sxs-lookup"><span data-stu-id="87977-1045">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="87977-1046">Existe</span><span class="sxs-lookup"><span data-stu-id="87977-1046">Exists</span></span>

<span data-ttu-id="87977-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1047">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="87977-1048">Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-1048">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="87977-1049">Enlace a una clase</span><span class="sxs-lookup"><span data-stu-id="87977-1049">Bind to a class</span></span>

<span data-ttu-id="87977-1050">La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="87977-1050">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="87977-1051">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="87977-1051">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="87977-1052">Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="87977-1052">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="87977-1053">El enlazador enlaza los valores con todas las propiedades de lectura o escritura públicas del tipo proporcionado.</span><span class="sxs-lookup"><span data-stu-id="87977-1053">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="87977-1054">Los campos **no** se enlazan.</span><span class="sxs-lookup"><span data-stu-id="87977-1054">Fields are **not** bound.</span></span>

<span data-ttu-id="87977-1055">La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="87977-1055">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

<span data-ttu-id="87977-1056">La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1056">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

<span data-ttu-id="87977-1057">Se crean los siguientes pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1057">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="87977-1058">Key</span><span class="sxs-lookup"><span data-stu-id="87977-1058">Key</span></span>                   | <span data-ttu-id="87977-1059">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-1059">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="87977-1060">starship:name</span><span class="sxs-lookup"><span data-stu-id="87977-1060">starship:name</span></span>         | <span data-ttu-id="87977-1061">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="87977-1061">USS Enterprise</span></span>                                    |
| <span data-ttu-id="87977-1062">starship:registry</span><span class="sxs-lookup"><span data-stu-id="87977-1062">starship:registry</span></span>     | <span data-ttu-id="87977-1063">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="87977-1063">NCC-1701</span></span>                                          |
| <span data-ttu-id="87977-1064">starship:class</span><span class="sxs-lookup"><span data-stu-id="87977-1064">starship:class</span></span>        | <span data-ttu-id="87977-1065">Constitution</span><span class="sxs-lookup"><span data-stu-id="87977-1065">Constitution</span></span>                                      |
| <span data-ttu-id="87977-1066">starship:length</span><span class="sxs-lookup"><span data-stu-id="87977-1066">starship:length</span></span>       | <span data-ttu-id="87977-1067">304.8</span><span class="sxs-lookup"><span data-stu-id="87977-1067">304.8</span></span>                                             |
| <span data-ttu-id="87977-1068">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="87977-1068">starship:commissioned</span></span> | <span data-ttu-id="87977-1069">False</span><span class="sxs-lookup"><span data-stu-id="87977-1069">False</span></span>                                             |
| <span data-ttu-id="87977-1070">trademark</span><span class="sxs-lookup"><span data-stu-id="87977-1070">trademark</span></span>             | <span data-ttu-id="87977-1071">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="87977-1071">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="87977-1072">La aplicación de ejemplo llama a `GetSection` con la clave `starship`.</span><span class="sxs-lookup"><span data-stu-id="87977-1072">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="87977-1073">Los pares clave-valor `starship` están aislados.</span><span class="sxs-lookup"><span data-stu-id="87977-1073">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="87977-1074">El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`.</span><span class="sxs-lookup"><span data-stu-id="87977-1074">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="87977-1075">Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="87977-1075">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="87977-1076">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="87977-1076">Bind to an object graph</span></span>

<span data-ttu-id="87977-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO.</span><span class="sxs-lookup"><span data-stu-id="87977-1077"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="87977-1078">Al igual que ocurre con el enlace de un objeto simple, solo se enlazan las propiedades públicas de lectura o escritura.</span><span class="sxs-lookup"><span data-stu-id="87977-1078">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="87977-1079">El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="87977-1079">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

<span data-ttu-id="87977-1080">La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1080">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

<span data-ttu-id="87977-1081">Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="87977-1081">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="87977-1082">La instancia enlazada se asigna a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="87977-1082">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="87977-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="87977-1083">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="87977-1084">`Get<T>` es más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="87977-1084">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="87977-1085">El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:</span><span class="sxs-lookup"><span data-stu-id="87977-1085">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="87977-1086">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="87977-1086">Bind an array to a class</span></span>

<span data-ttu-id="87977-1087">*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*</span><span class="sxs-lookup"><span data-stu-id="87977-1087">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="87977-1088"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-1088">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="87977-1089">Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.</span><span class="sxs-lookup"><span data-stu-id="87977-1089">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="87977-1090">El enlace se proporciona por convención.</span><span class="sxs-lookup"><span data-stu-id="87977-1090">Binding is provided by convention.</span></span> <span data-ttu-id="87977-1091">Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.</span><span class="sxs-lookup"><span data-stu-id="87977-1091">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="87977-1092">**Procesamiento de matriz en memoria**</span><span class="sxs-lookup"><span data-stu-id="87977-1092">**In-memory array processing**</span></span>

<span data-ttu-id="87977-1093">Considere los valores y las claves de configuración que se muestran en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="87977-1093">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="87977-1094">Key</span><span class="sxs-lookup"><span data-stu-id="87977-1094">Key</span></span>             | <span data-ttu-id="87977-1095">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-1095">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="87977-1096">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="87977-1096">array:entries:0</span></span> | <span data-ttu-id="87977-1097">value0</span><span class="sxs-lookup"><span data-stu-id="87977-1097">value0</span></span> |
| <span data-ttu-id="87977-1098">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="87977-1098">array:entries:1</span></span> | <span data-ttu-id="87977-1099">value1</span><span class="sxs-lookup"><span data-stu-id="87977-1099">value1</span></span> |
| <span data-ttu-id="87977-1100">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="87977-1100">array:entries:2</span></span> | <span data-ttu-id="87977-1101">value2</span><span class="sxs-lookup"><span data-stu-id="87977-1101">value2</span></span> |
| <span data-ttu-id="87977-1102">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="87977-1102">array:entries:4</span></span> | <span data-ttu-id="87977-1103">value4</span><span class="sxs-lookup"><span data-stu-id="87977-1103">value4</span></span> |
| <span data-ttu-id="87977-1104">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="87977-1104">array:entries:5</span></span> | <span data-ttu-id="87977-1105">value5</span><span class="sxs-lookup"><span data-stu-id="87977-1105">value5</span></span> |

<span data-ttu-id="87977-1106">Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:</span><span class="sxs-lookup"><span data-stu-id="87977-1106">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

<span data-ttu-id="87977-1107">La matriz omite un valor de índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="87977-1107">The array skips a value for index &num;3.</span></span> <span data-ttu-id="87977-1108">El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.</span><span class="sxs-lookup"><span data-stu-id="87977-1108">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="87977-1109">En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:</span><span class="sxs-lookup"><span data-stu-id="87977-1109">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

<span data-ttu-id="87977-1110">Los datos de configuración están enlazados al objeto:</span><span class="sxs-lookup"><span data-stu-id="87977-1110">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="87977-1111">También se puede usar la sintaxis de [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:</span><span class="sxs-lookup"><span data-stu-id="87977-1111">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

<span data-ttu-id="87977-1112">El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-1112">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="87977-1113">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-1113">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="87977-1114">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-1114">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="87977-1115">0</span><span class="sxs-lookup"><span data-stu-id="87977-1115">0</span></span>                            | <span data-ttu-id="87977-1116">value0</span><span class="sxs-lookup"><span data-stu-id="87977-1116">value0</span></span>                       |
| <span data-ttu-id="87977-1117">1</span><span class="sxs-lookup"><span data-stu-id="87977-1117">1</span></span>                            | <span data-ttu-id="87977-1118">value1</span><span class="sxs-lookup"><span data-stu-id="87977-1118">value1</span></span>                       |
| <span data-ttu-id="87977-1119">2</span><span class="sxs-lookup"><span data-stu-id="87977-1119">2</span></span>                            | <span data-ttu-id="87977-1120">value2</span><span class="sxs-lookup"><span data-stu-id="87977-1120">value2</span></span>                       |
| <span data-ttu-id="87977-1121">3</span><span class="sxs-lookup"><span data-stu-id="87977-1121">3</span></span>                            | <span data-ttu-id="87977-1122">value4</span><span class="sxs-lookup"><span data-stu-id="87977-1122">value4</span></span>                       |
| <span data-ttu-id="87977-1123">4</span><span class="sxs-lookup"><span data-stu-id="87977-1123">4</span></span>                            | <span data-ttu-id="87977-1124">value5</span><span class="sxs-lookup"><span data-stu-id="87977-1124">value5</span></span>                       |

<span data-ttu-id="87977-1125">El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="87977-1125">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="87977-1126">Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto.</span><span class="sxs-lookup"><span data-stu-id="87977-1126">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="87977-1127">No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.</span><span class="sxs-lookup"><span data-stu-id="87977-1127">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="87977-1128">El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExample` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-1128">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="87977-1129">Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExample.Entries` coincide con la matriz de configuración completa:</span><span class="sxs-lookup"><span data-stu-id="87977-1129">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="87977-1130">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="87977-1130">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="87977-1131">En `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="87977-1131">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="87977-1132">El par clave-valor que se muestra en la tabla se carga en la configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-1132">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="87977-1133">Key</span><span class="sxs-lookup"><span data-stu-id="87977-1133">Key</span></span>             | <span data-ttu-id="87977-1134">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-1134">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="87977-1135">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="87977-1135">array:entries:3</span></span> | <span data-ttu-id="87977-1136">value3</span><span class="sxs-lookup"><span data-stu-id="87977-1136">value3</span></span> |

<span data-ttu-id="87977-1137">Si la instancia de clase `ArrayExample` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExample.Entries` incluye el valor.</span><span class="sxs-lookup"><span data-stu-id="87977-1137">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="87977-1138">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-1138">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="87977-1139">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="87977-1139">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="87977-1140">0</span><span class="sxs-lookup"><span data-stu-id="87977-1140">0</span></span>                            | <span data-ttu-id="87977-1141">value0</span><span class="sxs-lookup"><span data-stu-id="87977-1141">value0</span></span>                       |
| <span data-ttu-id="87977-1142">1</span><span class="sxs-lookup"><span data-stu-id="87977-1142">1</span></span>                            | <span data-ttu-id="87977-1143">value1</span><span class="sxs-lookup"><span data-stu-id="87977-1143">value1</span></span>                       |
| <span data-ttu-id="87977-1144">2</span><span class="sxs-lookup"><span data-stu-id="87977-1144">2</span></span>                            | <span data-ttu-id="87977-1145">value2</span><span class="sxs-lookup"><span data-stu-id="87977-1145">value2</span></span>                       |
| <span data-ttu-id="87977-1146">3</span><span class="sxs-lookup"><span data-stu-id="87977-1146">3</span></span>                            | <span data-ttu-id="87977-1147">value3</span><span class="sxs-lookup"><span data-stu-id="87977-1147">value3</span></span>                       |
| <span data-ttu-id="87977-1148">4</span><span class="sxs-lookup"><span data-stu-id="87977-1148">4</span></span>                            | <span data-ttu-id="87977-1149">value4</span><span class="sxs-lookup"><span data-stu-id="87977-1149">value4</span></span>                       |
| <span data-ttu-id="87977-1150">5</span><span class="sxs-lookup"><span data-stu-id="87977-1150">5</span></span>                            | <span data-ttu-id="87977-1151">value5</span><span class="sxs-lookup"><span data-stu-id="87977-1151">value5</span></span>                       |

<span data-ttu-id="87977-1152">**Procesamiento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="87977-1152">**JSON array processing**</span></span>

<span data-ttu-id="87977-1153">Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero.</span><span class="sxs-lookup"><span data-stu-id="87977-1153">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="87977-1154">En el siguiente archivo de configuración, `subsection` es una matriz:</span><span class="sxs-lookup"><span data-stu-id="87977-1154">In the following configuration file, `subsection` is an array:</span></span>

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

<span data-ttu-id="87977-1155">El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="87977-1155">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="87977-1156">Key</span><span class="sxs-lookup"><span data-stu-id="87977-1156">Key</span></span>                     | <span data-ttu-id="87977-1157">Valor</span><span class="sxs-lookup"><span data-stu-id="87977-1157">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="87977-1158">json_array:key</span><span class="sxs-lookup"><span data-stu-id="87977-1158">json_array:key</span></span>          | <span data-ttu-id="87977-1159">valueA</span><span class="sxs-lookup"><span data-stu-id="87977-1159">valueA</span></span> |
| <span data-ttu-id="87977-1160">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="87977-1160">json_array:subsection:0</span></span> | <span data-ttu-id="87977-1161">valueB</span><span class="sxs-lookup"><span data-stu-id="87977-1161">valueB</span></span> |
| <span data-ttu-id="87977-1162">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="87977-1162">json_array:subsection:1</span></span> | <span data-ttu-id="87977-1163">valueC</span><span class="sxs-lookup"><span data-stu-id="87977-1163">valueC</span></span> |
| <span data-ttu-id="87977-1164">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="87977-1164">json_array:subsection:2</span></span> | <span data-ttu-id="87977-1165">valueD</span><span class="sxs-lookup"><span data-stu-id="87977-1165">valueD</span></span> |

<span data-ttu-id="87977-1166">En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="87977-1166">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

<span data-ttu-id="87977-1167">Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="87977-1167">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="87977-1168">Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="87977-1168">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="87977-1169">Índice de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="87977-1169">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="87977-1170">Valor de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="87977-1170">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="87977-1171">0</span><span class="sxs-lookup"><span data-stu-id="87977-1171">0</span></span>                                   | <span data-ttu-id="87977-1172">valueB</span><span class="sxs-lookup"><span data-stu-id="87977-1172">valueB</span></span>                              |
| <span data-ttu-id="87977-1173">1</span><span class="sxs-lookup"><span data-stu-id="87977-1173">1</span></span>                                   | <span data-ttu-id="87977-1174">valueC</span><span class="sxs-lookup"><span data-stu-id="87977-1174">valueC</span></span>                              |
| <span data-ttu-id="87977-1175">2</span><span class="sxs-lookup"><span data-stu-id="87977-1175">2</span></span>                                   | <span data-ttu-id="87977-1176">valueD</span><span class="sxs-lookup"><span data-stu-id="87977-1176">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="87977-1177">Proveedores de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="87977-1177">Custom configuration provider</span></span>

<span data-ttu-id="87977-1178">La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="87977-1178">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="87977-1179">El proveedor tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="87977-1179">The provider has the following characteristics:</span></span>

* <span data-ttu-id="87977-1180">La base de datos en memoria de EF se usa para fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="87977-1180">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="87977-1181">Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="87977-1181">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="87977-1182">El proveedor lee una tabla de base de datos en la configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="87977-1182">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="87977-1183">El proveedor no consulta la base de datos por clave.</span><span class="sxs-lookup"><span data-stu-id="87977-1183">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="87977-1184">La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="87977-1184">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="87977-1185">Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="87977-1185">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

<span data-ttu-id="87977-1186">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-1186">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="87977-1187">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="87977-1187">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="87977-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-1188">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="87977-1189">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="87977-1189">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="87977-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-1190">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="87977-1191">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="87977-1191">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="87977-1192">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="87977-1192">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="87977-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-1193">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="87977-1194">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="87977-1194">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="87977-1195">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-1195">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="87977-1196">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="87977-1196">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

## <a name="access-configuration-during-startup"></a><span data-ttu-id="87977-1197">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="87977-1197">Access configuration during startup</span></span>

<span data-ttu-id="87977-1198">Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="87977-1198">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="87977-1199">Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:</span><span class="sxs-lookup"><span data-stu-id="87977-1199">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="87977-1200">Para ver un ejemplo de cómo acceder a la configuración mediante métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods)</span><span class="sxs-lookup"><span data-stu-id="87977-1200">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="87977-1201">Acceso a la configuración en una página de Razor Pages o en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="87977-1201">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="87977-1202">Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.</span><span class="sxs-lookup"><span data-stu-id="87977-1202">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="87977-1203">En una página de las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="87977-1203">In a Razor Pages page:</span></span>

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

<span data-ttu-id="87977-1204">En una vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="87977-1204">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="87977-1205">Agregar configuración a partir de un ensamblado externo</span><span class="sxs-lookup"><span data-stu-id="87977-1205">Add configuration from an external assembly</span></span>

<span data-ttu-id="87977-1206">Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="87977-1206">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="87977-1207">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="87977-1207">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87977-1208">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="87977-1208">Additional resources</span></span>

* <xref:fundamentals/configuration/options>

::: moniker-end
