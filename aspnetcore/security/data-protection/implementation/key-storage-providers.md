---
title: Proveedores de almacenamiento de claves en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los proveedores de almacenamiento de claves en ASP.NET Core y cómo configurar ubicaciones de almacenamiento de claves.
manager: wpickett
ms.author: riande
ms.date: 01/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: e8b7804e93b812c2e710ab15510c2fbaa7c4866d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="key-storage-providers-in-aspnet-core"></a>Proveedores de almacenamiento de claves en ASP.NET Core

<a name="data-protection-implementation-key-storage-providers"></a>

De forma predeterminada, el sistema de protección de datos [emplea un método heurístico](xref:security/data-protection/configuration/default-settings) para determinar dónde se debe conservar el material de clave de cifrado. El desarrollador puede invalidar la heurística y especificar manualmente la ubicación.

> [!NOTE]
> Si especifica una ubicación de persistencia de clave explícita, el sistema de protección de datos se anular el registro el cifrado de claves de forma predeterminada en el mecanismo de rest que proporciona la heurística, por lo que ya no se cifrarán las claves en reposo. Se recomienda que, además [especificar un mecanismo de cifrado de clave explícita](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) para las aplicaciones de producción.

El sistema de protección de datos que se suministra con varios proveedores de almacenamiento de claves de forma predeterminada.

## <a name="file-system"></a>Sistema de archivos

Prevemos que muchas aplicaciones usarán un repositorio de clave basada en el sistema de archivos. Para configurar esto, llame a la [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rutina de configuración tal y como se muestra a continuación. Proporcionar un `DirectoryInfo` que apunta al repositorio que se deben almacenar las claves.

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

La `DirectoryInfo` puede apuntar a un directorio en el equipo local, o puede apuntar a una carpeta en un recurso compartido de red. Si señala a un directorio en el equipo local (y el escenario es que sólo las aplicaciones en el equipo local tendrá que utilizar este repositorio), considere el uso de [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) para cifrar las claves en reposo. En caso contrario, considere el uso de un [certificado X.509](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) para cifrar las claves en reposo.

## <a name="azure-and-redis"></a>Azure y Redis

El `Microsoft.AspNetCore.DataProtection.AzureStorage` y `Microsoft.AspNetCore.DataProtection.Redis` paquetes permiten almacenar las claves de protección de datos en el almacenamiento de Azure o una caché de Redis. Las claves se pueden compartir en varias instancias de una aplicación web. La aplicación de ASP.NET Core puede compartir las cookies de autenticación o protección CSRF en varios servidores. Para configurar en Azure, llame a uno de los [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) sobrecargas tal y como se muestra a continuación.

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

Vea también el [código de prueba de Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).

Para configurar en Redis, llame a uno de los [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) sobrecargas tal y como se muestra a continuación.

```
public void ConfigureServices(IServiceCollection services)
{
    // Connect to Redis database.
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");

    services.AddMvc();
}
```

Para obtener más información, vea las secciones siguientes:

- [StackExchange.Redis ConnectionMultiplexer](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [Caché en Redis de Azure](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- [Código de prueba de Redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).

## <a name="registry"></a>Registro

A veces, la aplicación podría no tener acceso de escritura al sistema de archivos. Considere un escenario donde se ejecuta una aplicación como una cuenta de servicio virtual (por ejemplo, la identidad del grupo de aplicaciones de w3wp.exe). En estos casos, el administrador puede haber aprovisionado una clave del registro que sea adecuado con las ACL para la identidad de la cuenta de servicio. Llame a la [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rutina de configuración tal y como se muestra a continuación. Proporcionar un `RegistryKey` que apunta a la ubicación donde se deben almacenar valores/claves criptográficos.

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

Si usa el registro del sistema como un mecanismo de persistencia, considere el uso de [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) para cifrar las claves en reposo.

## <a name="custom-key-repository"></a>Repositorio de clave personalizado

Si no resultan adecuados los mecanismos de forma predeterminada, el programador puede especificar su propio mecanismo de persistencia de clave proporcionando un personalizado `IXmlRepository`.
