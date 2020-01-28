---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: 141ae5cda7672159032013cbda1ef4bfa7c142dd
ms.sourcegitcommit: eca76bd065eb94386165a0269f1e95092f23fa58
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726974"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="a8936-103">Configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8936-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="a8936-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a8936-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a8936-105">La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*.</span><span class="sxs-lookup"><span data-stu-id="a8936-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="a8936-106">Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="a8936-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a8936-107">Azure Key Vault</span></span>
* <span data-ttu-id="a8936-108">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="a8936-108">Azure App Configuration</span></span>
* <span data-ttu-id="a8936-109">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="a8936-109">Command-line arguments</span></span>
* <span data-ttu-id="a8936-110">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="a8936-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="a8936-111">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="a8936-111">Directory files</span></span>
* <span data-ttu-id="a8936-112">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="a8936-112">Environment variables</span></span>
* <span data-ttu-id="a8936-113">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="a8936-113">In-memory .NET objects</span></span>
* <span data-ttu-id="a8936-114">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="a8936-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8936-115">Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen implícitamente en el marco.</span><span class="sxs-lookup"><span data-stu-id="a8936-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8936-116">Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="a8936-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="a8936-117">Los ejemplos de código que siguen y en la aplicación de ejemplo utilizan el espacio de nombres <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="a8936-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="a8936-118">El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="a8936-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="a8936-119">Las opciones usan clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a8936-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="a8936-120">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a8936-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="a8936-121">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8936-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="a8936-122">Configuración de host y de aplicación</span><span class="sxs-lookup"><span data-stu-id="a8936-122">Host versus app configuration</span></span>

<span data-ttu-id="a8936-123">Antes de configurar e iniciar la aplicación, se configura e inicia un *host*.</span><span class="sxs-lookup"><span data-stu-id="a8936-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="a8936-124">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="a8936-125">Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="a8936-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="a8936-126">Los pares clave-valor de configuración del host también se incluyen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="a8936-127">Para más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="a8936-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="a8936-128">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="a8936-128">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8936-129">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="a8936-129">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="a8936-130">`CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="a8936-130">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="a8936-131">El contenido siguiente es válido para las aplicaciones que usen el [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a8936-131">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="a8936-132">Para obtener más información sobre la configuración predeterminada al usar el [host de web](xref:fundamentals/host/web-host), vea la [versión de este tema para ASP.NET Core 2.2](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="a8936-132">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="a8936-133">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="a8936-133">Host configuration is provided from:</span></span>
  * <span data-ttu-id="a8936-134">Variables de entorno prefijadas con `DOTNET_` (por ejemplo, `DOTNET_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-134">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="a8936-135">El prefijo (`DOTNET_`) se quita cuando se cargan los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-135">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="a8936-136">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-136">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="a8936-137">La configuración predeterminada del host de web se ha establecido (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="a8936-137">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="a8936-138">Kestrel se usa como el servidor web y se ha configurado mediante los proveedores de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-138">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="a8936-139">Agregar el middleware de filtrado de host</span><span class="sxs-lookup"><span data-stu-id="a8936-139">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="a8936-140">Agregue el middleware de encabezados reenviados si la variable de entorno `ASPNETCORE_FORWARDEDHEADERS_ENABLED` se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="a8936-140">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="a8936-141">Habilite la integración con IIS.</span><span class="sxs-lookup"><span data-stu-id="a8936-141">Enable IIS integration.</span></span>
* <span data-ttu-id="a8936-142">La configuración de la aplicación la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="a8936-142">App configuration is provided from:</span></span>
  * <span data-ttu-id="a8936-143">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-143">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a8936-144">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-144">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a8936-145">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="a8936-145">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a8936-146">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-146">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="a8936-147">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-147">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8936-148">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="a8936-148">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="a8936-149">`CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="a8936-149">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="a8936-150">El contenido siguiente es válido para las aplicaciones que usen el [host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="a8936-150">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="a8936-151">Para obtener más información sobre la configuración predeterminada al usar el [host genérico](xref:fundamentals/host/generic-host), vea la [versión más reciente de ese tema](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="a8936-151">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="a8936-152">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="a8936-152">Host configuration is provided from:</span></span>
  * <span data-ttu-id="a8936-153">Variables de entorno prefijadas con `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-153">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="a8936-154">El prefijo (`ASPNETCORE_`) se quita cuando se cargan los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-154">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="a8936-155">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="a8936-156">La configuración de la aplicación la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="a8936-156">App configuration is provided from:</span></span>
  * <span data-ttu-id="a8936-157">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-157">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a8936-158">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-158">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="a8936-159">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="a8936-159">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="a8936-160">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-160">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="a8936-161">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="a8936-161">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="a8936-162">Seguridad</span><span class="sxs-lookup"><span data-stu-id="a8936-162">Security</span></span>

<span data-ttu-id="a8936-163">Adopte los procedimientos siguientes para proteger los datos de configuración confidenciales:</span><span class="sxs-lookup"><span data-stu-id="a8936-163">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="a8936-164">Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="a8936-164">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="a8936-165">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="a8936-165">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="a8936-166">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="a8936-166">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="a8936-167">Para obtener más información, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="a8936-167">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="a8936-168"><xref:security/app-secrets> &ndash; Se incluyen recomendaciones sobre el uso de variables de entorno para almacenar información confidencial.</span><span class="sxs-lookup"><span data-stu-id="a8936-168"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="a8936-169">El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local.</span><span class="sxs-lookup"><span data-stu-id="a8936-169">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="a8936-170">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="a8936-170">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="a8936-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) almacena de forma segura secretos de aplicación para aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8936-171">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="a8936-172">Para obtener más información, vea <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a8936-172">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="a8936-173">Datos de configuración jerárquica</span><span class="sxs-lookup"><span data-stu-id="a8936-173">Hierarchical configuration data</span></span>

<span data-ttu-id="a8936-174">La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-174">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="a8936-175">En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:</span><span class="sxs-lookup"><span data-stu-id="a8936-175">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="a8936-176">Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-176">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="a8936-177">Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:</span><span class="sxs-lookup"><span data-stu-id="a8936-177">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="a8936-178">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-178">section0:key0</span></span>
* <span data-ttu-id="a8936-179">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-179">section0:key1</span></span>
* <span data-ttu-id="a8936-180">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-180">section1:key0</span></span>
* <span data-ttu-id="a8936-181">section1:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-181">section1:key1</span></span>

<span data-ttu-id="a8936-182">Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-182"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="a8936-183">Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="a8936-183">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="a8936-184">Convenciones</span><span class="sxs-lookup"><span data-stu-id="a8936-184">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="a8936-185">Orígenes y proveedores</span><span class="sxs-lookup"><span data-stu-id="a8936-185">Sources and providers</span></span>

<span data-ttu-id="a8936-186">Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-186">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="a8936-187">Los proveedores de configuración que implementan la detección de cambios tienen la capacidad de volver a cargar la configuración cuando se cambia una configuración subyacente.</span><span class="sxs-lookup"><span data-stu-id="a8936-187">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="a8936-188">Por ejemplo, el proveedor de configuración de archivos (descrito más adelante en este tema) y el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) implementan la detección de cambios.</span><span class="sxs-lookup"><span data-stu-id="a8936-188">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="a8936-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-189"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="a8936-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages o <xref:Microsoft.AspNetCore.Mvc.Controller> de MVC para obtener la configuración de la clase.</span><span class="sxs-lookup"><span data-stu-id="a8936-190"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="a8936-191">En los ejemplos siguientes, se usa el campo `_config` para tener acceso a los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-191">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="a8936-192">Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.</span><span class="sxs-lookup"><span data-stu-id="a8936-192">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="a8936-193">Teclas</span><span class="sxs-lookup"><span data-stu-id="a8936-193">Keys</span></span>

<span data-ttu-id="a8936-194">Las claves de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="a8936-194">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="a8936-195">Las claves no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="a8936-195">Keys are case-insensitive.</span></span> <span data-ttu-id="a8936-196">Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.</span><span class="sxs-lookup"><span data-stu-id="a8936-196">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="a8936-197">Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.</span><span class="sxs-lookup"><span data-stu-id="a8936-197">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="a8936-198">Claves jerárquicas</span><span class="sxs-lookup"><span data-stu-id="a8936-198">Hierarchical keys</span></span>
  * <span data-ttu-id="a8936-199">Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="a8936-199">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="a8936-200">En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="a8936-200">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="a8936-201">Todas las plataformas admiten un carácter de subrayado doble (`__`), que se convierte automáticamente en un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="a8936-201">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="a8936-202">En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="a8936-202">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="a8936-203">Escriba código para reemplazar los guiones por dos puntos cuando los secretos se carguen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-203">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="a8936-204"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-204">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="a8936-205">El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="a8936-205">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="a8936-206">Valores</span><span class="sxs-lookup"><span data-stu-id="a8936-206">Values</span></span>

<span data-ttu-id="a8936-207">Los valores de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="a8936-207">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="a8936-208">Los valores son cadenas.</span><span class="sxs-lookup"><span data-stu-id="a8936-208">Values are strings.</span></span>
* <span data-ttu-id="a8936-209">Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.</span><span class="sxs-lookup"><span data-stu-id="a8936-209">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="a8936-210">Proveedores</span><span class="sxs-lookup"><span data-stu-id="a8936-210">Providers</span></span>

<span data-ttu-id="a8936-211">La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a8936-211">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="a8936-212">Proveedor</span><span class="sxs-lookup"><span data-stu-id="a8936-212">Provider</span></span> | <span data-ttu-id="a8936-213">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="a8936-213">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="a8936-214">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="a8936-214">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="a8936-215">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a8936-215">Azure Key Vault</span></span> |
| <span data-ttu-id="a8936-216">[Proveedor de Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentación de Azure)</span><span class="sxs-lookup"><span data-stu-id="a8936-216">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="a8936-217">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="a8936-217">Azure App Configuration</span></span> |
| [<span data-ttu-id="a8936-218">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="a8936-218">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="a8936-219">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="a8936-219">Command-line parameters</span></span> |
| [<span data-ttu-id="a8936-220">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="a8936-220">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="a8936-221">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="a8936-221">Custom source</span></span> |
| [<span data-ttu-id="a8936-222">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="a8936-222">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="a8936-223">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="a8936-223">Environment variables</span></span> |
| [<span data-ttu-id="a8936-224">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="a8936-224">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="a8936-225">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="a8936-225">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="a8936-226">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="a8936-226">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="a8936-227">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="a8936-227">Directory files</span></span> |
| [<span data-ttu-id="a8936-228">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="a8936-228">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="a8936-229">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="a8936-229">In-memory collections</span></span> |
| <span data-ttu-id="a8936-230">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="a8936-230">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="a8936-231">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="a8936-231">File in the user profile directory</span></span> |

<span data-ttu-id="a8936-232">Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="a8936-232">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="a8936-233">En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en el que el código los organiza.</span><span class="sxs-lookup"><span data-stu-id="a8936-233">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="a8936-234">Ordene los proveedores de configuración en el código para cumplir con las prioridades relacionadas con los orígenes de configuración subyacentes que la aplicación necesita.</span><span class="sxs-lookup"><span data-stu-id="a8936-234">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="a8936-235">Esta es una secuencia típica de proveedores de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-235">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="a8936-236">Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)</span><span class="sxs-lookup"><span data-stu-id="a8936-236">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="a8936-237">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a8936-237">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="a8936-238">[Secretos de usuario (administrador de secretos)](xref:security/app-secrets) (solo para entornos de desarrollo)</span><span class="sxs-lookup"><span data-stu-id="a8936-238">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="a8936-239">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="a8936-239">Environment variables</span></span>
1. <span data-ttu-id="a8936-240">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="a8936-240">Command-line arguments</span></span>

<span data-ttu-id="a8936-241">Una práctica común es colocar el proveedor de configuración de línea de comandos al final en una serie de proveedores para permitir que los argumentos de la línea de comandos reemplacen la configuración establecida por el resto de los proveedores.</span><span class="sxs-lookup"><span data-stu-id="a8936-241">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="a8936-242">La secuencia de proveedores anterior se usa cuando se inicializa un nuevo generador de host con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8936-242">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a8936-243">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8936-243">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="a8936-244">Configurar el generador de host con ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="a8936-244">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="a8936-245">Para configurar el generador de host, realice una llamada a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> y proporcione la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-245">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="a8936-246">`ConfigureHostConfiguration` se usa para inicializar <xref:Microsoft.Extensions.Hosting.IHostEnvironment> para su uso posterior en el proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="a8936-246">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="a8936-247">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="a8936-247">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="a8936-248">Configurar el generador de host con UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="a8936-248">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="a8936-249">Para configurar el generador de host, realice una llamada a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> en el generador de host con la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-249">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

::: moniker-end

## <a name="configureappconfiguration"></a><span data-ttu-id="a8936-250">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="a8936-250">ConfigureAppConfiguration</span></span>

<span data-ttu-id="a8936-251">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que `CreateDefaultBuilder` agrega automáticamente:</span><span class="sxs-lookup"><span data-stu-id="a8936-251">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="a8936-252">Reemplazar la configuración anterior con argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="a8936-252">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="a8936-253">Para proporcionar configuración de aplicaciones que se pueda reemplazar por argumentos de la línea de comandos, llame a `AddCommandLine` en último lugar:</span><span class="sxs-lookup"><span data-stu-id="a8936-253">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="a8936-254">Quitar proveedores agregados por CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="a8936-254">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="a8936-255">Para quitar los proveedores agregados por `CreateDefaultBuilder`, llame primero a [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) en [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="a8936-255">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="a8936-256">Usar la configuración durante el inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="a8936-256">Consume configuration during app startup</span></span>

<span data-ttu-id="a8936-257">La configuración proporcionada a la aplicación en `ConfigureAppConfiguration` está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8936-257">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8936-258">Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="a8936-258">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="a8936-259">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="a8936-259">Command-line Configuration Provider</span></span>

<span data-ttu-id="a8936-260"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a8936-260">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="a8936-261">Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8936-261">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a8936-262">`AddCommandLine` se llama automáticamente al llamar a `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="a8936-262">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="a8936-263">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8936-263">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="a8936-264">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="a8936-264">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a8936-265">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-265">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="a8936-266">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a8936-266">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="a8936-267">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a8936-267">Environment variables.</span></span>

<span data-ttu-id="a8936-268">`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final.</span><span class="sxs-lookup"><span data-stu-id="a8936-268">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="a8936-269">Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="a8936-269">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="a8936-270">`CreateDefaultBuilder` actúa cuando se construye el host.</span><span class="sxs-lookup"><span data-stu-id="a8936-270">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="a8936-271">Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.</span><span class="sxs-lookup"><span data-stu-id="a8936-271">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="a8936-272">Para las aplicaciones basadas en plantillas de ASP.NET Core, `CreateDefaultBuilder` ya realiza una llamada a `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="a8936-272">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a8936-273">Para agregar proveedores de configuración adicionales y mantener la capacidad de reemplazar la configuración de sus proveedores con argumentos de la línea de comandos, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddCommandLine` en último lugar.</span><span class="sxs-lookup"><span data-stu-id="a8936-273">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="a8936-274">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="a8936-274">**Example**</span></span>

<span data-ttu-id="a8936-275">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="a8936-275">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="a8936-276">Abra un símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a8936-276">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="a8936-277">Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="a8936-277">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="a8936-278">Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a8936-278">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a8936-279">Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="a8936-279">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="a8936-280">Argumentos</span><span class="sxs-lookup"><span data-stu-id="a8936-280">Arguments</span></span>

<span data-ttu-id="a8936-281">El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio.</span><span class="sxs-lookup"><span data-stu-id="a8936-281">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="a8936-282">El valor no es obligatorio si se usa un signo igual (por ejemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="a8936-282">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="a8936-283">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="a8936-283">Key prefix</span></span>               | <span data-ttu-id="a8936-284">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="a8936-284">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="a8936-285">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="a8936-285">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="a8936-286">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="a8936-286">Two dashes (`--`)</span></span>        | <span data-ttu-id="a8936-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="a8936-287">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="a8936-288">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="a8936-288">Forward slash (`/`)</span></span>      | <span data-ttu-id="a8936-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="a8936-289">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="a8936-290">Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.</span><span class="sxs-lookup"><span data-stu-id="a8936-290">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="a8936-291">Comandos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a8936-291">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="a8936-292">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="a8936-292">Switch mappings</span></span>

<span data-ttu-id="a8936-293">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="a8936-293">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="a8936-294">Al crear manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, proporcione un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="a8936-294">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="a8936-295">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a8936-295">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="a8936-296">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-296">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="a8936-297">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="a8936-297">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="a8936-298">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="a8936-298">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="a8936-299">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="a8936-299">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="a8936-300">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="a8936-300">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="a8936-301">Cree un diccionario de asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="a8936-301">Create a switch mappings dictionary.</span></span> <span data-ttu-id="a8936-302">En el ejemplo siguiente, se crean dos asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="a8936-302">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="a8936-303">Al compilar el host, llame a `AddCommandLine` con el diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="a8936-303">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="a8936-304">En el caso de las aplicaciones que usen las asignaciones de modificador, la llamada a `CreateDefaultBuilder` no tiene que pasar argumentos.</span><span class="sxs-lookup"><span data-stu-id="a8936-304">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="a8936-305">En la llamada de `AddCommandLine` del método `CreateDefaultBuilder`, no se incluyen modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8936-305">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a8936-306">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="a8936-306">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="a8936-307">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="a8936-307">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="a8936-308">Key</span><span class="sxs-lookup"><span data-stu-id="a8936-308">Key</span></span>       | <span data-ttu-id="a8936-309">Valor</span><span class="sxs-lookup"><span data-stu-id="a8936-309">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="a8936-310">Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:</span><span class="sxs-lookup"><span data-stu-id="a8936-310">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="a8936-311">Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="a8936-311">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="a8936-312">Key</span><span class="sxs-lookup"><span data-stu-id="a8936-312">Key</span></span>               | <span data-ttu-id="a8936-313">Valor</span><span class="sxs-lookup"><span data-stu-id="a8936-313">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="a8936-314">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="a8936-314">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="a8936-315"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a8936-315">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="a8936-316">Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8936-316">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="a8936-317">[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a8936-317">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="a8936-318">Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="a8936-318">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8936-319">`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `DOTNET_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host genérico](xref:fundamentals/host/generic-host) y al llamar a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8936-319">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="a8936-320">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8936-320">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8936-321">`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `ASPNETCORE_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host de web](xref:fundamentals/host/web-host) y al llamar a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8936-321">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="a8936-322">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8936-322">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="a8936-323">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="a8936-323">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a8936-324">Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.</span><span class="sxs-lookup"><span data-stu-id="a8936-324">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="a8936-325">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-325">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="a8936-326">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a8936-326">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="a8936-327">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a8936-327">Command-line arguments.</span></span>

<span data-ttu-id="a8936-328">El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="a8936-328">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="a8936-329">Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="a8936-329">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="a8936-330">Para proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddEnvironmentVariables` con el prefijo:</span><span class="sxs-lookup"><span data-stu-id="a8936-330">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="a8936-331">Llame a `AddEnvironmentVariables` en último lugar para permitir que las variables de entorno con el prefijo especificado invaliden los valores de otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="a8936-331">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="a8936-332">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="a8936-332">**Example**</span></span>

<span data-ttu-id="a8936-333">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="a8936-333">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="a8936-334">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8936-334">Run the sample app.</span></span> <span data-ttu-id="a8936-335">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a8936-335">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a8936-336">Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="a8936-336">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="a8936-337">El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.</span><span class="sxs-lookup"><span data-stu-id="a8936-337">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="a8936-338">Para mantener un número adecuado de variables de entorno en la lista representada por la aplicación, la aplicación filtrará las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a8936-338">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="a8936-339">Vea el archivo *Pages/Index.cshtml.cs* de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8936-339">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="a8936-340">Para exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a8936-340">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="a8936-341">Prefijos</span><span class="sxs-lookup"><span data-stu-id="a8936-341">Prefixes</span></span>

<span data-ttu-id="a8936-342">Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="a8936-342">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="a8936-343">Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-343">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="a8936-344">El prefijo se quita cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-344">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="a8936-345">Al crear el generador de host, las variables de entorno proporcionan la configuración del host.</span><span class="sxs-lookup"><span data-stu-id="a8936-345">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="a8936-346">Para obtener más información sobre el prefijo usado para estas variables de entorno, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8936-346">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="a8936-347">**Prefijos de cadena de conexión**</span><span class="sxs-lookup"><span data-stu-id="a8936-347">**Connection string prefixes**</span></span>

<span data-ttu-id="a8936-348">La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-348">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="a8936-349">Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="a8936-349">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="a8936-350">Prefijo de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="a8936-350">Connection string prefix</span></span> | <span data-ttu-id="a8936-351">Proveedor</span><span class="sxs-lookup"><span data-stu-id="a8936-351">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="a8936-352">Proveedor personalizado</span><span class="sxs-lookup"><span data-stu-id="a8936-352">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="a8936-353">MySQL</span><span class="sxs-lookup"><span data-stu-id="a8936-353">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="a8936-354">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="a8936-354">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="a8936-355">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a8936-355">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="a8936-356">Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:</span><span class="sxs-lookup"><span data-stu-id="a8936-356">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="a8936-357">La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="a8936-357">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="a8936-358">Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).</span><span class="sxs-lookup"><span data-stu-id="a8936-358">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="a8936-359">Clave de variable de entorno</span><span class="sxs-lookup"><span data-stu-id="a8936-359">Environment variable key</span></span> | <span data-ttu-id="a8936-360">Clave de configuración convertida</span><span class="sxs-lookup"><span data-stu-id="a8936-360">Converted configuration key</span></span> | <span data-ttu-id="a8936-361">Entrada de configuración del proveedor</span><span class="sxs-lookup"><span data-stu-id="a8936-361">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a8936-362">Entrada de configuración no creada.</span><span class="sxs-lookup"><span data-stu-id="a8936-362">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a8936-363">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a8936-363">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a8936-364">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="a8936-364">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a8936-365">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a8936-365">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a8936-366">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a8936-366">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="a8936-367">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="a8936-367">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="a8936-368">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a8936-368">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="a8936-369">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="a8936-369">File Configuration Provider</span></span>

<span data-ttu-id="a8936-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="a8936-370"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="a8936-371">Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:</span><span class="sxs-lookup"><span data-stu-id="a8936-371">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="a8936-372">Proveedor de configuración INI</span><span class="sxs-lookup"><span data-stu-id="a8936-372">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="a8936-373">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="a8936-373">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="a8936-374">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="a8936-374">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="a8936-375">Proveedor de configuración de INI</span><span class="sxs-lookup"><span data-stu-id="a8936-375">INI Configuration Provider</span></span>

<span data-ttu-id="a8936-376"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a8936-376">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="a8936-377">Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8936-377">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a8936-378">Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.</span><span class="sxs-lookup"><span data-stu-id="a8936-378">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="a8936-379">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="a8936-379">Overloads permit specifying:</span></span>

* <span data-ttu-id="a8936-380">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="a8936-380">Whether the file is optional.</span></span>
* <span data-ttu-id="a8936-381">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="a8936-381">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a8936-382"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="a8936-382">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a8936-383">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a8936-383">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="a8936-384">Un ejemplo genérico de un archivo de configuración INI:</span><span class="sxs-lookup"><span data-stu-id="a8936-384">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="a8936-385">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="a8936-385">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a8936-386">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-386">section0:key0</span></span>
* <span data-ttu-id="a8936-387">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-387">section0:key1</span></span>
* <span data-ttu-id="a8936-388">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="a8936-388">section1:subsection:key</span></span>
* <span data-ttu-id="a8936-389">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="a8936-389">section2:subsection0:key</span></span>
* <span data-ttu-id="a8936-390">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="a8936-390">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="a8936-391">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="a8936-391">JSON Configuration Provider</span></span>

<span data-ttu-id="a8936-392"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a8936-392">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="a8936-393">Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8936-393">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a8936-394">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="a8936-394">Overloads permit specifying:</span></span>

* <span data-ttu-id="a8936-395">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="a8936-395">Whether the file is optional.</span></span>
* <span data-ttu-id="a8936-396">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="a8936-396">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a8936-397"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="a8936-397">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a8936-398">Se llama automáticamente a `AddJsonFile` dos veces cuando un nuevo generador de host se inicializa con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8936-398">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a8936-399">Se llama al método para cargar la configuración desde:</span><span class="sxs-lookup"><span data-stu-id="a8936-399">The method is called to load configuration from:</span></span>

* <span data-ttu-id="a8936-400">*appsettings.json*: este archivo se lee primero.</span><span class="sxs-lookup"><span data-stu-id="a8936-400">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="a8936-401">La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-401">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="a8936-402">*appsettings.{Environment}.json*: la versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="a8936-402">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="a8936-403">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="a8936-403">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="a8936-404">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="a8936-404">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="a8936-405">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="a8936-405">Environment variables.</span></span>
* <span data-ttu-id="a8936-406">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a8936-406">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="a8936-407">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="a8936-407">Command-line arguments.</span></span>

<span data-ttu-id="a8936-408">El proveedor de configuración de JSON se establece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="a8936-408">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="a8936-409">Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="a8936-409">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="a8936-410">Llame a `ConfigureAppConfiguration` al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="a8936-410">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="a8936-411">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="a8936-411">**Example**</span></span>

<span data-ttu-id="a8936-412">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="a8936-412">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="a8936-413">La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8936-413">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="a8936-414">La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-414">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="a8936-415">Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:</span><span class="sxs-lookup"><span data-stu-id="a8936-415">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="a8936-416">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8936-416">Run the sample app.</span></span> <span data-ttu-id="a8936-417">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a8936-417">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a8936-418">La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-418">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="a8936-419">El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a8936-419">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="a8936-420">Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:</span><span class="sxs-lookup"><span data-stu-id="a8936-420">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="a8936-421">Abra el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-421">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="a8936-422">En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.</span><span class="sxs-lookup"><span data-stu-id="a8936-422">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="a8936-423">Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="a8936-423">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="a8936-424">La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-424">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="a8936-425">El nivel de registro de la clave `Logging:LogLevel:Default` es `Information`.</span><span class="sxs-lookup"><span data-stu-id="a8936-425">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="a8936-426">La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a8936-426">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="a8936-427">La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-427">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="a8936-428">Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:</span><span class="sxs-lookup"><span data-stu-id="a8936-428">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="a8936-429">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="a8936-429">Run the sample app.</span></span> <span data-ttu-id="a8936-430">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="a8936-430">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="a8936-431">La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-431">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="a8936-432">El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a8936-432">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="a8936-433">Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:</span><span class="sxs-lookup"><span data-stu-id="a8936-433">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="a8936-434">Abra el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-434">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="a8936-435">En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.</span><span class="sxs-lookup"><span data-stu-id="a8936-435">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="a8936-436">Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="a8936-436">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="a8936-437">La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a8936-437">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="a8936-438">El nivel de registro de la clave `Logging:LogLevel:Default` es `Warning`.</span><span class="sxs-lookup"><span data-stu-id="a8936-438">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

::: moniker-end

### <a name="xml-configuration-provider"></a><span data-ttu-id="a8936-439">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="a8936-439">XML Configuration Provider</span></span>

<span data-ttu-id="a8936-440"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="a8936-440">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="a8936-441">Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8936-441">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a8936-442">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="a8936-442">Overloads permit specifying:</span></span>

* <span data-ttu-id="a8936-443">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="a8936-443">Whether the file is optional.</span></span>
* <span data-ttu-id="a8936-444">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="a8936-444">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="a8936-445"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="a8936-445">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="a8936-446">El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-446">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="a8936-447">No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.</span><span class="sxs-lookup"><span data-stu-id="a8936-447">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="a8936-448">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a8936-448">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="a8936-449">Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:</span><span class="sxs-lookup"><span data-stu-id="a8936-449">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="a8936-450">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="a8936-450">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a8936-451">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-451">section0:key0</span></span>
* <span data-ttu-id="a8936-452">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-452">section0:key1</span></span>
* <span data-ttu-id="a8936-453">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-453">section1:key0</span></span>
* <span data-ttu-id="a8936-454">section1:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-454">section1:key1</span></span>

<span data-ttu-id="a8936-455">Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:</span><span class="sxs-lookup"><span data-stu-id="a8936-455">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="a8936-456">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="a8936-456">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a8936-457">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-457">section:section0:key:key0</span></span>
* <span data-ttu-id="a8936-458">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-458">section:section0:key:key1</span></span>
* <span data-ttu-id="a8936-459">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-459">section:section1:key:key0</span></span>
* <span data-ttu-id="a8936-460">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-460">section:section1:key:key1</span></span>

<span data-ttu-id="a8936-461">Los atributos se pueden usar para suministrar valores:</span><span class="sxs-lookup"><span data-stu-id="a8936-461">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="a8936-462">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="a8936-462">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="a8936-463">key:attribute</span><span class="sxs-lookup"><span data-stu-id="a8936-463">key:attribute</span></span>
* <span data-ttu-id="a8936-464">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="a8936-464">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="a8936-465">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="a8936-465">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="a8936-466"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-466">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="a8936-467">La clave es el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="a8936-467">The key is the file name.</span></span> <span data-ttu-id="a8936-468">El valor contiene el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="a8936-468">The value contains the file's contents.</span></span> <span data-ttu-id="a8936-469">El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.</span><span class="sxs-lookup"><span data-stu-id="a8936-469">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="a8936-470">Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8936-470">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="a8936-471">La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="a8936-471">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="a8936-472">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="a8936-472">Overloads permit specifying:</span></span>

* <span data-ttu-id="a8936-473">Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.</span><span class="sxs-lookup"><span data-stu-id="a8936-473">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="a8936-474">Si el directorio es opcional y la ruta de acceso al directorio.</span><span class="sxs-lookup"><span data-stu-id="a8936-474">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="a8936-475">El carácter de subrayado doble (`__`) se usa como delimitador de claves de configuración en los nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="a8936-475">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="a8936-476">Por ejemplo, el nombre de archivo `Logging__LogLevel__System` genera la clave de configuración `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="a8936-476">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="a8936-477">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="a8936-477">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="a8936-478">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="a8936-478">Memory Configuration Provider</span></span>

<span data-ttu-id="a8936-479"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-479">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="a8936-480">Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="a8936-480">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="a8936-481">El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="a8936-481">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="a8936-482">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-482">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="a8936-483">En el ejemplo siguiente, se crea un diccionario de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-483">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="a8936-484">El diccionario se usa con una llamada a `AddInMemoryCollection` para proporcionar la configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-484">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="a8936-485">GetValue</span><span class="sxs-lookup"><span data-stu-id="a8936-485">GetValue</span></span>

<span data-ttu-id="a8936-486">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un único valor de la configuración con una clave especificada y lo convierte al tipo especificado que no es de colección.</span><span class="sxs-lookup"><span data-stu-id="a8936-486">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="a8936-487">Una sobrecarga acepta un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="a8936-487">An overload accepts a default value.</span></span>

<span data-ttu-id="a8936-488">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a8936-488">The following example:</span></span>

* <span data-ttu-id="a8936-489">Se extrae el valor de cadena de la configuración con la clave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="a8936-489">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="a8936-490">Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.</span><span class="sxs-lookup"><span data-stu-id="a8936-490">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="a8936-491">Se escribe el valor como `int`.</span><span class="sxs-lookup"><span data-stu-id="a8936-491">Types the value as an `int`.</span></span>
* <span data-ttu-id="a8936-492">Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="a8936-492">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="a8936-493">GetSection, GetChildren y Exists</span><span class="sxs-lookup"><span data-stu-id="a8936-493">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="a8936-494">Para los ejemplos siguientes, considere el siguiente archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="a8936-494">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="a8936-495">Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:</span><span class="sxs-lookup"><span data-stu-id="a8936-495">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="a8936-496">Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-496">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="a8936-497">section0:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-497">section0:key0</span></span>
* <span data-ttu-id="a8936-498">section0:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-498">section0:key1</span></span>
* <span data-ttu-id="a8936-499">section1:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-499">section1:key0</span></span>
* <span data-ttu-id="a8936-500">section1:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-500">section1:key1</span></span>
* <span data-ttu-id="a8936-501">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-501">section2:subsection0:key0</span></span>
* <span data-ttu-id="a8936-502">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-502">section2:subsection0:key1</span></span>
* <span data-ttu-id="a8936-503">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="a8936-503">section2:subsection1:key0</span></span>
* <span data-ttu-id="a8936-504">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="a8936-504">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="a8936-505">GetSection</span><span class="sxs-lookup"><span data-stu-id="a8936-505">GetSection</span></span>

<span data-ttu-id="a8936-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.</span><span class="sxs-lookup"><span data-stu-id="a8936-506">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="a8936-507">Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:</span><span class="sxs-lookup"><span data-stu-id="a8936-507">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="a8936-508">`configSection` no tiene ningún valor, solo una clave y una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="a8936-508">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="a8936-509">De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:</span><span class="sxs-lookup"><span data-stu-id="a8936-509">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="a8936-510">`GetSection` nunca devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="a8936-510">`GetSection` never returns `null`.</span></span> <span data-ttu-id="a8936-511">Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.</span><span class="sxs-lookup"><span data-stu-id="a8936-511">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="a8936-512">Cuando `GetSection` devuelve una sección coincidente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> no se rellena.</span><span class="sxs-lookup"><span data-stu-id="a8936-512">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="a8936-513">Se devuelven <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> y <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> si la sección existe.</span><span class="sxs-lookup"><span data-stu-id="a8936-513">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="a8936-514">GetChildren</span><span class="sxs-lookup"><span data-stu-id="a8936-514">GetChildren</span></span>

<span data-ttu-id="a8936-515">Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:</span><span class="sxs-lookup"><span data-stu-id="a8936-515">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="a8936-516">Existe</span><span class="sxs-lookup"><span data-stu-id="a8936-516">Exists</span></span>

<span data-ttu-id="a8936-517">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-517">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="a8936-518">Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-518">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="a8936-519">Enlace a una clase</span><span class="sxs-lookup"><span data-stu-id="a8936-519">Bind to a class</span></span>

<span data-ttu-id="a8936-520">La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="a8936-520">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="a8936-521">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="a8936-521">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="a8936-522">Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="a8936-522">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="a8936-523">El enlazador enlaza los valores con todas las propiedades de lectura o escritura públicas del tipo proporcionado.</span><span class="sxs-lookup"><span data-stu-id="a8936-523">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="a8936-524">Los campos **no** se enlazan.</span><span class="sxs-lookup"><span data-stu-id="a8936-524">Fields are **not** bound.</span></span>

<span data-ttu-id="a8936-525">La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8936-525">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a8936-526">La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-526">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="a8936-527">Se crean los siguientes pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-527">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="a8936-528">Key</span><span class="sxs-lookup"><span data-stu-id="a8936-528">Key</span></span>                   | <span data-ttu-id="a8936-529">Valor</span><span class="sxs-lookup"><span data-stu-id="a8936-529">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="a8936-530">starship:name</span><span class="sxs-lookup"><span data-stu-id="a8936-530">starship:name</span></span>         | <span data-ttu-id="a8936-531">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="a8936-531">USS Enterprise</span></span>                                    |
| <span data-ttu-id="a8936-532">starship:registry</span><span class="sxs-lookup"><span data-stu-id="a8936-532">starship:registry</span></span>     | <span data-ttu-id="a8936-533">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="a8936-533">NCC-1701</span></span>                                          |
| <span data-ttu-id="a8936-534">starship:class</span><span class="sxs-lookup"><span data-stu-id="a8936-534">starship:class</span></span>        | <span data-ttu-id="a8936-535">Constitution</span><span class="sxs-lookup"><span data-stu-id="a8936-535">Constitution</span></span>                                      |
| <span data-ttu-id="a8936-536">starship:length</span><span class="sxs-lookup"><span data-stu-id="a8936-536">starship:length</span></span>       | <span data-ttu-id="a8936-537">304.8</span><span class="sxs-lookup"><span data-stu-id="a8936-537">304.8</span></span>                                             |
| <span data-ttu-id="a8936-538">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="a8936-538">starship:commissioned</span></span> | <span data-ttu-id="a8936-539">False</span><span class="sxs-lookup"><span data-stu-id="a8936-539">False</span></span>                                             |
| <span data-ttu-id="a8936-540">trademark</span><span class="sxs-lookup"><span data-stu-id="a8936-540">trademark</span></span>             | <span data-ttu-id="a8936-541">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="a8936-541">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="a8936-542">La aplicación de ejemplo llama a `GetSection` con la clave `starship`.</span><span class="sxs-lookup"><span data-stu-id="a8936-542">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="a8936-543">Los pares clave-valor `starship` están aislados.</span><span class="sxs-lookup"><span data-stu-id="a8936-543">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="a8936-544">El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`.</span><span class="sxs-lookup"><span data-stu-id="a8936-544">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="a8936-545">Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="a8936-545">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="a8936-546">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="a8936-546">Bind to an object graph</span></span>

<span data-ttu-id="a8936-547"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO.</span><span class="sxs-lookup"><span data-stu-id="a8936-547"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="a8936-548">Al igual que ocurre con el enlace de un objeto simple, solo se enlazan las propiedades públicas de lectura o escritura.</span><span class="sxs-lookup"><span data-stu-id="a8936-548">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="a8936-549">El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="a8936-549">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a8936-550">La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-550">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="a8936-551">Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="a8936-551">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="a8936-552">La instancia enlazada se asigna a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="a8936-552">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="a8936-553">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="a8936-553">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="a8936-554">`Get<T>` es más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="a8936-554">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="a8936-555">El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:</span><span class="sxs-lookup"><span data-stu-id="a8936-555">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="a8936-556">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="a8936-556">Bind an array to a class</span></span>

<span data-ttu-id="a8936-557">*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*</span><span class="sxs-lookup"><span data-stu-id="a8936-557">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="a8936-558"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-558">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="a8936-559">Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.</span><span class="sxs-lookup"><span data-stu-id="a8936-559">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="a8936-560">El enlace se proporciona por convención.</span><span class="sxs-lookup"><span data-stu-id="a8936-560">Binding is provided by convention.</span></span> <span data-ttu-id="a8936-561">Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.</span><span class="sxs-lookup"><span data-stu-id="a8936-561">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="a8936-562">**Procesamiento de matriz en memoria**</span><span class="sxs-lookup"><span data-stu-id="a8936-562">**In-memory array processing**</span></span>

<span data-ttu-id="a8936-563">Considere los valores y las claves de configuración que se muestran en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="a8936-563">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="a8936-564">Key</span><span class="sxs-lookup"><span data-stu-id="a8936-564">Key</span></span>             | <span data-ttu-id="a8936-565">Valor</span><span class="sxs-lookup"><span data-stu-id="a8936-565">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="a8936-566">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="a8936-566">array:entries:0</span></span> | <span data-ttu-id="a8936-567">value0</span><span class="sxs-lookup"><span data-stu-id="a8936-567">value0</span></span> |
| <span data-ttu-id="a8936-568">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="a8936-568">array:entries:1</span></span> | <span data-ttu-id="a8936-569">value1</span><span class="sxs-lookup"><span data-stu-id="a8936-569">value1</span></span> |
| <span data-ttu-id="a8936-570">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="a8936-570">array:entries:2</span></span> | <span data-ttu-id="a8936-571">value2</span><span class="sxs-lookup"><span data-stu-id="a8936-571">value2</span></span> |
| <span data-ttu-id="a8936-572">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="a8936-572">array:entries:4</span></span> | <span data-ttu-id="a8936-573">value4</span><span class="sxs-lookup"><span data-stu-id="a8936-573">value4</span></span> |
| <span data-ttu-id="a8936-574">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="a8936-574">array:entries:5</span></span> | <span data-ttu-id="a8936-575">value5</span><span class="sxs-lookup"><span data-stu-id="a8936-575">value5</span></span> |

<span data-ttu-id="a8936-576">Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:</span><span class="sxs-lookup"><span data-stu-id="a8936-576">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="a8936-577">La matriz omite un valor de índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="a8936-577">The array skips a value for index &num;3.</span></span> <span data-ttu-id="a8936-578">El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.</span><span class="sxs-lookup"><span data-stu-id="a8936-578">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="a8936-579">En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:</span><span class="sxs-lookup"><span data-stu-id="a8936-579">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a8936-580">Los datos de configuración están enlazados al objeto:</span><span class="sxs-lookup"><span data-stu-id="a8936-580">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="a8936-581">También se puede usar la sintaxis de [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:</span><span class="sxs-lookup"><span data-stu-id="a8936-581">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="a8936-582">El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-582">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="a8936-583">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="a8936-583">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="a8936-584">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="a8936-584">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="a8936-585">0</span><span class="sxs-lookup"><span data-stu-id="a8936-585">0</span></span>                            | <span data-ttu-id="a8936-586">value0</span><span class="sxs-lookup"><span data-stu-id="a8936-586">value0</span></span>                       |
| <span data-ttu-id="a8936-587">1</span><span class="sxs-lookup"><span data-stu-id="a8936-587">1</span></span>                            | <span data-ttu-id="a8936-588">value1</span><span class="sxs-lookup"><span data-stu-id="a8936-588">value1</span></span>                       |
| <span data-ttu-id="a8936-589">2</span><span class="sxs-lookup"><span data-stu-id="a8936-589">2</span></span>                            | <span data-ttu-id="a8936-590">value2</span><span class="sxs-lookup"><span data-stu-id="a8936-590">value2</span></span>                       |
| <span data-ttu-id="a8936-591">3</span><span class="sxs-lookup"><span data-stu-id="a8936-591">3</span></span>                            | <span data-ttu-id="a8936-592">value4</span><span class="sxs-lookup"><span data-stu-id="a8936-592">value4</span></span>                       |
| <span data-ttu-id="a8936-593">4</span><span class="sxs-lookup"><span data-stu-id="a8936-593">4</span></span>                            | <span data-ttu-id="a8936-594">value5</span><span class="sxs-lookup"><span data-stu-id="a8936-594">value5</span></span>                       |

<span data-ttu-id="a8936-595">El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="a8936-595">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="a8936-596">Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto.</span><span class="sxs-lookup"><span data-stu-id="a8936-596">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="a8936-597">No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.</span><span class="sxs-lookup"><span data-stu-id="a8936-597">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="a8936-598">El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExample` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-598">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="a8936-599">Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExample.Entries` coincide con la matriz de configuración completa:</span><span class="sxs-lookup"><span data-stu-id="a8936-599">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="a8936-600">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="a8936-600">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="a8936-601">En `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="a8936-601">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="a8936-602">El par clave-valor que se muestra en la tabla se carga en la configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-602">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="a8936-603">Key</span><span class="sxs-lookup"><span data-stu-id="a8936-603">Key</span></span>             | <span data-ttu-id="a8936-604">Valor</span><span class="sxs-lookup"><span data-stu-id="a8936-604">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="a8936-605">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="a8936-605">array:entries:3</span></span> | <span data-ttu-id="a8936-606">value3</span><span class="sxs-lookup"><span data-stu-id="a8936-606">value3</span></span> |

<span data-ttu-id="a8936-607">Si la instancia de clase `ArrayExample` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExample.Entries` incluye el valor.</span><span class="sxs-lookup"><span data-stu-id="a8936-607">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="a8936-608">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="a8936-608">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="a8936-609">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="a8936-609">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="a8936-610">0</span><span class="sxs-lookup"><span data-stu-id="a8936-610">0</span></span>                            | <span data-ttu-id="a8936-611">value0</span><span class="sxs-lookup"><span data-stu-id="a8936-611">value0</span></span>                       |
| <span data-ttu-id="a8936-612">1</span><span class="sxs-lookup"><span data-stu-id="a8936-612">1</span></span>                            | <span data-ttu-id="a8936-613">value1</span><span class="sxs-lookup"><span data-stu-id="a8936-613">value1</span></span>                       |
| <span data-ttu-id="a8936-614">2</span><span class="sxs-lookup"><span data-stu-id="a8936-614">2</span></span>                            | <span data-ttu-id="a8936-615">value2</span><span class="sxs-lookup"><span data-stu-id="a8936-615">value2</span></span>                       |
| <span data-ttu-id="a8936-616">3</span><span class="sxs-lookup"><span data-stu-id="a8936-616">3</span></span>                            | <span data-ttu-id="a8936-617">value3</span><span class="sxs-lookup"><span data-stu-id="a8936-617">value3</span></span>                       |
| <span data-ttu-id="a8936-618">4</span><span class="sxs-lookup"><span data-stu-id="a8936-618">4</span></span>                            | <span data-ttu-id="a8936-619">value4</span><span class="sxs-lookup"><span data-stu-id="a8936-619">value4</span></span>                       |
| <span data-ttu-id="a8936-620">5</span><span class="sxs-lookup"><span data-stu-id="a8936-620">5</span></span>                            | <span data-ttu-id="a8936-621">value5</span><span class="sxs-lookup"><span data-stu-id="a8936-621">value5</span></span>                       |

<span data-ttu-id="a8936-622">**Procesamiento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="a8936-622">**JSON array processing**</span></span>

<span data-ttu-id="a8936-623">Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero.</span><span class="sxs-lookup"><span data-stu-id="a8936-623">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="a8936-624">En el siguiente archivo de configuración, `subsection` es una matriz:</span><span class="sxs-lookup"><span data-stu-id="a8936-624">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="a8936-625">El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="a8936-625">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="a8936-626">Key</span><span class="sxs-lookup"><span data-stu-id="a8936-626">Key</span></span>                     | <span data-ttu-id="a8936-627">Valor</span><span class="sxs-lookup"><span data-stu-id="a8936-627">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="a8936-628">json_array:key</span><span class="sxs-lookup"><span data-stu-id="a8936-628">json_array:key</span></span>          | <span data-ttu-id="a8936-629">valueA</span><span class="sxs-lookup"><span data-stu-id="a8936-629">valueA</span></span> |
| <span data-ttu-id="a8936-630">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="a8936-630">json_array:subsection:0</span></span> | <span data-ttu-id="a8936-631">valueB</span><span class="sxs-lookup"><span data-stu-id="a8936-631">valueB</span></span> |
| <span data-ttu-id="a8936-632">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="a8936-632">json_array:subsection:1</span></span> | <span data-ttu-id="a8936-633">valueC</span><span class="sxs-lookup"><span data-stu-id="a8936-633">valueC</span></span> |
| <span data-ttu-id="a8936-634">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="a8936-634">json_array:subsection:2</span></span> | <span data-ttu-id="a8936-635">valueD</span><span class="sxs-lookup"><span data-stu-id="a8936-635">valueD</span></span> |

<span data-ttu-id="a8936-636">En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="a8936-636">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="a8936-637">Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="a8936-637">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="a8936-638">Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="a8936-638">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="a8936-639">Índice de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="a8936-639">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="a8936-640">Valor de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="a8936-640">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="a8936-641">0</span><span class="sxs-lookup"><span data-stu-id="a8936-641">0</span></span>                                   | <span data-ttu-id="a8936-642">valueB</span><span class="sxs-lookup"><span data-stu-id="a8936-642">valueB</span></span>                              |
| <span data-ttu-id="a8936-643">1</span><span class="sxs-lookup"><span data-stu-id="a8936-643">1</span></span>                                   | <span data-ttu-id="a8936-644">valueC</span><span class="sxs-lookup"><span data-stu-id="a8936-644">valueC</span></span>                              |
| <span data-ttu-id="a8936-645">2</span><span class="sxs-lookup"><span data-stu-id="a8936-645">2</span></span>                                   | <span data-ttu-id="a8936-646">valueD</span><span class="sxs-lookup"><span data-stu-id="a8936-646">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="a8936-647">Proveedores de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="a8936-647">Custom configuration provider</span></span>

<span data-ttu-id="a8936-648">La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="a8936-648">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="a8936-649">El proveedor tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="a8936-649">The provider has the following characteristics:</span></span>

* <span data-ttu-id="a8936-650">La base de datos en memoria de EF se usa para fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="a8936-650">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="a8936-651">Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="a8936-651">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="a8936-652">El proveedor lee una tabla de base de datos en la configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="a8936-652">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="a8936-653">El proveedor no consulta la base de datos por clave.</span><span class="sxs-lookup"><span data-stu-id="a8936-653">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="a8936-654">La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8936-654">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="a8936-655">Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a8936-655">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a8936-656">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-656">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="a8936-657">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="a8936-657">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="a8936-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-658">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a8936-659">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="a8936-659">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="a8936-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-660">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="a8936-661">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="a8936-661">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="a8936-662">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="a8936-662">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="a8936-663">Puesto que las [claves de configuración no distinguen entre mayúsculas y minúsculas](#keys), el diccionario empleado para iniciar la base de datos se crea con el comparador que tampoco hace tal distinción ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="a8936-663">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="a8936-664">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-664">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="a8936-665">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8936-665">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="a8936-666">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-666">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="a8936-667">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-667">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a8936-668">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-668">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="a8936-669">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="a8936-669">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="a8936-670">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-670">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a8936-671">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="a8936-671">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="a8936-672">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-672">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="a8936-673">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="a8936-673">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="a8936-674">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="a8936-674">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="a8936-675">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-675">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="a8936-676">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a8936-676">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="a8936-677">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-677">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="a8936-678">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="a8936-678">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="a8936-679">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="a8936-679">Access configuration during startup</span></span>

<span data-ttu-id="a8936-680">Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8936-680">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8936-681">Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:</span><span class="sxs-lookup"><span data-stu-id="a8936-681">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="a8936-682">Para ver un ejemplo de cómo acceder a la configuración mediante métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods)</span><span class="sxs-lookup"><span data-stu-id="a8936-682">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="a8936-683">Acceso a la configuración en una página de Razor Pages o en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="a8936-683">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="a8936-684">Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.</span><span class="sxs-lookup"><span data-stu-id="a8936-684">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="a8936-685">En una página de las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="a8936-685">In a Razor Pages page:</span></span>

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

<span data-ttu-id="a8936-686">En una vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="a8936-686">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="a8936-687">Agregar configuración a partir de un ensamblado externo</span><span class="sxs-lookup"><span data-stu-id="a8936-687">Add configuration from an external assembly</span></span>

<span data-ttu-id="a8936-688">Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="a8936-688">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="a8936-689">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="a8936-689">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a8936-690">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a8936-690">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
