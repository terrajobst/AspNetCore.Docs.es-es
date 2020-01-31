---
title: Configuración en ASP.NET Core
author: guardrex
description: Obtenga información sobre cómo usar la API de configuración para una aplicación ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2020
uid: fundamentals/configuration/index
ms.openlocfilehash: df49286c3f050b8e90cb5427cf03e2b896a39294
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885564"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="f0c1e-103">Configuración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f0c1e-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="f0c1e-104">Por [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f0c1e-105">La configuración de la aplicación en ASP.NET Core se basa en pares clave-valor establecidos por *proveedores de configuración*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-105">App configuration in ASP.NET Core is based on key-value pairs established by *configuration providers*.</span></span> <span data-ttu-id="f0c1e-106">Los proveedores de configuración leen los datos de configuración en los pares clave-valor de distintos orígenes de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-106">Configuration providers read configuration data into key-value pairs from a variety of configuration sources:</span></span>

* <span data-ttu-id="f0c1e-107">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f0c1e-107">Azure Key Vault</span></span>
* <span data-ttu-id="f0c1e-108">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="f0c1e-108">Azure App Configuration</span></span>
* <span data-ttu-id="f0c1e-109">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-109">Command-line arguments</span></span>
* <span data-ttu-id="f0c1e-110">Proveedores personalizados (instalados o creados)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-110">Custom providers (installed or created)</span></span>
* <span data-ttu-id="f0c1e-111">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="f0c1e-111">Directory files</span></span>
* <span data-ttu-id="f0c1e-112">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="f0c1e-112">Environment variables</span></span>
* <span data-ttu-id="f0c1e-113">Objetos de .NET en memoria</span><span class="sxs-lookup"><span data-stu-id="f0c1e-113">In-memory .NET objects</span></span>
* <span data-ttu-id="f0c1e-114">Archivos de configuración</span><span class="sxs-lookup"><span data-stu-id="f0c1e-114">Settings files</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f0c1e-115">Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen implícitamente en el marco.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-115">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included implicitly by the framework.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f0c1e-116">Los paquetes de configuración para escenarios de proveedor de configuración común ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) se incluyen en el [metapaquete Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-116">Configuration packages for common configuration provider scenarios ([Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/)) are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="f0c1e-117">Los ejemplos de código que siguen y en la aplicación de ejemplo utilizan el espacio de nombres <xref:Microsoft.Extensions.Configuration>:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-117">Code examples that follow and in the sample app use the <xref:Microsoft.Extensions.Configuration> namespace:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="f0c1e-118">El *patrón de opciones* es una extensión de los conceptos de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-118">The *options pattern* is an extension of the configuration concepts described in this topic.</span></span> <span data-ttu-id="f0c1e-119">Las opciones usan clases para representar grupos de configuraciones relacionadas.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-119">Options use classes to represent groups of related settings.</span></span> <span data-ttu-id="f0c1e-120">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-120">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f0c1e-121">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f0c1e-121">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/configuration/index/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="host-versus-app-configuration"></a><span data-ttu-id="f0c1e-122">Configuración de host y de aplicación</span><span class="sxs-lookup"><span data-stu-id="f0c1e-122">Host versus app configuration</span></span>

<span data-ttu-id="f0c1e-123">Antes de configurar e iniciar la aplicación, se configura e inicia un *host*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-123">Before the app is configured and started, a *host* is configured and launched.</span></span> <span data-ttu-id="f0c1e-124">El host es responsable de la administración del inicio y la duración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-124">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="f0c1e-125">Tanto la aplicación como el host se configuran mediante los proveedores de configuración que se describen en este tema.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-125">Both the app and the host are configured using the configuration providers described in this topic.</span></span> <span data-ttu-id="f0c1e-126">Los pares clave-valor de configuración del host también se incluyen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-126">Host configuration key-value pairs are also included in the app's configuration.</span></span> <span data-ttu-id="f0c1e-127">Para más información sobre cómo se usan los proveedores de configuración cuando se compila el host y cómo afectan los orígenes de configuración a la configuración del host, consulte <xref:fundamentals/index#host>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-127">For more information on how the configuration providers are used when the host is built and how configuration sources affect host configuration, see <xref:fundamentals/index#host>.</span></span>

## <a name="other-configuration"></a><span data-ttu-id="f0c1e-128">Otra configuración</span><span class="sxs-lookup"><span data-stu-id="f0c1e-128">Other configuration</span></span>

<span data-ttu-id="f0c1e-129">Este tema solo concierne a la *configuración de aplicaciones*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-129">This topic only pertains to *app configuration*.</span></span> <span data-ttu-id="f0c1e-130">Otros aspectos de la ejecución y el hospedaje de aplicaciones ASP.NET Core se configuran mediante archivos de configuración que no se describen en este tema:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-130">Other aspects of running and hosting ASP.NET Core apps are configured using configuration files not covered in this topic:</span></span>

* <span data-ttu-id="f0c1e-131">*launch.json*/*launchSettings.json* son archivos de configuración de herramientas para el entorno de desarrollo, que se describe a continuación:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-131">*launch.json*/*launchSettings.json* are tooling configuration files for the Development environment, described:</span></span>
  * <span data-ttu-id="f0c1e-132">En <xref:fundamentals/environments#development>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-132">In <xref:fundamentals/environments#development>.</span></span>
  * <span data-ttu-id="f0c1e-133">En el conjunto de documentación en el que se usan los archivos para configurar aplicaciones ASP.NET Core para escenarios de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-133">Across the documentation set where the files are used to configure ASP.NET Core apps for Development scenarios.</span></span>
* <span data-ttu-id="f0c1e-134">*web.config* es un archivo de configuración del servidor, que se describe en los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-134">*web.config* is a server configuration file, described in the following topics:</span></span>
  * <xref:host-and-deploy/iis/index>
  * <xref:host-and-deploy/aspnet-core-module>

<span data-ttu-id="f0c1e-135">Para más información sobre cómo migrar la configuración de aplicaciones de versiones anteriores de ASP.NET, consulte <xref:migration/proper-to-2x/index#store-configurations>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-135">For more information on migrating app configuration from earlier versions of ASP.NET, see <xref:migration/proper-to-2x/index#store-configurations>.</span></span>

## <a name="default-configuration"></a><span data-ttu-id="f0c1e-136">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="f0c1e-136">Default configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f0c1e-137">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-137">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="f0c1e-138">`CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-138">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="f0c1e-139">El contenido siguiente es válido para las aplicaciones que usen el [host genérico](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-139">The following applies to apps using the [Generic Host](xref:fundamentals/host/generic-host).</span></span> <span data-ttu-id="f0c1e-140">Para obtener más información sobre la configuración predeterminada al usar el [host de web](xref:fundamentals/host/web-host), vea la [versión de este tema para ASP.NET Core 2.2](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-140">For details on the default configuration when using the [Web Host](xref:fundamentals/host/web-host), see the [ASP.NET Core 2.2 version of this topic](/aspnet/core/fundamentals/configuration/?view=aspnetcore-2.2).</span></span>

* <span data-ttu-id="f0c1e-141">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-141">Host configuration is provided from:</span></span>
  * <span data-ttu-id="f0c1e-142">Variables de entorno prefijadas con `DOTNET_` (por ejemplo, `DOTNET_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-142">Environment variables prefixed with `DOTNET_` (for example, `DOTNET_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="f0c1e-143">El prefijo (`DOTNET_`) se quita cuando se cargan los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-143">The prefix (`DOTNET_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="f0c1e-144">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-144">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="f0c1e-145">La configuración predeterminada del host de web se ha establecido (`ConfigureWebHostDefaults`):</span><span class="sxs-lookup"><span data-stu-id="f0c1e-145">Web Host default configuration is established (`ConfigureWebHostDefaults`):</span></span>
  * <span data-ttu-id="f0c1e-146">Kestrel se usa como el servidor web y se ha configurado mediante los proveedores de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-146">Kestrel is used as the web server and configured using the app's configuration providers.</span></span>
  * <span data-ttu-id="f0c1e-147">Agregar el middleware de filtrado de host</span><span class="sxs-lookup"><span data-stu-id="f0c1e-147">Add Host Filtering Middleware.</span></span>
  * <span data-ttu-id="f0c1e-148">Agregue el middleware de encabezados reenviados si la variable de entorno `ASPNETCORE_FORWARDEDHEADERS_ENABLED` se establece en `true`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-148">Add Forwarded Headers Middleware if the `ASPNETCORE_FORWARDEDHEADERS_ENABLED` environment variable is set to `true`.</span></span>
  * <span data-ttu-id="f0c1e-149">Habilite la integración con IIS.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-149">Enable IIS integration.</span></span>
* <span data-ttu-id="f0c1e-150">La configuración de la aplicación la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-150">App configuration is provided from:</span></span>
  * <span data-ttu-id="f0c1e-151">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-151">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f0c1e-152">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-152">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f0c1e-153">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-153">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="f0c1e-154">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-154">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="f0c1e-155">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-155">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f0c1e-156">Las aplicaciones web basadas en las plantillas de [dotnet new](/dotnet/core/tools/dotnet-new) de ASP.NET Core llaman a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> al crear un host.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-156">Web apps based on the ASP.NET Core [dotnet new](/dotnet/core/tools/dotnet-new) templates call <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> when building a host.</span></span> <span data-ttu-id="f0c1e-157">`CreateDefaultBuilder` proporciona la configuración predeterminada para la aplicación en el orden siguiente:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-157">`CreateDefaultBuilder` provides default configuration for the app in the following order:</span></span>

<span data-ttu-id="f0c1e-158">El contenido siguiente es válido para las aplicaciones que usen el [host de web](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-158">The following applies to apps using the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="f0c1e-159">Para obtener más información sobre la configuración predeterminada al usar el [host genérico](xref:fundamentals/host/generic-host), vea la [versión más reciente de ese tema](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-159">For details on the default configuration when using the [Generic Host](xref:fundamentals/host/generic-host), see the [latest version of this topic](xref:fundamentals/configuration/index).</span></span>

* <span data-ttu-id="f0c1e-160">La configuración del host la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-160">Host configuration is provided from:</span></span>
  * <span data-ttu-id="f0c1e-161">Variables de entorno prefijadas con `ASPNETCORE_` (por ejemplo, `ASPNETCORE_ENVIRONMENT`) con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-161">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`) using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span> <span data-ttu-id="f0c1e-162">El prefijo (`ASPNETCORE_`) se quita cuando se cargan los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-162">The prefix (`ASPNETCORE_`) is stripped when the configuration key-value pairs are loaded.</span></span>
  * <span data-ttu-id="f0c1e-163">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-163">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>
* <span data-ttu-id="f0c1e-164">La configuración de la aplicación la proporcionan los siguientes elementos:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-164">App configuration is provided from:</span></span>
  * <span data-ttu-id="f0c1e-165">*appsettings.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-165">*appsettings.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f0c1e-166">*appsettings.{Entorno}.json* con el [Proveedor de configuración de archivo](#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-166">*appsettings.{Environment}.json* using the [File Configuration Provider](#file-configuration-provider).</span></span>
  * <span data-ttu-id="f0c1e-167">[Administrador de secretos](xref:security/app-secrets) cuando la aplicación se ejecuta en el entorno `Development` por medio del ensamblado de entrada.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-167">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="f0c1e-168">Variables de entorno con el [Proveedor de configuración de variables de entorno](#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-168">Environment variables using the [Environment Variables Configuration Provider](#environment-variables-configuration-provider).</span></span>
  * <span data-ttu-id="f0c1e-169">Argumentos de la línea de comandos con el [Proveedor de configuración de línea de comandos](#command-line-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-169">Command-line arguments using the [Command-line Configuration Provider](#command-line-configuration-provider).</span></span>

::: moniker-end

## <a name="security"></a><span data-ttu-id="f0c1e-170">Seguridad</span><span class="sxs-lookup"><span data-stu-id="f0c1e-170">Security</span></span>

<span data-ttu-id="f0c1e-171">Adopte los procedimientos siguientes para proteger los datos de configuración confidenciales:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-171">Adopt the following practices to secure sensitive configuration data:</span></span>

* <span data-ttu-id="f0c1e-172">Nunca almacene contraseñas u otros datos confidenciales en el código del proveedor de configuración o en archivos de configuración de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-172">Never store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span>
* <span data-ttu-id="f0c1e-173">No use secretos de producción en los entornos de desarrollo o pruebas.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-173">Don't use production secrets in development or test environments.</span></span>
* <span data-ttu-id="f0c1e-174">Especifique los secretos fuera del proyecto para que no se confirmen en un repositorio de código fuente de manera accidental.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-174">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span>

<span data-ttu-id="f0c1e-175">Para obtener más información, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-175">For more information, see the following topics:</span></span>

* <xref:fundamentals/environments>
* <span data-ttu-id="f0c1e-176"><xref:security/app-secrets> &ndash; Se incluyen recomendaciones sobre el uso de variables de entorno para almacenar información confidencial.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-176"><xref:security/app-secrets> &ndash; Includes advice on using environment variables to store sensitive data.</span></span> <span data-ttu-id="f0c1e-177">El Administrador de secretos usa el proveedor de configuración de archivo para almacenar secretos de usuario en un archivo JSON del sistema local.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-177">The Secret Manager uses the File Configuration Provider to store user secrets in a JSON file on the local system.</span></span> <span data-ttu-id="f0c1e-178">El proveedor de configuración de archivo se describe más adelante en este tema.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-178">The File Configuration Provider is described later in this topic.</span></span>

<span data-ttu-id="f0c1e-179">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) almacena de forma segura secretos de aplicación para aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-179">[Azure Key Vault](https://azure.microsoft.com/services/key-vault/) safely stores app secrets for ASP.NET Core apps.</span></span> <span data-ttu-id="f0c1e-180">Para obtener más información, vea <xref:security/key-vault-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-180">For more information, see <xref:security/key-vault-configuration>.</span></span>

## <a name="hierarchical-configuration-data"></a><span data-ttu-id="f0c1e-181">Datos de configuración jerárquica</span><span class="sxs-lookup"><span data-stu-id="f0c1e-181">Hierarchical configuration data</span></span>

<span data-ttu-id="f0c1e-182">La API de configuración es capaz de mantener los datos de configuración jerárquica al disminuir los datos jerárquicos mediante el uso de un delimitador en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-182">The Configuration API is capable of maintaining hierarchical configuration data by flattening the hierarchical data with the use of a delimiter in the configuration keys.</span></span>

<span data-ttu-id="f0c1e-183">En el siguiente archivo JSON, hay cuatro claves en una estructura jerárquica de dos secciones:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-183">In the following JSON file, four keys exist in a structured hierarchy of two sections:</span></span>

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

<span data-ttu-id="f0c1e-184">Cuando el archivo se lee en la configuración, se crean claves únicas para mantener la estructura de datos jerárquicos original del origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-184">When the file is read into configuration, unique keys are created to maintain the original hierarchical data structure of the configuration source.</span></span> <span data-ttu-id="f0c1e-185">Las secciones y claves se disminuyen con el uso de dos puntos (`:`) para mantener la estructura original:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-185">The sections and keys are flattened with the use of a colon (`:`) to maintain the original structure:</span></span>

* <span data-ttu-id="f0c1e-186">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-186">section0:key0</span></span>
* <span data-ttu-id="f0c1e-187">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-187">section0:key1</span></span>
* <span data-ttu-id="f0c1e-188">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-188">section1:key0</span></span>
* <span data-ttu-id="f0c1e-189">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-189">section1:key1</span></span>

<span data-ttu-id="f0c1e-190">Los métodos <xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> y <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> están disponibles para aislar las secciones y los elementos secundarios de una sección en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-190"><xref:Microsoft.Extensions.Configuration.ConfigurationSection.GetSection*> and <xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*> methods are available to isolate sections and children of a section in the configuration data.</span></span> <span data-ttu-id="f0c1e-191">Estos métodos se describen más adelante en la sección [GetSection, GetChildren y Exists](#getsection-getchildren-and-exists).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-191">These methods are described later in [GetSection, GetChildren, and Exists](#getsection-getchildren-and-exists).</span></span>

## <a name="conventions"></a><span data-ttu-id="f0c1e-192">Convenciones</span><span class="sxs-lookup"><span data-stu-id="f0c1e-192">Conventions</span></span>

### <a name="sources-and-providers"></a><span data-ttu-id="f0c1e-193">Orígenes y proveedores</span><span class="sxs-lookup"><span data-stu-id="f0c1e-193">Sources and providers</span></span>

<span data-ttu-id="f0c1e-194">Al iniciar la aplicación, los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-194">At app startup, configuration sources are read in the order that their configuration providers are specified.</span></span>

<span data-ttu-id="f0c1e-195">Los proveedores de configuración que implementan la detección de cambios tienen la capacidad de volver a cargar la configuración cuando se cambia una configuración subyacente.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-195">Configuration providers that implement change detection have the ability to reload configuration when an underlying setting is changed.</span></span> <span data-ttu-id="f0c1e-196">Por ejemplo, el proveedor de configuración de archivos (descrito más adelante en este tema) y el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) implementan la detección de cambios.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-196">For example, the File Configuration Provider (described later in this topic) and the [Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) implement change detection.</span></span>

<span data-ttu-id="f0c1e-197"><xref:Microsoft.Extensions.Configuration.IConfiguration> está disponible en el contenedor de [inserción de dependencias (DI)](xref:fundamentals/dependency-injection) de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-197"><xref:Microsoft.Extensions.Configuration.IConfiguration> is available in the app's [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f0c1e-198"><xref:Microsoft.Extensions.Configuration.IConfiguration> se puede insertar en <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> de Razor Pages o <xref:Microsoft.AspNetCore.Mvc.Controller> de MVC para obtener la configuración de la clase.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-198"><xref:Microsoft.Extensions.Configuration.IConfiguration> can be injected into a Razor Pages <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> or MVC <xref:Microsoft.AspNetCore.Mvc.Controller> to obtain configuration for the class.</span></span>

<span data-ttu-id="f0c1e-199">En los ejemplos siguientes, se usa el campo `_config` para tener acceso a los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-199">In the following examples, the `_config` field is used to access configuration values:</span></span>

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

<span data-ttu-id="f0c1e-200">Los proveedores de configuración no pueden usar la inserción de dependencias, porque no está disponible cuando el host los configura.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-200">Configuration providers can't utilize DI, as it's not available when they're set up by the host.</span></span>

### <a name="keys"></a><span data-ttu-id="f0c1e-201">Teclas</span><span class="sxs-lookup"><span data-stu-id="f0c1e-201">Keys</span></span>

<span data-ttu-id="f0c1e-202">Las claves de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-202">Configuration keys adopt the following conventions:</span></span>

* <span data-ttu-id="f0c1e-203">Las claves no distinguen mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-203">Keys are case-insensitive.</span></span> <span data-ttu-id="f0c1e-204">Por ejemplo, `ConnectionString` y `connectionstring` se consideran claves equivalente.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-204">For example, `ConnectionString` and `connectionstring` are treated as equivalent keys.</span></span>
* <span data-ttu-id="f0c1e-205">Si los mismos o distintos proveedores de configuración establecen un valor para la misma clave, el último valor establecido en la clave es el valor que se usa.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-205">If a value for the same key is set by the same or different configuration providers, the last value set on the key is the value used.</span></span>
* <span data-ttu-id="f0c1e-206">Claves jerárquicas</span><span class="sxs-lookup"><span data-stu-id="f0c1e-206">Hierarchical keys</span></span>
  * <span data-ttu-id="f0c1e-207">Dentro de la API de configuración, un separador de dos puntos (`:`) funciona en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-207">Within the Configuration API, a colon separator (`:`) works on all platforms.</span></span>
  * <span data-ttu-id="f0c1e-208">En las variables de entorno, puede que un separador de dos puntos no funcione en todas las plataformas.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-208">In environment variables, a colon separator may not work on all platforms.</span></span> <span data-ttu-id="f0c1e-209">Todas las plataformas admiten un carácter de subrayado doble (`__`), que se convierte automáticamente en un signo de dos puntos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-209">A double underscore (`__`) is supported by all platforms and is automatically converted into a colon.</span></span>
  * <span data-ttu-id="f0c1e-210">En Azure Key Vault, las claves jerárquicas usan `--` (dos guiones) como separador.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-210">In Azure Key Vault, hierarchical keys use `--` (two dashes) as a separator.</span></span> <span data-ttu-id="f0c1e-211">Escriba código para reemplazar los guiones por dos puntos cuando los secretos se carguen en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-211">Write code to replace the dashes with a colon when the secrets are loaded into the app's configuration.</span></span>
* <span data-ttu-id="f0c1e-212"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-212">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f0c1e-213">El enlace de matriz se describe en la sección [Enlace de una matriz a una clase](#bind-an-array-to-a-class).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-213">Array binding is described in the [Bind an array to a class](#bind-an-array-to-a-class) section.</span></span>

### <a name="values"></a><span data-ttu-id="f0c1e-214">Valores</span><span class="sxs-lookup"><span data-stu-id="f0c1e-214">Values</span></span>

<span data-ttu-id="f0c1e-215">Los valores de configuración adoptan las convenciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-215">Configuration values adopt the following conventions:</span></span>

* <span data-ttu-id="f0c1e-216">Los valores son cadenas.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-216">Values are strings.</span></span>
* <span data-ttu-id="f0c1e-217">Los valores NULL no se pueden almacenar en la configuración ni enlazar a los objetos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-217">Null values can't be stored in configuration or bound to objects.</span></span>

## <a name="providers"></a><span data-ttu-id="f0c1e-218">Proveedores</span><span class="sxs-lookup"><span data-stu-id="f0c1e-218">Providers</span></span>

<span data-ttu-id="f0c1e-219">La siguiente tabla muestra los proveedores de configuración disponibles para las aplicaciones de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-219">The following table shows the configuration providers available to ASP.NET Core apps.</span></span>

| <span data-ttu-id="f0c1e-220">Proveedor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-220">Provider</span></span> | <span data-ttu-id="f0c1e-221">Proporciona la configuración de&hellip;</span><span class="sxs-lookup"><span data-stu-id="f0c1e-221">Provides configuration from&hellip;</span></span> |
| -------- | ----------------------------------- |
| <span data-ttu-id="f0c1e-222">[Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-222">[Azure Key Vault Configuration Provider](xref:security/key-vault-configuration) (*Security* topics)</span></span> | <span data-ttu-id="f0c1e-223">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f0c1e-223">Azure Key Vault</span></span> |
| <span data-ttu-id="f0c1e-224">[Proveedor de Azure App Configuration](/azure/azure-app-configuration/quickstart-aspnet-core-app) (documentación de Azure)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-224">[Azure App Configuration Provider](/azure/azure-app-configuration/quickstart-aspnet-core-app) (Azure documentation)</span></span> | <span data-ttu-id="f0c1e-225">Configuración de aplicaciones de Azure</span><span class="sxs-lookup"><span data-stu-id="f0c1e-225">Azure App Configuration</span></span> |
| [<span data-ttu-id="f0c1e-226">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-226">Command-line Configuration Provider</span></span>](#command-line-configuration-provider) | <span data-ttu-id="f0c1e-227">Parámetros de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-227">Command-line parameters</span></span> |
| [<span data-ttu-id="f0c1e-228">Proveedor de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="f0c1e-228">Custom configuration provider</span></span>](#custom-configuration-provider) | <span data-ttu-id="f0c1e-229">Origen personalizado</span><span class="sxs-lookup"><span data-stu-id="f0c1e-229">Custom source</span></span> |
| [<span data-ttu-id="f0c1e-230">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="f0c1e-230">Environment Variables Configuration Provider</span></span>](#environment-variables-configuration-provider) | <span data-ttu-id="f0c1e-231">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="f0c1e-231">Environment variables</span></span> |
| [<span data-ttu-id="f0c1e-232">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="f0c1e-232">File Configuration Provider</span></span>](#file-configuration-provider) | <span data-ttu-id="f0c1e-233">Archivos (INI, JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-233">Files (INI, JSON, XML)</span></span> |
| [<span data-ttu-id="f0c1e-234">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="f0c1e-234">Key-per-file Configuration Provider</span></span>](#key-per-file-configuration-provider) | <span data-ttu-id="f0c1e-235">Archivos de directorio</span><span class="sxs-lookup"><span data-stu-id="f0c1e-235">Directory files</span></span> |
| [<span data-ttu-id="f0c1e-236">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="f0c1e-236">Memory Configuration Provider</span></span>](#memory-configuration-provider) | <span data-ttu-id="f0c1e-237">Colecciones en memoria</span><span class="sxs-lookup"><span data-stu-id="f0c1e-237">In-memory collections</span></span> |
| <span data-ttu-id="f0c1e-238">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) (temas de *Seguridad*)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-238">[User secrets (Secret Manager)](xref:security/app-secrets) (*Security* topics)</span></span> | <span data-ttu-id="f0c1e-239">Archivo en el directorio del perfil de usuario</span><span class="sxs-lookup"><span data-stu-id="f0c1e-239">File in the user profile directory</span></span> |

<span data-ttu-id="f0c1e-240">Los orígenes de configuración se leen en el orden en que se especifican sus proveedores de configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-240">Configuration sources are read in the order that their configuration providers are specified at startup.</span></span> <span data-ttu-id="f0c1e-241">En este tema, los proveedores de configuración se describen en orden alfabético y no en el orden en el que el código los organiza.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-241">The configuration providers described in this topic are described in alphabetical order, not in the order that the code arranges them.</span></span> <span data-ttu-id="f0c1e-242">Ordene los proveedores de configuración en el código para cumplir con las prioridades relacionadas con los orígenes de configuración subyacentes que la aplicación necesita.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-242">Order configuration providers in code to suit the priorities for the underlying configuration sources that the app requires.</span></span>

<span data-ttu-id="f0c1e-243">Esta es una secuencia típica de proveedores de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-243">A typical sequence of configuration providers is:</span></span>

1. <span data-ttu-id="f0c1e-244">Archivos (*appsettings.json*, *appsettings.{Environment}.json*, donde `{Environment}` es el entorno de hospedaje actual de la aplicación)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-244">Files (*appsettings.json*, *appsettings.{Environment}.json*, where `{Environment}` is the app's current hosting environment)</span></span>
1. [<span data-ttu-id="f0c1e-245">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f0c1e-245">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
1. <span data-ttu-id="f0c1e-246">[Secretos de usuario (administrador de secretos)](xref:security/app-secrets) (solo para entornos de desarrollo)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-246">[User secrets (Secret Manager)](xref:security/app-secrets) (Development environment only)</span></span>
1. <span data-ttu-id="f0c1e-247">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="f0c1e-247">Environment variables</span></span>
1. <span data-ttu-id="f0c1e-248">Argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-248">Command-line arguments</span></span>

<span data-ttu-id="f0c1e-249">Una práctica común es colocar el proveedor de configuración de línea de comandos al final en una serie de proveedores para permitir que los argumentos de la línea de comandos reemplacen la configuración establecida por el resto de los proveedores.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-249">A common practice is to position the Command-line Configuration Provider last in a series of providers to allow command-line arguments to override configuration set by the other providers.</span></span>

<span data-ttu-id="f0c1e-250">La secuencia de proveedores anterior se usa cuando se inicializa un nuevo generador de host con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-250">The preceding sequence of providers is used when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f0c1e-251">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-251">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="configure-the-host-builder-with-configurehostconfiguration"></a><span data-ttu-id="f0c1e-252">Configurar el generador de host con ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="f0c1e-252">Configure the host builder with ConfigureHostConfiguration</span></span>

<span data-ttu-id="f0c1e-253">Para configurar el generador de host, realice una llamada a <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> y proporcione la configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-253">To configure the host builder, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> and supply the configuration.</span></span> <span data-ttu-id="f0c1e-254">`ConfigureHostConfiguration` se usa para inicializar <xref:Microsoft.Extensions.Hosting.IHostEnvironment> para su uso posterior en el proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-254">`ConfigureHostConfiguration` is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> for use later in the build process.</span></span> <span data-ttu-id="f0c1e-255">Se puede llamar varias veces a `ConfigureHostConfiguration` con resultados de suma.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-255">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span>

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

## <a name="configure-the-host-builder-with-useconfiguration"></a><span data-ttu-id="f0c1e-256">Configurar el generador de host con UseConfiguration</span><span class="sxs-lookup"><span data-stu-id="f0c1e-256">Configure the host builder with UseConfiguration</span></span>

<span data-ttu-id="f0c1e-257">Para configurar el generador de host, realice una llamada a <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> en el generador de host con la configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-257">To configure the host builder, call <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> on the host builder with the configuration.</span></span>

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

## <a name="configureappconfiguration"></a><span data-ttu-id="f0c1e-258">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="f0c1e-258">ConfigureAppConfiguration</span></span>

<span data-ttu-id="f0c1e-259">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar los proveedores de configuración de la aplicación además de aquellos que `CreateDefaultBuilder` agrega automáticamente:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-259">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration providers in addition to those added automatically by `CreateDefaultBuilder`:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=20)]

::: moniker-end

### <a name="override-previous-configuration-with-command-line-arguments"></a><span data-ttu-id="f0c1e-260">Reemplazar la configuración anterior con argumentos de la línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-260">Override previous configuration with command-line arguments</span></span>

<span data-ttu-id="f0c1e-261">Para proporcionar configuración de aplicaciones que se pueda reemplazar por argumentos de la línea de comandos, llame a `AddCommandLine` en último lugar:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-261">To provide app configuration that can be overridden with command-line arguments, call `AddCommandLine` last:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

### <a name="remove-providers-added-by-createdefaultbuilder"></a><span data-ttu-id="f0c1e-262">Quitar proveedores agregados por CreateDefaultBuilder</span><span class="sxs-lookup"><span data-stu-id="f0c1e-262">Remove providers added by CreateDefaultBuilder</span></span>

<span data-ttu-id="f0c1e-263">Para quitar los proveedores agregados por `CreateDefaultBuilder`, llame primero a [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) en [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources):</span><span class="sxs-lookup"><span data-stu-id="f0c1e-263">To remove the providers added by `CreateDefaultBuilder`, call [Clear](/dotnet/api/system.collections.generic.icollection-1.clear) on the [IConfigurationBuilder.Sources](xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Sources) first:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.Sources.Clear();
    // Add providers here
})
```

### <a name="consume-configuration-during-app-startup"></a><span data-ttu-id="f0c1e-264">Usar la configuración durante el inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="f0c1e-264">Consume configuration during app startup</span></span>

<span data-ttu-id="f0c1e-265">La configuración proporcionada a la aplicación en `ConfigureAppConfiguration` está disponible durante el inicio de la aplicación, incluido `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-265">Configuration supplied to the app in `ConfigureAppConfiguration` is available during the app's startup, including `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f0c1e-266">Para obtener más información, vea la sección [Acceso a la configuración durante el inicio](#access-configuration-during-startup).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-266">For more information, see the [Access configuration during startup](#access-configuration-during-startup) section.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="f0c1e-267">Proveedor de configuración de línea de comandos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-267">Command-line Configuration Provider</span></span>

<span data-ttu-id="f0c1e-268"><xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> carga la configuración de pares clave-valor de argumento de la línea de comandos en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-268">The <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs at runtime.</span></span>

<span data-ttu-id="f0c1e-269">Para activar la configuración de línea de comandos, se llama al método de extensión <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-269">To activate command-line configuration, the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> extension method is called on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f0c1e-270">`AddCommandLine` se llama automáticamente al llamar a `CreateDefaultBuilder(string [])`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-270">`AddCommandLine` is automatically called when `CreateDefaultBuilder(string [])` is called.</span></span> <span data-ttu-id="f0c1e-271">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-271">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="f0c1e-272">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-272">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f0c1e-273">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-273">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="f0c1e-274">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-274">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="f0c1e-275">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-275">Environment variables.</span></span>

<span data-ttu-id="f0c1e-276">`CreateDefaultBuilder` agrega el proveedor de configuración de línea de comandos al final.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-276">`CreateDefaultBuilder` adds the Command-line Configuration Provider last.</span></span> <span data-ttu-id="f0c1e-277">Los argumentos de la línea de comandos que se pasan en tiempo de ejecución invalidan la configuración establecida por los otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-277">Command-line arguments passed at runtime override configuration set by the other providers.</span></span>

<span data-ttu-id="f0c1e-278">`CreateDefaultBuilder` actúa cuando se construye el host.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-278">`CreateDefaultBuilder` acts when the host is constructed.</span></span> <span data-ttu-id="f0c1e-279">Por tanto, la configuración de línea de comandos activada por `CreateDefaultBuilder` puede afectar la manera en que se configura el host.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-279">Therefore, command-line configuration activated by `CreateDefaultBuilder` can affect how the host is configured.</span></span>

<span data-ttu-id="f0c1e-280">Para las aplicaciones basadas en plantillas de ASP.NET Core, `CreateDefaultBuilder` ya realiza una llamada a `AddCommandLine`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-280">For apps based on the ASP.NET Core templates, `AddCommandLine` has already been called by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f0c1e-281">Para agregar proveedores de configuración adicionales y mantener la capacidad de reemplazar la configuración de sus proveedores con argumentos de la línea de comandos, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddCommandLine` en último lugar.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-281">To add additional configuration providers and maintain the ability to override configuration from those providers with command-line arguments, call the app's additional providers in `ConfigureAppConfiguration` and call `AddCommandLine` last.</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    // Call other providers here
    config.AddCommandLine(args);
})
```

<span data-ttu-id="f0c1e-282">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="f0c1e-282">**Example**</span></span>

<span data-ttu-id="f0c1e-283">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-283">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span>

1. <span data-ttu-id="f0c1e-284">Abra un símbolo del sistema en el directorio del proyecto.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-284">Open a command prompt in the project's directory.</span></span>
1. <span data-ttu-id="f0c1e-285">Proporcione un argumento de la línea de comandos para el comando `dotnet run`, `dotnet run CommandLineKey=CommandLineValue`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-285">Supply a command-line argument to the `dotnet run` command, `dotnet run CommandLineKey=CommandLineValue`.</span></span>
1. <span data-ttu-id="f0c1e-286">Una vez que se ejecuta la aplicación, abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-286">After the app is running, open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f0c1e-287">Observe que la salida contiene el par clave-valor para el argumento de línea de comandos de configuración proporcionado a `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-287">Observe that the output contains the key-value pair for the configuration command-line argument provided to `dotnet run`.</span></span>

### <a name="arguments"></a><span data-ttu-id="f0c1e-288">Argumentos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-288">Arguments</span></span>

<span data-ttu-id="f0c1e-289">El valor debe seguir a un signo igual (`=`) o la clave debe tener un prefijo (`--` o `/`) cuando el valor siga a un espacio.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-289">The value must follow an equals sign (`=`), or the key must have a prefix (`--` or `/`) when the value follows a space.</span></span> <span data-ttu-id="f0c1e-290">El valor no es obligatorio si se usa un signo igual (por ejemplo, `CommandLineKey=`).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-290">The value isn't required if an equals sign is used (for example, `CommandLineKey=`).</span></span>

| <span data-ttu-id="f0c1e-291">Prefijo de la clave</span><span class="sxs-lookup"><span data-stu-id="f0c1e-291">Key prefix</span></span>               | <span data-ttu-id="f0c1e-292">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="f0c1e-292">Example</span></span>                                                |
| ------------------------ | ------------------------------------------------------ |
| <span data-ttu-id="f0c1e-293">Sin prefijo</span><span class="sxs-lookup"><span data-stu-id="f0c1e-293">No prefix</span></span>                | `CommandLineKey1=value1`                               |
| <span data-ttu-id="f0c1e-294">Dos guiones (`--`)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-294">Two dashes (`--`)</span></span>        | <span data-ttu-id="f0c1e-295">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-295">`--CommandLineKey2=value2`, `--CommandLineKey2 value2`</span></span> |
| <span data-ttu-id="f0c1e-296">Barra diagonal (`/`)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-296">Forward slash (`/`)</span></span>      | <span data-ttu-id="f0c1e-297">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-297">`/CommandLineKey3=value3`, `/CommandLineKey3 value3`</span></span>   |

<span data-ttu-id="f0c1e-298">Dentro del mismo comando, no mezcle pares clave-valor de argumento de la línea de comandos que usan un signo igual con pares de clave-valor que usan un espacio.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-298">Within the same command, don't mix command-line argument key-value pairs that use an equals sign with key-value pairs that use a space.</span></span>

<span data-ttu-id="f0c1e-299">Comandos de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-299">Example commands:</span></span>

```dotnetcli
dotnet run CommandLineKey1=value1 --CommandLineKey2=value2 /CommandLineKey3=value3
dotnet run --CommandLineKey1 value1 /CommandLineKey2 value2
dotnet run CommandLineKey1= CommandLineKey2=value2
```

### <a name="switch-mappings"></a><span data-ttu-id="f0c1e-300">Asignaciones de modificador</span><span class="sxs-lookup"><span data-stu-id="f0c1e-300">Switch mappings</span></span>

<span data-ttu-id="f0c1e-301">Las asignaciones de modificador admiten la lógica de sustitución de nombres de clave.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-301">Switch mappings allow key name replacement logic.</span></span> <span data-ttu-id="f0c1e-302">Al crear manualmente la configuración con <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, proporcione un diccionario de reemplazos de modificador al método <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-302">When manually building configuration with a <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>, provide a dictionary of switch replacements to the <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*> method.</span></span>

<span data-ttu-id="f0c1e-303">Cuando se usa el diccionario de asignaciones de modificador, se comprueba en el diccionario si una clave coincide con la clave proporcionada por un argumento de línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-303">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="f0c1e-304">Si la clave de la línea de comandos se encuentra en el diccionario, se devuelve el valor del diccionario (el reemplazo de la clave) para establecer el par clave-valor en la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-304">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the key-value pair into the app's configuration.</span></span> <span data-ttu-id="f0c1e-305">Se requiere una asignación de conmutador para cualquier clave de línea de comandos precedida por un solo guion (`-`).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-305">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="f0c1e-306">Reglas de clave del diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-306">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="f0c1e-307">Los modificadores deben empezar por un guion (`-`) o guion doble (`--`).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-307">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="f0c1e-308">El diccionario de asignaciones de modificador no debe contener claves duplicadas.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-308">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="f0c1e-309">Cree un diccionario de asignaciones de modificador.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-309">Create a switch mappings dictionary.</span></span> <span data-ttu-id="f0c1e-310">En el ejemplo siguiente, se crean dos asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-310">In the following example, two switch mappings are created:</span></span>

```csharp
public static readonly Dictionary<string, string> _switchMappings = 
    new Dictionary<string, string>
    {
        { "-CLKey1", "CommandLineKey1" },
        { "-CLKey2", "CommandLineKey2" }
    };
```

<span data-ttu-id="f0c1e-311">Al compilar el host, llame a `AddCommandLine` con el diccionario de asignaciones de modificador:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-311">When the host is built, call `AddCommandLine` with the switch mappings dictionary:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddCommandLine(args, _switchMappings);
})
```

<span data-ttu-id="f0c1e-312">En el caso de las aplicaciones que usen las asignaciones de modificador, la llamada a `CreateDefaultBuilder` no tiene que pasar argumentos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-312">For apps that use switch mappings, the call to `CreateDefaultBuilder` shouldn't pass arguments.</span></span> <span data-ttu-id="f0c1e-313">En la llamada de `AddCommandLine` del método `CreateDefaultBuilder`, no se incluyen modificadores asignados y no hay forma de pasar el diccionario de asignación de modificador a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-313">The `CreateDefaultBuilder` method's `AddCommandLine` call doesn't include mapped switches, and there's no way to pass the switch mapping dictionary to `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f0c1e-314">La solución no es pasar los argumentos de `CreateDefaultBuilder`, sino que permitir que el método `AddCommandLine` del método `ConfigurationBuilder` procese tanto los argumentos como el diccionario de asignación de modificador.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-314">The solution isn't to pass the arguments to `CreateDefaultBuilder` but instead to allow the `ConfigurationBuilder` method's `AddCommandLine` method to process both the arguments and the switch mapping dictionary.</span></span>

<span data-ttu-id="f0c1e-315">Después de crear el diccionario de asignaciones de modificador, contiene los datos que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-315">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="f0c1e-316">Key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-316">Key</span></span>       | <span data-ttu-id="f0c1e-317">Valor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-317">Value</span></span>             |
| --------- | ----------------- |
| `-CLKey1` | `CommandLineKey1` |
| `-CLKey2` | `CommandLineKey2` |

<span data-ttu-id="f0c1e-318">Si las claves de asignación de conmutador se usan al iniciar la aplicación, la configuración recibe el valor de configuración en la clave que el diccionario suministra:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-318">If the switch-mapped keys are used when starting the app, configuration receives the configuration value on the key supplied by the dictionary:</span></span>

```dotnetcli
dotnet run -CLKey1=value1 -CLKey2=value2
```

<span data-ttu-id="f0c1e-319">Una vez que se ejecuta el comando anterior, la configuración contiene los valores que se muestran en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-319">After running the preceding command, configuration contains the values shown in the following table.</span></span>

| <span data-ttu-id="f0c1e-320">Key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-320">Key</span></span>               | <span data-ttu-id="f0c1e-321">Valor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-321">Value</span></span>    |
| ----------------- | -------- |
| `CommandLineKey1` | `value1` |
| `CommandLineKey2` | `value2` |

## <a name="environment-variables-configuration-provider"></a><span data-ttu-id="f0c1e-322">Proveedor de configuración de variables de entorno</span><span class="sxs-lookup"><span data-stu-id="f0c1e-322">Environment Variables Configuration Provider</span></span>

<span data-ttu-id="f0c1e-323"><xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> carga la configuración de pares clave-valor de variables de entorno en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-323">The <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs at runtime.</span></span>

<span data-ttu-id="f0c1e-324">Para activar la configuración de variables de entorno, llame al método de extensión <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-324">To activate environment variables configuration, call the <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

<span data-ttu-id="f0c1e-325">[Azure App Service](https://azure.microsoft.com/services/app-service/) permite establecer variables de entorno en Azure Portal que pueden invalidar la configuración de la aplicación mediante el proveedor de configuración de variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-325">[Azure App Service](https://azure.microsoft.com/services/app-service/) permits setting environment variables in the Azure Portal that can override app configuration using the Environment Variables Configuration Provider.</span></span> <span data-ttu-id="f0c1e-326">Para más información, consulte [Aplicaciones de Azure: Invalidación de la configuración de la aplicación mediante Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-326">For more information, see [Azure Apps: Override app configuration using the Azure Portal](xref:host-and-deploy/azure-apps/index#override-app-configuration-using-the-azure-portal).</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f0c1e-327">`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `DOTNET_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host genérico](xref:fundamentals/host/generic-host) y al llamar a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-327">`AddEnvironmentVariables` is used to load environment variables prefixed with `DOTNET_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Generic Host](xref:fundamentals/host/generic-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="f0c1e-328">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-328">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f0c1e-329">`AddEnvironmentVariables` se usa para cargar variables de entorno con el prefijo `ASPNETCORE_` para la [configuración de host](#host-versus-app-configuration) al inicializar un nuevo generador de host con el [host de web](xref:fundamentals/host/web-host) y al llamar a `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-329">`AddEnvironmentVariables` is used to load environment variables prefixed with `ASPNETCORE_` for [host configuration](#host-versus-app-configuration) when a new host builder is initialized with the [Web Host](xref:fundamentals/host/web-host) and `CreateDefaultBuilder` is called.</span></span> <span data-ttu-id="f0c1e-330">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-330">For more information, see the [Default configuration](#default-configuration) section.</span></span>

::: moniker-end

<span data-ttu-id="f0c1e-331">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-331">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f0c1e-332">Configuración de la aplicación desde variables de entorno sin prefijo mediante la llamada a `AddEnvironmentVariables` sin prefijo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-332">App configuration from unprefixed environment variables by calling `AddEnvironmentVariables` without a prefix.</span></span>
* <span data-ttu-id="f0c1e-333">Configuración opcional de los archivos *appsettings.json* y *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-333">Optional configuration from *appsettings.json* and *appsettings.{Environment}.json* files.</span></span>
* <span data-ttu-id="f0c1e-334">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-334">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="f0c1e-335">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-335">Command-line arguments.</span></span>

<span data-ttu-id="f0c1e-336">El proveedor de configuración de variables de entorno se llama una vez establecida la configuración desde los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-336">The Environment Variables Configuration Provider is called after configuration is established from user secrets and *appsettings* files.</span></span> <span data-ttu-id="f0c1e-337">Llamar al proveedor en esta posición permite que la lectura de las variables de entorno en tiempo de ejecución invaliden la configuración establecida por los secretos de usuario y los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-337">Calling the provider in this position allows the environment variables read at runtime to override configuration set by user secrets and *appsettings* files.</span></span>

<span data-ttu-id="f0c1e-338">Para proporcionar la configuración de la aplicación desde variables de entorno adicionales, llame a los proveedores adicionales de la aplicación en `ConfigureAppConfiguration` y llame a `AddEnvironmentVariables` con el prefijo:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-338">To provide app configuration from additional environment variables, call the app's additional providers in `ConfigureAppConfiguration` and call `AddEnvironmentVariables` with the prefix:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddEnvironmentVariables(prefix: "PREFIX_");
})
```

<span data-ttu-id="f0c1e-339">Llame a `AddEnvironmentVariables` en último lugar para permitir que las variables de entorno con el prefijo especificado invaliden los valores de otros proveedores.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-339">Call `AddEnvironmentVariables` last to allow environment variables with the given prefix to override values from other providers.</span></span>

<span data-ttu-id="f0c1e-340">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="f0c1e-340">**Example**</span></span>

<span data-ttu-id="f0c1e-341">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-341">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes a call to `AddEnvironmentVariables`.</span></span>

1. <span data-ttu-id="f0c1e-342">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-342">Run the sample app.</span></span> <span data-ttu-id="f0c1e-343">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-343">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f0c1e-344">Observe que el resultado contiene el par clave y valor para la variable de entorno `ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-344">Observe that the output contains the key-value pair for the environment variable `ENVIRONMENT`.</span></span> <span data-ttu-id="f0c1e-345">El valor refleja el entorno en el que se ejecuta la aplicación, habitualmente `Development` cuando se ejecuta de manera local.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-345">The value reflects the environment in which the app is running, typically `Development` when running locally.</span></span>

<span data-ttu-id="f0c1e-346">Para mantener un número adecuado de variables de entorno en la lista representada por la aplicación, la aplicación filtrará las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-346">To keep the list of environment variables rendered by the app short, the app filters environment variables.</span></span> <span data-ttu-id="f0c1e-347">Vea el archivo *Pages/Index.cshtml.cs* de la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-347">See the sample app's *Pages/Index.cshtml.cs* file.</span></span>

<span data-ttu-id="f0c1e-348">Para exponer todas las variables de entorno disponibles para la aplicación, cambie `FilteredConfiguration` en *Pages/Index.cshtml.cs* por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-348">To expose all of the environment variables available to the app, change the `FilteredConfiguration` in *Pages/Index.cshtml.cs* to the following:</span></span>

```csharp
FilteredConfiguration = _config.AsEnumerable();
```

### <a name="prefixes"></a><span data-ttu-id="f0c1e-349">Prefijos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-349">Prefixes</span></span>

<span data-ttu-id="f0c1e-350">Las variables de entorno que se cargan en la configuración de la aplicación se filtran cuando se proporciona un prefijo para el método `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-350">Environment variables loaded into the app's configuration are filtered when supplying a prefix to the `AddEnvironmentVariables` method.</span></span> <span data-ttu-id="f0c1e-351">Por ejemplo, para filtrar las variables de entorno en el prefijo `CUSTOM_`, suministre el prefijo para el proveedor de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-351">For example, to filter environment variables on the prefix `CUSTOM_`, supply the prefix to the configuration provider:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .AddEnvironmentVariables("CUSTOM_")
    .Build();
```

<span data-ttu-id="f0c1e-352">El prefijo se quita cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-352">The prefix is stripped off when the configuration key-value pairs are created.</span></span>

<span data-ttu-id="f0c1e-353">Al crear el generador de host, las variables de entorno proporcionan la configuración del host.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-353">When the host builder is created, host configuration is provided by environment variables.</span></span> <span data-ttu-id="f0c1e-354">Para obtener más información sobre el prefijo usado para estas variables de entorno, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-354">For more information on the prefix used for these environment variables, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="f0c1e-355">**Prefijos de cadena de conexión**</span><span class="sxs-lookup"><span data-stu-id="f0c1e-355">**Connection string prefixes**</span></span>

<span data-ttu-id="f0c1e-356">La API de configuración tiene reglas de procesamiento especial para cuatro variables de entorno de cadena de conexión relacionadas con la configuración de las cadenas de conexión de Azure para el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-356">The Configuration API has special processing rules for four connection string environment variables involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="f0c1e-357">Las variables de entorno con los prefijos que se muestran en la tabla se cargan en la aplicación si no se proporciona ningún prefijo a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-357">Environment variables with the prefixes shown in the table are loaded into the app if no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="f0c1e-358">Prefijo de cadena de conexión</span><span class="sxs-lookup"><span data-stu-id="f0c1e-358">Connection string prefix</span></span> | <span data-ttu-id="f0c1e-359">Proveedor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-359">Provider</span></span> |
| ------------------------ | -------- |
| `CUSTOMCONNSTR_` | <span data-ttu-id="f0c1e-360">Proveedor personalizado</span><span class="sxs-lookup"><span data-stu-id="f0c1e-360">Custom provider</span></span> |
| `MYSQLCONNSTR_` | [<span data-ttu-id="f0c1e-361">MySQL</span><span class="sxs-lookup"><span data-stu-id="f0c1e-361">MySQL</span></span>](https://www.mysql.com/) |
| `SQLAZURECONNSTR_` | [<span data-ttu-id="f0c1e-362">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="f0c1e-362">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database/) |
| `SQLCONNSTR_` | [<span data-ttu-id="f0c1e-363">SQL Server</span><span class="sxs-lookup"><span data-stu-id="f0c1e-363">SQL Server</span></span>](https://www.microsoft.com/sql-server/) |

<span data-ttu-id="f0c1e-364">Cuando una variable de entorno se detecta y carga en la configuración con cualquiera de los cuatro prefijos que se muestran en la tabla:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-364">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

* <span data-ttu-id="f0c1e-365">La clave de configuración se crea al quitar el prefijo de la variable de entorno y al agregar una sección de clave de configuración (`ConnectionStrings`).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-365">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
* <span data-ttu-id="f0c1e-366">Se crea un nuevo par clave-valor de configuración que representa el proveedor de conexión de base de datos (excepto para `CUSTOMCONNSTR_`, que no tiene ningún proveedor indicado).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-366">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="f0c1e-367">Clave de variable de entorno</span><span class="sxs-lookup"><span data-stu-id="f0c1e-367">Environment variable key</span></span> | <span data-ttu-id="f0c1e-368">Clave de configuración convertida</span><span class="sxs-lookup"><span data-stu-id="f0c1e-368">Converted configuration key</span></span> | <span data-ttu-id="f0c1e-369">Entrada de configuración del proveedor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-369">Provider configuration entry</span></span>                                                    |
| ------------------------ | --------------------------- | ------------------------------------------------------------------------------- |
| `CUSTOMCONNSTR_<KEY>`    | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f0c1e-370">Entrada de configuración no creada.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-370">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_<KEY>`     | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f0c1e-371">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-371">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f0c1e-372">Valor: `MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-372">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_<KEY>`  | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f0c1e-373">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-373">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f0c1e-374">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-374">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_<KEY>`       | `ConnectionStrings:<KEY>`   | <span data-ttu-id="f0c1e-375">Clave: `ConnectionStrings:<KEY>_ProviderName`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-375">Key: `ConnectionStrings:<KEY>_ProviderName`:</span></span><br><span data-ttu-id="f0c1e-376">Valor: `System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-376">Value: `System.Data.SqlClient`</span></span>  |

## <a name="file-configuration-provider"></a><span data-ttu-id="f0c1e-377">Proveedor de configuración de archivo</span><span class="sxs-lookup"><span data-stu-id="f0c1e-377">File Configuration Provider</span></span>

<span data-ttu-id="f0c1e-378"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> es la clase base para cargar la configuración del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-378"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="f0c1e-379">Los proveedores de configuración siguientes están dedicados a determinados tipos de archivo:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-379">The following configuration providers are dedicated to specific file types:</span></span>

* [<span data-ttu-id="f0c1e-380">Proveedor de configuración INI</span><span class="sxs-lookup"><span data-stu-id="f0c1e-380">INI Configuration Provider</span></span>](#ini-configuration-provider)
* [<span data-ttu-id="f0c1e-381">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="f0c1e-381">JSON Configuration Provider</span></span>](#json-configuration-provider)
* [<span data-ttu-id="f0c1e-382">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="f0c1e-382">XML Configuration Provider</span></span>](#xml-configuration-provider)

### <a name="ini-configuration-provider"></a><span data-ttu-id="f0c1e-383">Proveedor de configuración de INI</span><span class="sxs-lookup"><span data-stu-id="f0c1e-383">INI Configuration Provider</span></span>

<span data-ttu-id="f0c1e-384"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> carga la configuración desde pares clave-valor de archivo INI en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-384">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> loads configuration from INI file key-value pairs at runtime.</span></span>

<span data-ttu-id="f0c1e-385">Para activar la configuración de archivo INI, llame al método de extensión <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-385">To activate INI file configuration, call the <xref:Microsoft.Extensions.Configuration.IniConfigurationExtensions.AddIniFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f0c1e-386">Los dos puntos se pueden usar como un delimitador de sección en la configuración de archivo INI.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-386">The colon can be used to as a section delimiter in INI file configuration.</span></span>

<span data-ttu-id="f0c1e-387">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-387">Overloads permit specifying:</span></span>

* <span data-ttu-id="f0c1e-388">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-388">Whether the file is optional.</span></span>
* <span data-ttu-id="f0c1e-389">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-389">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f0c1e-390"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-390">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f0c1e-391">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-391">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddIniFile(
        "config.ini", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="f0c1e-392">Un ejemplo genérico de un archivo de configuración INI:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-392">A generic example of an INI configuration file:</span></span>

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

<span data-ttu-id="f0c1e-393">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-393">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f0c1e-394">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-394">section0:key0</span></span>
* <span data-ttu-id="f0c1e-395">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-395">section0:key1</span></span>
* <span data-ttu-id="f0c1e-396">section1:subsection:key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-396">section1:subsection:key</span></span>
* <span data-ttu-id="f0c1e-397">section2:subsection0:key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-397">section2:subsection0:key</span></span>
* <span data-ttu-id="f0c1e-398">section2:subsection1:key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-398">section2:subsection1:key</span></span>

### <a name="json-configuration-provider"></a><span data-ttu-id="f0c1e-399">Proveedor de configuración JSON</span><span class="sxs-lookup"><span data-stu-id="f0c1e-399">JSON Configuration Provider</span></span>

<span data-ttu-id="f0c1e-400"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> carga la configuración desde pares clave-valor de archivos JSON en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-400">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> loads configuration from JSON file key-value pairs during runtime.</span></span>

<span data-ttu-id="f0c1e-401">Para activar la configuración de archivo JSON, llame al método de extensión <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-401">To activate JSON file configuration, call the <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f0c1e-402">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-402">Overloads permit specifying:</span></span>

* <span data-ttu-id="f0c1e-403">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-403">Whether the file is optional.</span></span>
* <span data-ttu-id="f0c1e-404">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-404">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f0c1e-405"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-405">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f0c1e-406">Se llama automáticamente a `AddJsonFile` dos veces cuando un nuevo generador de host se inicializa con `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-406">`AddJsonFile` is automatically called twice when a new host builder is initialized with `CreateDefaultBuilder`.</span></span> <span data-ttu-id="f0c1e-407">Se llama al método para cargar la configuración desde:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-407">The method is called to load configuration from:</span></span>

* <span data-ttu-id="f0c1e-408">*appsettings.json*: este archivo se lee primero.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-408">*appsettings.json* &ndash; This file is read first.</span></span> <span data-ttu-id="f0c1e-409">La versión del entorno del archivo puede invalidar los valores que proporciona el archivo *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-409">The environment version of the file can override the values provided by the *appsettings.json* file.</span></span>
* <span data-ttu-id="f0c1e-410">*appsettings.{Environment}.json*: la versión del entorno del archivo se carga en función de [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-410">*appsettings.{Environment}.json* &ndash; The environment version of the file is loaded based on the [IHostingEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.EnvironmentName*).</span></span>

<span data-ttu-id="f0c1e-411">Para obtener más información, vea la sección [Configuración predeterminada](#default-configuration).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-411">For more information, see the [Default configuration](#default-configuration) section.</span></span>

<span data-ttu-id="f0c1e-412">`CreateDefaultBuilder` también carga:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-412">`CreateDefaultBuilder` also loads:</span></span>

* <span data-ttu-id="f0c1e-413">Variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-413">Environment variables.</span></span>
* <span data-ttu-id="f0c1e-414">[Secretos de usuario (Administrador de secretos)](xref:security/app-secrets) en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-414">[User secrets (Secret Manager)](xref:security/app-secrets) in the Development environment.</span></span>
* <span data-ttu-id="f0c1e-415">Argumentos de la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-415">Command-line arguments.</span></span>

<span data-ttu-id="f0c1e-416">El proveedor de configuración de JSON se establece en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-416">The JSON Configuration Provider is established first.</span></span> <span data-ttu-id="f0c1e-417">Por tanto, los secretos de usuario, las variables de entorno y los argumentos de la línea de comandos invalidan la configuración establecida por los archivos *appsettings*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-417">Therefore, user secrets, environment variables, and command-line arguments override configuration set by the *appsettings* files.</span></span>

<span data-ttu-id="f0c1e-418">Llame a `ConfigureAppConfiguration` al crear el host para especificar la configuración de la aplicación para archivos distintos de *appsettings.json* y *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-418">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration for files other than *appsettings.json* and *appsettings.{Environment}.json*:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddJsonFile(
        "config.json", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="f0c1e-419">**Ejemplo**</span><span class="sxs-lookup"><span data-stu-id="f0c1e-419">**Example**</span></span>

<span data-ttu-id="f0c1e-420">La aplicación de ejemplo aprovecha el método de conveniencia estático `CreateDefaultBuilder` para crear el host, que incluye una llamada a `AddJsonFile`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-420">The sample app takes advantage of the static convenience method `CreateDefaultBuilder` to build the host, which includes two calls to `AddJsonFile`:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="f0c1e-421">La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-421">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="f0c1e-422">La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-422">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="f0c1e-423">Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-423">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/3.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="f0c1e-424">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-424">Run the sample app.</span></span> <span data-ttu-id="f0c1e-425">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-425">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f0c1e-426">La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-426">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="f0c1e-427">El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-427">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="f0c1e-428">Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-428">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="f0c1e-429">Abra el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-429">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="f0c1e-430">En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-430">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="f0c1e-431">Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-431">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="f0c1e-432">La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-432">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="f0c1e-433">El nivel de registro de la clave `Logging:LogLevel:Default` es `Information`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-433">The log level for the key `Logging:LogLevel:Default` is `Information`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="f0c1e-434">La primera llamada a `AddJsonFile` carga la configuración desde *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-434">The first call to `AddJsonFile` loads configuration from *appsettings.json*:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.json)]

* <span data-ttu-id="f0c1e-435">La segunda llamada a `AddJsonFile` carga la configuración desde *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-435">The second call to `AddJsonFile` loads configuration from *appsettings.{Environment}.json*.</span></span> <span data-ttu-id="f0c1e-436">Para *appsettings.Development.json* en la aplicación de ejemplo, se carga el siguiente archivo:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-436">For *appsettings.Development.json* in the sample app, the following file is loaded:</span></span>

  [!code-json[](index/samples/2.x/ConfigurationSample/appsettings.Development.json)]

1. <span data-ttu-id="f0c1e-437">Ejecute la aplicación de ejemplo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-437">Run the sample app.</span></span> <span data-ttu-id="f0c1e-438">Abra un explorador a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-438">Open a browser to the app at `http://localhost:5000`.</span></span>
1. <span data-ttu-id="f0c1e-439">La salida contiene pares clave-valor para la configuración basada en el entorno de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-439">The output contains key-value pairs for the configuration based on the app's environment.</span></span> <span data-ttu-id="f0c1e-440">El nivel de registro de la clave `Logging:LogLevel:Default` es `Debug` al ejecutar la aplicación en el entorno de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-440">The log level for the key `Logging:LogLevel:Default` is `Debug` when running the app in the Development environment.</span></span>
1. <span data-ttu-id="f0c1e-441">Vuelva a ejecutar la aplicación de ejemplo en el entorno de producción:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-441">Run the sample app again in the Production environment:</span></span>
   1. <span data-ttu-id="f0c1e-442">Abra el archivo *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-442">Open the *Properties/launchSettings.json* file.</span></span>
   1. <span data-ttu-id="f0c1e-443">En el perfil de `ConfigurationSample`, cambie el valor de la variable de entorno de `ASPNETCORE_ENVIRONMENT` por `Production`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-443">In the `ConfigurationSample` profile, change the value of the `ASPNETCORE_ENVIRONMENT` environment variable to `Production`.</span></span>
   1. <span data-ttu-id="f0c1e-444">Guarde el archivo y ejecute la aplicación con `dotnet run` en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-444">Save the file and run the app with `dotnet run` in a command shell.</span></span>
1. <span data-ttu-id="f0c1e-445">La configuración de *appsettings.Development.json* ya no invalida la configuración de *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-445">The settings in the *appsettings.Development.json* no longer override the settings in *appsettings.json*.</span></span> <span data-ttu-id="f0c1e-446">El nivel de registro de la clave `Logging:LogLevel:Default` es `Warning`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-446">The log level for the key `Logging:LogLevel:Default` is `Warning`.</span></span>

::: moniker-end

### <a name="xml-configuration-provider"></a><span data-ttu-id="f0c1e-447">Proveedor de configuración XML</span><span class="sxs-lookup"><span data-stu-id="f0c1e-447">XML Configuration Provider</span></span>

<span data-ttu-id="f0c1e-448"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> carga la configuración desde pares clave-valor de archivo XML en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-448">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> loads configuration from XML file key-value pairs at runtime.</span></span>

<span data-ttu-id="f0c1e-449">Para activar la configuración de archivo XML, llame al método de extensión <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-449">To activate XML file configuration, call the <xref:Microsoft.Extensions.Configuration.XmlConfigurationExtensions.AddXmlFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f0c1e-450">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-450">Overloads permit specifying:</span></span>

* <span data-ttu-id="f0c1e-451">Si el archivo es opcional.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-451">Whether the file is optional.</span></span>
* <span data-ttu-id="f0c1e-452">Si la configuración se recarga si el archivo cambia.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-452">Whether the configuration is reloaded if the file changes.</span></span>
* <span data-ttu-id="f0c1e-453"><xref:Microsoft.Extensions.FileProviders.IFileProvider> que se usa para acceder al archivo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-453">The <xref:Microsoft.Extensions.FileProviders.IFileProvider> used to access the file.</span></span>

<span data-ttu-id="f0c1e-454">El nodo raíz del archivo de configuración se omite cuando se crean los pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-454">The root node of the configuration file is ignored when the configuration key-value pairs are created.</span></span> <span data-ttu-id="f0c1e-455">No especifique una definición de tipo de documento (DTD) ni el espacio de nombres en el archivo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-455">Don't specify a Document Type Definition (DTD) or namespace in the file.</span></span>

<span data-ttu-id="f0c1e-456">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-456">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddXmlFile(
        "config.xml", optional: true, reloadOnChange: true);
})
```

<span data-ttu-id="f0c1e-457">Los archivos de configuración XML pueden usar distintos nombres de elemento para las secciones repetidas:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-457">XML configuration files can use distinct element names for repeating sections:</span></span>

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

<span data-ttu-id="f0c1e-458">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-458">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f0c1e-459">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-459">section0:key0</span></span>
* <span data-ttu-id="f0c1e-460">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-460">section0:key1</span></span>
* <span data-ttu-id="f0c1e-461">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-461">section1:key0</span></span>
* <span data-ttu-id="f0c1e-462">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-462">section1:key1</span></span>

<span data-ttu-id="f0c1e-463">Los elementos de repetición que usan el mismo nombre de elemento funcionan si el atributo `name` se usa para distinguir los elementos:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-463">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

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

<span data-ttu-id="f0c1e-464">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-464">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f0c1e-465">section:section0:key:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-465">section:section0:key:key0</span></span>
* <span data-ttu-id="f0c1e-466">section:section0:key:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-466">section:section0:key:key1</span></span>
* <span data-ttu-id="f0c1e-467">section:section1:key:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-467">section:section1:key:key0</span></span>
* <span data-ttu-id="f0c1e-468">section:section1:key:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-468">section:section1:key:key1</span></span>

<span data-ttu-id="f0c1e-469">Los atributos se pueden usar para suministrar valores:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-469">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="f0c1e-470">El archivo de configuración anterior carga las siguientes claves con `value`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-470">The previous configuration file loads the following keys with `value`:</span></span>

* <span data-ttu-id="f0c1e-471">key:attribute</span><span class="sxs-lookup"><span data-stu-id="f0c1e-471">key:attribute</span></span>
* <span data-ttu-id="f0c1e-472">section:key:attribute</span><span class="sxs-lookup"><span data-stu-id="f0c1e-472">section:key:attribute</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="f0c1e-473">Proveedor de configuración de clave por archivo</span><span class="sxs-lookup"><span data-stu-id="f0c1e-473">Key-per-file Configuration Provider</span></span>

<span data-ttu-id="f0c1e-474"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> usa los archivos de un directorio como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-474">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="f0c1e-475">La clave es el nombre de archivo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-475">The key is the file name.</span></span> <span data-ttu-id="f0c1e-476">El valor contiene el contenido del archivo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-476">The value contains the file's contents.</span></span> <span data-ttu-id="f0c1e-477">El proveedor de configuración de clave por archivo se usa en escenarios de hospedaje de Docker.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-477">The Key-per-file Configuration Provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="f0c1e-478">Para activar la configuración de clave por archivo, llame al método de extensión <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-478">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="f0c1e-479">La ruta de acceso `directoryPath` a los archivos debe ser una ruta de acceso absoluta.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-479">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="f0c1e-480">Las sobrecargas permiten especificar:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-480">Overloads permit specifying:</span></span>

* <span data-ttu-id="f0c1e-481">Un delegado `Action<KeyPerFileConfigurationSource>` que configura el origen.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-481">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
* <span data-ttu-id="f0c1e-482">Si el directorio es opcional y la ruta de acceso al directorio.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-482">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="f0c1e-483">El carácter de subrayado doble (`__`) se usa como delimitador de claves de configuración en los nombres de archivo.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-483">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="f0c1e-484">Por ejemplo, el nombre de archivo `Logging__LogLevel__System` genera la clave de configuración `Logging:LogLevel:System`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-484">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="f0c1e-485">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-485">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");
    config.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="f0c1e-486">Proveedor de configuración de memoria</span><span class="sxs-lookup"><span data-stu-id="f0c1e-486">Memory Configuration Provider</span></span>

<span data-ttu-id="f0c1e-487"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> usa una colección en memoria como pares clave-valor de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-487">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="f0c1e-488">Para activar la configuración de colección en memoria, llame al método de extensión <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> en una instancia de <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-488">To activate in-memory collection configuration, call the <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span>

<span data-ttu-id="f0c1e-489">El proveedor de configuración se puede inicializar con un `IEnumerable<KeyValuePair<String,String>>`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-489">The configuration provider can be initialized with an `IEnumerable<KeyValuePair<String,String>>`.</span></span>

<span data-ttu-id="f0c1e-490">Llame a `ConfigureAppConfiguration` cuando cree el host para especificar la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-490">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration.</span></span>

<span data-ttu-id="f0c1e-491">En el ejemplo siguiente, se crea un diccionario de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-491">In the following example, a configuration dictionary is created:</span></span>

```csharp
public static readonly Dictionary<string, string> _dict = 
    new Dictionary<string, string>
    {
        {"MemoryCollectionKey1", "value1"},
        {"MemoryCollectionKey2", "value2"}
    };
```

<span data-ttu-id="f0c1e-492">El diccionario se usa con una llamada a `AddInMemoryCollection` para proporcionar la configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-492">The dictionary is used with a call to `AddInMemoryCollection` to provide the configuration:</span></span>

```csharp
.ConfigureAppConfiguration((hostingContext, config) =>
{
    config.AddInMemoryCollection(_dict);
})
```

## <a name="getvalue"></a><span data-ttu-id="f0c1e-493">GetValue</span><span class="sxs-lookup"><span data-stu-id="f0c1e-493">GetValue</span></span>

<span data-ttu-id="f0c1e-494">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extrae un único valor de la configuración con una clave especificada y lo convierte al tipo especificado que no es de colección.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-494">[ConfigurationBinder.GetValue\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.GetValue*) extracts a single value from configuration with a specified key and converts it to the specified noncollection type.</span></span> <span data-ttu-id="f0c1e-495">Una sobrecarga acepta un valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-495">An overload accepts a default value.</span></span>

<span data-ttu-id="f0c1e-496">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-496">The following example:</span></span>

* <span data-ttu-id="f0c1e-497">Se extrae el valor de cadena de la configuración con la clave `NumberKey`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-497">Extracts the string value from configuration with the key `NumberKey`.</span></span> <span data-ttu-id="f0c1e-498">Si `NumberKey` no se encuentra en las claves de configuración, se usa el valor predeterminado de `99`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-498">If `NumberKey` isn't found in the configuration keys, the default value of `99` is used.</span></span>
* <span data-ttu-id="f0c1e-499">Se escribe el valor como `int`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-499">Types the value as an `int`.</span></span>
* <span data-ttu-id="f0c1e-500">Se almacena el valor en la propiedad `NumberConfig` para usarlo en la página.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-500">Stores the value in the `NumberConfig` property for use by the page.</span></span>

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

## <a name="getsection-getchildren-and-exists"></a><span data-ttu-id="f0c1e-501">GetSection, GetChildren y Exists</span><span class="sxs-lookup"><span data-stu-id="f0c1e-501">GetSection, GetChildren, and Exists</span></span>

<span data-ttu-id="f0c1e-502">Para los ejemplos siguientes, considere el siguiente archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-502">For the examples that follow, consider the following JSON file.</span></span> <span data-ttu-id="f0c1e-503">Se encuentran cuatro claves en dos secciones, una de las cuales incluye un par de subsecciones:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-503">Four keys are found across two sections, one of which includes a pair of subsections:</span></span>

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

<span data-ttu-id="f0c1e-504">Cuando el archivo se lee en la configuración, se crean las siguientes claves jerárquicas únicas para contener los valores de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-504">When the file is read into configuration, the following unique hierarchical keys are created to hold the configuration values:</span></span>

* <span data-ttu-id="f0c1e-505">section0:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-505">section0:key0</span></span>
* <span data-ttu-id="f0c1e-506">section0:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-506">section0:key1</span></span>
* <span data-ttu-id="f0c1e-507">section1:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-507">section1:key0</span></span>
* <span data-ttu-id="f0c1e-508">section1:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-508">section1:key1</span></span>
* <span data-ttu-id="f0c1e-509">section2:subsection0:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-509">section2:subsection0:key0</span></span>
* <span data-ttu-id="f0c1e-510">section2:subsection0:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-510">section2:subsection0:key1</span></span>
* <span data-ttu-id="f0c1e-511">section2:subsection1:key0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-511">section2:subsection1:key0</span></span>
* <span data-ttu-id="f0c1e-512">section2:subsection1:key1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-512">section2:subsection1:key1</span></span>

### <a name="getsection"></a><span data-ttu-id="f0c1e-513">GetSection</span><span class="sxs-lookup"><span data-stu-id="f0c1e-513">GetSection</span></span>

<span data-ttu-id="f0c1e-514">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extrae una subsección de la configuración con la clave de subsección especificada.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-514">[IConfiguration.GetSection](xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection*) extracts a configuration subsection with the specified subsection key.</span></span>

<span data-ttu-id="f0c1e-515">Para devolver un <xref:Microsoft.Extensions.Configuration.IConfigurationSection> que contiene los pares clave-valor en `section1`, llame a `GetSection` y suministre el nombre de la sección:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-515">To return an <xref:Microsoft.Extensions.Configuration.IConfigurationSection> containing only the key-value pairs in `section1`, call `GetSection` and supply the section name:</span></span>

```csharp
var configSection = _config.GetSection("section1");
```

<span data-ttu-id="f0c1e-516">`configSection` no tiene ningún valor, solo una clave y una ruta de acceso.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-516">The `configSection` doesn't have a value, only a key and a path.</span></span>

<span data-ttu-id="f0c1e-517">De forma similar, para obtener los valores de las claves de `section2:subsection0`, llame a `GetSection` y suministre la ruta de acceso a la sección:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-517">Similarly, to obtain the values for keys in `section2:subsection0`, call `GetSection` and supply the section path:</span></span>

```csharp
var configSection = _config.GetSection("section2:subsection0");
```

<span data-ttu-id="f0c1e-518">`GetSection` nunca devuelve `null`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-518">`GetSection` never returns `null`.</span></span> <span data-ttu-id="f0c1e-519">Si no se encuentra una sección que coincida, se devuelve una `IConfigurationSection` vacía.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-519">If a matching section isn't found, an empty `IConfigurationSection` is returned.</span></span>

<span data-ttu-id="f0c1e-520">Cuando `GetSection` devuelve una sección coincidente, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> no se rellena.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-520">When `GetSection` returns a matching section, <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Value> isn't populated.</span></span> <span data-ttu-id="f0c1e-521">Se devuelven <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> y <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> si la sección existe.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-521">A <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Key> and <xref:Microsoft.Extensions.Configuration.IConfigurationSection.Path> are returned when the section exists.</span></span>

### <a name="getchildren"></a><span data-ttu-id="f0c1e-522">GetChildren</span><span class="sxs-lookup"><span data-stu-id="f0c1e-522">GetChildren</span></span>

<span data-ttu-id="f0c1e-523">Una llamada a [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) en `section2` obtiene una `IEnumerable<IConfigurationSection>` que incluye:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-523">A call to [IConfiguration.GetChildren](xref:Microsoft.Extensions.Configuration.IConfiguration.GetChildren*) on `section2` obtains an `IEnumerable<IConfigurationSection>` that includes:</span></span>

* `subsection0`
* `subsection1`

```csharp
var configSection = _config.GetSection("section2");

var children = configSection.GetChildren();
```

### <a name="exists"></a><span data-ttu-id="f0c1e-524">Existe</span><span class="sxs-lookup"><span data-stu-id="f0c1e-524">Exists</span></span>

<span data-ttu-id="f0c1e-525">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) para determinar si existe una sección de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-525">Use [ConfigurationExtensions.Exists](xref:Microsoft.Extensions.Configuration.ConfigurationExtensions.Exists*) to determine if a configuration section exists:</span></span>

```csharp
var sectionExists = _config.GetSection("section2:subsection2").Exists();
```

<span data-ttu-id="f0c1e-526">Dados los datos de ejemplo, `sectionExists` es `false` porque no hay una sección `section2:subsection2` en los datos de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-526">Given the example data, `sectionExists` is `false` because there isn't a `section2:subsection2` section in the configuration data.</span></span>

## <a name="bind-to-a-class"></a><span data-ttu-id="f0c1e-527">Enlace a una clase</span><span class="sxs-lookup"><span data-stu-id="f0c1e-527">Bind to a class</span></span>

<span data-ttu-id="f0c1e-528">La configuración se puede enlazar a clases que representan grupos de configuraciones relacionadas a través del *patrón de opciones*.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-528">Configuration can be bound to classes that represent groups of related settings using the *options pattern*.</span></span> <span data-ttu-id="f0c1e-529">Para obtener más información, vea <xref:fundamentals/configuration/options>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-529">For more information, see <xref:fundamentals/configuration/options>.</span></span>

<span data-ttu-id="f0c1e-530">Los valores de configuración se devuelven como cadenas, pero llamar a <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> permite la construcción de objetos [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-530">Configuration values are returned as strings, but calling <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> enables the construction of [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objects.</span></span> <span data-ttu-id="f0c1e-531">El enlazador enlaza los valores con todas las propiedades de lectura o escritura públicas del tipo proporcionado.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-531">The binder binds values to all of the public read/write properties of the type provided.</span></span> <span data-ttu-id="f0c1e-532">Los campos **no** se enlazan.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-532">Fields are **not** bound.</span></span>

<span data-ttu-id="f0c1e-533">La aplicación de ejemplo contiene un modelo `Starship` (*Models/Starship.cs*):</span><span class="sxs-lookup"><span data-stu-id="f0c1e-533">The sample app contains a `Starship` model (*Models/Starship.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/Starship.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f0c1e-534">La sección `starship` del archivo *starship.json* crea la configuración cuando la aplicación de ejemplo usa el proveedor de configuración JSON para cargar la configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-534">The `starship` section of the *starship.json* file creates the configuration when the sample app uses the JSON Configuration Provider to load the configuration:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/starship.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/starship.json)]

::: moniker-end

<span data-ttu-id="f0c1e-535">Se crean los siguientes pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-535">The following configuration key-value pairs are created:</span></span>

| <span data-ttu-id="f0c1e-536">Key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-536">Key</span></span>                   | <span data-ttu-id="f0c1e-537">Valor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-537">Value</span></span>                                             |
| --------------------- | ------------------------------------------------- |
| <span data-ttu-id="f0c1e-538">starship:name</span><span class="sxs-lookup"><span data-stu-id="f0c1e-538">starship:name</span></span>         | <span data-ttu-id="f0c1e-539">USS Enterprise</span><span class="sxs-lookup"><span data-stu-id="f0c1e-539">USS Enterprise</span></span>                                    |
| <span data-ttu-id="f0c1e-540">starship:registry</span><span class="sxs-lookup"><span data-stu-id="f0c1e-540">starship:registry</span></span>     | <span data-ttu-id="f0c1e-541">NCC-1701</span><span class="sxs-lookup"><span data-stu-id="f0c1e-541">NCC-1701</span></span>                                          |
| <span data-ttu-id="f0c1e-542">starship:class</span><span class="sxs-lookup"><span data-stu-id="f0c1e-542">starship:class</span></span>        | <span data-ttu-id="f0c1e-543">Constitution</span><span class="sxs-lookup"><span data-stu-id="f0c1e-543">Constitution</span></span>                                      |
| <span data-ttu-id="f0c1e-544">starship:length</span><span class="sxs-lookup"><span data-stu-id="f0c1e-544">starship:length</span></span>       | <span data-ttu-id="f0c1e-545">304.8</span><span class="sxs-lookup"><span data-stu-id="f0c1e-545">304.8</span></span>                                             |
| <span data-ttu-id="f0c1e-546">starship:commissioned</span><span class="sxs-lookup"><span data-stu-id="f0c1e-546">starship:commissioned</span></span> | <span data-ttu-id="f0c1e-547">False</span><span class="sxs-lookup"><span data-stu-id="f0c1e-547">False</span></span>                                             |
| <span data-ttu-id="f0c1e-548">trademark</span><span class="sxs-lookup"><span data-stu-id="f0c1e-548">trademark</span></span>             | <span data-ttu-id="f0c1e-549">Paramount Pictures Corp. https://www.paramount.com</span><span class="sxs-lookup"><span data-stu-id="f0c1e-549">Paramount Pictures Corp. https://www.paramount.com</span></span> |

<span data-ttu-id="f0c1e-550">La aplicación de ejemplo llama a `GetSection` con la clave `starship`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-550">The sample app calls `GetSection` with the `starship` key.</span></span> <span data-ttu-id="f0c1e-551">Los pares clave-valor `starship` están aislados.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-551">The `starship` key-value pairs are isolated.</span></span> <span data-ttu-id="f0c1e-552">El método `Bind` se llama en la subsección que pasa una instancia de la clase `Starship`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-552">The `Bind` method is called on the subsection passing in an instance of the `Starship` class.</span></span> <span data-ttu-id="f0c1e-553">Después de enlazar los valores de instancia, la instancia está asignada a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-553">After binding the instance values, the instance is assigned to a property for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_starship)]

::: moniker-end

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="f0c1e-554">Enlazar a un gráfico de objetos</span><span class="sxs-lookup"><span data-stu-id="f0c1e-554">Bind to an object graph</span></span>

<span data-ttu-id="f0c1e-555"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> es capaz de enlazar todo un gráfico de objetos POCO.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-555"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> is capable of binding an entire POCO object graph.</span></span> <span data-ttu-id="f0c1e-556">Al igual que ocurre con el enlace de un objeto simple, solo se enlazan las propiedades públicas de lectura o escritura.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-556">As with binding a simple object, only public read/write properties are bound.</span></span>

<span data-ttu-id="f0c1e-557">El ejemplo contiene un modelo `TvShow` cuyo gráfico de objetos incluye las clases `Metadata` y `Actors` (*Models/TvShow.cs*):</span><span class="sxs-lookup"><span data-stu-id="f0c1e-557">The sample contains a `TvShow` model whose object graph includes `Metadata` and `Actors` classes (*Models/TvShow.cs*):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/TvShow.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f0c1e-558">La aplicación de ejemplo tiene un archivo *tvshow.xml* que contiene los datos de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-558">The sample app has a *tvshow.xml* file containing the configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-xml[](index/samples/3.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-xml[](index/samples/2.x/ConfigurationSample/tvshow.xml)]

::: moniker-end

<span data-ttu-id="f0c1e-559">Configuración está enlazada a todo el gráfico de objetos `TvShow`con el método `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-559">Configuration is bound to the entire `TvShow` object graph with the `Bind` method.</span></span> <span data-ttu-id="f0c1e-560">La instancia enlazada se asigna a una propiedad para la representación:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-560">The bound instance is assigned to a property for rendering:</span></span>

```csharp
var tvShow = new TvShow();
_config.GetSection("tvshow").Bind(tvShow);
TvShow = tvShow;
```

<span data-ttu-id="f0c1e-561">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) enlaza y devuelve el tipo especificado.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-561">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) binds and returns the specified type.</span></span> <span data-ttu-id="f0c1e-562">`Get<T>` es más conveniente que usar `Bind`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-562">`Get<T>` is more convenient than using `Bind`.</span></span> <span data-ttu-id="f0c1e-563">El código siguiente muestra cómo usar `Get<T>` con el ejemplo anterior, lo que permite que la instancia enlazada se asigne directamente a la propiedad que se usa para la representación:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-563">The following code shows how to use `Get<T>` with the preceding example, which allows the bound instance to be directly assigned to the property used for rendering:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_tvshow)]

::: moniker-end

## <a name="bind-an-array-to-a-class"></a><span data-ttu-id="f0c1e-564">Enlace de una matriz a una clase</span><span class="sxs-lookup"><span data-stu-id="f0c1e-564">Bind an array to a class</span></span>

<span data-ttu-id="f0c1e-565">*La aplicación de ejemplo muestra los conceptos que se explican en esta sección.*</span><span class="sxs-lookup"><span data-stu-id="f0c1e-565">*The sample app demonstrates the concepts explained in this section.*</span></span>

<span data-ttu-id="f0c1e-566"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> admite enlazar matrices a objetos con los índices de matriz en las claves de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-566">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind*> supports binding arrays to objects using array indices in configuration keys.</span></span> <span data-ttu-id="f0c1e-567">Cualquier formato de matriz que expone un segmento de clave numérica (`:0:`, `:1:`, &hellip; `:{n}:`) es capaz de enlazar una matriz a una matriz de clase POCO.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-567">Any array format that exposes a numeric key segment (`:0:`, `:1:`, &hellip; `:{n}:`) is capable of array binding to a POCO class array.</span></span>

> [!NOTE]
> <span data-ttu-id="f0c1e-568">El enlace se proporciona por convención.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-568">Binding is provided by convention.</span></span> <span data-ttu-id="f0c1e-569">Los proveedores de configuración personalizados no son necesarios para implementar el enlace de matriz.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-569">Custom configuration providers aren't required to implement array binding.</span></span>

<span data-ttu-id="f0c1e-570">**Procesamiento de matriz en memoria**</span><span class="sxs-lookup"><span data-stu-id="f0c1e-570">**In-memory array processing**</span></span>

<span data-ttu-id="f0c1e-571">Considere los valores y las claves de configuración que se muestran en esta tabla.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-571">Consider the configuration keys and values shown in the following table.</span></span>

| <span data-ttu-id="f0c1e-572">Key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-572">Key</span></span>             | <span data-ttu-id="f0c1e-573">Valor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-573">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="f0c1e-574">array:entries:0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-574">array:entries:0</span></span> | <span data-ttu-id="f0c1e-575">value0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-575">value0</span></span> |
| <span data-ttu-id="f0c1e-576">array:entries:1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-576">array:entries:1</span></span> | <span data-ttu-id="f0c1e-577">value1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-577">value1</span></span> |
| <span data-ttu-id="f0c1e-578">array:entries:2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-578">array:entries:2</span></span> | <span data-ttu-id="f0c1e-579">value2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-579">value2</span></span> |
| <span data-ttu-id="f0c1e-580">array:entries:4</span><span class="sxs-lookup"><span data-stu-id="f0c1e-580">array:entries:4</span></span> | <span data-ttu-id="f0c1e-581">value4</span><span class="sxs-lookup"><span data-stu-id="f0c1e-581">value4</span></span> |
| <span data-ttu-id="f0c1e-582">array:entries:5</span><span class="sxs-lookup"><span data-stu-id="f0c1e-582">array:entries:5</span></span> | <span data-ttu-id="f0c1e-583">value5</span><span class="sxs-lookup"><span data-stu-id="f0c1e-583">value5</span></span> |

<span data-ttu-id="f0c1e-584">Estas claves y valores se cargan en la aplicación de ejemplo a través del proveedor de configuración de memoria:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-584">These keys and values are loaded in the sample app using the Memory Configuration Provider:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=5-12,22)]

::: moniker-end

<span data-ttu-id="f0c1e-585">La matriz omite un valor de índice &num;3.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-585">The array skips a value for index &num;3.</span></span> <span data-ttu-id="f0c1e-586">El enlazador de configuración no es capaz de enlazar los valores NULL ni de crear entradas NULL en los objetos enlazados, lo que queda claro cuando se muestra el resultado de enlazar esta matriz a un objeto.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-586">The configuration binder isn't capable of binding null values or creating null entries in bound objects, which becomes clear in a moment when the result of binding this array to an object is demonstrated.</span></span>

<span data-ttu-id="f0c1e-587">En la aplicación de ejemplo, hay disponible una clase POCO para almacenar los datos de configuración enlazados:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-587">In the sample app, a POCO class is available to hold the bound configuration data:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/ArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f0c1e-588">Los datos de configuración están enlazados al objeto:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-588">The configuration data is bound to the object:</span></span>

```csharp
var arrayExample = new ArrayExample();
_config.GetSection("array").Bind(arrayExample);
```

<span data-ttu-id="f0c1e-589">También se puede usar la sintaxis de [ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*), lo que genera un código más compacto:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-589">[ConfigurationBinder.Get\<T>](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get*) syntax can also be used, which results in more compact code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Pages/Index.cshtml.cs?name=snippet_array)]

::: moniker-end

<span data-ttu-id="f0c1e-590">El objeto enlazado, una instancia de `ArrayExample`, recibe los datos de la matriz desde la configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-590">The bound object, an instance of `ArrayExample`, receives the array data from configuration.</span></span>

| <span data-ttu-id="f0c1e-591">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-591">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="f0c1e-592">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-592">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="f0c1e-593">0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-593">0</span></span>                            | <span data-ttu-id="f0c1e-594">value0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-594">value0</span></span>                       |
| <span data-ttu-id="f0c1e-595">1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-595">1</span></span>                            | <span data-ttu-id="f0c1e-596">value1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-596">value1</span></span>                       |
| <span data-ttu-id="f0c1e-597">2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-597">2</span></span>                            | <span data-ttu-id="f0c1e-598">value2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-598">value2</span></span>                       |
| <span data-ttu-id="f0c1e-599">3</span><span class="sxs-lookup"><span data-stu-id="f0c1e-599">3</span></span>                            | <span data-ttu-id="f0c1e-600">value4</span><span class="sxs-lookup"><span data-stu-id="f0c1e-600">value4</span></span>                       |
| <span data-ttu-id="f0c1e-601">4</span><span class="sxs-lookup"><span data-stu-id="f0c1e-601">4</span></span>                            | <span data-ttu-id="f0c1e-602">value5</span><span class="sxs-lookup"><span data-stu-id="f0c1e-602">value5</span></span>                       |

<span data-ttu-id="f0c1e-603">El índice &num;3 en el objeto enlazado contiene los datos de configuración para la clave de configuración `array:4` y su valor de `value4`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-603">Index &num;3 in the bound object holds the configuration data for the `array:4` configuration key and its value of `value4`.</span></span> <span data-ttu-id="f0c1e-604">Cuando se enlazan datos de configuración que contienen una matriz, los índices de la matriz en las claves de configuración solo se usan para iterar los datos de configuración al crear el objeto.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-604">When configuration data containing an array is bound, the array indices in the configuration keys are merely used to iterate the configuration data when creating the object.</span></span> <span data-ttu-id="f0c1e-605">No se puede conservar un valor NULL en los datos de configuración y no se crea una entrada con valores NULL en un objeto enlazado cuando una matriz en las claves de configuración omite uno o más índices.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-605">A null value can't be retained in configuration data, and a null-valued entry isn't created in a bound object when an array in configuration keys skip one or more indices.</span></span>

<span data-ttu-id="f0c1e-606">El elemento de configuración omitido para el índice &num;3 se puede proporcionar antes del enlace a la instancia `ArrayExample` por cualquier proveedor de configuración que genera el par clave-valor correcto en la configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-606">The missing configuration item for index &num;3 can be supplied before binding to the `ArrayExample` instance by any configuration provider that produces the correct key-value pair in configuration.</span></span> <span data-ttu-id="f0c1e-607">Si el ejemplo incluía un proveedor de configuración JSON adicional con el par clave-valor omitido, `ArrayExample.Entries` coincide con la matriz de configuración completa:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-607">If the sample included an additional JSON Configuration Provider with the missing key-value pair, the `ArrayExample.Entries` matches the complete configuration array:</span></span>

<span data-ttu-id="f0c1e-608">*missing_value.json*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-608">*missing_value.json*:</span></span>

```json
{
  "array:entries:3": "value3"
}
```

<span data-ttu-id="f0c1e-609">En `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-609">In `ConfigureAppConfiguration`:</span></span>

```csharp
config.AddJsonFile(
    "missing_value.json", optional: false, reloadOnChange: false);
```

<span data-ttu-id="f0c1e-610">El par clave-valor que se muestra en la tabla se carga en la configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-610">The key-value pair shown in the table is loaded into configuration.</span></span>

| <span data-ttu-id="f0c1e-611">Key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-611">Key</span></span>             | <span data-ttu-id="f0c1e-612">Valor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-612">Value</span></span>  |
| :-------------: | :----: |
| <span data-ttu-id="f0c1e-613">array:entries:3</span><span class="sxs-lookup"><span data-stu-id="f0c1e-613">array:entries:3</span></span> | <span data-ttu-id="f0c1e-614">value3</span><span class="sxs-lookup"><span data-stu-id="f0c1e-614">value3</span></span> |

<span data-ttu-id="f0c1e-615">Si la instancia de clase `ArrayExample` se enlaza después de que el proveedor de configuración JSON incluye la entrada para el índice &num;3, la matriz `ArrayExample.Entries` incluye el valor.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-615">If the `ArrayExample` class instance is bound after the JSON Configuration Provider includes the entry for index &num;3, the `ArrayExample.Entries` array includes the value.</span></span>

| <span data-ttu-id="f0c1e-616">Índice de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-616">`ArrayExample.Entries` Index</span></span> | <span data-ttu-id="f0c1e-617">Valor de `ArrayExample.Entries`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-617">`ArrayExample.Entries` Value</span></span> |
| :--------------------------: | :--------------------------: |
| <span data-ttu-id="f0c1e-618">0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-618">0</span></span>                            | <span data-ttu-id="f0c1e-619">value0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-619">value0</span></span>                       |
| <span data-ttu-id="f0c1e-620">1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-620">1</span></span>                            | <span data-ttu-id="f0c1e-621">value1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-621">value1</span></span>                       |
| <span data-ttu-id="f0c1e-622">2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-622">2</span></span>                            | <span data-ttu-id="f0c1e-623">value2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-623">value2</span></span>                       |
| <span data-ttu-id="f0c1e-624">3</span><span class="sxs-lookup"><span data-stu-id="f0c1e-624">3</span></span>                            | <span data-ttu-id="f0c1e-625">value3</span><span class="sxs-lookup"><span data-stu-id="f0c1e-625">value3</span></span>                       |
| <span data-ttu-id="f0c1e-626">4</span><span class="sxs-lookup"><span data-stu-id="f0c1e-626">4</span></span>                            | <span data-ttu-id="f0c1e-627">value4</span><span class="sxs-lookup"><span data-stu-id="f0c1e-627">value4</span></span>                       |
| <span data-ttu-id="f0c1e-628">5</span><span class="sxs-lookup"><span data-stu-id="f0c1e-628">5</span></span>                            | <span data-ttu-id="f0c1e-629">value5</span><span class="sxs-lookup"><span data-stu-id="f0c1e-629">value5</span></span>                       |

<span data-ttu-id="f0c1e-630">**Procesamiento de matriz JSON**</span><span class="sxs-lookup"><span data-stu-id="f0c1e-630">**JSON array processing**</span></span>

<span data-ttu-id="f0c1e-631">Si un archivo JSON contiene una matriz, se crean claves de configuración para los elementos de la matriz con un índice de sección basado en cero.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-631">If a JSON file contains an array, configuration keys are created for the array elements with a zero-based section index.</span></span> <span data-ttu-id="f0c1e-632">En el siguiente archivo de configuración, `subsection` es una matriz:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-632">In the following configuration file, `subsection` is an array:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-json[](index/samples/3.x/ConfigurationSample/json_array.json)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-json[](index/samples/2.x/ConfigurationSample/json_array.json)]

::: moniker-end

<span data-ttu-id="f0c1e-633">El proveedor de configuración JSON lee los datos de configuración en los siguientes pares clave-valor:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-633">The JSON Configuration Provider reads the configuration data into the following key-value pairs:</span></span>

| <span data-ttu-id="f0c1e-634">Key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-634">Key</span></span>                     | <span data-ttu-id="f0c1e-635">Valor</span><span class="sxs-lookup"><span data-stu-id="f0c1e-635">Value</span></span>  |
| ----------------------- | :----: |
| <span data-ttu-id="f0c1e-636">json_array:key</span><span class="sxs-lookup"><span data-stu-id="f0c1e-636">json_array:key</span></span>          | <span data-ttu-id="f0c1e-637">valueA</span><span class="sxs-lookup"><span data-stu-id="f0c1e-637">valueA</span></span> |
| <span data-ttu-id="f0c1e-638">json_array:subsection:0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-638">json_array:subsection:0</span></span> | <span data-ttu-id="f0c1e-639">valueB</span><span class="sxs-lookup"><span data-stu-id="f0c1e-639">valueB</span></span> |
| <span data-ttu-id="f0c1e-640">json_array:subsection:1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-640">json_array:subsection:1</span></span> | <span data-ttu-id="f0c1e-641">valueC</span><span class="sxs-lookup"><span data-stu-id="f0c1e-641">valueC</span></span> |
| <span data-ttu-id="f0c1e-642">json_array:subsection:2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-642">json_array:subsection:2</span></span> | <span data-ttu-id="f0c1e-643">valueD</span><span class="sxs-lookup"><span data-stu-id="f0c1e-643">valueD</span></span> |

<span data-ttu-id="f0c1e-644">En la aplicación de ejemplo, la clase POCO siguiente está disponible para enlazar los pares clave-valor de configuración:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-644">In the sample app, the following POCO class is available to bind the configuration key-value pairs:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/JsonArrayExample.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="f0c1e-645">Después del enlace, `JsonArrayExample.Key` contiene el valor `valueA`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-645">After binding, `JsonArrayExample.Key` holds the value `valueA`.</span></span> <span data-ttu-id="f0c1e-646">Los valores de la subsección se almacenan en la propiedad de la matriz POCO, `Subsection`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-646">The subsection values are stored in the POCO array property, `Subsection`.</span></span>

| <span data-ttu-id="f0c1e-647">Índice de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-647">`JsonArrayExample.Subsection` Index</span></span> | <span data-ttu-id="f0c1e-648">Valor de `JsonArrayExample.Subsection`</span><span class="sxs-lookup"><span data-stu-id="f0c1e-648">`JsonArrayExample.Subsection` Value</span></span> |
| :---------------------------------: | :---------------------------------: |
| <span data-ttu-id="f0c1e-649">0</span><span class="sxs-lookup"><span data-stu-id="f0c1e-649">0</span></span>                                   | <span data-ttu-id="f0c1e-650">valueB</span><span class="sxs-lookup"><span data-stu-id="f0c1e-650">valueB</span></span>                              |
| <span data-ttu-id="f0c1e-651">1</span><span class="sxs-lookup"><span data-stu-id="f0c1e-651">1</span></span>                                   | <span data-ttu-id="f0c1e-652">valueC</span><span class="sxs-lookup"><span data-stu-id="f0c1e-652">valueC</span></span>                              |
| <span data-ttu-id="f0c1e-653">2</span><span class="sxs-lookup"><span data-stu-id="f0c1e-653">2</span></span>                                   | <span data-ttu-id="f0c1e-654">valueD</span><span class="sxs-lookup"><span data-stu-id="f0c1e-654">valueD</span></span>                              |

## <a name="custom-configuration-provider"></a><span data-ttu-id="f0c1e-655">Proveedores de configuración personalizada</span><span class="sxs-lookup"><span data-stu-id="f0c1e-655">Custom configuration provider</span></span>

<span data-ttu-id="f0c1e-656">La aplicación de ejemplo muestra cómo crear un proveedor de configuración básica que lee los pares clave-valor de configuración desde una base de datos mediante [Entity Framework (EF)](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-656">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF)](/ef/core/).</span></span>

<span data-ttu-id="f0c1e-657">El proveedor tiene las siguientes características:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-657">The provider has the following characteristics:</span></span>

* <span data-ttu-id="f0c1e-658">La base de datos en memoria de EF se usa para fines de demostración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-658">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="f0c1e-659">Para usar una base de datos que requiere una cadena de conexión, implemente un `ConfigurationBuilder` secundario para suministrar la cadena de conexión desde otro proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-659">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
* <span data-ttu-id="f0c1e-660">El proveedor lee una tabla de base de datos en la configuración en el inicio.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-660">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="f0c1e-661">El proveedor no consulta la base de datos por clave.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-661">The provider doesn't query the database on a per-key basis.</span></span>
* <span data-ttu-id="f0c1e-662">La función de recarga en cambio no se implementa, por lo que actualizar la base de datos después del inicio de la aplicación no afecta a la configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-662">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="f0c1e-663">Defina una entidad `EFConfigurationValue` para almacenar los valores de configuración en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-663">Define an `EFConfigurationValue` entity for storing configuration values in the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f0c1e-664">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-664">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="f0c1e-665">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-665">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="f0c1e-666">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-666">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="f0c1e-667">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-667">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="f0c1e-668">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-668">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="f0c1e-669">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-669">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="f0c1e-670">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-670">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="f0c1e-671">Puesto que las [claves de configuración no distinguen entre mayúsculas y minúsculas](#keys), el diccionario empleado para iniciar la base de datos se crea con el comparador que tampoco hace tal distinción ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span><span class="sxs-lookup"><span data-stu-id="f0c1e-671">Since [configuration keys are case-insensitive](#keys), the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="f0c1e-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-672">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="f0c1e-673">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-673">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="f0c1e-674">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-674">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="f0c1e-675">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-675">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/3.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f0c1e-676">*Models/EFConfigurationValue.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-676">*Models/EFConfigurationValue.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Models/EFConfigurationValue.cs?name=snippet1)]

<span data-ttu-id="f0c1e-677">Agregue un `EFConfigurationContext` para almacenar y tener acceso a los valores configurados.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-677">Add an `EFConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="f0c1e-678">*EFConfigurationProvider/EFConfigurationContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-678">*EFConfigurationProvider/EFConfigurationContext.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="f0c1e-679">Cree una clase que implemente <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-679">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="f0c1e-680">*EFConfigurationProvider/EFConfigurationSource.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-680">*EFConfigurationProvider/EFConfigurationSource.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationSource.cs?name=snippet1)]

<span data-ttu-id="f0c1e-681">Cree el proveedor de configuración personalizado heredando de <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-681">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="f0c1e-682">El proveedor de configuración inicializa la base de datos cuando está vacía.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-682">The configuration provider initializes the database when it's empty.</span></span>

<span data-ttu-id="f0c1e-683">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-683">*EFConfigurationProvider/EFConfigurationProvider.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/EFConfigurationProvider/EFConfigurationProvider.cs?name=snippet1)]

<span data-ttu-id="f0c1e-684">Un método de extensión `AddEFConfiguration` permite agregar el origen de configuración a un `ConfigurationBuilder`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-684">An `AddEFConfiguration` extension method permits adding the configuration source to a `ConfigurationBuilder`.</span></span>

<span data-ttu-id="f0c1e-685">*Extensions/EntityFrameworkExtensions.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-685">*Extensions/EntityFrameworkExtensions.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Extensions/EntityFrameworkExtensions.cs?name=snippet1)]

<span data-ttu-id="f0c1e-686">En el código siguiente se muestra cómo puede usar el `EFConfigurationProvider` personalizado en *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-686">The following code shows how to use the custom `EFConfigurationProvider` in *Program.cs*:</span></span>

[!code-csharp[](index/samples/2.x/ConfigurationSample/Program.cs?name=snippet_Program&highlight=29-30)]

::: moniker-end

## <a name="access-configuration-during-startup"></a><span data-ttu-id="f0c1e-687">Acceso a la configuración durante el inicio</span><span class="sxs-lookup"><span data-stu-id="f0c1e-687">Access configuration during startup</span></span>

<span data-ttu-id="f0c1e-688">Inserte `IConfiguration` en el constructor `Startup` para acceder a los valores de configuración en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-688">Inject `IConfiguration` into the `Startup` constructor to access configuration values in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="f0c1e-689">Para acceder a la configuración en `Startup.Configure`, inserte `IConfiguration` directamente en el método o use la instancia desde el constructor:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-689">To access configuration in `Startup.Configure`, either inject `IConfiguration` directly into the method or use the instance from the constructor:</span></span>

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

<span data-ttu-id="f0c1e-690">Para ver un ejemplo de cómo acceder a la configuración mediante métodos de conveniencia de inicio, consulte [Inicio de la aplicación: Métodos de conveniencia](xref:fundamentals/startup#convenience-methods)</span><span class="sxs-lookup"><span data-stu-id="f0c1e-690">For an example of accessing configuration using startup convenience methods, see [App startup: Convenience methods](xref:fundamentals/startup#convenience-methods).</span></span>

## <a name="access-configuration-in-a-razor-pages-page-or-mvc-view"></a><span data-ttu-id="f0c1e-691">Acceso a la configuración en una página de Razor Pages o en una vista de MVC.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-691">Access configuration in a Razor Pages page or MVC view</span></span>

<span data-ttu-id="f0c1e-692">Para obtener acceso a los valores de configuración en una página de Razor Pages o una vista de MVC, agregue una [directiva using](xref:mvc/views/razor#using) ([referencia de C#: directiva using](/dotnet/csharp/language-reference/keywords/using-directive)) para el [espacio de nombres Microsoft.Extensions.Configuration](xref:Microsoft.Extensions.Configuration) e inyecte <xref:Microsoft.Extensions.Configuration.IConfiguration> en la página o en la vista.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-692">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](xref:Microsoft.Extensions.Configuration) and inject <xref:Microsoft.Extensions.Configuration.IConfiguration> into the page or view.</span></span>

<span data-ttu-id="f0c1e-693">En una página de las páginas de Razor:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-693">In a Razor Pages page:</span></span>

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

<span data-ttu-id="f0c1e-694">En una vista de MVC:</span><span class="sxs-lookup"><span data-stu-id="f0c1e-694">In an MVC view:</span></span>

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

## <a name="add-configuration-from-an-external-assembly"></a><span data-ttu-id="f0c1e-695">Agregar configuración a partir de un ensamblado externo</span><span class="sxs-lookup"><span data-stu-id="f0c1e-695">Add configuration from an external assembly</span></span>

<span data-ttu-id="f0c1e-696">Una implementación de <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> permite agregar mejoras a una aplicación al iniciarla a partir de un ensamblado externo fuera de la clase `Startup` de esta.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-696">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="f0c1e-697">Para obtener más información, vea <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="f0c1e-697">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f0c1e-698">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f0c1e-698">Additional resources</span></span>

* <xref:fundamentals/configuration/options>
