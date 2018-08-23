---
title: Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar y recuperar información confidencial como secretos de aplicación durante el desarrollo de una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/16/2018
uid: security/app-secrets
ms.openlocfilehash: 35c316230c19aa69a0dac26ec25a6e017f102237
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "41839231"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="8369a-103">Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8369a-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="8369a-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="8369a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="8369a-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8369a-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8369a-106">Este documento explica técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8369a-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="8369a-107">Nunca debe almacenar las contraseñas u otros datos confidenciales en el código fuente, y no debería usar secretos de producción en el desarrollo o modo de prueba.</span><span class="sxs-lookup"><span data-stu-id="8369a-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="8369a-108">Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="8369a-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="8369a-109">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="8369a-109">Environment variables</span></span>

<span data-ttu-id="8369a-110">Las variables de entorno se usan para evitar el almacenamiento de secretos de aplicación en el código o en archivos de configuración local.</span><span class="sxs-lookup"><span data-stu-id="8369a-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="8369a-111">Las variables de entorno invalidan los valores de configuración para todos los orígenes de configuración especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="8369a-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="8369a-112">Configurar la lectura de valores de variables de entorno mediante una llamada a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="8369a-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="8369a-113">Considere la posibilidad de una aplicación web de ASP.NET Core en el que **cuentas de usuario individuales** está habilitada la seguridad.</span><span class="sxs-lookup"><span data-stu-id="8369a-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="8369a-114">Una cadena de conexión de base de datos predeterminada se incluye en el proyecto *appsettings.json* archivo con la clave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="8369a-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="8369a-115">La cadena de conexión predeterminada es LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="8369a-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="8369a-116">Durante la implementación de aplicaciones, el `DefaultConnection` se puede invalidar el valor de clave con el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="8369a-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="8369a-117">La variable de entorno puede almacenar la cadena de conexión completa con credenciales confidenciales.</span><span class="sxs-lookup"><span data-stu-id="8369a-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="8369a-118">Las variables de entorno se almacenan normalmente en texto sin formato y sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="8369a-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="8369a-119">Si la máquina o el proceso se ve comprometido, las variables de entorno pueden tener acceso por partes de confianza.</span><span class="sxs-lookup"><span data-stu-id="8369a-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="8369a-120">Medidas adicionales para evitar la divulgación de secretos de usuario pueden ser necesarias.</span><span class="sxs-lookup"><span data-stu-id="8369a-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="8369a-121">Administrador de secretos</span><span class="sxs-lookup"><span data-stu-id="8369a-121">Secret Manager</span></span>

<span data-ttu-id="8369a-122">La herramienta Secret Manager almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8369a-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="8369a-123">En este contexto, un fragmento de datos confidenciales es un secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8369a-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="8369a-124">Los secretos de aplicación se almacenan en una ubicación independiente desde el árbol del proyecto.</span><span class="sxs-lookup"><span data-stu-id="8369a-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="8369a-125">Los secretos de aplicación se asociado con un proyecto específico o se comparten entre varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="8369a-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="8369a-126">No se comprueban los secretos de aplicación en control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="8369a-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="8369a-127">La herramienta Secret Manager no cifra los secretos almacenados y no debe tratarse como un almacén de confianza.</span><span class="sxs-lookup"><span data-stu-id="8369a-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="8369a-128">Es solo con fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="8369a-128">It's for development purposes only.</span></span> <span data-ttu-id="8369a-129">Las claves y valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="8369a-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="8369a-130">Cómo funciona la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="8369a-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="8369a-131">La herramienta Secret Manager abstrae los detalles de implementación, como dónde y cómo se almacenan los valores.</span><span class="sxs-lookup"><span data-stu-id="8369a-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="8369a-132">Puede usar la herramienta sin conocer estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="8369a-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="8369a-133">Los valores se almacenan en un archivo de configuración de JSON en una carpeta de perfil de usuario protegidos por el sistema en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="8369a-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="8369a-134">Windows</span><span class="sxs-lookup"><span data-stu-id="8369a-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="8369a-135">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="8369a-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="8369a-136">macOS</span><span class="sxs-lookup"><span data-stu-id="8369a-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="8369a-137">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="8369a-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="8369a-138">Linux</span><span class="sxs-lookup"><span data-stu-id="8369a-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="8369a-139">Ruta de acceso de archivo del sistema:</span><span class="sxs-lookup"><span data-stu-id="8369a-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="8369a-140">En la anterior, las rutas de acceso de archivo, reemplace `<user_secrets_id>` con el `UserSecretsId` valor especificado en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="8369a-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="8369a-141">No escriba código que depende de la ubicación o el formato de datos guardados con la herramienta Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="8369a-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="8369a-142">Pueden cambiar estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="8369a-142">These implementation details may change.</span></span> <span data-ttu-id="8369a-143">Por ejemplo, los valores de secreto no se cifran, pero podrían ser en el futuro.</span><span class="sxs-lookup"><span data-stu-id="8369a-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="8369a-144">Instalar la herramienta Secret Manager</span><span class="sxs-lookup"><span data-stu-id="8369a-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="8369a-145">La herramienta Secret Manager está empaquetado con la CLI de .NET Core SDK de .NET Core 2.1.300 o posterior.</span><span class="sxs-lookup"><span data-stu-id="8369a-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="8369a-146">Para las versiones de SDK de .NET Core 2.1.300 antes de la instalación de herramientas es necesaria.</span><span class="sxs-lookup"><span data-stu-id="8369a-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="8369a-147">Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión del SDK de .NET Core instalado.</span><span class="sxs-lookup"><span data-stu-id="8369a-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="8369a-148">Se mostrará una advertencia si se usa el SDK de .NET Core incluye la herramienta:</span><span class="sxs-lookup"><span data-stu-id="8369a-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="8369a-149">Instalar el [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) paquete de NuGet en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8369a-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="8369a-150">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8369a-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="8369a-151">Ejecute el siguiente comando en un shell de comandos para validar la instalación de herramienta:</span><span class="sxs-lookup"><span data-stu-id="8369a-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="8369a-152">La herramienta Secret Manager muestra el uso de ejemplo, las opciones y ayuda del comando:</span><span class="sxs-lookup"><span data-stu-id="8369a-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="8369a-153">Debe estar en el mismo directorio que el *.csproj* archivo va a ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="8369a-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="8369a-154">Establezca un secreto</span><span class="sxs-lookup"><span data-stu-id="8369a-154">Set a secret</span></span>

<span data-ttu-id="8369a-155">La herramienta Secret Manager opera en valores de configuración de específicas del proyecto almacenados en su perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="8369a-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="8369a-156">Para usar secretos de usuario, defina un `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="8369a-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="8369a-157">El valor de `UserSecretsId` es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="8369a-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="8369a-158">Los desarrolladores suelen generan un GUID para el `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="8369a-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="8369a-159">En Visual Studio, haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar secretos de usuario** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="8369a-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="8369a-160">Este movimiento se agrega un `UserSecretsId` elemento, se rellena con un GUID, a la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="8369a-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="8369a-161">Visual Studio abre un *secrets.json* archivo en el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="8369a-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="8369a-162">Reemplace el contenido de *secrets.json* con los pares clave-valor que se almacenará.</span><span class="sxs-lookup"><span data-stu-id="8369a-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="8369a-163">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8369a-163">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="8369a-164">Se aplana la estructura JSON después de las modificaciones a través de `dotnet user-secrets remove` o `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="8369a-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="8369a-165">Por ejemplo, ejecutar `dotnet user-secrets remove "Movies:ConnectionString"` contrae el `Movies` literal de objeto.</span><span class="sxs-lookup"><span data-stu-id="8369a-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="8369a-166">El archivo modificado se parece a lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8369a-166">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="8369a-167">Definir un secreto de la aplicación que consta de una clave y su valor.</span><span class="sxs-lookup"><span data-stu-id="8369a-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="8369a-168">El secreto está asociado con el proyecto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="8369a-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="8369a-169">Por ejemplo, ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="8369a-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="8369a-170">En el ejemplo anterior, los dos puntos denota que `Movies` es un objeto literal con una `ServiceApiKey` propiedad.</span><span class="sxs-lookup"><span data-stu-id="8369a-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="8369a-171">La herramienta Secret Manager puede usarse también de otros directorios.</span><span class="sxs-lookup"><span data-stu-id="8369a-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="8369a-172">Use la `--project` opción para proporcionar la ruta de acceso de archivo del sistema en el que el *.csproj* archivo existe.</span><span class="sxs-lookup"><span data-stu-id="8369a-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="8369a-173">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8369a-173">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="8369a-174">Establezca varios secretos</span><span class="sxs-lookup"><span data-stu-id="8369a-174">Set multiple secrets</span></span>

<span data-ttu-id="8369a-175">Un lote de los secretos se puede establecer mediante la canalización de JSON para el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="8369a-175">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="8369a-176">En el ejemplo siguiente, la *input.json* contenido del archivo se canaliza hacia el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="8369a-176">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="8369a-177">Windows</span><span class="sxs-lookup"><span data-stu-id="8369a-177">Windows</span></span>](#tab/windows)

<span data-ttu-id="8369a-178">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8369a-178">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="8369a-179">macOS</span><span class="sxs-lookup"><span data-stu-id="8369a-179">macOS</span></span>](#tab/macos)

<span data-ttu-id="8369a-180">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8369a-180">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="8369a-181">Linux</span><span class="sxs-lookup"><span data-stu-id="8369a-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="8369a-182">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="8369a-182">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="8369a-183">Obtener acceso a un secreto</span><span class="sxs-lookup"><span data-stu-id="8369a-183">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8369a-184">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="8369a-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="8369a-185">Si el proyecto tiene como destino .NET Framework, instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="8369a-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="8369a-186">En ASP.NET Core 2.0 o posterior, el origen de configuración de los secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar una nueva instancia del host con valores predeterminados preconfigurados.</span><span class="sxs-lookup"><span data-stu-id="8369a-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="8369a-187">`CreateDefaultBuilder` las llamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) cuando el [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) es [desarrollo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="8369a-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="8369a-188">Cuando `CreateDefaultBuilder` no llama durante la construcción de host, agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="8369a-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="8369a-189">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="8369a-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="8369a-190">Instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="8369a-190">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="8369a-191">Agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="8369a-191">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="8369a-192">Se pueden recuperar secretos del usuario a través de la `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="8369a-192">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="8369a-193">Los secretos se asignan a un objeto POCO</span><span class="sxs-lookup"><span data-stu-id="8369a-193">Map secrets to a POCO</span></span>

<span data-ttu-id="8369a-194">Asignación de un literal de objeto completo a un objeto POCO (una clase .NET simple con propiedades) es útil para la agregación de las propiedades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="8369a-194">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="8369a-195">Para asignar los secretos anteriores a un objeto POCO, utilice el `Configuration` API [el enlace del gráfico de objetos](xref:fundamentals/configuration/index#bind-to-an-object-graph) característica.</span><span class="sxs-lookup"><span data-stu-id="8369a-195">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="8369a-196">El código siguiente se enlaza a una personalizada `MovieSettings` POCO y tiene acceso a la `ServiceApiKey` valor de propiedad:</span><span class="sxs-lookup"><span data-stu-id="8369a-196">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="8369a-197">El `Movies:ConnectionString` y `Movies:ServiceApiKey` secretos se asignan a las propiedades correspondientes en `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="8369a-197">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="8369a-198">Cadena de reemplazo con secretos</span><span class="sxs-lookup"><span data-stu-id="8369a-198">String replacement with secrets</span></span>

<span data-ttu-id="8369a-199">No es seguro almacenar contraseñas como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="8369a-199">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="8369a-200">Por ejemplo, una cadena de conexión de base de datos se almacena en *appsettings.json* puede incluir una contraseña para el usuario especificado:</span><span class="sxs-lookup"><span data-stu-id="8369a-200">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="8369a-201">Un enfoque más seguro es almacenar la contraseña como un secreto.</span><span class="sxs-lookup"><span data-stu-id="8369a-201">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="8369a-202">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8369a-202">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="8369a-203">Quitar el `Password` par clave-valor de la cadena de conexión en *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="8369a-203">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="8369a-204">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="8369a-204">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="8369a-205">Se puede establecer el valor del secreto en un [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) del objeto [contraseña](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propiedad para completar la cadena de conexión:</span><span class="sxs-lookup"><span data-stu-id="8369a-205">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="8369a-206">Enumera los secretos</span><span class="sxs-lookup"><span data-stu-id="8369a-206">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="8369a-207">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="8369a-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="8369a-208">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="8369a-208">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="8369a-209">En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="8369a-209">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="8369a-210">Quitar un secreto único</span><span class="sxs-lookup"><span data-stu-id="8369a-210">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="8369a-211">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="8369a-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="8369a-212">La aplicación *secrets.json* archivo se modificó para quitar el par de clave y valor asociado con el `MoviesConnectionString` clave:</span><span class="sxs-lookup"><span data-stu-id="8369a-212">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="8369a-213">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="8369a-213">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="8369a-214">Quitar todos los secretos</span><span class="sxs-lookup"><span data-stu-id="8369a-214">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="8369a-215">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="8369a-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="8369a-216">Se han eliminado todos los secretos de usuario para la aplicación de la *secrets.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="8369a-216">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="8369a-217">Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:</span><span class="sxs-lookup"><span data-stu-id="8369a-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="8369a-218">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="8369a-218">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
