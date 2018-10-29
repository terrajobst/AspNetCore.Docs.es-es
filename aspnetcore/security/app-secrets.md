---
title: Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo almacenar y recuperar información confidencial como secretos de aplicación durante el desarrollo de una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/24/2018
uid: security/app-secrets
ms.openlocfilehash: 385d0ecc6ea19d5f84a9fe3c2754f5256a2a5576
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207438"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), y [Scott Addie](https://github.com/scottaddie)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

Este documento explica técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación ASP.NET Core. Nunca almacene contraseñas u otros datos confidenciales en el código fuente. Secretos de producción no deben usarse para el desarrollo o prueba. Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variables de entorno

Las variables de entorno se usan para evitar el almacenamiento de secretos de aplicación en el código o en archivos de configuración local. Las variables de entorno invalidan los valores de configuración para todos los orígenes de configuración especificada anteriormente.

::: moniker range="<= aspnetcore-1.1"

Configurar la lectura de valores de variables de entorno mediante una llamada a [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) en el `Startup` constructor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Considere la posibilidad de una aplicación web de ASP.NET Core en el que **cuentas de usuario individuales** está habilitada la seguridad. Una cadena de conexión de base de datos predeterminada se incluye en el proyecto *appsettings.json* archivo con la clave `DefaultConnection`. La cadena de conexión predeterminada es LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña. Durante la implementación de aplicaciones, el `DefaultConnection` se puede invalidar el valor de clave con el valor de la variable de entorno. La variable de entorno puede almacenar la cadena de conexión completa con credenciales confidenciales.

> [!WARNING]
> Las variables de entorno se almacenan normalmente en texto sin formato y sin cifrar. Si la máquina o el proceso se ve comprometido, las variables de entorno pueden tener acceso por partes de confianza. Medidas adicionales para evitar la divulgación de secretos de usuario pueden ser necesarias.

## <a name="secret-manager"></a>Administrador de secretos

La herramienta Secret Manager almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core. En este contexto, un fragmento de datos confidenciales es un secreto de la aplicación. Los secretos de aplicación se almacenan en una ubicación independiente desde el árbol del proyecto. Los secretos de aplicación se asociado con un proyecto específico o se comparten entre varios proyectos. No se comprueban los secretos de aplicación en control de código fuente.

> [!WARNING]
> La herramienta Secret Manager no cifra los secretos almacenados y no debe tratarse como un almacén de confianza. Es solo con fines de desarrollo. Las claves y valores se almacenan en un archivo de configuración de JSON en el directorio del perfil de usuario.

## <a name="how-the-secret-manager-tool-works"></a>Cómo funciona la herramienta Secret Manager

La herramienta Secret Manager abstrae los detalles de implementación, como dónde y cómo se almacenan los valores. Puede usar la herramienta sin conocer estos detalles de implementación. Los valores se almacenan en un archivo de configuración de JSON en una carpeta de perfil de usuario protegidos por el sistema en el equipo local:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Ruta de acceso de archivo del sistema:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Ruta de acceso de archivo del sistema:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Ruta de acceso de archivo del sistema:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

En la anterior, las rutas de acceso de archivo, reemplace `<user_secrets_id>` con el `UserSecretsId` valor especificado en el *.csproj* archivo.

No escriba código que depende de la ubicación o el formato de datos guardados con la herramienta Secret Manager. Pueden cambiar estos detalles de implementación. Por ejemplo, los valores de secreto no se cifran, pero podrían ser en el futuro.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Instalar la herramienta Secret Manager

La herramienta Secret Manager está empaquetado con la CLI de .NET Core SDK de .NET Core 2.1.300 o posterior. Para las versiones de SDK de .NET Core 2.1.300 antes de la instalación de herramientas es necesaria.

> [!TIP]
> Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión del SDK de .NET Core instalado.

Se mostrará una advertencia si se usa el SDK de .NET Core incluye la herramienta:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Instalar el [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) paquete de NuGet en el proyecto de ASP.NET Core. Por ejemplo:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Ejecute el siguiente comando en un shell de comandos para validar la instalación de herramienta:

```console
dotnet user-secrets -h
```

La herramienta Secret Manager muestra el uso de ejemplo, las opciones y ayuda del comando:

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
> Debe estar en el mismo directorio que el *.csproj* archivo va a ejecutar las herramientas que se definen en el *.csproj* del archivo `DotNetCliToolReference` elementos.

::: moniker-end

## <a name="set-a-secret"></a>Establezca un secreto

La herramienta Secret Manager opera en valores de configuración de específicas del proyecto almacenados en su perfil de usuario. Para usar secretos de usuario, defina un `UserSecretsId` elemento dentro de un `PropertyGroup` de la *.csproj* archivo. El valor de `UserSecretsId` es arbitrario, pero es único para el proyecto. Los desarrolladores suelen generan un GUID para el `UserSecretsId`.

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> En Visual Studio, haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar secretos de usuario** en el menú contextual. Este movimiento se agrega un `UserSecretsId` elemento, se rellena con un GUID, a la *.csproj* archivo. Visual Studio abre un *secrets.json* archivo en el editor de texto. Reemplace el contenido de *secrets.json* con los pares clave-valor que se almacenará. Por ejemplo:
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> Se aplana la estructura JSON después de las modificaciones a través de `dotnet user-secrets remove` o `dotnet user-secrets set`. Por ejemplo, ejecutar `dotnet user-secrets remove "Movies:ConnectionString"` contrae el `Movies` literal de objeto. El archivo modificado se parece a lo siguiente:
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

Definir un secreto de la aplicación que consta de una clave y su valor. El secreto está asociado con el proyecto `UserSecretsId` valor. Por ejemplo, ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

En el ejemplo anterior, los dos puntos denota que `Movies` es un objeto literal con una `ServiceApiKey` propiedad.

La herramienta Secret Manager puede usarse también de otros directorios. Use la `--project` opción para proporcionar la ruta de acceso de archivo del sistema en el que el *.csproj* archivo existe. Por ejemplo:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Establezca varios secretos

Un lote de los secretos se puede establecer mediante la canalización de JSON para el `set` comando. En el ejemplo siguiente, la *input.json* contenido del archivo se canaliza hacia el `set` comando.

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

::: moniker range=">= aspnetcore-2.0"

El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager. Si el proyecto tiene como destino .NET Framework, instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.

En ASP.NET Core 2.0 o posterior, el origen de configuración de los secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto llama a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) para inicializar una nueva instancia del host con valores predeterminados preconfigurados. `CreateDefaultBuilder` las llamadas [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) cuando el [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) es [desarrollo](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Cuando `CreateDefaultBuilder` no llama durante la construcción de host, agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

El [API de configuración de ASP.NET Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos de Secret Manager. Instalar el [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) paquete NuGet.

Agregar el origen de configuración de los secretos de usuario con una llamada a [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) en el `Startup` constructor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Se pueden recuperar secretos del usuario a través de la `Configuration` API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Los secretos se asignan a un objeto POCO

Asignación de un literal de objeto completo a un objeto POCO (una clase .NET simple con propiedades) es útil para la agregación de las propiedades relacionadas.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Para asignar los secretos anteriores a un objeto POCO, utilice el `Configuration` API [el enlace del gráfico de objetos](xref:fundamentals/configuration/index#bind-to-an-object-graph) característica. El código siguiente se enlaza a una personalizada `MovieSettings` POCO y tiene acceso a la `ServiceApiKey` valor de propiedad:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

El `Movies:ConnectionString` y `Movies:ServiceApiKey` secretos se asignan a las propiedades correspondientes en `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Cadena de reemplazo con secretos

No es seguro almacenar contraseñas como texto sin formato. Por ejemplo, una cadena de conexión de base de datos se almacena en *appsettings.json* puede incluir una contraseña para el usuario especificado:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Un enfoque más seguro es almacenar la contraseña como un secreto. Por ejemplo:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Quitar el `Password` par clave-valor de la cadena de conexión en *appsettings.json*. Por ejemplo:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Se puede establecer el valor del secreto en un [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) del objeto [contraseña](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) propiedad para completar la cadena de conexión:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Enumera los secretos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets list
```

Aparece el siguiente resultado:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos dentro de *secrets.json*.

## <a name="remove-a-single-secret"></a>Quitar un secreto único

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

La aplicación *secrets.json* archivo se modificó para quitar el par de clave y valor asociado con el `MoviesConnectionString` clave:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Quitar todos los secretos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que el *.csproj* archivo existe:

```console
dotnet user-secrets clear
```

Se han eliminado todos los secretos de usuario para la aplicación de la *secrets.json* archivo:

```json
{}
```

Ejecutando `dotnet user-secrets list` muestra el mensaje siguiente:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
