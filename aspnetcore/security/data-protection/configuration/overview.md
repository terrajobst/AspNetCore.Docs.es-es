---
title: Configurar la protección de datos de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo configurar la protección de datos en ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/11/2019
uid: security/data-protection/configuration/overview
ms.openlocfilehash: ee43427fa1e82a365d49df50567b4ca7afb5a5d3
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516253"
---
# <a name="configure-aspnet-core-data-protection"></a>Configurar la protección de datos de ASP.NET Core

Cuando se inicializa el sistema de protección de datos, se aplica [configuración predeterminada](xref:security/data-protection/configuration/default-settings) según el entorno operativo. Esta configuración es normalmente adecuada para las aplicaciones que se ejecutan en una sola máquina. Hay casos donde un desarrollador puede querer cambiar la configuración predeterminada:

* La aplicación se reparte entre varias máquinas.
* Por motivos de cumplimiento.

Para estos escenarios, el sistema de protección de datos ofrece una API de configuración completo.

> [!WARNING]
> Al igual que los archivos de configuración, el conjunto de claves de protección de datos deben protegerse mediante los permisos adecuados. Puede elegir cifrar las claves en reposo, pero esto no impide que los atacantes crear nuevas claves. Por lo tanto, la seguridad de la aplicación se ve afectada. La ubicación de almacenamiento configurada con protección de datos debe tener su acceso limitado a la propia aplicación, similar a la manera en que desea proteger los archivos de configuración. Por ejemplo, si decide almacenar su conjunto de claves en el disco, utilice permisos del sistema de archivos. Asegúrese sólo la identidad bajo la que se ejecuta la aplicación web ha lectura, escritura y crear el acceso a ese directorio. Si usa Azure Table Storage, solo la aplicación web debe tener la capacidad de leer, escribir o crear nuevas entradas en el almacén de tablas, etcetera.
>
> El método de extensión [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) devuelve un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder` expone métodos de extensión que se pueden encadenar juntos para configurar la protección de datos de opciones.

::: moniker range=">= aspnetcore-2.1"

## <a name="protectkeyswithazurekeyvault"></a>ProtectKeysWithAzureKeyVault

Para almacenar las claves en [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), configurar el sistema con [ProtectKeysWithAzureKeyVault](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault) en el `Startup` clase:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToAzureBlobStorage(new Uri("<blobUriWithSasToken>"))
        .ProtectKeysWithAzureKeyVault("<keyIdentifier>", "<clientId>", "<clientSecret>");
}
```

Establecer la ubicación de almacenamiento del conjunto de claves (por ejemplo, [PersistKeysToAzureBlobStorage](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.persistkeystoazureblobstorage)). Dado que una llamada a la ubicación que se debe establecer `ProtectKeysWithAzureKeyVault` implementa un [IXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmlencryptor) que deshabilita la configuración de protección automática de los datos, incluida la ubicación de almacenamiento del conjunto de claves. El ejemplo anterior usa Azure Blob Storage para conservar el conjunto de claves. Para obtener más información, consulte [proveedores de almacenamiento de claves: Azure y Redis](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis). También puede conservar el conjunto de claves localmente con [PersistKeysToFileSystem](xref:security/data-protection/implementation/key-storage-providers#file-system).

El `keyIdentifier` es el identificador de clave de almacén de claves utilizado para el cifrado de clave. Por ejemplo, una clave creada en el almacén de claves denominado `dataprotection` en el `contosokeyvault` tiene el identificador de clave `https://contosokeyvault.vault.azure.net/keys/dataprotection/`. Proporcione la aplicación con **Unwrap Key** y **Wrap Key** permisos para el almacén de claves.

`ProtectKeysWithAzureKeyVault` sobrecargas:

* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, KeyVaultClient, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_Microsoft_Azure_KeyVault_KeyVaultClient_System_String_) permite el uso de un [KeyVaultClient](/dotnet/api/microsoft.azure.keyvault.keyvaultclient) para habilitar el sistema de protección de datos usar el almacén de claves.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, X509Certificate2)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_Security_Cryptography_X509Certificates_X509Certificate2_) permite el uso de un `ClientId` y [X509Certificate](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) para habilitar el sistema de protección de datos usar el almacén de claves.
* [ProtectKeysWithAzureKeyVault (IDataProtectionBuilder, String, String, String)](/dotnet/api/microsoft.aspnetcore.dataprotection.azuredataprotectionbuilderextensions.protectkeyswithazurekeyvault#Microsoft_AspNetCore_DataProtection_AzureDataProtectionBuilderExtensions_ProtectKeysWithAzureKeyVault_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_System_String_System_String_) permite el uso de un `ClientId` y `ClientSecret` para habilitar el sistema de protección de datos usar el almacén de claves.

::: moniker-end

## <a name="persistkeystofilesystem"></a>PersistKeysToFileSystem

Para almacenar las claves en un recurso compartido UNC en lugar de en el *% LOCALAPPDATA %* ubicación predeterminada, configure el sistema con [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

> [!WARNING]
> Si cambia la ubicación de persistencia de clave, el sistema ya no automáticamente cifra las claves en reposo, puesto que desconoce si DPAPI es un mecanismo de cifrado adecuado.

## <a name="protectkeyswith"></a>ProtectKeysWith\*

Puede configurar el sistema para proteger las claves en reposo mediante una llamada a cualquiera de los [ProtectKeysWith\* ](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions) API de configuración. Tenga en cuenta el ejemplo siguiente, que almacena las claves en un recurso compartido UNC y cifra estas claves en reposo con un certificado X.509 concreto:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

::: moniker range=">= aspnetcore-2.1"

En ASP.NET Core 2.1 o posterior, puede proporcionar un [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate), como cargar un certificado desde un archivo:

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

Consulte [clave de cifrado en reposo](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más ejemplos e información sobre los mecanismos de cifrado de claves integrada.

::: moniker range=">= aspnetcore-2.1"

## <a name="unprotectkeyswithanycertificate"></a>UnprotectKeysWithAnyCertificate

En ASP.NET Core 2.1 o posterior, puede girar certificados y descifrar las claves en reposo mediante una matriz de [X509Certificate2](/dotnet/api/system.security.cryptography.x509certificates.x509certificate2) certificados con [UnprotectKeysWithAnyCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.unprotectkeyswithanycertificate):

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

## <a name="setdefaultkeylifetime"></a>SetDefaultKeyLifetime

Para configurar el sistema para usar una vigencia de clave de 14 días en lugar del predeterminado de 90 días, use [SetDefaultKeyLifetime](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setdefaultkeylifetime):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

## <a name="setapplicationname"></a>SetApplicationName

De forma predeterminada, el sistema de protección de datos permite aislar las aplicaciones entre sí basándose en sus rutas de acceso raíz del contenido, incluso si comparte el mismo repositorio clave físico. Esto evita que las aplicaciones desde la comprensión de los demás cargas protegidas.

Para compartir protegido cargas entre aplicaciones:

* Configurar <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.SetApplicationName*> en cada aplicación con el mismo valor.
* Utilice la misma versión de la pila de la API de protección de datos a través de las aplicaciones. Realizar **cualquier** de las siguientes acciones en archivos de proyecto de las aplicaciones:
  * Hacer referencia a la misma versión del marco compartido a través de la [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app).
  * Hace referencia al mismo [paquete de protección de datos](xref:security/data-protection/introduction#package-layout) versión.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Puede tener un escenario donde no desea que una aplicación para implementar automáticamente las claves (y crear nuevas claves) como enfocar la expiración. Un ejemplo de esto podría ser configuradas en una relación principal/secundario, donde solo la aplicación principal es responsable de interés de administración de claves y las aplicaciones secundarias solo tienen una vista de solo lectura del anillo de clave de aplicaciones. Las aplicaciones secundarias pueden configurarse para tratar el conjunto de claves como de solo lectura mediante la configuración del sistema con <xref:Microsoft.AspNetCore.DataProtection.DataProtectionBuilderExtensions.DisableAutomaticKeyGeneration*>:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Aislamiento por aplicación

Cuando el sistema de protección de datos se proporciona un host de ASP.NET Core, aísla automáticamente las aplicaciones entre sí, incluso si esas aplicaciones se ejecutan en la misma cuenta de proceso de trabajo y están usando el mismo material de clave maestra. Esto es algo similar para el modificador IsolateApps desde de System.Web `<machineKey>` elemento.

El mecanismo de aislamiento se funciona teniendo en cuenta cada aplicación en el equipo local como un único inquilino, por lo tanto el <xref:Microsoft.AspNetCore.DataProtection.IDataProtector> ha modificado para cualquier aplicación incluye automáticamente el identificador de aplicación como discriminador. Identificador único de la aplicación es la ruta de acceso física de la aplicación:

* Para las aplicaciones hospedadas en [IIS](xref:fundamentals/servers/index#iis-http-server), el identificador único es la ruta de acceso física de IIS de la aplicación. Si una aplicación se implementa en un entorno de granja de servidores web, este valor es estable, suponiendo que los entornos de IIS se configuran de forma similar en todos los equipos en la granja de servidores web.
* Para aplicaciones autohospedadas que se ejecutan el [servidor Kestrel](xref:fundamentals/servers/index#kestrel), el identificador único es la ruta de acceso física a la aplicación en el disco.

El identificador único está diseñado para sobrevivir restablecimientos&mdash;tanto de la aplicación individual de la propia máquina.

Este mecanismo de aislamiento se da por supuesto que las aplicaciones no son malintencionadas. Una aplicación malintencionada siempre puede afectar a cualquier otra aplicación que se ejecutan en la misma cuenta de proceso de trabajo. En un entorno de hospedaje compartido donde las aplicaciones no son de confianza mutua, el proveedor de hospedaje debe tomar medidas para garantizar el aislamiento de nivel de sistema operativo entre aplicaciones, incluida la separación de las aplicaciones subyacentes de repositorios de claves.

Si el sistema de protección de datos no se proporciona un host de ASP.NET Core (por ejemplo, si crea instancias de él a través de la `DataProtectionProvider` tipo concreto) se deshabilita el aislamiento de la aplicación de forma predeterminada. Cuando se deshabilita el aislamiento de aplicaciones, todas las aplicaciones respaldadas por el mismo material de clave pueden compartir las cargas de siempre proporcionen adecuado [fines](xref:security/data-protection/consumer-apis/purpose-strings). Para proporcionar aislamiento de aplicaciones en este entorno, llame a la [SetApplicationName](#setapplicationname) método en la configuración de objetos y proporcione un nombre único para cada aplicación.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Cambio de los algoritmos con UseCryptographicAlgorithms

La pila de protección de datos permite cambiar el algoritmo predeterminado usado por las claves recién generadas. La manera más sencilla de hacerlo es llamar a [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) desde la devolución de llamada de configuración:

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

::: moniker-end

El algoritmo de cifrado predeterminada es AES-256-CBC y el valor predeterminado ValidationAlgorithm es HMACSHA256. La directiva predeterminada se puede establecer un administrador del sistema a través de un [todo el equipo directiva](xref:security/data-protection/configuration/machine-wide-policy), pero una llamada explícita a `UseCryptographicAlgorithms` invalida la directiva predeterminada.

Una llamada a `UseCryptographicAlgorithms` le permite especificar el algoritmo deseado de una lista predefinida integrada. No es necesario preocuparse por la implementación del algoritmo. En el escenario anterior, el sistema de protección de datos intenta usar la implementación de CNG de AES si se ejecuta en Windows. En caso contrario, recurre a los recursos administrados [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) clase.

Puede especificar manualmente una implementación a través de una llamada a [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Algoritmos de cambio no afecta a las claves existentes en el conjunto de claves. Solo afecta a las claves recién generadas.

### <a name="specifying-custom-managed-algorithms"></a>Especificación de algoritmos administrados personalizados

::: moniker range=">= aspnetcore-2.0"

Para especificar algoritmos administrados personalizados, cree un [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instancia que apunta a los tipos de implementación:

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para especificar algoritmos administrados personalizados, cree un [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instancia que apunta a los tipos de implementación:

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

::: moniker-end

Por lo general el \*propiedades de tipo deben apuntar a hormigón, implementaciones (a través de un constructor público sin parámetros) se pueden crear instancias de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) y [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), aunque el sistema casos especiales los algunos valores como `typeof(Aes)` para mayor comodidad.

> [!NOTE]
> El SymmetricAlgorithm debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 64 bits ≥ y debe admitir el cifrado de modo CBC con relleno PKCS #7. El KeyedHashAlgorithm debe tener un tamaño de la síntesis de > = 128 bits, y debe ser compatible con claves de una longitud igual a la longitud del resumen del algoritmo hash. El KeyedHashAlgorithm no es estrictamente necesaria para ser HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Especificar los algoritmos personalizados de CNG de Windows

::: moniker range=">= aspnetcore-2.0"

Para especificar un algoritmo personalizado de CNG de Windows con cifrado CBC-modo de validación de HMAC, cree un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instancia que contiene la información algorítmica:

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para especificar un algoritmo personalizado de CNG de Windows con cifrado CBC-modo de validación de HMAC, cree un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instancia que contiene la información algorítmica:

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

::: moniker-end

> [!NOTE]
> El algoritmo de cifrado de bloques simétricos debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de > = 64 bits, y debe admitir el cifrado de modo CBC con relleno PKCS #7. El algoritmo hash debe tener un tamaño de la síntesis de > = 128 bits y deben admitir que se abre con el BCRYPT\_ALG\_controlar\_HMAC\_indicador de marca. El \*las propiedades del proveedor se pueden establecer en null para usar el proveedor predeterminado para el algoritmo especificado. Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.

::: moniker range=">= aspnetcore-2.0"

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo contador de Galois/con la validación, cree un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instancia que contiene la información algorítmica:

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

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo contador de Galois/con la validación, cree un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instancia que contiene la información algorítmica:

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

::: moniker-end

> [!NOTE]
> El algoritmo de cifrado de bloques simétricos debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de 128 bits exactamente, y debe admitir el cifrado de GCM. Puede establecer el [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propiedad en null para usar el proveedor predeterminado para el algoritmo especificado. Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.

### <a name="specifying-other-custom-algorithms"></a>Especificar otros algoritmos personalizados

Aunque no se expone como una API de primera clase, el sistema de protección de datos es lo suficientemente extensible como para permitir la especificación de casi cualquier tipo de algoritmo. Por ejemplo, es posible mantener todas las claves contenidas dentro de un módulo de seguridad de Hardware (HSM) y para proporcionar una implementación personalizada de las principales rutinas de cifrado y descifrado. Consulte [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) en [principales de extensibilidad de criptografía](xref:security/data-protection/extensibility/core-crypto) para obtener más información.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Conservar las claves cuando se hospedan en un contenedor de Docker

Cuando se hospedan en un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenedor, las claves deben mantenerse en uno:

* Una carpeta que es un volumen de Docker que se conserva más allá de la duración del contenedor, como un volumen compartido o un volumen montado en host.
* Un proveedor externo, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/).

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/data-protection/configuration/non-di-scenarios>
* <xref:security/data-protection/configuration/machine-wide-policy>
* <xref:host-and-deploy/web-farm>
* <xref:security/data-protection/implementation/key-storage-providers>
