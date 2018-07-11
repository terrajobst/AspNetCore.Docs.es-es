---
title: Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar y recuperar información confidencial como secretos de aplicación durante el desarrollo de una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126916"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="00513-103">Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="00513-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="00513-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="00513-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="00513-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="00513-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="00513-106">Este documento explica técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00513-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="00513-107">Nunca debe almacenar las contraseñas u otros datos confidenciales en el código fuente, y no debería usar secretos de producción en el desarrollo o modo de prueba.</span><span class="sxs-lookup"><span data-stu-id="00513-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="00513-108">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="00513-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="00513-109">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="00513-109">Environment variables</span></span>

<span data-ttu-id="00513-110">Las variables de entorno se usan para evitar el almacenamiento de secretos de aplicación en el código o en archivos de configuración local.</span><span class="sxs-lookup"><span data-stu-id="00513-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="00513-111">Las variables de entorno invalidan los valores de configuración para todos los orígenes de configuración especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="00513-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="00513-112">Configurar la lectura de valores de variables de entorno mediante una llamada a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="00513-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

<span data-ttu-id="00513-113">Considere la posibilidad de una aplicación web de ASP.NET Core en el que **cuentas de usuario individuales** está habilitada la seguridad.</span><span class="sxs-lookup"><span data-stu-id="00513-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="00513-114">Una cadena de conexión de base de datos predeterminada se incluye en el proyecto *appsettings.json* archivo con la clave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="00513-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="00513-115">La cadena de conexión predeterminada es LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="00513-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="00513-116">Durante la implementación de aplicaciones, el `DefaultConnection` se puede invalidar el valor de clave con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="00513-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="00513-117">La variable de entorno puede almacenar la cadena de conexión completa con credenciales confidenciales.</span><span class="sxs-lookup"><span data-stu-id="00513-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="00513-118">Las variables de entorno se almacenan normalmente en texto sin formato y sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="00513-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="00513-119">Si la máquina o el proceso se ve comprometido, las variables de entorno pueden tener acceso por partes de confianza.</span><span class="sxs-lookup"><span data-stu-id="00513-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="00513-120">Medidas adicionales para evitar la divulgación de secretos de usuario pueden ser necesarias.</span><span class="sxs-lookup"><span data-stu-id="00513-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="00513-121">Administrador de secretos</span><span class="sxs-lookup"><span data-stu-id="00513-121">Secret Manager</span></span>

<span data-ttu-id="00513-122">La herramienta Secret Manager almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00513-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="00513-123">En este contexto, un fragmento de datos confidenciales es un secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="00513-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="00513-124">Los secretos de aplicación se almacenan en una ubicación independiente desde el árbol del proyecto.</span><span class="sxs-lookup"><span data-stu-id="00513-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="00513-125">Los secretos de aplicación se asociado con un proyecto específico o se comparten entre varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="00513-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="00513-126">No se comprueban los secretos de aplicación en control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="00513-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="00513-127">La herramienta Secret Manager no cifra los secretos almacenados y no debe tratarse como un almacén de confianza.</span><span class="sxs-lookup"><span data-stu-id="00513-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="00513-128">Es solo con fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="00513-128">It's for development purposes only.</span></span> <span data-ttu-id="00513-129">Las claves y valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="00513-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="00513-130">Cómo funciona la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="00513-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="00513-131">La herramienta Secret Manager abstrae los detalles de implementación, como dónde y cómo se almacenan los valores.</span><span class="sxs-lookup"><span data-stu-id="00513-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="00513-132">Puede usar la herramienta sin conocer estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="00513-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="00513-133">Los valores se almacenan en un archivo de configuración de JSON en una carpeta de perfil de usuario protegidos por el sistema en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="00513-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="00513-134">Windows</span><span class="sxs-lookup"><span data-stu-id="00513-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="00513-135">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="00513-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="00513-136">macOS</span><span class="sxs-lookup"><span data-stu-id="00513-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="00513-137">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="00513-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="00513-138">Linux</span><span class="sxs-lookup"><span data-stu-id="00513-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="00513-139">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="00513-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="00513-140">En la anterior, las rutas de acceso de archivo, reemplace `<user_secrets_id>` con el `UserSecretsId` valor especificado en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="00513-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="00513-141">No escriba código que depende de la ubicación o el formato de datos guardados con la herramienta Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="00513-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="00513-142">Pueden cambiar estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="00513-142">These implementation details may change.</span></span> <span data-ttu-id="00513-143">Por ejemplo, los valores de secreto no se cifran, pero podrían ser en el futuro.</span><span class="sxs-lookup"><span data-stu-id="00513-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="00513-144">Instalar la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="00513-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="00513-145">La herramienta Secret Manager se incluye con la CLI de .NET Core a partir del SDK de .NET Core 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="00513-145">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="00513-146">Para las versiones de SDK de .NET Core 2.1.300 antes de la instalación de herramientas es necesaria.</span><span class="sxs-lookup"><span data-stu-id="00513-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="00513-147">Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión del SDK de .NET Core instalado.</span><span class="sxs-lookup"><span data-stu-id="00513-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="00513-148">Se mostrará una advertencia si se usa el SDK de .NET Core incluye la herramienta:</span><span class="sxs-lookup"><span data-stu-id="00513-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="00513-149">Instalar el [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) paquete de NuGet en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="00513-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="00513-150">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="00513-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

<span data-ttu-id="00513-151">Ejecute el siguiente comando en un shell de comandos para validar la instalación de herramienta:</span><span class="sxs-lookup"><span data-stu-id="00513-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="00513-152">La herramienta Secret Manager muestra el uso de ejemplo, las opciones y ayuda del comando:</span><span class="sxs-lookup"><span data-stu-id="00513-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="00513-153">Debe estar en el mismo directorio que el *.csproj* archivo va a ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="00513-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="00513-154">Establezca un secreto</span><span class="sxs-lookup"><span data-stu-id="00513-154">Set a secret</span></span>

<span data-ttu-id="00513-155">La herramienta Secret Manager opera en valores de configuración de específicas del proyecto almacenados en su perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="00513-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="00513-156">Para usar secretos de usuario, defina un `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="00513-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="00513-157">El valor de `UserSecretsId` es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="00513-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="00513-158">Los desarrolladores suelen generan un GUID para el `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="00513-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> <span data-ttu-id="00513-159">En Visual Studio, haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar secretos de usuario** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="00513-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="00513-160">Este movimiento se agrega un `UserSecretsId` elemento, se rellena con un GUID, a la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="00513-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="00513-161">Visual Studio abre un *secrets.json* archivo en el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="00513-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="00513-162">Reemplace el contenido de *secrets.json* con los pares clave-valor que se almacenará.</span><span class="sxs-lookup"><span data-stu-id="00513-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="00513-163">Por ejemplo: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)].</span><span class="sxs-lookup"><span data-stu-id="00513-163">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="00513-164">Definir un secreto de la aplicación que consta de una clave y su valor.</span><span class="sxs-lookup"><span data-stu-id="00513-164">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="00513-165">El secreto está asociado con el proyecto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="00513-165">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="00513-166">Por ejemplo, ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="00513-166">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="00513-167">En el ejemplo anterior, los dos puntos denota que `Movies` es un objeto literal con una `ServiceApiKey` propiedad.</span><span class="sxs-lookup"><span data-stu-id="00513-167">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="00513-168">La herramienta Secret Manager puede usarse también de otros directorios.</span><span class="sxs-lookup"><span data-stu-id="00513-168">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="00513-169">Use la `--project` opción para proporcionar la ruta de acceso de archivo del sistema en el que el *.csproj* archivo existe.</span><span class="sxs-lookup"><span data-stu-id="00513-169">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="00513-170">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="00513-170">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="00513-171">Establezca varios secretos</span><span class="sxs-lookup"><span data-stu-id="00513-171">Set multiple secrets</span></span>

<span data-ttu-id="00513-172">Un lote de los secretos se puede establecer mediante la canalización de JSON para el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="00513-172">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="00513-173">En el ejemplo siguiente, la *input.json* contenido del archivo se canaliza hacia el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="00513-173">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="00513-174">Windows</span><span class="sxs-lookup"><span data-stu-id="00513-174">Windows</span></span>](#tab/windows)

<span data-ttu-id="00513-175">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="00513-175">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="00513-176">macOS</span><span class="sxs-lookup"><span data-stu-id="00513-176">macOS</span></span>](#tab/macos)

<span data-ttu-id="00513-177">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="00513-177">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="00513-178">Linux</span><span class="sxs-lookup"><span data-stu-id="00513-178">Linux</span></span>](#tab/linux)

<span data-ttu-id="00513-179">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="00513-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="00513-180">Obtener acceso a un secreto</span><span class="sxs-lookup"><span data-stu-id="00513-180">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="00513-181">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="00513-181">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="00513-182">Instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="00513-182">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="00513-183">Agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="00513-183">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="00513-184">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="00513-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="00513-185">Si el proyecto tiene como destino .NET Framework, instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="00513-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="00513-186">En ASP.NET Core 2.0 o posterior, el origen de configuración de los secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar una nueva instancia del host con valores predeterminados preconfigurados.</span><span class="sxs-lookup"><span data-stu-id="00513-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="00513-187">`CreateDefaultBuilder` las llamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) cuando el [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) es [desarrollo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="00513-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="00513-188">Cuando `CreateDefaultBuilder` no llama durante la construcción de host, agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="00513-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

<span data-ttu-id="00513-189">Se pueden recuperar secretos del usuario a través de la `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="00513-189">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="00513-190">Cadena de reemplazo con secretos</span><span class="sxs-lookup"><span data-stu-id="00513-190">String replacement with secrets</span></span>

<span data-ttu-id="00513-191">No es seguro almacenar contraseñas como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="00513-191">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="00513-192">Por ejemplo, una cadena de conexión de base de datos se almacena en *appsettings.json* puede incluir una contraseña para el usuario especificado:</span><span class="sxs-lookup"><span data-stu-id="00513-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="00513-193">Un enfoque más seguro es almacenar la contraseña como un secreto.</span><span class="sxs-lookup"><span data-stu-id="00513-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="00513-194">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="00513-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="00513-195">Quitar el `Password` par clave-valor de la cadena de conexión en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="00513-195">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="00513-196">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="00513-196">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="00513-197">Se puede establecer el valor del secreto en un [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) del objeto [contraseña](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propiedad para completar la cadena de conexión:</span><span class="sxs-lookup"><span data-stu-id="00513-197">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="00513-198">Enumera los secretos</span><span class="sxs-lookup"><span data-stu-id="00513-198">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="00513-199">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="00513-199">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="00513-200">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="00513-200">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="00513-201">En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="00513-201">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="00513-202">Quitar un secreto único</span><span class="sxs-lookup"><span data-stu-id="00513-202">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="00513-203">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="00513-203">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="00513-204">La aplicación *secrets.json* archivo se modificó para quitar el par de clave y valor asociado con el `MoviesConnectionString` clave:</span><span class="sxs-lookup"><span data-stu-id="00513-204">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="00513-205">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="00513-205">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="00513-206">Quitar todos los secretos</span><span class="sxs-lookup"><span data-stu-id="00513-206">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="00513-207">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="00513-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="00513-208">Se han eliminado todos los secretos de usuario para la aplicación de la *secrets.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="00513-208">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="00513-209">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="00513-209">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="00513-210">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="00513-210">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
