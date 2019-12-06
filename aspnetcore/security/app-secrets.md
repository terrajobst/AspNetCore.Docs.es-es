---
title: Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core
author: rick-anderson
description: Aprenda a almacenar y recuperar información confidencial como secretos de la aplicación durante el desarrollo de una aplicación ASP.NET Core.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: 0bbd6af01ce3a29d3931faa2853a50dc895490cc
ms.sourcegitcommit: fd2483f0a384b1c479c5b4af025ee46917db1919
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74868035"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Almacenamiento seguro de secretos de aplicación en el desarrollo en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)y [Scott Addie](https://github.com/scottaddie)

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

En este documento se explican técnicas para almacenar y recuperar datos confidenciales durante el desarrollo de una aplicación ASP.NET Core. Nunca almacene contraseñas u otros datos confidenciales en el código fuente. Los secretos de producción no deben usarse para el desarrollo o las pruebas. Los secretos no deben implementarse con la aplicación. En su lugar, los secretos deben estar disponibles en el entorno de producción a través de un medio controlado como variables de entorno, Azure Key Vault, etc. Puede almacenar y proteger los secretos de prueba y producción de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Variables de entorno

Las variables de entorno se usan para evitar el almacenamiento de secretos de aplicación en código o en archivos de configuración local. Las variables de entorno invalidan los valores de configuración de todos los orígenes de configuración especificados previamente.

::: moniker range="<= aspnetcore-1.1"

Configure la lectura de los valores de las variables de entorno mediante una llamada a <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> en el constructor de `Startup`:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

Considere una ASP.NET Core aplicación web en la que está habilitada la seguridad de **cuentas de usuario individuales** . Se incluye una cadena de conexión de base de datos predeterminada en el archivo *appSettings. JSON* del proyecto con la clave `DefaultConnection`. La cadena de conexión predeterminada es para LocalDB, que se ejecuta en modo de usuario y no requiere una contraseña. Durante la implementación de la aplicación, el valor de la clave de `DefaultConnection` se puede invalidar con el valor de una variable de entorno. La variable de entorno puede almacenar la cadena de conexión completa con credenciales confidenciales.

> [!WARNING]
> Normalmente, las variables de entorno se almacenan en texto sin cifrar. Si el equipo o el proceso se ve comprometido, las entidades de entorno pueden tener acceso a las variables de entorno que no son de confianza. Pueden ser necesarias medidas adicionales para evitar la divulgación de secretos de usuario.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>Administrador de secretos

La herramienta Administrador de secretos almacena datos confidenciales durante el desarrollo de un proyecto de ASP.NET Core. En este contexto, una parte de la información confidencial es un secreto de la aplicación. Los secretos de la aplicación se almacenan en una ubicación independiente del árbol del proyecto. Los secretos de la aplicación se asocian a un proyecto específico o se comparten entre varios proyectos. Los secretos de la aplicación no se protegen en el control de código fuente.

> [!WARNING]
> La herramienta Administrador de secretos no cifra los secretos almacenados y no debe tratarse como un almacén de confianza. Solo es para fines de desarrollo. Las claves y los valores se almacenan en un archivo de configuración JSON en el directorio del perfil de usuario.

## <a name="how-the-secret-manager-tool-works"></a>Cómo funciona la herramienta Administrador de secretos

La herramienta Administrador de secretos abstrae los detalles de implementación, como dónde y cómo se almacenan los valores. Puede usar la herramienta sin conocer estos detalles de implementación. Los valores se almacenan en un archivo de configuración JSON en una carpeta de Perfil de usuario protegida por el sistema en el equipo local:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Ruta de acceso del sistema de archivos:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux/macOS](#tab/linux+macos)

Ruta de acceso del sistema de archivos:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

En las rutas de acceso de archivo anteriores, reemplace `<user_secrets_id>` por el valor de `UserSecretsId` especificado en el archivo *. csproj* .

No Escriba código que dependa de la ubicación o el formato de los datos guardados con la herramienta Administrador de secretos. Estos detalles de implementación pueden cambiar. Por ejemplo, los valores de secreto no están cifrados, pero podrían estar en el futuro.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Instalar la herramienta Administrador de secretos

La herramienta de administración de secretos está incluida en el CLI de .NET Core en SDK de .NET Core 2.1.300 o posterior. En el caso de las versiones de SDK de .NET Core antes de 2.1.300, es necesaria la instalación de la herramienta.

> [!TIP]
> Ejecute `dotnet --version` desde un shell de comandos para ver el número de versión de SDK de .NET Core instalado.

Se muestra una advertencia si el SDK de .NET Core que se está usando incluye la herramienta:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Instale el paquete de NuGet [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) en el proyecto de ASP.net Core. Por ejemplo:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Ejecute el siguiente comando en un shell de comandos para validar la instalación de la herramienta:

```dotnetcli
dotnet user-secrets -h
```

La herramienta Administrador de secretos muestra el uso de ejemplo, las opciones y la ayuda de comandos:

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
> Debe estar en el mismo directorio que el archivo *. csproj* para ejecutar las herramientas definidas en los elementos `DotNetCliToolReference` del archivo *. csproj* .

::: moniker-end

## <a name="enable-secret-storage"></a>Habilitar el almacenamiento de secretos

La herramienta Administrador de secretos funciona en opciones de configuración específicas del proyecto almacenadas en el perfil de usuario.

::: moniker range=">= aspnetcore-3.0"

La herramienta Administrador de secretos incluye un comando `init` en SDK de .NET Core 3.0.100 o posterior. Para usar los secretos de usuario, ejecute el siguiente comando en el directorio del proyecto:

```dotnetcli
dotnet user-secrets init
```

El comando anterior agrega un elemento `UserSecretsId` dentro de un `PropertyGroup` del archivo *. csproj* . De forma predeterminada, el texto interno de `UserSecretsId` es un GUID. El texto interno es arbitrario, pero es único para el proyecto.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Para usar secretos de usuario, defina un elemento `UserSecretsId` dentro de un `PropertyGroup` del archivo *. csproj* . El texto interno de `UserSecretsId` es arbitrario, pero es único para el proyecto. Normalmente, los desarrolladores generan un GUID para el `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> En Visual Studio, haga clic con el botón derecho en el proyecto en Explorador de soluciones y seleccione **administrar secretos de usuario** en el menú contextual. Este gesto agrega un elemento `UserSecretsId`, rellenado con un GUID, al archivo *. csproj* .

## <a name="set-a-secret"></a>Establecer un secreto

Defina un secreto de la aplicación que conste de una clave y su valor. El secreto está asociado con el valor de `UserSecretsId` del proyecto. Por ejemplo, ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

En el ejemplo anterior, el signo de dos puntos denota que `Movies` es un literal de objeto con una propiedad `ServiceApiKey`.

La herramienta Administrador de secretos también se puede usar desde otros directorios. Use la opción `--project` para proporcionar la ruta de acceso del sistema de archivos en la que existe el archivo *. csproj* . Por ejemplo:

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Simplificación de la estructura de JSON en Visual Studio

El gesto de **administrar secretos de usuario** de Visual Studio abre un archivo *Secrets. JSON* en el editor de texto. Reemplace el contenido de *Secrets. JSON* por los pares clave-valor que se van a almacenar. Por ejemplo:

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

La estructura JSON se alisa después de las modificaciones a través de `dotnet user-secrets remove` o `dotnet user-secrets set`. Por ejemplo, al ejecutar `dotnet user-secrets remove "Movies:ConnectionString"` se contrae el literal de objeto `Movies`. El archivo modificado es similar al siguiente:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Establecer varios secretos

Un lote de secretos se puede establecer mediante el JSON de canalización en el comando `set`. En el ejemplo siguiente, el contenido del archivo *Input. JSON* se canaliza al comando `set`.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Abra un shell de comandos y ejecute el siguiente comando:

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux/macOS](#tab/linux+macos)

Abra un shell de comandos y ejecute el siguiente comando:

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Acceder a un secreto

La [API de configuración de ASP.net Core](xref:fundamentals/configuration/index) proporciona acceso a los secretos del administrador de secretos.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Si el proyecto tiene como destino .NET Framework, instale el paquete NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

En ASP.NET Core 2,0 o posterior, el origen de configuración de los secretos de usuario se agrega automáticamente en modo de desarrollo cuando el proyecto llama <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> para inicializar una nueva instancia del host con los valores predeterminados preconfigurados. `CreateDefaultBuilder` llama <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> cuando se <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>el <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Cuando no se llama a `CreateDefaultBuilder`, agregue el origen de configuración de secretos de usuario explícitamente llamando a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> en el constructor de `Startup`. Llame solo a `AddUserSecrets` cuando la aplicación se ejecute en el entorno de desarrollo, como se muestra en el ejemplo siguiente:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Instale el paquete NuGet [Microsoft. Extensions. Configuration. UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) .

Agregue el origen de configuración de secretos de usuario con una llamada a <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> en el constructor de `Startup`:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Los secretos de usuario se pueden recuperar a través de la API de `Configuration`:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Asignación de secretos a un POCO

La asignación de un literal de objeto completo a un POCO (una clase .NET simple con propiedades) es útil para agregar propiedades relacionadas.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Para asignar los secretos anteriores a un POCO, use la característica de [enlace de gráficos de objetos](xref:fundamentals/configuration/index#bind-to-an-object-graph) de la API de `Configuration`. El código siguiente enlaza a un `MovieSettings` POCO personalizado y obtiene acceso al valor de la propiedad `ServiceApiKey`:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

Los secretos de `Movies:ConnectionString` y `Movies:ServiceApiKey` se asignan a las propiedades correspondientes en `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Reemplazo de cadenas con secretos

Almacenar las contraseñas en texto sin formato no es seguro. Por ejemplo, una cadena de conexión de base de datos almacenada en *appSettings. JSON* puede incluir una contraseña para el usuario especificado:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Un enfoque más seguro consiste en almacenar la contraseña como un secreto. Por ejemplo:

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

Quite el `Password` par clave-valor de la cadena de conexión en *appSettings. JSON*. Por ejemplo:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

El valor del secreto se puede establecer en la propiedad <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> de un objeto <xref:System.Data.SqlClient.SqlConnectionStringBuilder> para completar la cadena de conexión:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Enumeración de los secretos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :

```dotnetcli
dotnet user-secrets list
```

Aparece el siguiente resultado:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

En el ejemplo anterior, un signo de dos puntos en los nombres de clave denota la jerarquía de objetos en *Secrets. JSON*.

## <a name="remove-a-single-secret"></a>Quitar un único secreto

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

El archivo *Secrets. JSON* de la aplicación se modificó para quitar el par clave-valor asociado a la clave de `MoviesConnectionString`:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Al ejecutar `dotnet user-secrets list` se muestra el siguiente mensaje:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Quitar todos los secretos

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Ejecute el siguiente comando desde el directorio en el que existe el archivo *. csproj* :

```dotnetcli
dotnet user-secrets clear
```

Todos los secretos de usuario de la aplicación se han eliminado del archivo *Secrets. JSON* :

```json
{}
```

Al ejecutar `dotnet user-secrets list` se muestra el siguiente mensaje:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Recursos adicionales

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
