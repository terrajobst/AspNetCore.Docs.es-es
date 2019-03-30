---
title: Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar y recuperar información confidencial como secretos de aplicación durante el desarrollo de una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 18313f8284e81d196cbe786f494a607ee97a299f
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750972"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="a2ebd-103">Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2ebd-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="a2ebd-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="a2ebd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="a2ebd-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a2ebd-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a2ebd-106">Este documento explica técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="a2ebd-107">Nunca almacene contraseñas u otros datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="a2ebd-108">Secretos de producción no deben usarse para el desarrollo o prueba.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="a2ebd-109">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="a2ebd-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="a2ebd-110">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="a2ebd-110">Environment variables</span></span>

<span data-ttu-id="a2ebd-111">Las variables de entorno se usan para evitar el almacenamiento de secretos de aplicación en el código o en archivos de configuración local.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="a2ebd-112">Las variables de entorno invalidan los valores de configuración para todos los orígenes de configuración especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="a2ebd-113">Configurar la lectura de valores de variables de entorno mediante una llamada a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="a2ebd-114">Considere la posibilidad de una aplicación web de ASP.NET Core en el que **cuentas de usuario individuales** está habilitada la seguridad.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="a2ebd-115">Una cadena de conexión de base de datos predeterminada se incluye en el proyecto *appsettings.json* archivo con la clave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="a2ebd-116">La cadena de conexión predeterminada es LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="a2ebd-117">Durante la implementación de aplicaciones, el `DefaultConnection` se puede invalidar el valor de clave con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="a2ebd-118">La variable de entorno puede almacenar la cadena de conexión completa con credenciales confidenciales.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="a2ebd-119">Las variables de entorno se almacenan normalmente en texto sin formato y sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="a2ebd-120">Si la máquina o el proceso se ve comprometido, las variables de entorno pueden tener acceso por partes de confianza.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="a2ebd-121">Medidas adicionales para evitar la divulgación de secretos de usuario pueden ser necesarias.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="a2ebd-122">Administrador de secretos</span><span class="sxs-lookup"><span data-stu-id="a2ebd-122">Secret Manager</span></span>

<span data-ttu-id="a2ebd-123">La herramienta Secret Manager almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="a2ebd-124">En este contexto, un fragmento de datos confidenciales es un secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="a2ebd-125">Los secretos de aplicación se almacenan en una ubicación independiente desde el árbol del proyecto.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="a2ebd-126">Los secretos de aplicación se asociado con un proyecto específico o se comparten entre varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="a2ebd-127">No se comprueban los secretos de aplicación en control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="a2ebd-128">La herramienta Secret Manager no cifra los secretos almacenados y no debe tratarse como un almacén de confianza.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="a2ebd-129">Es solo con fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-129">It's for development purposes only.</span></span> <span data-ttu-id="a2ebd-130">Las claves y valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="a2ebd-131">Cómo funciona la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="a2ebd-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="a2ebd-132">La herramienta Secret Manager abstrae los detalles de implementación, como dónde y cómo se almacenan los valores.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="a2ebd-133">Puede usar la herramienta sin conocer estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="a2ebd-134">Los valores se almacenan en un archivo de configuración de JSON en una carpeta de perfil de usuario protegidos por el sistema en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="a2ebd-135">Windows</span><span class="sxs-lookup"><span data-stu-id="a2ebd-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="a2ebd-136">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="a2ebd-137">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="a2ebd-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="a2ebd-138">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="a2ebd-139">En la anterior, las rutas de acceso de archivo, reemplace `<user_secrets_id>` con el `UserSecretsId` valor especificado en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="a2ebd-140">No escriba código que depende de la ubicación o el formato de datos guardados con la herramienta Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="a2ebd-141">Pueden cambiar estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-141">These implementation details may change.</span></span> <span data-ttu-id="a2ebd-142">Por ejemplo, los valores de secreto no se cifran, pero podrían ser en el futuro.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="a2ebd-143">Instalar la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="a2ebd-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="a2ebd-144">La herramienta Secret Manager está empaquetado con la CLI de .NET Core SDK de .NET Core 2.1.300 o posterior.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="a2ebd-145">Para las versiones de SDK de .NET Core 2.1.300 antes de la instalación de herramientas es necesaria.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="a2ebd-146">Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión del SDK de .NET Core instalado.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="a2ebd-147">Se mostrará una advertencia si se usa el SDK de .NET Core incluye la herramienta:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="a2ebd-148">Instalar el [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) paquete de NuGet en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="a2ebd-149">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="a2ebd-150">Ejecute el siguiente comando en un shell de comandos para validar la instalación de herramienta:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="a2ebd-151">La herramienta Secret Manager muestra el uso de ejemplo, las opciones y ayuda del comando:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> <span data-ttu-id="a2ebd-152">Debe estar en el mismo directorio que el *.csproj* archivo va a ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="a2ebd-153">Habilitar el almacenamiento de secretos</span><span class="sxs-lookup"><span data-stu-id="a2ebd-153">Enable secret storage</span></span>

<span data-ttu-id="a2ebd-154">La herramienta Secret Manager opera en valores de configuración de específicas del proyecto almacenados en su perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2ebd-155">La herramienta Secret Manager incluye una `init` comando 3.0.100 del SDK de .NET Core o posterior.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="a2ebd-156">Para usar secretos de usuario, ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-156">To use user secrets, run the following command in the project directory:</span></span>

```console
dotnet user-secrets init
```

<span data-ttu-id="a2ebd-157">El comando anterior agrega una `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="a2ebd-158">De forma predeterminada, el texto interno de `UserSecretsId` es un GUID.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="a2ebd-159">El texto interno es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="a2ebd-160">Para usar secretos de usuario, defina un `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="a2ebd-161">El texto interno de `UserSecretsId` es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="a2ebd-162">Los desarrolladores suelen generan un GUID para el `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="a2ebd-163">En Visual Studio, haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar secretos de usuario** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="a2ebd-164">Este movimiento se agrega un `UserSecretsId` elemento, se rellena con un GUID, a la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="a2ebd-165">Establezca un secreto</span><span class="sxs-lookup"><span data-stu-id="a2ebd-165">Set a secret</span></span>

<span data-ttu-id="a2ebd-166">Definir un secreto de la aplicación que consta de una clave y su valor.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="a2ebd-167">El secreto está asociado con el proyecto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="a2ebd-168">Por ejemplo, ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="a2ebd-169">En el ejemplo anterior, los dos puntos denota que `Movies` es un objeto literal con una `ServiceApiKey` propiedad.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="a2ebd-170">La herramienta Secret Manager puede usarse también de otros directorios.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="a2ebd-171">Use la `--project` opción para proporcionar la ruta de acceso de archivo del sistema en el que el *.csproj* archivo existe.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="a2ebd-172">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="a2ebd-173">Estructura JSON acoplado en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a2ebd-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="a2ebd-174">Visual Studio **administrar secretos de usuario** gestos se abre un *secrets.json* archivo en el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="a2ebd-175">Reemplace el contenido de *secrets.json* con los pares clave-valor que se almacenará.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="a2ebd-176">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="a2ebd-177">Se aplana la estructura JSON después de las modificaciones a través de `dotnet user-secrets remove` o `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="a2ebd-178">Por ejemplo, ejecutar `dotnet user-secrets remove "Movies:ConnectionString"` contrae el `Movies` literal de objeto.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="a2ebd-179">El archivo modificado se parece a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="a2ebd-180">Establezca varios secretos</span><span class="sxs-lookup"><span data-stu-id="a2ebd-180">Set multiple secrets</span></span>

<span data-ttu-id="a2ebd-181">Un lote de los secretos se puede establecer mediante la canalización de JSON para el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="a2ebd-182">En el ejemplo siguiente, la *input.json* contenido del archivo se canaliza hacia el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="a2ebd-183">Windows</span><span class="sxs-lookup"><span data-stu-id="a2ebd-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="a2ebd-184">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-184">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="a2ebd-185">Linux / macOS</span><span class="sxs-lookup"><span data-stu-id="a2ebd-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="a2ebd-186">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-186">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="a2ebd-187">Obtener acceso a un secreto</span><span class="sxs-lookup"><span data-stu-id="a2ebd-187">Access a secret</span></span>

<span data-ttu-id="a2ebd-188">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="a2ebd-189">Si el proyecto tiene como destino .NET Framework, instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a2ebd-190">En ASP.NET Core 2.0 o posterior, el origen de configuración de los secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto llama a <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> para inicializar una nueva instancia del host con valores predeterminados preconfigurados.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="a2ebd-191">`CreateDefaultBuilder` las llamadas <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> cuando el <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> es <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="a2ebd-192">Cuando `CreateDefaultBuilder` no llama, agregar el origen de configuración de los secretos de usuario de forma explícita mediante una llamada a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> en el `Startup` constructor.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="a2ebd-193">Llamar a `AddUserSecrets` solo cuando la aplicación se ejecuta en el entorno de desarrollo, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="a2ebd-194">Instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="a2ebd-195">Agregar el origen de configuración de los secretos de usuario con una llamada a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="a2ebd-196">Se pueden recuperar secretos del usuario a través de la `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="a2ebd-197">Los secretos se asignan a un objeto POCO</span><span class="sxs-lookup"><span data-stu-id="a2ebd-197">Map secrets to a POCO</span></span>

<span data-ttu-id="a2ebd-198">Asignación de un literal de objeto completo a un objeto POCO (una clase .NET simple con propiedades) es útil para la agregación de las propiedades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="a2ebd-199">Para asignar los secretos anteriores a un objeto POCO, utilice el `Configuration` API [el enlace del gráfico de objetos](xref:fundamentals/configuration/index#bind-to-an-object-graph) característica.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="a2ebd-200">El código siguiente se enlaza a una personalizada `MovieSettings` POCO y tiene acceso a la `ServiceApiKey` valor de propiedad:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="a2ebd-201">El `Movies:ConnectionString` y `Movies:ServiceApiKey` secretos se asignan a las propiedades correspondientes en `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="a2ebd-202">Cadena de reemplazo con secretos</span><span class="sxs-lookup"><span data-stu-id="a2ebd-202">String replacement with secrets</span></span>

<span data-ttu-id="a2ebd-203">No es seguro almacenar contraseñas como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="a2ebd-204">Por ejemplo, una cadena de conexión de base de datos se almacena en *appsettings.json* puede incluir una contraseña para el usuario especificado:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="a2ebd-205">Un enfoque más seguro es almacenar la contraseña como un secreto.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="a2ebd-206">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-206">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="a2ebd-207">Quitar el `Password` par clave-valor de la cadena de conexión en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="a2ebd-208">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="a2ebd-209">Se puede establecer el valor del secreto en un <xref:System.Data.SqlClient.SqlConnectionStringBuilder> del objeto <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> propiedad para completar la cadena de conexión:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="a2ebd-210">Enumera los secretos</span><span class="sxs-lookup"><span data-stu-id="a2ebd-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="a2ebd-211">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="a2ebd-212">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="a2ebd-213">En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="a2ebd-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="a2ebd-214">Quitar un secreto único</span><span class="sxs-lookup"><span data-stu-id="a2ebd-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="a2ebd-215">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="a2ebd-216">La aplicación *secrets.json* archivo se modificó para quitar el par de clave y valor asociado con el `MoviesConnectionString` clave:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="a2ebd-217">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="a2ebd-218">Quitar todos los secretos</span><span class="sxs-lookup"><span data-stu-id="a2ebd-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="a2ebd-219">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="a2ebd-220">Se han eliminado todos los secretos de usuario para la aplicación de la *secrets.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="a2ebd-221">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="a2ebd-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="a2ebd-222">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a2ebd-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
