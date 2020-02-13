---
title: Proveedores de almacenamiento de claves en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los proveedores de almacenamiento de claves en ASP.NET Core y cómo configurar ubicaciones de almacenamiento de claves.
ms.author: riande
ms.date: 12/05/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 219ebc471de32d15e4a43c938eef156c52e5f11e
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172588"
---
# <a name="key-storage-providers-in-aspnet-core"></a>Proveedores de almacenamiento de claves en ASP.NET Core

De forma predeterminada, el sistema de protección de datos [emplea un mecanismo de detección](xref:security/data-protection/configuration/default-settings) para determinar dónde se deben conservar las claves criptográficas. El desarrollador puede invalidar el mecanismo de detección predeterminado y especificar manualmente la ubicación.

> [!WARNING]
> Si especifica una ubicación de persistencia de clave explícita, el sistema de protección de datos anula el registro el cifrado de clave predeterminado en el mecanismo de rest, por lo que las claves ya no se cifran en reposo. Se recomienda [especificar además un mecanismo de cifrado de claves explícito](xref:security/data-protection/implementation/key-encryption-at-rest) para las implementaciones de producción.

## <a name="file-system"></a>Sistema de archivos

Para configurar un repositorio de claves basado en sistema de archivos, llame a la rutina de configuración [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) como se muestra a continuación. Proporcione un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) que apunte al repositorio donde se deben almacenar las claves:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

El `DirectoryInfo` puede apuntar a un directorio en el equipo local o puede apuntar a una carpeta de un recurso compartido de red. Si apunta a un directorio en el equipo local (y el escenario es que solo las aplicaciones del equipo local requieren acceso para usar este repositorio), considere la posibilidad de usar [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (en Windows) para cifrar las claves en reposo. En caso contrario, considere la posibilidad de utilizar un [certificado X. 509](xref:security/data-protection/implementation/key-encryption-at-rest) para cifrar las claves en reposo.

## <a name="azure-storage"></a>Azure Storage

El paquete [Microsoft. AspNetCore. AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) permite almacenar las claves de protección de datos en Azure BLOB Storage. Las claves se pueden compartir entre varias instancias de una aplicación web. Las aplicaciones pueden compartir las cookies de autenticación o la protección de CSRF en varios servidores.

Para configurar el proveedor de Azure Blob Storage, llame a una de las sobrecargas de [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) .

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

Si la aplicación web se ejecuta como un servicio de Azure, los tokens de autenticación se pueden crear automáticamente con [Microsoft. Azure. Services. AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).

```csharp
var tokenProvider = new AzureServiceTokenProvider();
var token = await tokenProvider.GetAccessTokenAsync("https://storage.azure.com/");
var credentials = new StorageCredentials(new TokenCredential(token));
var storageAccount = new CloudStorageAccount(credentials, "mystorageaccount", "core.windows.net", useHttps: true);
var client = storageAccount.CreateCloudBlobClient();
var container = client.GetContainerReference("my-key-container");

// optional - provision the container automatically
await container.CreateIfNotExistsAsync();

services.AddDataProtection()
    .PersistKeysToAzureBlobStorage(container, "keys.xml");
```

Vea [más detalles sobre la configuración de la autenticación de servicio a servicio.](/azure/key-vault/service-to-service-authentication)

## <a name="redis"></a>Redis

::: moniker range=">= aspnetcore-2.2"

El paquete [Microsoft. AspNetCore. StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) permite almacenar las claves de protección de datos en una caché en Redis. Las claves se pueden compartir entre varias instancias de una aplicación web. Las aplicaciones pueden compartir las cookies de autenticación o la protección de CSRF en varios servidores.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

El paquete [Microsoft. AspNetCore. desproteger. Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) permite almacenar las claves de protección de datos en una caché en Redis. Las claves se pueden compartir entre varias instancias de una aplicación web. Las aplicaciones pueden compartir las cookies de autenticación o la protección de CSRF en varios servidores.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Para configurar en Redis, llame a una de las sobrecargas de [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToStackExchangeRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Para configurar en Redis, llame a una de las sobrecargas de [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) :

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

Para obtener más información, vea los temas siguientes:

* [StackExchange. Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [Azure Redis Cache](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [ASP.NET Core ejemplos de protección de los mismos](https://github.com/dotnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a>Registro

**Solo se aplica a las implementaciones de Windows.**

A veces, la aplicación podría no tener acceso de escritura al sistema de archivos. Considere un escenario en el que una aplicación se ejecuta como una cuenta de servicio virtual (como la identidad del grupo de aplicaciones de *w3wp. exe*). En estos casos, el administrador puede aprovisionar una clave del registro que es accesible mediante la identidad de la cuenta de servicio. Llame al método de extensión [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) como se muestra a continuación. Proporcione una [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) que apunte a la ubicación donde se deben almacenar las claves criptográficas:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> Se recomienda usar [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) para cifrar las claves en reposo.

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a>Entity Framework Core

El paquete [Microsoft. AspNetCore. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) proporciona un mecanismo para almacenar las claves de protección de datos en una base de datos mediante Entity Framework Core. El paquete NuGet de `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` debe agregarse al archivo de proyecto, no es parte del [metapaquete Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app).

Con este paquete, las claves se pueden compartir entre varias instancias de una aplicación web.

Para configurar el proveedor de EF Core, llame al método [PersistKeysToDbContext\<TContext >](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) :

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-20)]

El parámetro genérico, `TContext`, debe heredar de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) e implementar [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

Cree la tabla `DataProtectionKeys`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Ejecute los siguientes comandos en la ventana de la **consola del administrador de paquetes** (PMC):

```powershell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute los siguientes comandos en un shell de comandos:

```dotnetcli
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

`MyKeysContext` es el `DbContext` definido en el ejemplo de código anterior. Si utiliza un `DbContext` con un nombre diferente, sustituya el nombre de `DbContext` por `MyKeysContext`.

La clase o entidad `DataProtectionKeys` adopta la estructura que se muestra en la tabla siguiente.

| Propiedad o campo | Tipo CLR | Tipo de SQL              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | `int`, PK, not null   |
| `FriendlyName` | `string` | `nvarchar(MAX)`, null |
| `Xml`          | `string` | `nvarchar(MAX)`, null |

::: moniker-end

## <a name="custom-key-repository"></a>Repositorio de claves personalizado

Si los mecanismos integrados no son adecuados, el desarrollador puede especificar su propio mecanismo de persistencia de claves proporcionando un [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository)personalizado.
