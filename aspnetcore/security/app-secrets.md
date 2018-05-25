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
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://github.com/scottaddie)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

Este documento explica técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación de ASP.NET Core. Nunca debe almacenar las contraseñas u otros datos confidenciales en el código fuente, y no debe utilizar secretos de producción en el desarrollo o modo de prueba. Puede almacenar y proteger los secretos de prueba y producción Azure con el [proveedor de configuración de almacén de claves de Azure](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variables de entorno

Las variables de entorno se utilizan para evitar el almacenamiento de secretos de la aplicación en el código o en archivos de configuración local. Las variables de entorno invalidan los valores de configuración de todos los orígenes de configuración especificada anteriormente.

::: moniker range="<= aspnetcore-1.1"
Configurar la lectura de los valores de variables de entorno mediante una llamada a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el `Startup` constructor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Piense en una aplicación web de ASP.NET Core en la que **cuentas de usuario individuales** está habilitada la seguridad. Una cadena de conexión de base de datos predeterminada se incluye en el proyecto *appSettings.JSON que se* archivo con la clave `DefaultConnection`. La cadena de conexión predeterminada es LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña. Durante la implementación de aplicaciones, el `DefaultConnection` clave-valor puede reemplazarse por el valor de la variable de entorno. La variable de entorno puede almacenar la cadena de conexión completa con las credenciales confidenciales.

> [!WARNING]
> Las variables de entorno se almacenan normalmente en texto sin formato y sin cifrar. Si la máquina o el proceso se ve comprometido, confianza pueden tener acceso a las variables de entorno. Saber qué medidas adicionales para evitar la divulgación de información confidencial del usuario pueden ser necesarias.

## <a name="secret-manager"></a>Administrador de secreto

La herramienta Administrador de secreto almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core. En este contexto, una parte de los datos confidenciales es un secreto de la aplicación. Secretos de la aplicación se almacenan en una ubicación independiente desde el árbol del proyecto. Los secretos de la aplicación se asociada a un proyecto específico o se comparten entre varios proyectos. Los secretos de aplicación no se protegen en el control de origen.

> [!WARNING]
> La herramienta Administrador de secreto no cifra los secretos almacenados y no debe tratarse como un almacén de confianza. Es solo con fines de desarrollo. Las claves y los valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.

## <a name="how-the-secret-manager-tool-works"></a>Cómo funciona la herramienta Administrador de secreto

La herramienta Administrador de secreto abstrae los detalles de implementación, como dónde y cómo se almacenan los valores. Puede usar la herramienta sin conocer estos detalles de implementación. Los valores se almacenan en un archivo de configuración de JSON en una carpeta de perfiles de usuarios protegidos por el sistema en el equipo local:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Ruta de acceso de sistema de archivos:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Ruta de acceso de sistema de archivos:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Ruta de acceso de sistema de archivos:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

En los anteriores, las rutas de acceso de archivo, reemplace `<user_secrets_id>` con el `UserSecretsId` valor especificado en el *.csproj* archivo.

No escriba código que depende de la ubicación o el formato de datos que se guardan con la herramienta Administrador de secreto. Pueden cambiar estos detalles de implementación. Por ejemplo, los valores secretos no se cifran, pero pudieron ser en el futuro.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Instalar la herramienta Administrador de secreto

La herramienta Administrador de secreto se incluye con la CLI de núcleo de .NET a partir de .NET Core SDK 2.1.300. Para las versiones de .NET Core SDK anteriores 2.1.300, es necesaria la instalación de herramientas.

> [!TIP]
> Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión de .NET Core SDK instalado.

Se mostrará una advertencia si se usa .NET Core SDK incluye la herramienta:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Instalar el [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) paquete de NuGet en el proyecto de ASP.NET Core. Por ejemplo:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Ejecute el siguiente comando en un shell de comandos para validar la instalación de la herramienta:

```console
dotnet user-secrets -h
```

La herramienta Administrador de secreto muestra ejemplo de uso, opciones y la Ayuda de comando:

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
> Debe estar en el mismo directorio que el *.csproj* archivo para ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` elementos.
::: moniker-end

## <a name="set-a-secret"></a>Establecer un secreto

La herramienta Administrador de secreto opera en valores de configuración específicos del proyecto almacenados en su perfil de usuario. Para utilizar secretos del usuario, definir una `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo. El valor de `UserSecretsId` es arbitrario, pero es único para el proyecto. Los desarrolladores suelen generan un GUID para el `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> En Visual Studio, haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar secretos del usuario** en el menú contextual. Este movimiento agrega un `UserSecretsId` elemento, que se rellena con un GUID a la *.csproj* archivo. Visual Studio abre un *secrets.json* archivo en el editor de texto. Reemplace el contenido de *secrets.json* con los pares clave-valor que se almacenará. Por ejemplo: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)].

Definir un secreto de la aplicación que consta de una clave y su valor. El secreto está asociado con el proyecto `UserSecretsId` valor. Por ejemplo, ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

En el ejemplo anterior, los dos puntos denota que `Movies` es un objeto literal con un `ServiceApiKey` propiedad.

La herramienta Administrador de secreto puede utilizarse desde otros directorios demasiado. Use la `--project` opción para proporcionar la ruta de acceso del sistema de archivos en el que el *.csproj* archivo existe. Por ejemplo:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Establecer varios secretos

Un lote de secretos puede establecerse mediante la canalización de JSON a la `set` comando. En el ejemplo siguiente, la *input.json* contenido del archivo se canaliza hacia el `set` comando.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Abra un shell de comandos y ejecute el siguiente comando:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Abra un shell de comandos y ejecute el siguiente comando:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Abra un shell de comandos y ejecute el siguiente comando:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Obtener acceso a un secreto

::: moniker range="<= aspnetcore-1.1"
El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de administrador de secreto. Instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.

Agregar el origen de configuración de usuario secretos con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de administrador de secreto. Si el proyecto tiene como destino .NET Framework, instale el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.

En el núcleo de ASP.NET 2.0 o posterior, el origen de configuración de secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto se llama [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar una nueva instancia del host con valores predeterminados preconfigurados. `CreateDefaultBuilder` llamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) cuando el [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) es [desarrollo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Cuando `CreateDefaultBuilder` no llama durante la construcción de host, agregue el origen de configuración de usuario secretos con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Secretos del usuario se pueden recuperar a través de la `Configuration` API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Cadena de reemplazo con secretos

Almacenar contraseñas como texto sin formato es arriesgado. Por ejemplo, una cadena de conexión de base de datos se almacena en *appSettings.JSON que se* puede incluir una contraseña para el usuario especificado:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Un enfoque más seguro consiste en almacenar la contraseña como un secreto. Por ejemplo:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Reemplace la contraseña en *appSettings.JSON que se* con un marcador de posición. En el ejemplo siguiente, `{0}` se utiliza como marcador de posición para formar un [cadena de formato compuesto](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Valor del secreto se puede insertar en el marcador de posición para completar la cadena de conexión:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
::: moniker-end

## <a name="list-the-secrets"></a>Enumeración de los secretos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets list
```

Aparecerá el siguiente resultado:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos dentro de *secrets.json*.

## <a name="remove-a-single-secret"></a>Quitar un secreto único

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

La aplicación *secrets.json* archivo se modificó para quitar el par de clave y valor asociado a la `MoviesConnectionString` clave:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Ejecuta `dotnet user-secrets list` muestra el siguiente mensaje:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Quitar todos los secretos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets clear
```

Se han eliminado todos los secretos de usuario de la aplicación de la *secrets.json* archivo:

```json
{}
```

Ejecuta `dotnet user-secrets list` muestra el siguiente mensaje:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
