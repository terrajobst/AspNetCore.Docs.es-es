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
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="3d28b-103">Proveedores de almacenamiento de claves en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d28b-103">Key storage providers in ASP.NET Core</span></span>

<a name="data-protection-implementation-key-storage-providers"></a>

<span data-ttu-id="3d28b-104">De forma predeterminada, el sistema de protección de datos [emplea un método heurístico](xref:security/data-protection/configuration/default-settings) para determinar dónde se debe conservar el material de clave de cifrado.</span><span class="sxs-lookup"><span data-stu-id="3d28b-104">By default the data protection system [employs a heuristic](xref:security/data-protection/configuration/default-settings) to determine where cryptographic key material should be persisted.</span></span> <span data-ttu-id="3d28b-105">El desarrollador puede invalidar la heurística y especificar manualmente la ubicación.</span><span class="sxs-lookup"><span data-stu-id="3d28b-105">The developer can override the heuristic and manually specify the location.</span></span>

> [!NOTE]
> <span data-ttu-id="3d28b-106">Si especifica una ubicación de persistencia de clave explícita, el sistema de protección de datos se anular el registro el cifrado de claves de forma predeterminada en el mecanismo de rest que proporciona la heurística, por lo que ya no se cifrarán las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="3d28b-106">If you specify an explicit key persistence location, the data protection system will deregister the default key encryption at rest mechanism that the heuristic provided, so keys will no longer be encrypted at rest.</span></span> <span data-ttu-id="3d28b-107">Se recomienda que, además [especificar un mecanismo de cifrado de clave explícita](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) para las aplicaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="3d28b-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest-providers) for production applications.</span></span>

<span data-ttu-id="3d28b-108">El sistema de protección de datos que se suministra con varios proveedores de almacenamiento de claves de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="3d28b-108">The data protection system ships with several in-box key storage providers.</span></span>

## <a name="file-system"></a><span data-ttu-id="3d28b-109">Sistema de archivos</span><span class="sxs-lookup"><span data-stu-id="3d28b-109">File system</span></span>

<span data-ttu-id="3d28b-110">Prevemos que muchas aplicaciones usarán un repositorio de clave basada en el sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="3d28b-110">We anticipate that many apps will use a file system-based key repository.</span></span> <span data-ttu-id="3d28b-111">Para configurar esto, llame a la [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rutina de configuración tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3d28b-111">To configure this, call the [PersistKeysToFileSystem](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="3d28b-112">Proporcionar un `DirectoryInfo` que apunta al repositorio que se deben almacenar las claves.</span><span class="sxs-lookup"><span data-stu-id="3d28b-112">Provide a `DirectoryInfo` pointing to the repository where keys should be stored.</span></span>

```csharp
sc.AddDataProtection()
       // persist keys to a specific directory
       .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
   ```

<span data-ttu-id="3d28b-113">La `DirectoryInfo` puede apuntar a un directorio en el equipo local, o puede apuntar a una carpeta en un recurso compartido de red.</span><span class="sxs-lookup"><span data-stu-id="3d28b-113">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="3d28b-114">Si señala a un directorio en el equipo local (y el escenario es que sólo las aplicaciones en el equipo local tendrá que utilizar este repositorio), considere el uso de [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) para cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="3d28b-114">If pointing to a directory on the local machine (and the scenario is that only applications on the local machine will need to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span> <span data-ttu-id="3d28b-115">En caso contrario, considere el uso de un [certificado X.509](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) para cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="3d28b-115">Otherwise consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-and-redis"></a><span data-ttu-id="3d28b-116">Azure y Redis</span><span class="sxs-lookup"><span data-stu-id="3d28b-116">Azure and Redis</span></span>

<span data-ttu-id="3d28b-117">El `Microsoft.AspNetCore.DataProtection.AzureStorage` y `Microsoft.AspNetCore.DataProtection.Redis` paquetes permiten almacenar las claves de protección de datos en el almacenamiento de Azure o una caché de Redis.</span><span class="sxs-lookup"><span data-stu-id="3d28b-117">The `Microsoft.AspNetCore.DataProtection.AzureStorage` and `Microsoft.AspNetCore.DataProtection.Redis` packages allow storing your data protection keys in Azure Storage or a Redis cache.</span></span> <span data-ttu-id="3d28b-118">Las claves se pueden compartir en varias instancias de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="3d28b-118">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="3d28b-119">La aplicación de ASP.NET Core puede compartir las cookies de autenticación o protección CSRF en varios servidores.</span><span class="sxs-lookup"><span data-stu-id="3d28b-119">Your ASP.NET Core app can share authentication cookies or CSRF protection across multiple servers.</span></span> <span data-ttu-id="3d28b-120">Para configurar en Azure, llame a uno de los [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) sobrecargas tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3d28b-120">To configure on Azure, call one of the [PersistKeysToAzureBlobStorage](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.AzureStorage/AzureDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));

    services.AddMvc();
}
```

<span data-ttu-id="3d28b-121">Vea también el [código de prueba de Azure](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="3d28b-121">See also the [Azure test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/AzureBlob/Program.cs).</span></span>

<span data-ttu-id="3d28b-122">Para configurar en Redis, llame a uno de los [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) sobrecargas tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3d28b-122">To configure on Redis, call one of the [PersistKeysToRedis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection.Redis/RedisDataProtectionBuilderExtensions.cs) overloads as shown below.</span></span>

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

<span data-ttu-id="3d28b-123">Para obtener más información, vea las secciones siguientes:</span><span class="sxs-lookup"><span data-stu-id="3d28b-123">See the following for more information:</span></span>

- [<span data-ttu-id="3d28b-124">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="3d28b-124">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
- [<span data-ttu-id="3d28b-125">Caché en Redis de Azure</span><span class="sxs-lookup"><span data-stu-id="3d28b-125">Azure Redis Cache</span></span>](https://docs.microsoft.com/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
- <span data-ttu-id="3d28b-126">[Código de prueba de Redis](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="3d28b-126">[Redis test code](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/samples/Redis/Program.cs).</span></span>

## <a name="registry"></a><span data-ttu-id="3d28b-127">Registro</span><span class="sxs-lookup"><span data-stu-id="3d28b-127">Registry</span></span>

<span data-ttu-id="3d28b-128">A veces, la aplicación podría no tener acceso de escritura al sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="3d28b-128">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="3d28b-129">Considere un escenario donde se ejecuta una aplicación como una cuenta de servicio virtual (por ejemplo, la identidad del grupo de aplicaciones de w3wp.exe).</span><span class="sxs-lookup"><span data-stu-id="3d28b-129">Consider a scenario where an app is running as a virtual service account (such as w3wp.exe's app pool identity).</span></span> <span data-ttu-id="3d28b-130">En estos casos, el administrador puede haber aprovisionado una clave del registro que sea adecuado con las ACL para la identidad de la cuenta de servicio.</span><span class="sxs-lookup"><span data-stu-id="3d28b-130">In these cases, the administrator may have provisioned a registry key that's appropriate ACLed for the service account identity.</span></span> <span data-ttu-id="3d28b-131">Llame a la [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) rutina de configuración tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="3d28b-131">Call the [PersistKeysToRegistry](https://github.com/aspnet/DataProtection/blob/rel/1.1.0/src/Microsoft.AspNetCore.DataProtection/DataProtectionBuilderExtensions.cs) configuration routine as shown below.</span></span> <span data-ttu-id="3d28b-132">Proporcionar un `RegistryKey` que apunta a la ubicación donde se deben almacenar valores/claves criptográficos.</span><span class="sxs-lookup"><span data-stu-id="3d28b-132">Provide a `RegistryKey` pointing to the location where cryptographic keys/values should be stored.</span></span>

```csharp
   sc.AddDataProtection()
       // persist keys to a specific location in the system registry
       .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
   ```

<span data-ttu-id="3d28b-133">Si usa el registro del sistema como un mecanismo de persistencia, considere el uso de [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) para cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="3d28b-133">If you use the system registry as a persistence mechanism, consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest) to encrypt the keys at rest.</span></span>

## <a name="custom-key-repository"></a><span data-ttu-id="3d28b-134">Repositorio de clave personalizado</span><span class="sxs-lookup"><span data-stu-id="3d28b-134">Custom key repository</span></span>

<span data-ttu-id="3d28b-135">Si no resultan adecuados los mecanismos de forma predeterminada, el programador puede especificar su propio mecanismo de persistencia de clave proporcionando un personalizado `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="3d28b-135">If the in-box mechanisms are not appropriate, the developer can specify their own key persistence mechanism by providing a custom `IXmlRepository`.</span></span>
