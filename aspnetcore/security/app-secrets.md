---
title: "Ubicación de almacenamiento segura de secretos de aplicación durante el desarrollo de ASP.NET Core"
author: rick-anderson
description: "Muestra cómo almacenar secretos de forma segura durante el desarrollo"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 09/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 280819a6a0afb72311f0d50f7d3b83a942e9fcc3
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="1c682-104">Ubicación de almacenamiento segura de secretos de aplicación durante el desarrollo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1c682-104">Safe storage of app secrets during development in ASP.NET Core</span></span>

<span data-ttu-id="1c682-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="1c682-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="1c682-106">Este documento muestra cómo puede usar la herramienta Administrador de secreto en el desarrollo para mantener secretos fuera del código.</span><span class="sxs-lookup"><span data-stu-id="1c682-106">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="1c682-107">El punto más importante es nunca debe almacenar las contraseñas u otros datos confidenciales en el código fuente y no utilice secretos de producción en modo de desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="1c682-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="1c682-108">En su lugar, puede usar el [configuración](../fundamentals/configuration.md) sistema para leer estos valores de variables de entorno o de valores almacenados mediante el Administrador de secreto de la herramienta.</span><span class="sxs-lookup"><span data-stu-id="1c682-108">You can instead use the [configuration](../fundamentals/configuration.md) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="1c682-109">La herramienta Administrador de secreto ayuda a evitar que los datos confidenciales que se protegen en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="1c682-109">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="1c682-110">El [configuración](../fundamentals/configuration.md) sistema pueda leer los secretos almacenados con la herramienta Administrador de secreto se describe en este artículo.</span><span class="sxs-lookup"><span data-stu-id="1c682-110">The [configuration](../fundamentals/configuration.md) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="1c682-111">La herramienta Administrador de secreto se usa solo en el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1c682-111">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="1c682-112">Puede proteger los secretos de prueba y producción Azure con el [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) proveedor de configuración.</span><span class="sxs-lookup"><span data-stu-id="1c682-112">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="1c682-113">Vea [proveedor de configuración de almacén de claves de Azure](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="1c682-113">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="1c682-114">Variables de entorno</span><span class="sxs-lookup"><span data-stu-id="1c682-114">Environment variables</span></span>

<span data-ttu-id="1c682-115">Para evitar almacenar secretos de aplicación en el código o en archivos de configuración local, almacenar secretos en variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="1c682-115">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="1c682-116">Puede configurar la [configuración](../fundamentals/configuration.md) framework para leer los valores de variables de entorno mediante una llamada a `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="1c682-116">You can setup the [configuration](../fundamentals/configuration.md) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="1c682-117">A continuación, puede utilizar variables de entorno para reemplazar los valores de configuración para todos los orígenes de configuración especificada anteriormente.</span><span class="sxs-lookup"><span data-stu-id="1c682-117">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="1c682-118">Por ejemplo, si crea una nueva aplicación web de ASP.NET Core con cuentas de usuario individuales, agregará una cadena de conexión predeterminada para el *appSettings.JSON que se* archivo en el proyecto con la clave `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="1c682-118">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="1c682-119">La cadena de conexión predeterminada está configurado para utilizar LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña.</span><span class="sxs-lookup"><span data-stu-id="1c682-119">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="1c682-120">Al implementar la aplicación en un servidor de producción o de prueba, puede invalidar la `DefaultConnection` valor de clave con una configuración de variable de entorno que contiene la cadena de conexión (posiblemente con credenciales confidenciales) para una base de datos de producción o de prueba servidor.</span><span class="sxs-lookup"><span data-stu-id="1c682-120">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="1c682-121">Las variables de entorno se almacenan normalmente en texto sin formato y no se cifran.</span><span class="sxs-lookup"><span data-stu-id="1c682-121">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="1c682-122">Si la máquina o el proceso se ve comprometido, las variables de entorno son accesibles por entidades de confianza.</span><span class="sxs-lookup"><span data-stu-id="1c682-122">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="1c682-123">Saber qué medidas adicionales para evitar la divulgación de información confidencial del usuario todavía pueden ser necesarias.</span><span class="sxs-lookup"><span data-stu-id="1c682-123">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="1c682-124">Administrador de secreto</span><span class="sxs-lookup"><span data-stu-id="1c682-124">Secret Manager</span></span>

<span data-ttu-id="1c682-125">La herramienta Administrador de secreto almacena datos confidenciales en el trabajo de desarrollo fuera de su árbol de proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c682-125">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="1c682-126">La herramienta Administrador de secreto es una herramienta de proyecto que puede usarse para almacenar secretos de un [.NET Core](https://www.microsoft.com/net/core) proyecto durante el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1c682-126">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="1c682-127">Con la herramienta Administrador de secreto, puede asociar los secretos de aplicación a un proyecto específico y compartirlos en varios proyectos.</span><span class="sxs-lookup"><span data-stu-id="1c682-127">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="1c682-128">La herramienta Administrador de secreto no cifra los secretos almacenados y no debe tratarse como un almacén de confianza.</span><span class="sxs-lookup"><span data-stu-id="1c682-128">The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store.</span></span> <span data-ttu-id="1c682-129">Es solo con fines de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="1c682-129">It is for development purposes only.</span></span> <span data-ttu-id="1c682-130">Las claves y los valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="1c682-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="1c682-131">Instalar la herramienta Administrador de secreto</span><span class="sxs-lookup"><span data-stu-id="1c682-131">Installing the Secret Manager tool</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1c682-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1c682-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="1c682-133">Haga clic en el proyecto en el Explorador de soluciones y seleccione **editar \<Nombre_proyecto\>.csproj** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="1c682-133">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="1c682-134">Agregue la línea resaltada en el *.csproj* y guardar archivos para restaurar el paquete NuGet asociado:</span><span class="sxs-lookup"><span data-stu-id="1c682-134">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="1c682-135">Haga clic de nuevo en el proyecto en el Explorador de soluciones y seleccione **administrar secretos del usuario** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="1c682-135">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="1c682-136">Este movimiento agrega un nuevo `UserSecretsId` nodo dentro de un `PropertyGroup` de la *.csproj* archivo, como se resalta en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c682-136">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="1c682-137">Guardar modificados *.csproj* archivo también se abre un `secrets.json` archivo en el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="1c682-137">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="1c682-138">Reemplace el contenido de la `secrets.json` archivo con el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c682-138">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1c682-139">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1c682-139">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1c682-140">Agregar `Microsoft.Extensions.SecretManager.Tools` a la *.csproj* de archivos y ejecutar `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="1c682-140">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span> <span data-ttu-id="1c682-141">Puede usar los mismos pasos para instalar la herramienta de administrador de secreto mediante la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="1c682-141">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="1c682-142">Probar la herramienta Administrador de secreto ejecutando el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="1c682-142">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="1c682-143">La herramienta Administrador de secreto mostrará uso, opciones y ayuda del comando.</span><span class="sxs-lookup"><span data-stu-id="1c682-143">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="1c682-144">Debe estar en el mismo directorio que el *.csproj* archivo para ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` nodos.</span><span class="sxs-lookup"><span data-stu-id="1c682-144">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="1c682-145">La herramienta Administrador de secreto opera en valores de configuración específicos de proyecto que se almacenan en el perfil de usuario.</span><span class="sxs-lookup"><span data-stu-id="1c682-145">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="1c682-146">Para utilizar secretos del usuario, debe especificar el proyecto una `UserSecretsId` valor en su *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="1c682-146">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="1c682-147">El valor de `UserSecretsId` es arbitrario, pero es generalmente único para el proyecto.</span><span class="sxs-lookup"><span data-stu-id="1c682-147">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="1c682-148">Los desarrolladores suelen generan un GUID para el `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="1c682-148">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="1c682-149">Agregar un `UserSecretsId` para el proyecto en el *.csproj* archivo:</span><span class="sxs-lookup"><span data-stu-id="1c682-149">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="1c682-150">Utilice la herramienta Administrador de secreto para establecer un secreto.</span><span class="sxs-lookup"><span data-stu-id="1c682-150">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="1c682-151">Por ejemplo, en una ventana de comandos desde el directorio del proyecto, escriba lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1c682-151">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="1c682-152">Puede ejecutar la herramienta Administrador de secreto desde otros directorios, pero debe utilizar el `--project` opción para pasar la ruta de acceso a la *.csproj* archivo:</span><span class="sxs-lookup"><span data-stu-id="1c682-152">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="1c682-153">También puede usar la herramienta Administrador de secreto para enumerar, quitar y borrar secretos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1c682-153">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

-----

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="1c682-154">Obtener acceso a información confidencial del usuario mediante la configuración</span><span class="sxs-lookup"><span data-stu-id="1c682-154">Accessing user secrets via configuration</span></span>

<span data-ttu-id="1c682-155">Obtener acceso a los secretos de administrador de secreto a través del sistema de configuración.</span><span class="sxs-lookup"><span data-stu-id="1c682-155">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="1c682-156">Agregar el `Microsoft.Extensions.Configuration.UserSecrets` empaquetar y ejecutar `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="1c682-156">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="1c682-157">Agregar el origen de configuración de secretos de usuario para el `Startup` método:</span><span class="sxs-lookup"><span data-stu-id="1c682-157">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="1c682-158">Puede tener acceso a los secretos del usuario a través de la API de configuración:</span><span class="sxs-lookup"><span data-stu-id="1c682-158">You can access user secrets via the configuration API:</span></span>

[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="1c682-159">Cómo funciona la herramienta Administrador de secreto</span><span class="sxs-lookup"><span data-stu-id="1c682-159">How the Secret Manager tool works</span></span>

<span data-ttu-id="1c682-160">La herramienta Administrador de secreto abstrae los detalles de implementación, como dónde y cómo se almacenan los valores.</span><span class="sxs-lookup"><span data-stu-id="1c682-160">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="1c682-161">Puede usar la herramienta sin conocer estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="1c682-161">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="1c682-162">En la versión actual, los valores se almacenan en un [JSON](http://json.org/) archivo de configuración en el directorio del perfil de usuario:</span><span class="sxs-lookup"><span data-stu-id="1c682-162">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="1c682-163">Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="1c682-163">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="1c682-164">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="1c682-164">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="1c682-165">Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="1c682-165">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="1c682-166">El valor de `userSecretsId` proviene del valor especificado en *.csproj* archivo.</span><span class="sxs-lookup"><span data-stu-id="1c682-166">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="1c682-167">No debe escribirse código que depende de la ubicación o el formato de los datos guardados con la herramienta Administrador de secreto, dado que podrían cambiar estos detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="1c682-167">You should not write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="1c682-168">Por ejemplo, los valores secretos están *no* cifra hoy en día, pero también podría ser algún día.</span><span class="sxs-lookup"><span data-stu-id="1c682-168">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1c682-169">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1c682-169">Additional Resources</span></span>

* [<span data-ttu-id="1c682-170">Configuración</span><span class="sxs-lookup"><span data-stu-id="1c682-170">Configuration</span></span>](../fundamentals/configuration.md)
