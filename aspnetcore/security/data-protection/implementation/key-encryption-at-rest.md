---
title: Cifrado de claves en reposo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación del cifrado de clave de protección de datos de ASP.NET Core en reposo.
ms.author: riande
ms.date: 07/16/2018
uid: security/data-protection/implementation/key-encryption-at-rest
ms.openlocfilehash: 52c3137dbe467096364b42430c92aecc7c15e313
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219295"
---
# <a name="key-encryption-at-rest-in-aspnet-core"></a><span data-ttu-id="f7ca5-103">Cifrado de claves en reposo en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7ca5-103">Key encryption at rest in ASP.NET Core</span></span>

<span data-ttu-id="f7ca5-104">El sistema de protección de datos [emplea un mecanismo de detección predeterminada](xref:security/data-protection/configuration/default-settings) para determinar las claves criptográficas cómo se cifren en reposo.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-104">The data protection system [employs a discovery mechanism by default](xref:security/data-protection/configuration/default-settings) to determine how cryptographic keys should be encrypted at rest.</span></span> <span data-ttu-id="f7ca5-105">El desarrollador puede invalidar el mecanismo de detección y especificar manualmente cómo se deben cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-105">The developer can override the discovery mechanism and manually specify how keys should be encrypted at rest.</span></span>

> [!WARNING]
> <span data-ttu-id="f7ca5-106">Si especifica una explícita [ubicación de persistencia de la clave](xref:security/data-protection/implementation/key-storage-providers), el cifrado de clave predeterminado en el mecanismo de rest anula el registro del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-106">If you specify an explicit [key persistence location](xref:security/data-protection/implementation/key-storage-providers), the data protection system deregisters the default key encryption at rest mechanism.</span></span> <span data-ttu-id="f7ca5-107">Por lo tanto, las claves ya no se cifran en reposo.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-107">Consequently, keys are no longer encrypted at rest.</span></span> <span data-ttu-id="f7ca5-108">Se recomienda [especifica un mecanismo de cifrado de claves explícitas](xref:security/data-protection/implementation/key-encryption-at-rest) para las implementaciones de producción.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-108">We recommend that you [specify an explicit key encryption mechanism](xref:security/data-protection/implementation/key-encryption-at-rest) for production deployments.</span></span> <span data-ttu-id="f7ca5-109">En este tema, se describen las opciones de mecanismo de cifrado en reposo.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-109">The encryption-at-rest mechanism options are described in this topic.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a><span data-ttu-id="f7ca5-110">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="f7ca5-110">Azure Key Vault</span></span>

<span data-ttu-id="f7ca5-111">Para almacenar las claves en [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurar el sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) en el `Startup` clase:</span><span class="sxs-lookup"><span data-stu-id="f7ca5-111">To store keys in [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configure the system with [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) in the `Startup` class:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

<span data-ttu-id="f7ca5-112">Para obtener más información, consulte [configurar protección de datos de ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span><span class="sxs-lookup"><span data-stu-id="f7ca5-112">For more information, see [Configure ASP.NET Core Data Protection: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).</span></span>

::: moniker-end

## <a name="windows-dpapi"></a><span data-ttu-id="f7ca5-113">DPAPI de Windows</span><span class="sxs-lookup"><span data-stu-id="f7ca5-113">Windows DPAPI</span></span>

<span data-ttu-id="f7ca5-114">**Solo se aplica a las implementaciones de Windows.**</span><span class="sxs-lookup"><span data-stu-id="f7ca5-114">**Only applies to Windows deployments.**</span></span>

<span data-ttu-id="f7ca5-115">Cuando se utiliza DPAPI de Windows, el material de clave se cifra con [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) antes de que se conservan en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-115">When Windows DPAPI is used, key material is encrypted with [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) before being persisted to storage.</span></span> <span data-ttu-id="f7ca5-116">DPAPI es un mecanismo de cifrado adecuado para los datos que no se leen nunca fuera de la máquina actual (aunque es posible realizar una copia de estas claves hasta que Active Directory; vea [DPAPI y perfiles móviles](https://support.microsoft.com/kb/309408/#6)).</span><span class="sxs-lookup"><span data-stu-id="f7ca5-116">DPAPI is an appropriate encryption mechanism for data that's never read outside of the current machine (though it's possible to back these keys up to Active Directory; see [DPAPI and Roaming Profiles](https://support.microsoft.com/kb/309408/#6)).</span></span> <span data-ttu-id="f7ca5-117">Para configurar el cifrado de claves en reposo DPAPI, llame a uno de los [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) métodos de extensión:</span><span class="sxs-lookup"><span data-stu-id="f7ca5-117">To configure DPAPI key-at-rest encryption, call one of the [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) extension methods:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

<span data-ttu-id="f7ca5-118">Si `ProtectKeysWithDpapi` se llama sin parámetros, solo la cuenta de usuario de Windows actual puede descifrar el conjunto de claves persistente.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-118">If `ProtectKeysWithDpapi` is called with no parameters, only the current Windows user account can decipher the persisted key ring.</span></span> <span data-ttu-id="f7ca5-119">Opcionalmente, puede especificar que cualquier cuenta de usuario en el equipo (no solo la cuenta de usuario actual) podrá descifrar el conjunto de claves:</span><span class="sxs-lookup"><span data-stu-id="f7ca5-119">You can optionally specify that any user account on the machine (not just the current user account) be able to decipher the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a><span data-ttu-id="f7ca5-120">Certificado X.509</span><span class="sxs-lookup"><span data-stu-id="f7ca5-120">X.509 certificate</span></span>

<span data-ttu-id="f7ca5-121">Si la aplicación se reparte entre varias máquinas, puede ser conveniente distribuir un certificado X.509 compartido entre las máquinas y configure las aplicaciones hospedadas para usar el certificado para el cifrado de claves en reposo:</span><span class="sxs-lookup"><span data-stu-id="f7ca5-121">If the app is spread across multiple machines, it may be convenient to distribute a shared X.509 certificate across the machines and configure the hosted apps to use the certificate for encryption of keys at rest:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

<span data-ttu-id="f7ca5-122">Debido a limitaciones de .NET Framework, se admiten solo los certificados con claves privadas de CAPI.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-122">Due to .NET Framework limitations, only certificates with CAPI private keys are supported.</span></span> <span data-ttu-id="f7ca5-123">Ver el contenido siguiente para buscar posibles soluciones a estas limitaciones.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-123">See the content below for possible workarounds to these limitations.</span></span>

::: moniker-end

## <a name="windows-dpapi-ng"></a><span data-ttu-id="f7ca5-124">DPAPI de Windows-NG</span><span class="sxs-lookup"><span data-stu-id="f7ca5-124">Windows DPAPI-NG</span></span>

<span data-ttu-id="f7ca5-125">**Este mecanismo solo está disponible en Windows 8/Windows Server 2012 o posterior.**</span><span class="sxs-lookup"><span data-stu-id="f7ca5-125">**This mechanism is available only on Windows 8/Windows Server 2012 or later.**</span></span>

<span data-ttu-id="f7ca5-126">A partir de Windows 8, sistema operativo Windows es compatible con DPAPI-NG (también denominado CNG DPAPI).</span><span class="sxs-lookup"><span data-stu-id="f7ca5-126">Beginning with Windows 8, Windows OS supports DPAPI-NG (also called CNG DPAPI).</span></span> <span data-ttu-id="f7ca5-127">Para obtener más información, consulte [sobre DPAPI de CNG](/windows/desktop/SecCNG/cng-dpapi).</span><span class="sxs-lookup"><span data-stu-id="f7ca5-127">For more information, see [About CNG DPAPI](/windows/desktop/SecCNG/cng-dpapi).</span></span>

<span data-ttu-id="f7ca5-128">La entidad de seguridad se codifica como una regla de descriptor de protección.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-128">The principal is encoded as a protection descriptor rule.</span></span> <span data-ttu-id="f7ca5-129">En el ejemplo siguiente que llama a [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), solo el usuario de dominio con el SID especificado puede descifrar el conjunto de claves:</span><span class="sxs-lookup"><span data-stu-id="f7ca5-129">In the following example that calls [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), only the domain-joined user with the specified SID can decrypt the key ring:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="f7ca5-130">También hay una sobrecarga sin parámetros de `ProtectKeysWithDpapiNG`.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-130">There's also a parameterless overload of `ProtectKeysWithDpapiNG`.</span></span> <span data-ttu-id="f7ca5-131">Use este método de conveniencia para especificar la regla "SID = {CURRENT_ACCOUNT_SID}", donde *CURRENT_ACCOUNT_SID* es el SID de la cuenta de usuario de Windows actual:</span><span class="sxs-lookup"><span data-stu-id="f7ca5-131">Use this convenience method to specify the rule "SID={CURRENT_ACCOUNT_SID}", where *CURRENT_ACCOUNT_SID* is the SID of the current Windows user account:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

<span data-ttu-id="f7ca5-132">En este escenario, el controlador de dominio de AD es responsable de distribuir las claves de cifrado utilizadas por las operaciones de DPAPI NG.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-132">In this scenario, the AD domain controller is responsible for distributing the encryption keys used by the DPAPI-NG operations.</span></span> <span data-ttu-id="f7ca5-133">El usuario de destino pueda descifrar la carga cifrada desde cualquier equipo unido al dominio (siempre que el proceso se ejecuta bajo su identidad).</span><span class="sxs-lookup"><span data-stu-id="f7ca5-133">The target user can decipher the encrypted payload from any domain-joined machine (provided that the process is running under their identity).</span></span>

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a><span data-ttu-id="f7ca5-134">Basada en certificados de cifrado con DPAPI de Windows-NG</span><span class="sxs-lookup"><span data-stu-id="f7ca5-134">Certificate-based encryption with Windows DPAPI-NG</span></span>

<span data-ttu-id="f7ca5-135">Si la aplicación se ejecuta en Windows 8.1 o Windows Server 2012 R2 o versiones posteriores, puede usar Windows DPAPI-NG para realizar el cifrado basada en certificados.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-135">If the app is running on Windows 8.1/Windows Server 2012 R2 or later, you can use Windows DPAPI-NG to perform certificate-based encryption.</span></span> <span data-ttu-id="f7ca5-136">Utilice la cadena de descriptor de la regla "certificado = HashId:THUMBPRINT", donde *huella digital* es la huella digital con codificación hexadecimal SHA1 del certificado:</span><span class="sxs-lookup"><span data-stu-id="f7ca5-136">Use the rule descriptor string "CERTIFICATE=HashId:THUMBPRINT", where *THUMBPRINT* is the hex-encoded SHA1 thumbprint of the certificate:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

<span data-ttu-id="f7ca5-137">Cualquier aplicación que señala a este repositorio debe estar ejecutándose en Windows 8.1 o Windows Server 2012 R2 o posterior para descifrar las claves.</span><span class="sxs-lookup"><span data-stu-id="f7ca5-137">Any app pointed at this repository must be running on Windows 8.1/Windows Server 2012 R2 or later to decipher the keys.</span></span>

## <a name="custom-key-encryption"></a><span data-ttu-id="f7ca5-138">Cifrado de claves personalizado</span><span class="sxs-lookup"><span data-stu-id="f7ca5-138">Custom key encryption</span></span>

<span data-ttu-id="f7ca5-139">Si los mecanismos en el equipo no son adecuados, el desarrollador puede especificar su propio mecanismo de cifrado de claves al proporcionar una personalizada [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="f7ca5-139">If the in-box mechanisms aren't appropriate, the developer can specify their own key encryption mechanism by providing a custom [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).</span></span>
