---
title: Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar y recuperar información confidencial como secretos de aplicación durante el desarrollo de una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/24/2018
uid: security/app-secrets
ms.openlocfilehash: 1ecd9b87b9e4aa2c511349fe407c0cea9dc9f921
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028276"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="32d3d-103">Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="32d3d-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="32d3d-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="32d3d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="32d3d-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="32d3d-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="32d3d-106">Este documento explica técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d3d-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="32d3d-107">Nunca almacene contraseñas u otros datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="32d3d-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="32d3d-108">Secretos de producción no deben usarse para el desarrollo o prueba.</span><span class="sxs-lookup"><span data-stu-id="32d3d-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="32d3d-109">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="32d3d-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="32d3d-110">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="32d3d-110">Environment variables</span></span>

<span data-ttu-id="32d3d-111">Las variables de entorno se usan para evitar el almacenamiento de secretos de aplicación en el código o en archivos de configuración local.</span><span class="sxs-lookup"><span data-stu-id="32d3d-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="32d3d-112">Las variables de entorno invalidan los valores de configuración para todos los orígenes de configuración especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="32d3d-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="32d3d-113">Configurar la lectura de valores de variables de entorno mediante una llamada a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="32d3d-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="32d3d-114">Considere la posibilidad de una aplicación web de ASP.NET Core en el que **cuentas de usuario individuales** está habilitada la seguridad.</span><span class="sxs-lookup"><span data-stu-id="32d3d-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="32d3d-115">Una cadena de conexión de base de datos predeterminada se incluye en el proyecto *appsettings.json* archivo con la clave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="32d3d-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="32d3d-116">La cadena de conexión predeterminada es LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="32d3d-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="32d3d-117">Durante la implementación de aplicaciones, el `DefaultConnection` se puede invalidar el valor de clave con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="32d3d-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="32d3d-118">La variable de entorno puede almacenar la cadena de conexión completa con credenciales confidenciales.</span><span class="sxs-lookup"><span data-stu-id="32d3d-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="32d3d-119">Las variables de entorno se almacenan normalmente en texto sin formato y sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="32d3d-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="32d3d-120">Si la máquina o el proceso se ve comprometido, las variables de entorno pueden tener acceso por partes de confianza.</span><span class="sxs-lookup"><span data-stu-id="32d3d-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="32d3d-121">Medidas adicionales para evitar la divulgación de secretos de usuario pueden ser necesarias.</span><span class="sxs-lookup"><span data-stu-id="32d3d-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="32d3d-122">Administrador de secretos</span><span class="sxs-lookup"><span data-stu-id="32d3d-122">Secret Manager</span></span>

<span data-ttu-id="32d3d-123">La herramienta Secret Manager almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d3d-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="32d3d-124">En este contexto, un fragmento de datos confidenciales es un secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="32d3d-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="32d3d-125">Los secretos de aplicación se almacenan en una ubicación independiente desde el árbol del proyecto.</span><span class="sxs-lookup"><span data-stu-id="32d3d-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="32d3d-126">Los secretos de aplicación se asociado con un proyecto específico o se comparten entre varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="32d3d-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="32d3d-127">No se comprueban los secretos de aplicación en control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="32d3d-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="32d3d-128">La herramienta Secret Manager no cifra los secretos almacenados y no debe tratarse como un almacén de confianza.</span><span class="sxs-lookup"><span data-stu-id="32d3d-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="32d3d-129">Es solo con fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="32d3d-129">It's for development purposes only.</span></span> <span data-ttu-id="32d3d-130">Las claves y valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="32d3d-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="32d3d-131">Cómo funciona la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="32d3d-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="32d3d-132">La herramienta Secret Manager abstrae los detalles de implementación, como dónde y cómo se almacenan los valores.</span><span class="sxs-lookup"><span data-stu-id="32d3d-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="32d3d-133">Puede usar la herramienta sin conocer estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="32d3d-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="32d3d-134">Los valores se almacenan en un archivo de configuración de JSON en una carpeta de perfil de usuario protegidos por el sistema en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="32d3d-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="32d3d-135">Windows</span><span class="sxs-lookup"><span data-stu-id="32d3d-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="32d3d-136">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="32d3d-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="32d3d-137">macOS</span><span class="sxs-lookup"><span data-stu-id="32d3d-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="32d3d-138">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="32d3d-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="32d3d-139">Linux</span><span class="sxs-lookup"><span data-stu-id="32d3d-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="32d3d-140">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="32d3d-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="32d3d-141">En la anterior, las rutas de acceso de archivo, reemplace `<user_secrets_id>` con el `UserSecretsId` valor especificado en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="32d3d-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="32d3d-142">No escriba código que depende de la ubicación o el formato de datos guardados con la herramienta Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="32d3d-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="32d3d-143">Pueden cambiar estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="32d3d-143">These implementation details may change.</span></span> <span data-ttu-id="32d3d-144">Por ejemplo, los valores de secreto no se cifran, pero podrían ser en el futuro.</span><span class="sxs-lookup"><span data-stu-id="32d3d-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="32d3d-145">Instalar la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="32d3d-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="32d3d-146">La herramienta Secret Manager está empaquetado con la CLI de .NET Core SDK de .NET Core 2.1.300 o posterior.</span><span class="sxs-lookup"><span data-stu-id="32d3d-146">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="32d3d-147">Para las versiones de SDK de .NET Core 2.1.300 antes de la instalación de herramientas es necesaria.</span><span class="sxs-lookup"><span data-stu-id="32d3d-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="32d3d-148">Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión del SDK de .NET Core instalado.</span><span class="sxs-lookup"><span data-stu-id="32d3d-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="32d3d-149">Se mostrará una advertencia si se usa el SDK de .NET Core incluye la herramienta:</span><span class="sxs-lookup"><span data-stu-id="32d3d-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="32d3d-150">Instalar el [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) paquete de NuGet en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="32d3d-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="32d3d-151">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32d3d-151">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="32d3d-152">Ejecute el siguiente comando en un shell de comandos para validar la instalación de herramienta:</span><span class="sxs-lookup"><span data-stu-id="32d3d-152">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="32d3d-153">La herramienta Secret Manager muestra el uso de ejemplo, las opciones y ayuda del comando:</span><span class="sxs-lookup"><span data-stu-id="32d3d-153">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="32d3d-154">Debe estar en el mismo directorio que el *.csproj* archivo va a ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="32d3d-154">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="32d3d-155">Establezca un secreto</span><span class="sxs-lookup"><span data-stu-id="32d3d-155">Set a secret</span></span>

<span data-ttu-id="32d3d-156">La herramienta Secret Manager opera en valores de configuración de específicas del proyecto almacenados en su perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="32d3d-156">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="32d3d-157">Para usar secretos de usuario, defina un `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="32d3d-157">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="32d3d-158">El valor de `UserSecretsId` es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="32d3d-158">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="32d3d-159">Los desarrolladores suelen generan un GUID para el `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="32d3d-159">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="32d3d-160">En Visual Studio, haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar secretos de usuario** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="32d3d-160">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="32d3d-161">Este movimiento se agrega un `UserSecretsId` elemento, se rellena con un GUID, a la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="32d3d-161">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="32d3d-162">Visual Studio abre un *secrets.json* archivo en el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="32d3d-162">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="32d3d-163">Reemplace el contenido de *secrets.json* con los pares clave-valor que se almacenará.</span><span class="sxs-lookup"><span data-stu-id="32d3d-163">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="32d3d-164">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32d3d-164">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="32d3d-165">Se aplana la estructura JSON después de las modificaciones a través de `dotnet user-secrets remove` o `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="32d3d-165">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="32d3d-166">Por ejemplo, ejecutar `dotnet user-secrets remove "Movies:ConnectionString"` contrae el `Movies` literal de objeto.</span><span class="sxs-lookup"><span data-stu-id="32d3d-166">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="32d3d-167">El archivo modificado se parece a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="32d3d-167">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="32d3d-168">Definir un secreto de la aplicación que consta de una clave y su valor.</span><span class="sxs-lookup"><span data-stu-id="32d3d-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="32d3d-169">El secreto está asociado con el proyecto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="32d3d-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="32d3d-170">Por ejemplo, ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="32d3d-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="32d3d-171">En el ejemplo anterior, los dos puntos denota que `Movies` es un objeto literal con una `ServiceApiKey` propiedad.</span><span class="sxs-lookup"><span data-stu-id="32d3d-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="32d3d-172">La herramienta Secret Manager puede usarse también de otros directorios.</span><span class="sxs-lookup"><span data-stu-id="32d3d-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="32d3d-173">Use la `--project` opción para proporcionar la ruta de acceso de archivo del sistema en el que el *.csproj* archivo existe.</span><span class="sxs-lookup"><span data-stu-id="32d3d-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="32d3d-174">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32d3d-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="32d3d-175">Establezca varios secretos</span><span class="sxs-lookup"><span data-stu-id="32d3d-175">Set multiple secrets</span></span>

<span data-ttu-id="32d3d-176">Un lote de los secretos se puede establecer mediante la canalización de JSON para el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="32d3d-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="32d3d-177">En el ejemplo siguiente, la *input.json* contenido del archivo se canaliza hacia el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="32d3d-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="32d3d-178">Windows</span><span class="sxs-lookup"><span data-stu-id="32d3d-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="32d3d-179">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="32d3d-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="32d3d-180">macOS</span><span class="sxs-lookup"><span data-stu-id="32d3d-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="32d3d-181">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="32d3d-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="32d3d-182">Linux</span><span class="sxs-lookup"><span data-stu-id="32d3d-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="32d3d-183">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="32d3d-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="32d3d-184">Obtener acceso a un secreto</span><span class="sxs-lookup"><span data-stu-id="32d3d-184">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="32d3d-185">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="32d3d-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="32d3d-186">Si el proyecto tiene como destino .NET Framework, instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="32d3d-186">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="32d3d-187">En ASP.NET Core 2.0 o posterior, el origen de configuración de los secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar una nueva instancia del host con valores predeterminados preconfigurados.</span><span class="sxs-lookup"><span data-stu-id="32d3d-187">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="32d3d-188">`CreateDefaultBuilder` las llamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) cuando el [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) es [desarrollo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="32d3d-188">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="32d3d-189">Cuando `CreateDefaultBuilder` no llama durante la construcción de host, agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="32d3d-189">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="32d3d-190">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="32d3d-190">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="32d3d-191">Instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="32d3d-191">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="32d3d-192">Agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="32d3d-192">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="32d3d-193">Se pueden recuperar secretos del usuario a través de la `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="32d3d-193">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="32d3d-194">Los secretos se asignan a un objeto POCO</span><span class="sxs-lookup"><span data-stu-id="32d3d-194">Map secrets to a POCO</span></span>

<span data-ttu-id="32d3d-195">Asignación de un literal de objeto completo a un objeto POCO (una clase .NET simple con propiedades) es útil para la agregación de las propiedades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="32d3d-195">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="32d3d-196">Para asignar los secretos anteriores a un objeto POCO, utilice el `Configuration` API [el enlace del gráfico de objetos](xref:fundamentals/configuration/index#bind-to-an-object-graph) característica.</span><span class="sxs-lookup"><span data-stu-id="32d3d-196">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="32d3d-197">El código siguiente se enlaza a una personalizada `MovieSettings` POCO y tiene acceso a la `ServiceApiKey` valor de propiedad:</span><span class="sxs-lookup"><span data-stu-id="32d3d-197">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="32d3d-198">El `Movies:ConnectionString` y `Movies:ServiceApiKey` secretos se asignan a las propiedades correspondientes en `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="32d3d-198">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="32d3d-199">Cadena de reemplazo con secretos</span><span class="sxs-lookup"><span data-stu-id="32d3d-199">String replacement with secrets</span></span>

<span data-ttu-id="32d3d-200">No es seguro almacenar contraseñas como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="32d3d-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="32d3d-201">Por ejemplo, una cadena de conexión de base de datos se almacena en *appsettings.json* puede incluir una contraseña para el usuario especificado:</span><span class="sxs-lookup"><span data-stu-id="32d3d-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="32d3d-202">Un enfoque más seguro es almacenar la contraseña como un secreto.</span><span class="sxs-lookup"><span data-stu-id="32d3d-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="32d3d-203">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32d3d-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="32d3d-204">Quitar el `Password` par clave-valor de la cadena de conexión en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="32d3d-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="32d3d-205">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="32d3d-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="32d3d-206">Se puede establecer el valor del secreto en un [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) del objeto [contraseña](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propiedad para completar la cadena de conexión:</span><span class="sxs-lookup"><span data-stu-id="32d3d-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="32d3d-207">Enumera los secretos</span><span class="sxs-lookup"><span data-stu-id="32d3d-207">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="32d3d-208">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="32d3d-208">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="32d3d-209">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="32d3d-209">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="32d3d-210">En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="32d3d-210">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="32d3d-211">Quitar un secreto único</span><span class="sxs-lookup"><span data-stu-id="32d3d-211">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="32d3d-212">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="32d3d-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="32d3d-213">La aplicación *secrets.json* archivo se modificó para quitar el par de clave y valor asociado con el `MoviesConnectionString` clave:</span><span class="sxs-lookup"><span data-stu-id="32d3d-213">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="32d3d-214">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="32d3d-214">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="32d3d-215">Quitar todos los secretos</span><span class="sxs-lookup"><span data-stu-id="32d3d-215">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="32d3d-216">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="32d3d-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="32d3d-217">Se han eliminado todos los secretos de usuario para la aplicación de la *secrets.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="32d3d-217">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="32d3d-218">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="32d3d-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="32d3d-219">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="32d3d-219">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
