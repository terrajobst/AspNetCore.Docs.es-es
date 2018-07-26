---
title: Configurar la protección de datos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar la protección de datos en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/17/2017
uid: security/data-protection/configuration/overview
ms.openlocfilehash: fd3e5dff114c81eae07186e735a15314d984dcc3
ms.sourcegitcommit: 5338b1ed9e2ef225ab565d6cba072b474fd9324d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39243115"
---
# <a name="configure-aspnet-core-data-protection"></a><span data-ttu-id="b85e5-103">Configurar la protección de datos de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b85e5-103">Configure ASP.NET Core Data Protection</span></span>

<span data-ttu-id="b85e5-104">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b85e5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b85e5-105">Cuando se inicializa el sistema de protección de datos, se aplica [configuración predeterminada](xref:security/data-protection/configuration/default-settings) según el entorno operativo.</span><span class="sxs-lookup"><span data-stu-id="b85e5-105">When the Data Protection system is initialized, it applies [default settings](xref:security/data-protection/configuration/default-settings) based on the operational environment.</span></span> <span data-ttu-id="b85e5-106">Esta configuración es normalmente adecuada para las aplicaciones que se ejecutan en una sola máquina.</span><span class="sxs-lookup"><span data-stu-id="b85e5-106">These settings are generally appropriate for apps running on a single machine.</span></span> <span data-ttu-id="b85e5-107">Hay casos donde un desarrollador puede querer cambiar la configuración predeterminada:</span><span class="sxs-lookup"><span data-stu-id="b85e5-107">There are cases where a developer may want to change the default settings:</span></span>

* <span data-ttu-id="b85e5-108">La aplicación se reparte entre varias máquinas.</span><span class="sxs-lookup"><span data-stu-id="b85e5-108">The app is spread across multiple machines.</span></span>
* <span data-ttu-id="b85e5-109">Por motivos de cumplimiento.</span><span class="sxs-lookup"><span data-stu-id="b85e5-109">For compliance reasons.</span></span>

<span data-ttu-id="b85e5-110">Para estos escenarios, el sistema de protección de datos ofrece una API de configuración completo.</span><span class="sxs-lookup"><span data-stu-id="b85e5-110">For these scenarios, the Data Protection system offers a rich configuration API.</span></span>

> [!WARNING]
> <span data-ttu-id="b85e5-111">Al igual que los archivos de configuración, el conjunto de claves de protección de datos deben protegerse mediante los permisos adecuados.</span><span class="sxs-lookup"><span data-stu-id="b85e5-111">Similar to configuration files, the data protection key ring should be protected using appropriate permissions.</span></span> <span data-ttu-id="b85e5-112">Puede elegir cifrar las claves en reposo, pero esto no impide que los atacantes crear nuevas claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-112">You can choose to encrypt keys at rest, but this doesn't prevent attackers from creating new keys.</span></span> <span data-ttu-id="b85e5-113">Por lo tanto, la seguridad de la aplicación se ve afectada.</span><span class="sxs-lookup"><span data-stu-id="b85e5-113">Consequently, your app's security is impacted.</span></span> <span data-ttu-id="b85e5-114">La ubicación de almacenamiento configurada con protección de datos debe tener su acceso limitado a la propia aplicación, similar a la manera en que desea proteger los archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="b85e5-114">The storage location configured with Data Protection should have its access limited to the app itself, similar to the way you would protect configuration files.</span></span> <span data-ttu-id="b85e5-115">Por ejemplo, si decide almacenar su conjunto de claves en el disco, utilice permisos del sistema de archivos.</span><span class="sxs-lookup"><span data-stu-id="b85e5-115">For example, if you choose to store your key ring on disk, use file system permissions.</span></span> <span data-ttu-id="b85e5-116">Asegúrese sólo la identidad bajo la que se ejecuta la aplicación web ha lectura, escritura y crear el acceso a ese directorio.</span><span class="sxs-lookup"><span data-stu-id="b85e5-116">Ensure only the identity under which your web app runs has read, write, and create access to that directory.</span></span> <span data-ttu-id="b85e5-117">Si usa Azure Table Storage, solo la aplicación web debe tener la capacidad de leer, escribir o crear nuevas entradas en el almacén de tablas, etcetera.</span><span class="sxs-lookup"><span data-stu-id="b85e5-117">If you use Azure Table Storage, only the web app should have the ability to read, write, or create new entries in the table store, etc.</span></span>
>
> <span data-ttu-id="b85e5-118">El método de extensión [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) devuelve un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="b85e5-118">The extension method [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) returns an [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder).</span></span> <span data-ttu-id="b85e5-119">`IDataProtectionBuilder` expone métodos de extensión que se pueden encadenar juntos para configurar la protección de datos de opciones.</span><span class="sxs-lookup"><span data-stu-id="b85e5-119">`IDataProtectionBuilder` exposes extension methods that you can chain together to configure Data Protection options.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a><span data-ttu-id="b85e5-120">ProtectKeysWithAzureKeyVault</span><span class="sxs-lookup"><span data-stu-id="b85e5-120">ProtectKeysWithAzureKeyVault</span></span>

<span data-ttu-id="b85e5-121">Para almacenar las claves en [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurar el sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) en el `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="b85e5-121">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="b85e5-122">Establecer la ubicación de almacenamiento del conjunto de claves (por ejemplo, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span><span class="sxs-lookup"><span data-stu-id="b85e5-122">Set the key ring storage location (for example, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)).</span></span> <span data-ttu-id="b85e5-123">Dado que una llamada a la ubicación que se debe establecer `ProtectKeysWithAzureKeyVault` implementa un [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) que deshabilita la configuración de protección automática de los datos, incluida la ubicación de almacenamiento del conjunto de claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-123">The location must be set because calling `ProtectKeysWithAzureKeyVault` implements an [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) that disables automatic data protection settings, including the key ring storage location.</span></span> <span data-ttu-id="b85e5-124">El ejemplo anterior usa Azure Blob Storage para conservar el conjunto de claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-124">The preceding example uses Azure Blob Storage to persist the key ring.</span></span> <span data-ttu-id="b85e5-125">Para obtener más información, consulte [proveedores de almacenamiento de claves: Azure y Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span><span class="sxs-lookup"><span data-stu-id="b85e5-125">For more information, see [Key storage providers: Azure and Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis).</span></span> <span data-ttu-id="b85e5-126">También puede conservar el conjunto de claves localmente con [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span><span class="sxs-lookup"><span data-stu-id="b85e5-126">You can also persist the key ring locally with [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).</span></span>

<span data-ttu-id="b85e5-127">El `keyIdentifier` es el identificador de clave de almacén de claves utilizado para el cifrado de clave (por ejemplo, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span><span class="sxs-lookup"><span data-stu-id="b85e5-127">The `keyIdentifier` is the key vault key identifier used for key encryption (for example, `https://contosokeyvault.vault.azure.net/keys/dataprotection/`).</span></span>

<span data-ttu-id="b85e5-128">`ProtectKeysWithAzureKeyVault` sobrecargas:</span><span class="sxs-lookup"><span data-stu-id="b85e5-128">`ProtectKeysWithAzureKeyVault` overloads:</span></span>

* <span data-ttu-id="b85e5-129">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permite el uso de un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) para habilitar el sistema de protección de datos usar el almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-129">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permits the use of a [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b85e5-130">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permite el uso de un `ClientId` y [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) para habilitar el sistema de protección de datos usar el almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-130">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permits the use of a `ClientId` and [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to enable the data protection system to use the key vault.</span></span>
* <span data-ttu-id="b85e5-131">[ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permite el uso de un `ClientId` y `ClientSecret` para habilitar el sistema de protección de datos usar el almacén de claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-131">[ProtectKeysWithAzureKeyVault(IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permits the use of a `ClientId` and `ClientSecret` to enable the data protection system to use the key vault.</span></span>

::: moniker-end

## <a name="persistkeystofilesystem"></a><span data-ttu-id="b85e5-132">PersistKeysToFileSystem</span><span class="sxs-lookup"><span data-stu-id="b85e5-132">PersistKeysToFileSystem</span></span>

<span data-ttu-id="b85e5-133">Para almacenar las claves en un recurso compartido UNC en lugar de en el *% LOCALAPPDATA %* ubicación predeterminada, configure el sistema con [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span><span class="sxs-lookup"><span data-stu-id="b85e5-133">To store keys on a UNC share instead of at the *%LOCALAPPDATA%* default location, configure the system with [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> <span data-ttu-id="b85e5-134">Si cambia la ubicación de persistencia de clave, el sistema ya no automáticamente cifra las claves en reposo, puesto que desconoce si DPAPI es un mecanismo de cifrado adecuado.</span><span class="sxs-lookup"><span data-stu-id="b85e5-134">If you change the key persistence location, the system no longer automatically encrypts keys at rest, since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

## <a name="protectkeyswith"></a><span data-ttu-id="b85e5-135">ProtectKeysWith\*</span><span class="sxs-lookup"><span data-stu-id="b85e5-135">ProtectKeysWith\*</span></span>

<span data-ttu-id="b85e5-136">Puede configurar el sistema para proteger las claves en reposo mediante una llamada a cualquiera de los [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API de configuración.</span><span class="sxs-lookup"><span data-stu-id="b85e5-136">You can configure the system to protect keys at rest by calling any of the [ProtectKeysWith\*](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) configuration APIs.</span></span> <span data-ttu-id="b85e5-137">Tenga en cuenta el ejemplo siguiente, que almacena las claves en un recurso compartido UNC y cifra estas claves en reposo con un certificado X.509 concreto:</span><span class="sxs-lookup"><span data-stu-id="b85e5-137">Consider the example below, which stores keys on a UNC share and encrypts those keys at rest with a specific X.509 certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b85e5-138">En ASP.NET Core 2.1 o posterior, puede proporcionar un [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), como cargar un certificado desde un archivo:</span><span class="sxs-lookup"><span data-stu-id="b85e5-138">In ASP.NET Core 2.1 or later, you can provide an [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) to [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), such as a certificate loaded from a file:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
}
```

::: moniker-end

<span data-ttu-id="b85e5-139">Consulte [clave de cifrado en reposo](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más ejemplos e información sobre los mecanismos de cifrado de claves integrada.</span><span class="sxs-lookup"><span data-stu-id="b85e5-139">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more examples and discussion on the built-in key encryption mechanisms.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a><span data-ttu-id="b85e5-140">UnprotectKeysWithAnyCertificate</span><span class="sxs-lookup"><span data-stu-id="b85e5-140">UnprotectKeysWithAnyCertificate</span></span>

<span data-ttu-id="b85e5-141">En ASP.NET Core 2.1 o posterior, puede girar certificados y descifrar las claves en reposo mediante una matriz de [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificados con [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span><span class="sxs-lookup"><span data-stu-id="b85e5-141">In ASP.NET Core 2.1 or later, you can rotate certificates and decrypt keys at rest using an array of [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificates with [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate(
            new X509Certificate2("certificate.pfx", "password"));
        .UnprotectKeysWithAnyCertificate(
            new X509Certificate2("certificate_old_1.pfx", "password_1"),
            new X509Certificate2("certificate_old_2.pfx", "password_2"));
}
```

::: moniker-end

## <a name="setdefaultkeylifetime"></a><span data-ttu-id="b85e5-142">SetDefaultKeyLifetime</span><span class="sxs-lookup"><span data-stu-id="b85e5-142">SetDefaultKeyLifetime</span></span>

<span data-ttu-id="b85e5-143">Para configurar el sistema para usar una vigencia de clave de 14 días en lugar del predeterminado de 90 días, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span><span class="sxs-lookup"><span data-stu-id="b85e5-143">To configure the system to use a key lifetime of 14 days instead of the default 90 days, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a><span data-ttu-id="b85e5-144">SetApplicationName</span><span class="sxs-lookup"><span data-stu-id="b85e5-144">SetApplicationName</span></span>

<span data-ttu-id="b85e5-145">De forma predeterminada, el sistema de protección de datos aísla las aplicaciones entre sí, incluso si comparte el mismo repositorio clave físico.</span><span class="sxs-lookup"><span data-stu-id="b85e5-145">By default, the Data Protection system isolates apps from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="b85e5-146">Esto evita que las aplicaciones desde la comprensión de los demás cargas protegidas.</span><span class="sxs-lookup"><span data-stu-id="b85e5-146">This prevents the apps from understanding each other's protected payloads.</span></span> <span data-ttu-id="b85e5-147">Para compartir las cargas protegidas entre dos aplicaciones, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) con el mismo valor para cada aplicación:</span><span class="sxs-lookup"><span data-stu-id="b85e5-147">To share protected payloads between two apps, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) with the same value for each app:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a><span data-ttu-id="b85e5-148">DisableAutomaticKeyGeneration</span><span class="sxs-lookup"><span data-stu-id="b85e5-148">DisableAutomaticKeyGeneration</span></span>

<span data-ttu-id="b85e5-149">Puede tener un escenario donde no desea que una aplicación para implementar automáticamente las claves (y crear nuevas claves) como enfocar la expiración.</span><span class="sxs-lookup"><span data-stu-id="b85e5-149">You may have a scenario where you don't want an app to automatically roll keys (create new keys) as they approach expiration.</span></span> <span data-ttu-id="b85e5-150">Un ejemplo de esto podría ser configuradas en una relación principal/secundario, donde solo la aplicación principal es responsable de interés de administración de claves y las aplicaciones secundarias solo tienen una vista de solo lectura del anillo de clave de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="b85e5-150">One example of this might be apps set up in a primary/secondary relationship, where only the primary app is responsible for key management concerns and secondary apps simply have a read-only view of the key ring.</span></span> <span data-ttu-id="b85e5-151">Las aplicaciones secundarias pueden configurarse para tratar el conjunto de claves como de solo lectura mediante la configuración del sistema con [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span><span class="sxs-lookup"><span data-stu-id="b85e5-151">The secondary apps can be configured to treat the key ring as read-only by configuring the system with [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a><span data-ttu-id="b85e5-152">Aislamiento por aplicación</span><span class="sxs-lookup"><span data-stu-id="b85e5-152">Per-application isolation</span></span>

<span data-ttu-id="b85e5-153">Cuando el sistema de protección de datos se proporciona un host de ASP.NET Core, aísla automáticamente las aplicaciones entre sí, incluso si esas aplicaciones se ejecutan en la misma cuenta de proceso de trabajo y están usando el mismo material de clave maestra.</span><span class="sxs-lookup"><span data-stu-id="b85e5-153">When the Data Protection system is provided by an ASP.NET Core host, it automatically isolates apps from one another, even if those apps are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="b85e5-154">Esto es algo similar para el modificador IsolateApps desde de System.Web  **\<machineKey >** elemento.</span><span class="sxs-lookup"><span data-stu-id="b85e5-154">This is somewhat similar to the IsolateApps modifier from System.Web's **\<machineKey>** element.</span></span>

<span data-ttu-id="b85e5-155">El mecanismo de aislamiento se funciona teniendo en cuenta cada aplicación en el equipo local como un único inquilino, por lo tanto el [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) ha modificado para cualquier aplicación incluye automáticamente el identificador de aplicación como discriminador.</span><span class="sxs-lookup"><span data-stu-id="b85e5-155">The isolation mechanism works by considering each app on the local machine as a unique tenant, thus the [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) rooted for any given app automatically includes the app ID as a discriminator.</span></span> <span data-ttu-id="b85e5-156">Identificador único de la aplicación procede de uno de estos dos lugares:</span><span class="sxs-lookup"><span data-stu-id="b85e5-156">The app's unique ID comes from one of two places:</span></span>

1. <span data-ttu-id="b85e5-157">Si la aplicación se hospeda en IIS, el identificador único es la ruta de acceso de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b85e5-157">If the app is hosted in IIS, the unique identifier is the app's configuration path.</span></span> <span data-ttu-id="b85e5-158">Si una aplicación se implementa en un entorno de granja de servidores web, este valor debe ser estable suponiendo que los entornos de IIS se configuran de forma similar en todos los equipos en la granja de servidores web.</span><span class="sxs-lookup"><span data-stu-id="b85e5-158">If an app is deployed in a web farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the web farm.</span></span>

2. <span data-ttu-id="b85e5-159">Si la aplicación no está hospedada en IIS, el identificador único es la ruta de acceso física de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b85e5-159">If the app isn't hosted in IIS, the unique identifier is the physical path of the app.</span></span>

<span data-ttu-id="b85e5-160">El identificador único está diseñado para sobrevivir restablecimientos &mdash; tanto de la aplicación individual de la propia máquina.</span><span class="sxs-lookup"><span data-stu-id="b85e5-160">The unique identifier is designed to survive resets &mdash; both of the individual app and of the machine itself.</span></span>

<span data-ttu-id="b85e5-161">Este mecanismo de aislamiento se da por supuesto que las aplicaciones no son malintencionadas.</span><span class="sxs-lookup"><span data-stu-id="b85e5-161">This isolation mechanism assumes that the apps are not malicious.</span></span> <span data-ttu-id="b85e5-162">Una aplicación malintencionada siempre puede afectar a cualquier otra aplicación que se ejecutan en la misma cuenta de proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="b85e5-162">A malicious app can always impact any other app running under the same worker process account.</span></span> <span data-ttu-id="b85e5-163">En un entorno de hospedaje compartido donde las aplicaciones no son de confianza mutua, el proveedor de hospedaje debe tomar medidas para garantizar el aislamiento de nivel de sistema operativo entre aplicaciones, incluida la separación de las aplicaciones subyacentes de repositorios de claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-163">In a shared hosting environment where apps are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between apps, including separating the apps' underlying key repositories.</span></span>

<span data-ttu-id="b85e5-164">Si el sistema de protección de datos no se proporciona un host de ASP.NET Core (por ejemplo, si crea instancias de él a través de la `DataProtectionProvider` tipo concreto) se deshabilita el aislamiento de la aplicación de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b85e5-164">If the Data Protection system isn't provided by an ASP.NET Core host (for example, if you instantiate it via the `DataProtectionProvider` concrete type) app isolation is disabled by default.</span></span> <span data-ttu-id="b85e5-165">Cuando se deshabilita el aislamiento de aplicaciones, todas las aplicaciones respaldadas por el mismo material de clave pueden compartir las cargas de siempre proporcionen adecuado [fines](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="b85e5-165">When app isolation is disabled, all apps backed by the same keying material can share payloads as long as they provide the appropriate [purposes](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="b85e5-166">Para proporcionar aislamiento de aplicaciones en este entorno, llame a la [SetApplicationName](#setapplicationname) método en la configuración de objetos y proporcione un nombre único para cada aplicación.</span><span class="sxs-lookup"><span data-stu-id="b85e5-166">To provide app isolation in this environment, call the [SetApplicationName](#setapplicationname) method on the configuration object and provide a unique name for each app.</span></span>

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a><span data-ttu-id="b85e5-167">Cambio de los algoritmos con UseCryptographicAlgorithms</span><span class="sxs-lookup"><span data-stu-id="b85e5-167">Changing algorithms with UseCryptographicAlgorithms</span></span>

<span data-ttu-id="b85e5-168">La pila de protección de datos permite cambiar el algoritmo predeterminado usado por las claves recién generadas.</span><span class="sxs-lookup"><span data-stu-id="b85e5-168">The Data Protection stack allows you to change the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="b85e5-169">La manera más sencilla de hacerlo es llamar a [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) desde la devolución de llamada de configuración:</span><span class="sxs-lookup"><span data-stu-id="b85e5-169">The simplest way to do this is to call [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) from the configuration callback:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b85e5-170">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-170">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b85e5-171">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-171">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="b85e5-172">El algoritmo de cifrado predeterminada es AES-256-CBC y el valor predeterminado ValidationAlgorithm es HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="b85e5-172">The default EncryptionAlgorithm is AES-256-CBC, and the default ValidationAlgorithm is HMACSHA256.</span></span> <span data-ttu-id="b85e5-173">La directiva predeterminada se puede establecer un administrador del sistema a través de un [todo el equipo directiva](xref:security/data-protection/configuration/machine-wide-policy), pero una llamada explícita a `UseCryptographicAlgorithms` invalida la directiva predeterminada.</span><span class="sxs-lookup"><span data-stu-id="b85e5-173">The default policy can be set by a system administrator via a [machine-wide policy](xref:security/data-protection/configuration/machine-wide-policy), but an explicit call to `UseCryptographicAlgorithms` overrides the default policy.</span></span>

<span data-ttu-id="b85e5-174">Una llamada a `UseCryptographicAlgorithms` le permite especificar el algoritmo deseado de una lista predefinida integrada.</span><span class="sxs-lookup"><span data-stu-id="b85e5-174">Calling `UseCryptographicAlgorithms` allows you to specify the desired algorithm from a predefined built-in list.</span></span> <span data-ttu-id="b85e5-175">No es necesario preocuparse por la implementación del algoritmo.</span><span class="sxs-lookup"><span data-stu-id="b85e5-175">You don't need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="b85e5-176">En el escenario anterior, el sistema de protección de datos intenta usar la implementación de CNG de AES si se ejecuta en Windows.</span><span class="sxs-lookup"><span data-stu-id="b85e5-176">In the scenario above, the Data Protection system attempts to use the CNG implementation of AES if running on Windows.</span></span> <span data-ttu-id="b85e5-177">En caso contrario, recurre a los recursos administrados [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) clase.</span><span class="sxs-lookup"><span data-stu-id="b85e5-177">Otherwise, it falls back to the managed [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) class.</span></span>

<span data-ttu-id="b85e5-178">Puede especificar manualmente una implementación a través de una llamada a [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span><span class="sxs-lookup"><span data-stu-id="b85e5-178">You can manually specify an implementation via a call to [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).</span></span>

> [!TIP]
> <span data-ttu-id="b85e5-179">Algoritmos de cambio no afecta a las claves existentes en el conjunto de claves.</span><span class="sxs-lookup"><span data-stu-id="b85e5-179">Changing algorithms doesn't affect existing keys in the key ring.</span></span> <span data-ttu-id="b85e5-180">Solo afecta a las claves recién generadas.</span><span class="sxs-lookup"><span data-stu-id="b85e5-180">It only affects newly-generated keys.</span></span>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="b85e5-181">Especificación de algoritmos administrados personalizados</span><span class="sxs-lookup"><span data-stu-id="b85e5-181">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b85e5-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b85e5-183">Para especificar algoritmos administrados personalizados, cree un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instancia que apunta a los tipos de implementación:</span><span class="sxs-lookup"><span data-stu-id="b85e5-183">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptorConfiguration()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b85e5-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b85e5-185">Para especificar algoritmos administrados personalizados, cree un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instancia que apunta a los tipos de implementación:</span><span class="sxs-lookup"><span data-stu-id="b85e5-185">To specify custom managed algorithms, create a [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instance that points to the implementation types:</span></span>

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new ManagedAuthenticatedEncryptionSettings()
    {
        // A type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // A type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

<span data-ttu-id="b85e5-186">Por lo general el \*propiedades de tipo deben apuntar a hormigón, implementaciones (a través de un constructor público sin parámetros) se pueden crear instancias de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) y [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), aunque el sistema casos especiales los algunos valores como `typeof(Aes)` para mayor comodidad.</span><span class="sxs-lookup"><span data-stu-id="b85e5-186">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) and [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), though the system special-cases some values like `typeof(Aes)` for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="b85e5-187">El SymmetricAlgorithm debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 64 bits ≥ y debe admitir el cifrado de modo CBC con relleno PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="b85e5-187">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b85e5-188">El KeyedHashAlgorithm debe tener un tamaño de la síntesis de > = 128 bits, y debe ser compatible con claves de una longitud igual a la longitud del resumen del algoritmo hash.</span><span class="sxs-lookup"><span data-stu-id="b85e5-188">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="b85e5-189">El KeyedHashAlgorithm no es estrictamente necesaria para ser HMAC.</span><span class="sxs-lookup"><span data-stu-id="b85e5-189">The KeyedHashAlgorithm isn't strictly required to be HMAC.</span></span>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="b85e5-190">Especificar los algoritmos personalizados de CNG de Windows</span><span class="sxs-lookup"><span data-stu-id="b85e5-190">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b85e5-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b85e5-192">Para especificar un algoritmo personalizado de CNG de Windows con cifrado CBC-modo de validación de HMAC, cree un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instancia que contiene la información algorítmica:</span><span class="sxs-lookup"><span data-stu-id="b85e5-192">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b85e5-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b85e5-194">Para especificar un algoritmo personalizado de CNG de Windows con cifrado CBC-modo de validación de HMAC, cree un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instancia que contiene la información algorítmica:</span><span class="sxs-lookup"><span data-stu-id="b85e5-194">To specify a custom Windows CNG algorithm using CBC-mode encryption with HMAC validation, create a [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngCbcAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256,

        // Passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> <span data-ttu-id="b85e5-195">El algoritmo de cifrado de bloques simétricos debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de > = 64 bits, y debe admitir el cifrado de modo CBC con relleno PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="b85e5-195">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of >= 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="b85e5-196">El algoritmo hash debe tener un tamaño de la síntesis de > = 128 bits y deben admitir que se abre con el BCRYPT\_ALG\_controlar\_HMAC\_indicador de marca.</span><span class="sxs-lookup"><span data-stu-id="b85e5-196">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT\_ALG\_HANDLE\_HMAC\_FLAG flag.</span></span> <span data-ttu-id="b85e5-197">El \*las propiedades del proveedor se pueden establecer en null para usar el proveedor predeterminado para el algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="b85e5-197">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b85e5-198">Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b85e5-198">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b85e5-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="b85e5-200">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo contador de Galois/con la validación, cree un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instancia que contiene la información algorítmica:</span><span class="sxs-lookup"><span data-stu-id="b85e5-200">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b85e5-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b85e5-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="b85e5-202">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo contador de Galois/con la validación, cree un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instancia que contiene la información algorítmica:</span><span class="sxs-lookup"><span data-stu-id="b85e5-202">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption with validation, create a [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instance that contains the algorithmic information:</span></span>

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(
        new CngGcmAuthenticatedEncryptionSettings()
    {
        // Passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // Specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> <span data-ttu-id="b85e5-203">El algoritmo de cifrado de bloques simétricos debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de 128 bits exactamente, y debe admitir el cifrado de GCM.</span><span class="sxs-lookup"><span data-stu-id="b85e5-203">The symmetric block cipher algorithm must have a key length of >= 128 bits, a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="b85e5-204">Puede establecer el [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propiedad en null para usar el proveedor predeterminado para el algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="b85e5-204">You can set the [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) property to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="b85e5-205">Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b85e5-205">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="b85e5-206">Especificar otros algoritmos personalizados</span><span class="sxs-lookup"><span data-stu-id="b85e5-206">Specifying other custom algorithms</span></span>

<span data-ttu-id="b85e5-207">Aunque no se expone como una API de primera clase, el sistema de protección de datos es lo suficientemente extensible como para permitir la especificación de casi cualquier tipo de algoritmo.</span><span class="sxs-lookup"><span data-stu-id="b85e5-207">Though not exposed as a first-class API, the Data Protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="b85e5-208">Por ejemplo, es posible mantener todas las claves contenidas dentro de un módulo de seguridad de Hardware (HSM) y para proporcionar una implementación personalizada de las principales rutinas de cifrado y descifrado.</span><span class="sxs-lookup"><span data-stu-id="b85e5-208">For example, it's possible to keep all keys contained within a Hardware Security Module (HSM) and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="b85e5-209">Consulte [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) en [principales de extensibilidad de criptografía](xref:security/data-protection/extensibility/core-crypto) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="b85e5-209">See [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) in [Core cryptography extensibility](xref:security/data-protection/extensibility/core-crypto) for more information.</span></span>

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a><span data-ttu-id="b85e5-210">Conservar las claves cuando se hospedan en un contenedor de Docker</span><span class="sxs-lookup"><span data-stu-id="b85e5-210">Persisting keys when hosting in a Docker container</span></span>

<span data-ttu-id="b85e5-211">Cuando se hospedan en un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenedor, las claves deben mantenerse en uno:</span><span class="sxs-lookup"><span data-stu-id="b85e5-211">When hosting in a [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) container, keys should be maintained in either:</span></span>

* <span data-ttu-id="b85e5-212">Una carpeta que es un volumen de Docker que se conserva más allá de la duración del contenedor, como un volumen compartido o un volumen montado en host.</span><span class="sxs-lookup"><span data-stu-id="b85e5-212">A folder that's a Docker volume that persists beyond the container's lifetime, such as a shared volume or a host-mounted volume.</span></span>
* <span data-ttu-id="b85e5-213">Un proveedor externo, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/).</span><span class="sxs-lookup"><span data-stu-id="b85e5-213">An external provider, such as [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) or [Redis](https://redis.io/).</span></span>

## <a name="see-also"></a><span data-ttu-id="b85e5-214">Vea también</span><span class="sxs-lookup"><span data-stu-id="b85e5-214">See also</span></span>

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
