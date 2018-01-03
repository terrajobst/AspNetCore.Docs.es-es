---
title: "Configuración de protección de datos en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo configurar la protección de datos en ASP.NET Core."
keywords: "ASP.NET Core, protección de datos, configuración"
ms.author: riande
manager: wpickett
ms.date: 07/17/2017
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: 20e3d974e7790cd01f78f8db09225b5887f1772a
ms.sourcegitcommit: 281f0c614543a6c3db565ea4655b70fe49b61d84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2018
---
# <a name="configuring-data-protection-in-aspnet-core"></a>Configuración de protección de datos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Cuando se inicializa el sistema de protección de datos, se aplica [configuración predeterminada](xref:security/data-protection/configuration/default-settings) basado en el entorno operativo. Esta configuración es suelen ser adecuada para aplicaciones que se ejecutan en un único equipo. Hay casos donde un desarrollador puede cambiar la configuración predeterminada, es posible porque la aplicación se distribuye entre varias máquinas o por motivos de cumplimiento. En estos casos, el sistema de protección de datos ofrece una API enriquecida de configuración.

Hay un método de extensión [AddDataProtection](/dotnet/api/microsoft.extensions.dependencyinjection.dataprotectionservicecollectionextensions.adddataprotection) que devuelve un [IDataProtectionBuilder](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionbuilder). `IDataProtectionBuilder`expone métodos de extensión que se pueden encadenar juntos para configurar la protección de datos de opciones.

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
> Si cambia la ubicación de persistencia de clave, el sistema cifra ya no automáticamente claves en reposo, ya que no sabe si DPAPI es un mecanismo de cifrado adecuado.

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

Vea [clave de cifrado de datos almacenados](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más ejemplos e información sobre los mecanismos de cifrado de clave integrado.

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

De forma predeterminada, el sistema de protección de datos aísla aplicaciones entre sí, incluso si comparte el mismo repositorio clave físico. Esto evita que las aplicaciones de descripción de todas las demás cargas protegido. Para compartir protegidos cargas entre las dos aplicaciones, use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) con el mismo valor para cada aplicación:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("shared app name");
}
```

## <a name="disableautomatickeygeneration"></a>DisableAutomaticKeyGeneration

Puede que tenga un escenario donde no desea que una aplicación para revertir automáticamente las claves (crear nuevas claves) tal y como se aproximen a expiración. Un ejemplo de esto podría ser aplicaciones configuradas en una relación principal/secundario, donde solo la aplicación principal es responsable de problemas de administración de claves y aplicaciones secundarias solo tienen una vista de solo lectura del anillo de clave. Las aplicaciones secundarias pueden configurarse para tratar el anillo de clave como de solo lectura configurando el sistema con [DisableAutomaticKeyGeneration](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.disableautomatickeygeneration):

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

## <a name="per-application-isolation"></a>Aislamiento por aplicación

Cuando el sistema de protección de datos se proporciona un host de ASP.NET Core, aísla automáticamente aplicaciones entre sí, incluso si esas aplicaciones se ejecuten en la misma cuenta de proceso de trabajo y están usando el mismo material de clave maestro. Esto es similar al modificador IsolateApps de System.Web  **\<machineKey >** elemento.

El mecanismo de aislamiento funciona teniendo en cuenta cada aplicación en el equipo local como un único inquilino, por lo tanto la [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) para cualquier aplicación determinada incluye automáticamente el identificador de aplicación como un discriminador de la raíz. Identificador único de la aplicación procede de uno de estos dos lugares:

1. Si la aplicación se hospeda en IIS, el identificador único es la ruta de acceso de configuración de la aplicación. Si una aplicación se implementa en un entorno de granja de servidores web, este valor debería ser estable suponiendo que los entornos de IIS están configurados de forma similar en todos los equipos de la granja de servidores web.

2. Si la aplicación no está hospedada en IIS, el identificador único es la ruta de acceso física de la aplicación.

El identificador único está diseñado para sobrevivir restablece &mdash; tanto de la aplicación individual de la propia máquina.

Este mecanismo de aislamiento se da por supuesto que las aplicaciones no son malintencionadas. Una aplicación malintencionada siempre puede afectar a cualquier otra aplicación que se ejecuta en la misma cuenta de proceso de trabajo. En un entorno de hospedaje compartido donde las aplicaciones no son de confianza mutua, el proveedor de hospedaje debe tomar medidas para garantizar el aislamiento de nivel de sistema operativo entre aplicaciones, incluida la separación de las aplicaciones subyacentes repositorios de clave.

Si el sistema de protección de datos no se proporcionó un host de ASP.NET Core (por ejemplo, si crea instancias de él a través de la `DataProtectionProvider` tipo concreto) se deshabilita el aislamiento de la aplicación de forma predeterminada. Cuando se deshabilita el aislamiento de la aplicación, respaldadas por el mismo material de claves de todas las aplicaciones pueden compartir cargas mientras proporcionan adecuado [fines](xref:security/data-protection/consumer-apis/purpose-strings). Para proporcionar aislamiento de aplicaciones en este entorno, llame a la [SetApplicationName](#setapplicationname) método en la configuración de objeto y proporcione un nombre único para cada aplicación.

## <a name="changing-algorithms-with-usecryptographicalgorithms"></a>Cambiar algoritmos con UseCryptographicAlgorithms

La pila de protección de datos permite cambiar el algoritmo predeterminado usado por claves recién generado. La manera más sencilla de hacerlo consiste en llamar a [UseCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecryptographicalgorithms) de la devolución de llamada de configuración:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(
        new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

El algoritmo de cifrado predeterminada es AES-256-CBC y el valor predeterminado ValidationAlgorithm es HMACSHA256. La directiva predeterminada se puede establecer un administrador del sistema a través de un [todo el equipo directiva](xref:security/data-protection/configuration/machine-wide-policy), pero una llamada explícita a `UseCryptographicAlgorithms` invalida la directiva predeterminada.

Al llamar a `UseCryptographicAlgorithms` le permite especificar el algoritmo deseado de una lista predefinida de integrados. No tiene que preocuparse acerca de la implementación del algoritmo. En el escenario anterior, el sistema de protección de datos intenta usar la implementación de CNG de AES si se ejecuta en Windows. En caso contrario, vuelve a los recursos administrados [System.Security.Cryptography.Aes](/dotnet/api/system.security.cryptography.aes) clase.

Puede especificar manualmente una implementación a través de una llamada a [UseCustomCryptographicAlgorithms](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.usecustomcryptographicalgorithms).

> [!TIP]
> Algoritmos de cambio no afecta a las claves existentes en el anillo de clave. Solo afecta a las claves recién generado.

### <a name="specifying-custom-managed-algorithms"></a>Especificar algoritmos administrados personalizados

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar algoritmos administrados personalizados, cree una [ManagedAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.managedauthenticatedencryptorconfiguration) instancia que señala a los tipos de implementación:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar algoritmos administrados personalizados, cree una [ManagedAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.managedauthenticatedencryptionsettings) instancia que señala a los tipos de implementación:

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

Por lo general el \*propiedades de tipo deben apuntar a concreto, las implementaciones (a través de un constructor sin parámetros público) se pueden crear instancias de [SymmetricAlgorithm](/dotnet/api/system.security.cryptography.symmetricalgorithm) y [KeyedHashAlgorithm](/dotnet/api/system.security.cryptography.keyedhashalgorithm), aunque el casos de especial del sistema, como algunos valores `typeof(Aes)` para su comodidad.

> [!NOTE]
> El SymmetricAlgorithm debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 64 bits ≥ y debe admitir el cifrado del modo CBC con relleno PKCS #7. El KeyedHashAlgorithm debe tener un tamaño de síntesis de > = 128 bits, y debe ser compatible con claves de una longitud igual a la longitud de texto implícita del algoritmo de hash. El KeyedHashAlgorithm no es estrictamente necesaria para ser HMAC.

### <a name="specifying-custom-windows-cng-algorithms"></a>Especificar algoritmos personalizados de CNG de Windows

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo CBC con validación de HMAC, cree un [CngCbcAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration) instancia que contiene la información de algoritmo:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado de modo CBC con validación de HMAC, cree un [CngCbcAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cngcbcauthenticatedencryptionsettings) instancia que contiene la información de algoritmo:

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
> El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de > = 64 bits, y debe admitir el cifrado del modo CBC con relleno PKCS #7. El algoritmo hash debe tener un tamaño de síntesis de > = 128 bits y debe ser compatible con que se abre con la BCRYPT\_ALG\_controlar\_HMAC\_indicador de marca. El \*propiedades del proveedor se pueden establecer en null para utilizar el proveedor predeterminado para el algoritmo especificado. Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador con la validación, cree un [CngGcmAuthenticatedEncryptorConfiguration](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cnggcmauthenticatedencryptorconfiguration) instancia que contiene la información de algoritmo:

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador con la validación, cree un [CngGcmAuthenticatedEncryptionSettings](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.cnggcmauthenticatedencryptionsettings) instancia que contiene la información de algoritmo:

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
> El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de > = 128 bits, un tamaño de bloque de 128 bits exactamente, y debe ser compatible con cifrado de GCM. Puede establecer la [EncryptionAlgorithmProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.configurationmodel.cngcbcauthenticatedencryptorconfiguration.encryptionalgorithmprovider) propiedad en null para utilizar el proveedor predeterminado para el algoritmo especificado. Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.

### <a name="specifying-other-custom-algorithms"></a>Especificar otros algoritmos personalizados

Aunque no se expone como una API de primera clase, el sistema de protección de datos es lo suficientemente extensible para permitir la especificación de casi cualquier tipo de algoritmo. Por ejemplo, es posible mantener todas las claves contenidas dentro de un módulo de seguridad de Hardware (HSM) y para proporcionar una implementación personalizada del núcleo de rutinas de cifrado y descifrado. Vea [IAuthenticatedEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.authenticatedencryption.iauthenticatedencryptor) en [principales de extensibilidad de criptografía](xref:security/data-protection/extensibility/core-crypto) para obtener más información.

## <a name="persisting-keys-when-hosting-in-a-docker-container"></a>Claves de persistencia cuando se hospedan en un contenedor de Docker

Cuando se hospedan en un [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/) contenedor, las claves deben mantenerse en la vista:

* Una carpeta que es un volumen de Docker que se conserva más allá de la duración del contenedor, como un volumen compartido o un volumen montado de host.
* Un proveedor externo, como [el almacén de claves de Azure](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/).

## <a name="see-also"></a>Vea también

* [Escenarios no compatibles con DI](xref:security/data-protection/configuration/non-di-scenarios)
* [Directiva para toda la máquina](xref:security/data-protection/configuration/machine-wide-policy)
