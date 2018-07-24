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
# <a name="key-encryption-at-rest-in-aspnet-core"></a>Cifrado de claves en reposo en ASP.NET Core

El sistema de protección de datos [emplea un mecanismo de detección predeterminada](xref:security/data-protection/configuration/default-settings) para determinar las claves criptográficas cómo se cifren en reposo. El desarrollador puede invalidar el mecanismo de detección y especificar manualmente cómo se deben cifrar las claves en reposo.

> [!WARNING]
> Si especifica una explícita [ubicación de persistencia de la clave](xref:security/data-protection/implementation/key-storage-providers), el cifrado de clave predeterminado en el mecanismo de rest anula el registro del sistema de protección de datos. Por lo tanto, las claves ya no se cifran en reposo. Se recomienda [especifica un mecanismo de cifrado de claves explícitas](xref:security/data-protection/implementation/key-encryption-at-rest) para las implementaciones de producción. En este tema, se describen las opciones de mecanismo de cifrado en reposo.

::: moniker range=">= aspnetcore-2.1"

## <a name="azure-key-vault"></a>Azure Key Vault

Para almacenar las claves en [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurar el sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) en el `Startup` clase:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Para obtener más información, consulte [configurar protección de datos de ASP.NET Core: ProtectKeysWithAzureKeyVault](xref:security/data-protection/configuration/overview#protectkeyswithazurekeyvault).

::: moniker-end

## <a name="windows-dpapi"></a>DPAPI de Windows

**Solo se aplica a las implementaciones de Windows.**

Cuando se utiliza DPAPI de Windows, el material de clave se cifra con [CryptProtectData](/windows/desktop/api/dpapi/nf-dpapi-cryptprotectdata) antes de que se conservan en el almacenamiento. DPAPI es un mecanismo de cifrado adecuado para los datos que no se leen nunca fuera de la máquina actual (aunque es posible realizar una copia de estas claves hasta que Active Directory; vea [DPAPI y perfiles móviles](https://support.microsoft.com/kb/309408/#6)). Para configurar el cifrado de claves en reposo DPAPI, llame a uno de los [ProtectKeysWithDpapi](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpapi) métodos de extensión:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Only the local user account can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi();
}
```

Si `ProtectKeysWithDpapi` se llama sin parámetros, solo la cuenta de usuario de Windows actual puede descifrar el conjunto de claves persistente. Opcionalmente, puede especificar que cualquier cuenta de usuario en el equipo (no solo la cuenta de usuario actual) podrá descifrar el conjunto de claves:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // All user accounts on the machine can decrypt the keys
    services.AddDataProtection()
        .ProtectKeysWithDpapi(protectToLocalMachine: true);
}
```

::: moniker range=">= aspnetcore-2.0"

## <a name="x509-certificate"></a>Certificado X.509

Si la aplicación se reparte entre varias máquinas, puede ser conveniente distribuir un certificado X.509 compartido entre las máquinas y configure las aplicaciones hospedadas para usar el certificado para el cifrado de claves en reposo:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithCertificate("3BCE558E2AD3E0E34A7743EAB5AEA2A9BD2575A0");
}
```

Debido a limitaciones de .NET Framework, se admiten solo los certificados con claves privadas de CAPI. Ver el contenido siguiente para buscar posibles soluciones a estas limitaciones.

::: moniker-end

## <a name="windows-dpapi-ng"></a>DPAPI de Windows-NG

**Este mecanismo solo está disponible en Windows 8/Windows Server 2012 o posterior.**

A partir de Windows 8, sistema operativo Windows es compatible con DPAPI-NG (también denominado CNG DPAPI). Para obtener más información, consulte [sobre DPAPI de CNG](/windows/desktop/SecCNG/cng-dpapi).

La entidad de seguridad se codifica como una regla de descriptor de protección. En el ejemplo siguiente que llama a [ProtectKeysWithDpapiNG](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithdpaping), solo el usuario de dominio con el SID especificado puede descifrar el conjunto de claves:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Uses the descriptor rule "SID=S-1-5-21-..."
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("SID=S-1-5-21-...",
        flags: DpapiNGProtectionDescriptorFlags.None);
}
```

También hay una sobrecarga sin parámetros de `ProtectKeysWithDpapiNG`. Use este método de conveniencia para especificar la regla "SID = {CURRENT_ACCOUNT_SID}", donde *CURRENT_ACCOUNT_SID* es el SID de la cuenta de usuario de Windows actual:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Use the descriptor rule "SID={current account SID}"
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG();
}
```

En este escenario, el controlador de dominio de AD es responsable de distribuir las claves de cifrado utilizadas por las operaciones de DPAPI NG. El usuario de destino pueda descifrar la carga cifrada desde cualquier equipo unido al dominio (siempre que el proceso se ejecuta bajo su identidad).

## <a name="certificate-based-encryption-with-windows-dpapi-ng"></a>Basada en certificados de cifrado con DPAPI de Windows-NG

Si la aplicación se ejecuta en Windows 8.1 o Windows Server 2012 R2 o versiones posteriores, puede usar Windows DPAPI-NG para realizar el cifrado basada en certificados. Utilice la cadena de descriptor de la regla "certificado = HashId:THUMBPRINT", donde *huella digital* es la huella digital con codificación hexadecimal SHA1 del certificado:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .ProtectKeysWithDpapiNG("CERTIFICATE=HashId:3BCE558E2...B5AEA2A9BD2575A0",
            flags: DpapiNGProtectionDescriptorFlags.None);
}
```

Cualquier aplicación que señala a este repositorio debe estar ejecutándose en Windows 8.1 o Windows Server 2012 R2 o posterior para descifrar las claves.

## <a name="custom-key-encryption"></a>Cifrado de claves personalizado

Si los mecanismos en el equipo no son adecuados, el desarrollador puede especificar su propio mecanismo de cifrado de claves al proporcionar una personalizada [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor).
