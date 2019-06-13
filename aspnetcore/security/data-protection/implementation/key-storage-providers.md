---
title: Proveedores de almacenamiento de claves en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los proveedores de almacenamiento de claves en ASP.NET Core y cómo configurar ubicaciones de almacenamiento de claves.
ms.author: riande
ms.date: 06/11/2019
uid: security/data-protection/implementation/key-storage-providers
ms.openlocfilehash: 64c7e6b25d5b4acc72e96747a77826efaeb693fd
ms.sourcegitcommit: 335a88c1b6e7f0caa8a3a27db57c56664d676d34
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/12/2019
ms.locfileid: "67034770"
---
# <a name="key-storage-providers-in-aspnet-core"></a><span data-ttu-id="89c5d-103">Proveedores de almacenamiento de claves en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="89c5d-103">Key storage providers in ASP.NET Core</span></span>

<span data-ttu-id="89c5d-104">El sistema de protección de datos [emplea un mecanismo de detección predeterminada](xref:security/data-protection/configuration/default-settings) para determinar dónde se deben conservar las claves criptográficas.</span><span class="sxs-lookup"><span data-stu-id="89c5d-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine where cryptographic keys should be persisted.</span></span> <span data-ttu-id="89c5d-105">El desarrollador puede invalidar el mecanismo de detección predeterminado y especificar manualmente la ubicación.</span><span class="sxs-lookup"><span data-stu-id="89c5d-105">The developer can override the default discovery mechanism and manually specify the location.</span></span>

> [!WARNING]
> <span data-ttu-id="89c5d-106">Si especifica una ubicación de persistencia de clave explícita, el sistema de protección de datos anula el registro el cifrado de clave predeterminado en el mecanismo de rest, por lo que las claves ya no se cifran en reposo.</span><span class="sxs-lookup"><span data-stu-id="89c5d-106">If you specify an explicit key persistence location, the data protection system deregisters the default key encryption at rest mechanism, so keys are no longer encrypted at rest.</span></span> <span data-ttu-id="89c5d-107">Se recomienda que, además [especifica un mecanismo de cifrado de claves explícitas](xref:security/data-protection/implementation/key-encryption-at-rest) para las implementaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="89c5d-107">It's recommended that you additionally [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span>

## <a name="file-system"></a><span data-ttu-id="89c5d-108">Sistema de archivos</span><span class="sxs-lookup"><span data-stu-id="89c5d-108">File system</span></span>

<span data-ttu-id="89c5d-109">Para configurar un repositorio clave basada en el sistema de archivos, llame a la [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) rutina de configuración como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="89c5d-109">To configure a file system-based key repository, call the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) configuration routine as shown below.</span></span> <span data-ttu-id="89c5d-110">Proporcione un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) que apunta al repositorio donde se deben almacenar las claves:</span><span class="sxs-lookup"><span data-stu-id="89c5d-110">Provide a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) pointing to the repository where keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"c:\temp-keys\"));
}
```

<span data-ttu-id="89c5d-111">El `DirectoryInfo` puede apuntar a un directorio en el equipo local, o puede señalar a una carpeta en un recurso compartido de red.</span><span class="sxs-lookup"><span data-stu-id="89c5d-111">The `DirectoryInfo` can point to a directory on the local machine, or it can point to a folder on a network share.</span></span> <span data-ttu-id="89c5d-112">Si señala a un directorio en el equipo local (y el escenario es que solo las aplicaciones en el equipo local requieren acceso a usar este repositorio), considere el uso de [DPAPI de Windows](xref:security/data-protection/implementation/key-encryption-at-rest) (en Windows) para cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="89c5d-112">If pointing to a directory on the local machine (and the scenario is that only apps on the local machine require access to use this repository), consider using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) (on Windows) to encrypt the keys at rest.</span></span> <span data-ttu-id="89c5d-113">De lo contrario, considere el uso de un [certificado X.509](xref:security/data-protection/implementation/key-encryption-at-rest) para cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="89c5d-113">Otherwise, consider using an [X.509 certificate](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt keys at rest.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="89c5d-114">Almacenamiento de Azure</span><span class="sxs-lookup"><span data-stu-id="89c5d-114">Azure Storage</span></span>

<span data-ttu-id="89c5d-115">El [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) paquete permite almacenar las claves de protección de datos en Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="89c5d-115">The [Microsoft.AspNetCore.DataProtection.AzureStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.AzureStorage/) package allows storing data protection keys in Azure Blob Storage.</span></span> <span data-ttu-id="89c5d-116">Las claves se pueden compartir entre varias instancias de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="89c5d-116">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="89c5d-117">Las aplicaciones pueden compartir las cookies de autenticación o la protección de CSRF en varios servidores.</span><span class="sxs-lookup"><span data-stu-id="89c5d-117">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

<span data-ttu-id="89c5d-118">Para configurar el proveedor de almacenamiento de blobs de Azure, llame a uno de los [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) sobrecargas.</span><span class="sxs-lookup"><span data-stu-id="89c5d-118">To configure the Azure Blob Storage provider, call one of the [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage) overloads.</span></span> 

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blob URI including SAS token>"));
}
```

<span data-ttu-id="89c5d-119">Si la aplicación web se ejecuta como un servicio de Azure, los tokens de autenticación pueden crearse automáticamente con [ Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span><span class="sxs-lookup"><span data-stu-id="89c5d-119">If the web app is running as an Azure service, authentication tokens can be automatically created using [ Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication/).</span></span> 

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

<span data-ttu-id="89c5d-120">Vea [más detalles acerca de cómo configurar la autenticación de servicio a servicio.](/azure/key-vault/service-to-service-authentication)</span><span class="sxs-lookup"><span data-stu-id="89c5d-120">See [more details about configuring service-to-service authentication.](/azure/key-vault/service-to-service-authentication)</span></span>

## <a name="redis"></a><span data-ttu-id="89c5d-121">Redis</span><span class="sxs-lookup"><span data-stu-id="89c5d-121">Redis</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="89c5d-122">El [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) paquete permite almacenar las claves de protección de datos en una caché en Redis.</span><span class="sxs-lookup"><span data-stu-id="89c5d-122">The [Microsoft.AspNetCore.DataProtection.StackExchangeRedis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.StackExchangeRedis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="89c5d-123">Las claves se pueden compartir entre varias instancias de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="89c5d-123">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="89c5d-124">Las aplicaciones pueden compartir las cookies de autenticación o la protección de CSRF en varios servidores.</span><span class="sxs-lookup"><span data-stu-id="89c5d-124">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="89c5d-125">El [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) paquete permite almacenar las claves de protección de datos en una caché en Redis.</span><span class="sxs-lookup"><span data-stu-id="89c5d-125">The [Microsoft.AspNetCore.DataProtection.Redis](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Redis/) package allows storing data protection keys in a Redis cache.</span></span> <span data-ttu-id="89c5d-126">Las claves se pueden compartir entre varias instancias de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="89c5d-126">Keys can be shared across several instances of a web app.</span></span> <span data-ttu-id="89c5d-127">Las aplicaciones pueden compartir las cookies de autenticación o la protección de CSRF en varios servidores.</span><span class="sxs-lookup"><span data-stu-id="89c5d-127">Apps can share authentication cookies or CSRF protection across multiple servers.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="89c5d-128">Para configurar Redis, llame a uno de los [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) sobrecargas:</span><span class="sxs-lookup"><span data-stu-id="89c5d-128">To configure on Redis, call one of the [PersistKeysToStackExchangeRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredisdataprotectionbuilderextensions.persistkeystostackexchangeredis) overloads:</span></span>

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

<span data-ttu-id="89c5d-129">Para configurar Redis, llame a uno de los [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) sobrecargas:</span><span class="sxs-lookup"><span data-stu-id="89c5d-129">To configure on Redis, call one of the [PersistKeysToRedis](/dotnet/api/microsoft.aspnetcore.dataprotection.redisdataprotectionbuilderextensions.persistkeystoredis) overloads:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    var redis = ConnectionMultiplexer.Connect("<URI>");
    services.AddDataProtection()
        .PersistKeysToRedis(redis, "DataProtection-Keys");
}
```

::: moniker-end

<span data-ttu-id="89c5d-130">Para obtener más información, vea los temas siguientes:</span><span class="sxs-lookup"><span data-stu-id="89c5d-130">For more information, see the following topics:</span></span>

* [<span data-ttu-id="89c5d-131">StackExchange.Redis ConnectionMultiplexer</span><span class="sxs-lookup"><span data-stu-id="89c5d-131">StackExchange.Redis ConnectionMultiplexer</span></span>](https://github.com/StackExchange/StackExchange.Redis/blob/master/docs/Basics.md)
* [<span data-ttu-id="89c5d-132">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="89c5d-132">Azure Redis Cache</span></span>](/azure/redis-cache/cache-dotnet-how-to-use-azure-redis-cache#connect-to-the-cache)
* [<span data-ttu-id="89c5d-133">ejemplos de ASPNET/DataProtection</span><span class="sxs-lookup"><span data-stu-id="89c5d-133">aspnet/DataProtection samples</span></span>](https://github.com/aspnet/AspNetCore/tree/2.2.0/src/DataProtection/samples)

## <a name="registry"></a><span data-ttu-id="89c5d-134">Registro</span><span class="sxs-lookup"><span data-stu-id="89c5d-134">Registry</span></span>

<span data-ttu-id="89c5d-135">**Solo se aplica a las implementaciones de Windows.**</span><span class="sxs-lookup"><span data-stu-id="89c5d-135">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="89c5d-136">A veces, la aplicación podría no tener acceso de escritura al sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="89c5d-136">Sometimes the app might not have write access to the file system.</span></span> <span data-ttu-id="89c5d-137">Considere un escenario donde se ejecuta una aplicación como una cuenta de servicio virtual (como *w3wp.exe*de identidad del grupo de aplicación).</span><span class="sxs-lookup"><span data-stu-id="89c5d-137">Consider a scenario where an app is running as a virtual service account (such as *w3wp.exe*'s app pool identity).</span></span> <span data-ttu-id="89c5d-138">En estos casos, el administrador puede aprovisionar una clave del registro que es accesible mediante la identidad de la cuenta de servicio.</span><span class="sxs-lookup"><span data-stu-id="89c5d-138">In these cases, the administrator can provision a registry key that's accessible by the service account identity.</span></span> <span data-ttu-id="89c5d-139">Llame a la [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) método de extensión tal como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="89c5d-139">Call the [PersistKeysToRegistry](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystoregistry) extension method as shown below.</span></span> <span data-ttu-id="89c5d-140">Proporcione un [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) apunta a la ubicación donde se deben almacenar las claves criptográficas:</span><span class="sxs-lookup"><span data-stu-id="89c5d-140">Provide a [RegistryKey](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository.registrykey) pointing to the location where cryptographic keys should be stored:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToRegistry(Registry.CurrentUser.OpenSubKey(@"SOFTWARE\Sample\keys"));
}
```

> [!IMPORTANT]
> <span data-ttu-id="89c5d-141">Se recomienda usar [DPAPI de Windows](xref:security/data-protection/implementation/key-encryption-at-rest) para cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="89c5d-141">We recommend using [Windows DPAPI](xref:security/data-protection/implementation/key-encryption-at-rest) to encrypt the keys at rest.</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="entity-framework-core"></a><span data-ttu-id="89c5d-142">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="89c5d-142">Entity Framework Core</span></span>

<span data-ttu-id="89c5d-143">El [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) paquete proporciona un mecanismo para almacenar las claves de protección de datos a una base de datos mediante Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="89c5d-143">The [Microsoft.AspNetCore.DataProtection.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.EntityFrameworkCore/) package provides a mechanism for storing data protection keys to a database using Entity Framework Core.</span></span> <span data-ttu-id="89c5d-144">El `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` paquete NuGet debe agregarse al archivo de proyecto, no es parte de la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="89c5d-144">The `Microsoft.AspNetCore.DataProtection.EntityFrameworkCore` NuGet package must be added to the project file, it's not part of the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="89c5d-145">Con este paquete, las claves se pueden compartir entre varias instancias de una aplicación web.</span><span class="sxs-lookup"><span data-stu-id="89c5d-145">With this package, keys can be shared across multiple instances of a web app.</span></span>

<span data-ttu-id="89c5d-146">Para configurar el proveedor de EF Core, llame a la [ `PersistKeysToDbContext<TContext>` ](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) método:</span><span class="sxs-lookup"><span data-stu-id="89c5d-146">To configure the EF Core provider, call the [`PersistKeysToDbContext<TContext>`](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcoredataprotectionextensions.persistkeystodbcontext) method:</span></span>

[!code-csharp[Main](key-storage-providers/sample/Startup.cs?name=snippet&highlight=13-15)]

<span data-ttu-id="89c5d-147">El parámetro genérico, `TContext`, debe heredar de [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) y [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span><span class="sxs-lookup"><span data-stu-id="89c5d-147">The generic parameter, `TContext`, must inherit from [DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and [IDataProtectionKeyContext](/dotnet/api/microsoft.aspnetcore.dataprotection.entityframeworkcore.idataprotectionkeycontext):</span></span>

[!code-csharp[Main](key-storage-providers/sample/MyKeysContext.cs)]

<span data-ttu-id="89c5d-148">Crear el `DataProtectionKeys` tabla.</span><span class="sxs-lookup"><span data-stu-id="89c5d-148">Create the `DataProtectionKeys` table.</span></span> 

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="89c5d-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="89c5d-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="89c5d-150">Ejecute los comandos siguientes en el **Package Manager Console** ventana (PMC):</span><span class="sxs-lookup"><span data-stu-id="89c5d-150">Execute the following commands in the **Package Manager Console** (PMC) window:</span></span>

```PowerShell
Add-Migration AddDataProtectionKeys -Context MyKeysContext
Update-Database -Context MyKeysContext
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="89c5d-151">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="89c5d-151">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="89c5d-152">Ejecute los siguientes comandos en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="89c5d-152">Execute the following commands in a command shell:</span></span>

```console
dotnet ef migrations add AddDataProtectionKeys --context MyKeysContext
dotnet ef database update --context MyKeysContext
```

---

<span data-ttu-id="89c5d-153">`MyKeysContext` es el `DbContext` definido en el ejemplo de código anterior.</span><span class="sxs-lookup"><span data-stu-id="89c5d-153">`MyKeysContext` is the `DbContext` defined in the preceding code sample.</span></span> <span data-ttu-id="89c5d-154">Si usas un `DbContext` con un nombre diferente, sustituya su `DbContext` nombre `MyKeysContext`.</span><span class="sxs-lookup"><span data-stu-id="89c5d-154">If you're using a `DbContext` with a different name, substitute your `DbContext` name for `MyKeysContext`.</span></span>

<span data-ttu-id="89c5d-155">La `DataProtectionKeys` clase/entidad adopta la estructura que se muestra en la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="89c5d-155">The `DataProtectionKeys` class/entity adopts the structure shown in the following table.</span></span>

| <span data-ttu-id="89c5d-156">Propiedad o campo.</span><span class="sxs-lookup"><span data-stu-id="89c5d-156">Property/Field</span></span> | <span data-ttu-id="89c5d-157">Tipo CLR</span><span class="sxs-lookup"><span data-stu-id="89c5d-157">CLR Type</span></span> | <span data-ttu-id="89c5d-158">Tipo de SQL</span><span class="sxs-lookup"><span data-stu-id="89c5d-158">SQL Type</span></span>              |
| -------------- | -------- | --------------------- |
| `Id`           | `int`    | <span data-ttu-id="89c5d-159">`int`, PK, no es null</span><span class="sxs-lookup"><span data-stu-id="89c5d-159">`int`, PK, not null</span></span>   |
| `FriendlyName` | `string` | <span data-ttu-id="89c5d-160">`nvarchar(MAX)`, null</span><span class="sxs-lookup"><span data-stu-id="89c5d-160">`nvarchar(MAX)`, null</span></span> |
| `Xml`          | `string` | <span data-ttu-id="89c5d-161">`nvarchar(MAX)`, null</span><span class="sxs-lookup"><span data-stu-id="89c5d-161">`nvarchar(MAX)`, null</span></span> |

::: moniker-end

## <a name="custom-key-repository"></a><span data-ttu-id="89c5d-162">Repositorio de claves personalizado</span><span class="sxs-lookup"><span data-stu-id="89c5d-162">Custom key repository</span></span>

<span data-ttu-id="89c5d-163">Si los mecanismos en el equipo no son adecuados, el desarrollador puede especificar su propio mecanismo de persistencia de clave al proporcionar una personalizada [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span><span class="sxs-lookup"><span data-stu-id="89c5d-163">If the in-box mechanisms aren't appropriate, the developer can specify their own key persistence mechanism by providing a custom [IXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.ixmlrepository).</span></span>
