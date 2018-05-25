---
title: Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo almacenar y recuperar información confidencial como secretos de aplicación durante el desarrollo de una aplicación de ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: ece2bf541df2b4acac60a88767cc57ede473bd49
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="73232-103">Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73232-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="73232-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="73232-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="73232-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73232-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="73232-106">Este documento explica técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73232-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="73232-107">Nunca debe almacenar las contraseñas u otros datos confidenciales en el código fuente, y no debe utilizar secretos de producción en el desarrollo o modo de prueba.</span><span class="sxs-lookup"><span data-stu-id="73232-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="73232-108">Puede almacenar y proteger los secretos de prueba y producción Azure con el [proveedor de configuración de almacén de claves de Azure](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="73232-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="73232-109">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="73232-109">Environment variables</span></span>

<span data-ttu-id="73232-110">Las variables de entorno se utilizan para evitar el almacenamiento de secretos de la aplicación en el código o en archivos de configuración local.</span><span class="sxs-lookup"><span data-stu-id="73232-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="73232-111">Las variables de entorno invalidan los valores de configuración de todos los orígenes de configuración especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="73232-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="73232-112">Configurar la lectura de los valores de variables de entorno mediante una llamada a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="73232-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="73232-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="73232-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="73232-114">Piense en una aplicación web de ASP.NET Core en la que **cuentas de usuario individuales** está habilitada la seguridad.</span><span class="sxs-lookup"><span data-stu-id="73232-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="73232-115">Una cadena de conexión de base de datos predeterminada se incluye en el proyecto *appSettings.JSON que se* archivo con la clave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="73232-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="73232-116">La cadena de conexión predeterminada es LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="73232-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="73232-117">Durante la implementación de aplicaciones, el `DefaultConnection` clave-valor puede reemplazarse por el valor de la variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="73232-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="73232-118">La variable de entorno puede almacenar la cadena de conexión completa con las credenciales confidenciales.</span><span class="sxs-lookup"><span data-stu-id="73232-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="73232-119">Las variables de entorno se almacenan normalmente en texto sin formato y sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="73232-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="73232-120">Si la máquina o el proceso se ve comprometido, confianza pueden tener acceso a las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="73232-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="73232-121">Saber qué medidas adicionales para evitar la divulgación de información confidencial del usuario pueden ser necesarias.</span><span class="sxs-lookup"><span data-stu-id="73232-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="73232-122">Administrador de secreto</span><span class="sxs-lookup"><span data-stu-id="73232-122">Secret Manager</span></span>

<span data-ttu-id="73232-123">La herramienta Administrador de secreto almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73232-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="73232-124">En este contexto, una parte de los datos confidenciales es un secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="73232-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="73232-125">Secretos de la aplicación se almacenan en una ubicación independiente desde el árbol del proyecto.</span><span class="sxs-lookup"><span data-stu-id="73232-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="73232-126">Los secretos de la aplicación se asociada a un proyecto específico o se comparten entre varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="73232-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="73232-127">Los secretos de aplicación no se protegen en el control de origen.</span><span class="sxs-lookup"><span data-stu-id="73232-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="73232-128">La herramienta Administrador de secreto no cifra los secretos almacenados y no debe tratarse como un almacén de confianza.</span><span class="sxs-lookup"><span data-stu-id="73232-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="73232-129">Es solo con fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="73232-129">It's for development purposes only.</span></span> <span data-ttu-id="73232-130">Las claves y los valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="73232-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="73232-131">Cómo funciona la herramienta Administrador de secreto</span><span class="sxs-lookup"><span data-stu-id="73232-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="73232-132">La herramienta Administrador de secreto abstrae los detalles de implementación, como dónde y cómo se almacenan los valores.</span><span class="sxs-lookup"><span data-stu-id="73232-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="73232-133">Puede usar la herramienta sin conocer estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="73232-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="73232-134">Los valores se almacenan en un archivo de configuración de JSON en una carpeta de perfiles de usuarios protegidos por el sistema en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="73232-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="73232-135">Windows</span><span class="sxs-lookup"><span data-stu-id="73232-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="73232-136">Ruta de acceso de sistema de archivos:</span><span class="sxs-lookup"><span data-stu-id="73232-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="73232-137">macOS</span><span class="sxs-lookup"><span data-stu-id="73232-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="73232-138">Ruta de acceso de sistema de archivos:</span><span class="sxs-lookup"><span data-stu-id="73232-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="73232-139">Linux</span><span class="sxs-lookup"><span data-stu-id="73232-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="73232-140">Ruta de acceso de sistema de archivos:</span><span class="sxs-lookup"><span data-stu-id="73232-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="73232-141">En los anteriores, las rutas de acceso de archivo, reemplace `<user_secrets_id>` con el `UserSecretsId` valor especificado en el *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="73232-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="73232-142">No escriba código que depende de la ubicación o el formato de datos que se guardan con la herramienta Administrador de secreto.</span><span class="sxs-lookup"><span data-stu-id="73232-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="73232-143">Pueden cambiar estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="73232-143">These implementation details may change.</span></span> <span data-ttu-id="73232-144">Por ejemplo, los valores secretos no se cifran, pero pudieron ser en el futuro.</span><span class="sxs-lookup"><span data-stu-id="73232-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="73232-145">Instalar la herramienta Administrador de secreto</span><span class="sxs-lookup"><span data-stu-id="73232-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="73232-146">La herramienta Administrador de secreto se incluye con la CLI de núcleo de .NET a partir de .NET Core SDK 2.1.300.</span><span class="sxs-lookup"><span data-stu-id="73232-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="73232-147">Para las versiones de .NET Core SDK anteriores 2.1.300, es necesaria la instalación de herramientas.</span><span class="sxs-lookup"><span data-stu-id="73232-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="73232-148">Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión de .NET Core SDK instalado.</span><span class="sxs-lookup"><span data-stu-id="73232-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="73232-149">Se mostrará una advertencia si se usa .NET Core SDK incluye la herramienta:</span><span class="sxs-lookup"><span data-stu-id="73232-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="73232-150">Instalar el [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) paquete de NuGet en el proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73232-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="73232-151">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="73232-151">For example:</span></span>

<span data-ttu-id="73232-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="73232-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="73232-153">Ejecute el siguiente comando en un shell de comandos para validar la instalación de la herramienta:</span><span class="sxs-lookup"><span data-stu-id="73232-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="73232-154">La herramienta Administrador de secreto muestra ejemplo de uso, opciones y la Ayuda de comando:</span><span class="sxs-lookup"><span data-stu-id="73232-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="73232-155">Debe estar en el mismo directorio que el *.csproj* archivo para ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` elementos.</span><span class="sxs-lookup"><span data-stu-id="73232-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="73232-156">Establecer un secreto</span><span class="sxs-lookup"><span data-stu-id="73232-156">Set a secret</span></span>

<span data-ttu-id="73232-157">La herramienta Administrador de secreto opera en valores de configuración específicos del proyecto almacenados en su perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="73232-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="73232-158">Para utilizar secretos del usuario, definir una `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="73232-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="73232-159">El valor de `UserSecretsId` es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="73232-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="73232-160">Los desarrolladores suelen generan un GUID para el `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="73232-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="73232-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="73232-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="73232-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="73232-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="73232-163">En Visual Studio, haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar secretos del usuario** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="73232-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="73232-164">Este movimiento agrega un `UserSecretsId` elemento, que se rellena con un GUID a la *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="73232-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="73232-165">Visual Studio abre un *secrets.json* archivo en el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="73232-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="73232-166">Reemplace el contenido de *secrets.json* con los pares clave-valor que se almacenará.</span><span class="sxs-lookup"><span data-stu-id="73232-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="73232-167">Por ejemplo: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)].</span><span class="sxs-lookup"><span data-stu-id="73232-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="73232-168">Definir un secreto de la aplicación que consta de una clave y su valor.</span><span class="sxs-lookup"><span data-stu-id="73232-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="73232-169">El secreto está asociado con el proyecto `UserSecretsId` valor.</span><span class="sxs-lookup"><span data-stu-id="73232-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="73232-170">Por ejemplo, ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="73232-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="73232-171">En el ejemplo anterior, los dos puntos denota que `Movies` es un objeto literal con un `ServiceApiKey` propiedad.</span><span class="sxs-lookup"><span data-stu-id="73232-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="73232-172">La herramienta Administrador de secreto puede utilizarse desde otros directorios demasiado.</span><span class="sxs-lookup"><span data-stu-id="73232-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="73232-173">Use la `--project` opción para proporcionar la ruta de acceso del sistema de archivos en el que el *.csproj* archivo existe.</span><span class="sxs-lookup"><span data-stu-id="73232-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="73232-174">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="73232-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="73232-175">Establecer varios secretos</span><span class="sxs-lookup"><span data-stu-id="73232-175">Set multiple secrets</span></span>

<span data-ttu-id="73232-176">Un lote de secretos puede establecerse mediante la canalización de JSON a la `set` comando.</span><span class="sxs-lookup"><span data-stu-id="73232-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="73232-177">En el ejemplo siguiente, la *input.json* contenido del archivo se canaliza hacia el `set` comando.</span><span class="sxs-lookup"><span data-stu-id="73232-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="73232-178">Windows</span><span class="sxs-lookup"><span data-stu-id="73232-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="73232-179">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="73232-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="73232-180">macOS</span><span class="sxs-lookup"><span data-stu-id="73232-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="73232-181">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="73232-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="73232-182">Linux</span><span class="sxs-lookup"><span data-stu-id="73232-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="73232-183">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="73232-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="73232-184">Obtener acceso a un secreto</span><span class="sxs-lookup"><span data-stu-id="73232-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="73232-185">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de administrador de secreto.</span><span class="sxs-lookup"><span data-stu-id="73232-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="73232-186">Instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="73232-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="73232-187">Agregar el origen de configuración de usuario secretos con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="73232-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="73232-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="73232-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="73232-189">El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de administrador de secreto.</span><span class="sxs-lookup"><span data-stu-id="73232-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="73232-190">Si el proyecto tiene como destino .NET Framework, instale el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="73232-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="73232-191">En el núcleo de ASP.NET 2.0 o posterior, el origen de configuración de secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto se llama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar una nueva instancia del host con valores predeterminados preconfigurados.</span><span class="sxs-lookup"><span data-stu-id="73232-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="73232-192">`CreateDefaultBuilder` llamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) cuando el [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) es [desarrollo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="73232-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="73232-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="73232-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="73232-194">Cuando `CreateDefaultBuilder` no llama durante la construcción de host, agregue el origen de configuración de usuario secretos con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:</span><span class="sxs-lookup"><span data-stu-id="73232-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="73232-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="73232-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="73232-196">Secretos del usuario se pueden recuperar a través de la `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="73232-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="73232-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="73232-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="73232-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="73232-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="73232-199">Cadena de reemplazo con secretos</span><span class="sxs-lookup"><span data-stu-id="73232-199">String replacement with secrets</span></span>

<span data-ttu-id="73232-200">Almacenar contraseñas como texto sin formato es arriesgado.</span><span class="sxs-lookup"><span data-stu-id="73232-200">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="73232-201">Por ejemplo, una cadena de conexión de base de datos se almacena en *appSettings.JSON que se* puede incluir una contraseña para el usuario especificado:</span><span class="sxs-lookup"><span data-stu-id="73232-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="73232-202">Un enfoque más seguro consiste en almacenar la contraseña como un secreto.</span><span class="sxs-lookup"><span data-stu-id="73232-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="73232-203">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="73232-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="73232-204">Reemplace la contraseña en *appSettings.JSON que se* con un marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="73232-204">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="73232-205">En el ejemplo siguiente, `{0}` se utiliza como marcador de posición para formar un [cadena de formato compuesto](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="73232-205">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="73232-206">Valor del secreto se puede insertar en el marcador de posición para completar la cadena de conexión:</span><span class="sxs-lookup"><span data-stu-id="73232-206">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="73232-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="73232-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="73232-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="73232-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="73232-209">Enumeración de los secretos</span><span class="sxs-lookup"><span data-stu-id="73232-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="73232-210">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="73232-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="73232-211">Aparecerá el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="73232-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="73232-212">En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos dentro de *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="73232-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="73232-213">Quitar un secreto único</span><span class="sxs-lookup"><span data-stu-id="73232-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="73232-214">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="73232-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="73232-215">La aplicación *secrets.json* archivo se modificó para quitar el par de clave y valor asociado a la `MoviesConnectionString` clave:</span><span class="sxs-lookup"><span data-stu-id="73232-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="73232-216">Ejecuta `dotnet user-secrets list` muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="73232-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="73232-217">Quitar todos los secretos</span><span class="sxs-lookup"><span data-stu-id="73232-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="73232-218">Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:</span><span class="sxs-lookup"><span data-stu-id="73232-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="73232-219">Se han eliminado todos los secretos de usuario de la aplicación de la *secrets.json* archivo:</span><span class="sxs-lookup"><span data-stu-id="73232-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="73232-220">Ejecuta `dotnet user-secrets list` muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="73232-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="73232-221">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="73232-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
