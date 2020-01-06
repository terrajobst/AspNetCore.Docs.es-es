---
title: Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core
author: rick-anderson
description: Aprenda a almacenar y recuperar información confidencial como secretos de la aplicación durante el desarrollo de una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: 9b36ae64fbe277cd81ed22ba7b21b0a035082dbd
ms.sourcegitcommit: c815a9465e7b1bab44ce1643ec345b33e6cf1598
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/02/2020
ms.locfileid: "75606797"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="4c5b9-103">Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4c5b9-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="4c5b9-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)y [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="4c5b9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="4c5b9-105">[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4c5b9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="4c5b9-106">En este documento se explican técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación ASP.NET Core en una máquina de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-106">This document explains techniques for storing and retrieving sensitive data during development of an ASP.NET Core app on a development machine.</span></span> <span data-ttu-id="4c5b9-107">Nunca almacene contraseñas u otros datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="4c5b9-108">Los secretos de producción no deben usarse para el desarrollo o las pruebas.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="4c5b9-109">Los secretos no deben implementarse con la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-109">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="4c5b9-110">En su lugar, los secretos deben estar disponibles en el entorno de producción a través de un medio controlado como variables de entorno, Azure Key Vault, etc. Puede almacenar y proteger los secretos de prueba y producción de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="4c5b9-110">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="4c5b9-111">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="4c5b9-111">Environment variables</span></span>

<span data-ttu-id="4c5b9-112">Las variables de entorno se usan para evitar el almacenamiento de secretos de aplicación en código o en archivos de configuración local.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-112">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="4c5b9-113">Las variables de entorno invalidan los valores de configuración de todos los orígenes de configuración especificados previamente.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-113">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="4c5b9-114">Configure la lectura de los valores de las variables de entorno mediante una llamada a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> en el constructor de `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-114">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="4c5b9-115">Considere una ASP.NET Core aplicación web en la que está habilitada la seguridad de **cuentas de usuario individuales** .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="4c5b9-116">Se incluye una cadena de conexión de base de datos predeterminada en el archivo *appSettings. JSON* del proyecto con la clave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="4c5b9-117">La cadena de conexión predeterminada es para LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="4c5b9-118">Durante la implementación de la aplicación, el valor de la clave de `DefaultConnection` se puede invalidar con el valor de una variable de entorno.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="4c5b9-119">La variable de entorno puede almacenar la cadena de conexión completa con credenciales confidenciales.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="4c5b9-120">Normalmente, las variables de entorno se almacenan en texto sin cifrar.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="4c5b9-121">Si el equipo o el proceso se ve comprometido, las entidades de entorno pueden tener acceso a las variables de entorno que no son de confianza.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="4c5b9-122">Pueden ser necesarias medidas adicionales para evitar la divulgación de secretos de usuario.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="4c5b9-123">Administrador de secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-123">Secret Manager</span></span>

<span data-ttu-id="4c5b9-124">La herramienta Administrador de secretos almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="4c5b9-125">En este contexto, una parte de la información confidencial es un secreto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="4c5b9-126">Los secretos de la aplicación se almacenan en una ubicación independiente del árbol del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="4c5b9-127">Los secretos de la aplicación se asocian a un proyecto específico o se comparten entre varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="4c5b9-128">Los secretos de la aplicación no se protegen en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="4c5b9-129">La herramienta Administrador de secretos no cifra los secretos almacenados y no debe tratarse como un almacén de confianza.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="4c5b9-130">Solo es para fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-130">It's for development purposes only.</span></span> <span data-ttu-id="4c5b9-131">Las claves y los valores se almacenan en un archivo de configuración JSON en el directorio del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="4c5b9-132">Cómo funciona la herramienta Administrador de secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="4c5b9-133">La herramienta Administrador de secretos abstrae los detalles de implementación, como dónde y cómo se almacenan los valores.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="4c5b9-134">Puede usar la herramienta sin conocer estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="4c5b9-135">Los valores se almacenan en un archivo de configuración JSON en una carpeta de Perfil de usuario protegida por el sistema en el equipo local:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-135">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4c5b9-136">Windows</span><span class="sxs-lookup"><span data-stu-id="4c5b9-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="4c5b9-137">Ruta de acceso del sistema de archivos:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="4c5b9-138">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="4c5b9-138">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="4c5b9-139">Ruta de acceso del sistema de archivos:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="4c5b9-140">En las rutas de acceso de archivo anteriores, reemplace `<user_secrets_id>` por el valor de `UserSecretsId` especificado en el archivo *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="4c5b9-141">No Escriba código que dependa de la ubicación o el formato de los datos guardados con la herramienta Administrador de secretos.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="4c5b9-142">Estos detalles de implementación pueden cambiar.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-142">These implementation details may change.</span></span> <span data-ttu-id="4c5b9-143">Por ejemplo, los valores de secreto no están cifrados, pero podrían estar en el futuro.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="4c5b9-144">Instalar la herramienta Administrador de secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="4c5b9-145">La herramienta de administración de secretos está incluida en el CLI de .NET Core en SDK de .NET Core 2.1.300 o posterior.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="4c5b9-146">En el caso de las versiones de SDK de .NET Core antes de 2.1.300, es necesaria la instalación de la herramienta.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="4c5b9-147">Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión de SDK de .NET Core instalado.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="4c5b9-148">Se muestra una advertencia si el SDK de .NET Core que se está usando incluye la herramienta:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="4c5b9-149">Instale el paquete de NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) en el proyecto de ASP.net Core.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="4c5b9-150">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="4c5b9-151">Ejecute el siguiente comando en un shell de comandos para validar la instalación de la herramienta:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="4c5b9-152">La herramienta Administrador de secretos muestra el uso de ejemplo, las opciones y la ayuda de comandos:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="4c5b9-153">Debe estar en el mismo directorio que el archivo *. csproj* para ejecutar las herramientas definidas en los elementos `DotNetCliToolReference` del archivo *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="4c5b9-154">Habilitar el almacenamiento de secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-154">Enable secret storage</span></span>

<span data-ttu-id="4c5b9-155">La herramienta Administrador de secretos funciona en opciones de configuración específicas del proyecto almacenadas en el perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="4c5b9-156">La herramienta Administrador de secretos incluye un comando `init` en SDK de .NET Core 3.0.100 o posterior.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-156">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="4c5b9-157">Para usar los secretos de usuario, ejecute el siguiente comando en el directorio del proyecto:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-157">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="4c5b9-158">El comando anterior agrega un elemento `UserSecretsId` dentro de un `PropertyGroup` del archivo *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-158">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="4c5b9-159">De forma predeterminada, el texto interno de `UserSecretsId` es un GUID.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-159">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="4c5b9-160">El texto interno es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-160">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="4c5b9-161">Para usar secretos de usuario, defina un elemento `UserSecretsId` dentro de un `PropertyGroup` del archivo *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-161">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="4c5b9-162">El texto interno de `UserSecretsId` es arbitrario, pero es único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-162">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="4c5b9-163">Normalmente, los desarrolladores generan un GUID para el `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-163">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="4c5b9-164">En Visual Studio, haga clic con el botón derecho en el proyecto en Explorador de soluciones y seleccione **administrar secretos de usuario** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-164">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="4c5b9-165">Este gesto agrega un elemento `UserSecretsId`, rellenado con un GUID, al archivo *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-165">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="4c5b9-166">Establecer un secreto</span><span class="sxs-lookup"><span data-stu-id="4c5b9-166">Set a secret</span></span>

<span data-ttu-id="4c5b9-167">Defina un secreto de la aplicación que conste de una clave y su valor.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="4c5b9-168">El secreto está asociado con el valor de `UserSecretsId` del proyecto.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="4c5b9-169">Por ejemplo, ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="4c5b9-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="4c5b9-170">En el ejemplo anterior, el signo de dos puntos denota que `Movies` es un literal de objeto con una propiedad `ServiceApiKey`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="4c5b9-171">La herramienta Administrador de secretos también se puede usar desde otros directorios.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="4c5b9-172">Use la opción `--project` para proporcionar la ruta de acceso del sistema de archivos en la que existe el archivo *. csproj* .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="4c5b9-173">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-173">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="4c5b9-174">Simplificación de la estructura de JSON en Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4c5b9-174">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="4c5b9-175">El gesto de **administrar secretos de usuario** de Visual Studio abre un archivo *Secrets. JSON* en el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-175">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="4c5b9-176">Reemplace el contenido de *Secrets. JSON* por los pares clave-valor que se van a almacenar.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-176">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="4c5b9-177">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-177">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="4c5b9-178">La estructura JSON se alisa después de las modificaciones a través de `dotnet user-secrets remove` o `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-178">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="4c5b9-179">Por ejemplo, al ejecutar `dotnet user-secrets remove "Movies:ConnectionString"` se contrae el literal de objeto `Movies`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-179">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="4c5b9-180">El archivo modificado es similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-180">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="4c5b9-181">Establecer varios secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-181">Set multiple secrets</span></span>

<span data-ttu-id="4c5b9-182">Un lote de secretos se puede establecer mediante el JSON de canalización en el comando `set`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-182">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="4c5b9-183">En el ejemplo siguiente, el contenido del archivo *Input. JSON* se canaliza al comando `set`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-183">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4c5b9-184">Windows</span><span class="sxs-lookup"><span data-stu-id="4c5b9-184">Windows</span></span>](#tab/windows)

<span data-ttu-id="4c5b9-185">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-185">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="4c5b9-186">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="4c5b9-186">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="4c5b9-187">Abra un shell de comandos y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-187">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="4c5b9-188">Acceder a un secreto</span><span class="sxs-lookup"><span data-stu-id="4c5b9-188">Access a secret</span></span>

<span data-ttu-id="4c5b9-189">La [API de configuración de ASP.net Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos del administrador de secretos.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="4c5b9-190">Si el proyecto tiene como destino .NET Framework, instale el paquete NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-190">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4c5b9-191">En ASP.NET Core 2,0 o posterior, el origen de configuración de los secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto llama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> para inicializar una nueva instancia del host con los valores predeterminados preconfigurados.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="4c5b9-192">`CreateDefaultBuilder` llama <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> cuando se <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>el <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-192">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="4c5b9-193">Cuando no se llama a `CreateDefaultBuilder`, agregue el origen de configuración de secretos de usuario explícitamente llamando a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> en el constructor de `Startup`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-193">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="4c5b9-194">Llame solo a `AddUserSecrets` cuando la aplicación se ejecute en el entorno de desarrollo, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-194">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="4c5b9-195">Instale el paquete NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .</span><span class="sxs-lookup"><span data-stu-id="4c5b9-195">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="4c5b9-196">Agregue el origen de configuración de secretos de usuario con una llamada a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> en el constructor de `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-196">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="4c5b9-197">Los secretos de usuario se pueden recuperar a través de la API de `Configuration`:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-197">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="4c5b9-198">Asignación de secretos a un POCO</span><span class="sxs-lookup"><span data-stu-id="4c5b9-198">Map secrets to a POCO</span></span>

<span data-ttu-id="4c5b9-199">La asignación de un literal de objeto completo a un POCO (una clase .NET simple con propiedades) es útil para agregar propiedades relacionadas.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-199">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="4c5b9-200">Para asignar los secretos anteriores a un POCO, use la característica de [enlace de gráficos de objetos](xref:fundamentals/configuration/index#bind-to-an-object-graph) de la API de `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-200">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="4c5b9-201">El código siguiente enlaza a un `MovieSettings` POCO personalizado y obtiene acceso al valor de la propiedad `ServiceApiKey`:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-201">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="4c5b9-202">Los secretos de `Movies:ConnectionString` y `Movies:ServiceApiKey` se asignan a las propiedades correspondientes en `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-202">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="4c5b9-203">Reemplazo de cadenas con secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-203">String replacement with secrets</span></span>

<span data-ttu-id="4c5b9-204">Almacenar las contraseñas en texto sin formato no es seguro.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-204">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="4c5b9-205">Por ejemplo, una cadena de conexión de base de datos almacenada en *appSettings. JSON* puede incluir una contraseña para el usuario especificado:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-205">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="4c5b9-206">Un enfoque más seguro consiste en almacenar la contraseña como un secreto.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-206">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="4c5b9-207">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-207">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="4c5b9-208">Quite el `Password` par clave-valor de la cadena de conexión en *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-208">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="4c5b9-209">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-209">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="4c5b9-210">El valor del secreto se puede establecer en la propiedad <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> de un objeto <xref:System.Data.SqlClient.SqlConnectionStringBuilder> para completar la cadena de conexión:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-210">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="4c5b9-211">Enumeración de los secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-211">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="4c5b9-212">Ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="4c5b9-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="4c5b9-213">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-213">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="4c5b9-214">En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos en *Secrets. JSON*.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-214">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="4c5b9-215">Quitar un único secreto</span><span class="sxs-lookup"><span data-stu-id="4c5b9-215">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="4c5b9-216">Ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="4c5b9-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="4c5b9-217">El archivo *Secrets. JSON* de la aplicación se modificó para quitar el par clave-valor asociado a la clave de `MoviesConnectionString`:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-217">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="4c5b9-218">Al ejecutar `dotnet user-secrets list` se muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="4c5b9-219">Quitar todos los secretos</span><span class="sxs-lookup"><span data-stu-id="4c5b9-219">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="4c5b9-220">Ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :</span><span class="sxs-lookup"><span data-stu-id="4c5b9-220">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="4c5b9-221">Todos los secretos de usuario de la aplicación se han eliminado del archivo *Secrets. JSON* :</span><span class="sxs-lookup"><span data-stu-id="4c5b9-221">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="4c5b9-222">Al ejecutar `dotnet user-secrets list` se muestra el siguiente mensaje:</span><span class="sxs-lookup"><span data-stu-id="4c5b9-222">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="4c5b9-223">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="4c5b9-223">Additional resources</span></span>

* <span data-ttu-id="4c5b9-224">Vea [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/16328) para obtener información sobre cómo obtener acceso a Secret Manager desde IIS.</span><span class="sxs-lookup"><span data-stu-id="4c5b9-224">See [this issue](https://github.com/aspnet/AspNetCore.Docs/issues/16328) for information on accessing Secret Manager from IIS.</span></span>
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
