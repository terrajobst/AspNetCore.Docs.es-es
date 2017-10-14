---
title: "Configurar la protección de datos"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0e4881a3-a94d-4e35-9c1c-f025d65dcff0
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/overview
ms.openlocfilehash: d35e0e806999ffd2e0f8f82e0adfc940ea2b503d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="configuring-data-protection"></a>Configurar la protección de datos

<a name="data-protection-configuring"></a>

Cuando se inicializa el sistema de protección de datos aplica algunos [configuración predeterminada](default-settings.md#data-protection-default-settings) basado en el entorno operativo. Esta configuración es normalmente adecuada para aplicaciones que se ejecutan en un único equipo. Hay algunos casos donde un programador podría desear cambiar estas (quizás porque su aplicación se distribuye entre varias máquinas o por motivos de cumplimiento), y para estos escenarios, el sistema de protección de datos ofrece una API de configuración completo.

<a name="data-protection-configuration-callback"></a>

Hay un método de extensión AddDataProtection que devuelve un IDataProtectionBuilder que a su vez expone métodos de extensión que se pueden encadenar juntos para configurar la protección de datos de varias opciones. Por ejemplo, para almacenar las claves en un recurso compartido UNC en lugar de % LOCALAPPDATA % (predeterminado), configure el sistema de la manera siguiente:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> Si cambia la ubicación de la clave de persistencia, el sistema, ya no se cifrarán automáticamente claves en reposo ya que no sabe si DPAPI es un mecanismo de cifrado adecuado.

<a name="configuring-x509-certificate"></a>

Puede configurar el sistema para proteger las claves en reposo mediante una llamada a cualquiera de lo ProtectKeysWith\* API de configuración. Considere el ejemplo siguiente, que almacena las claves en un recurso compartido UNC y cifra estas claves en reposo con un certificado X.509 concreto.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

Vea [clave cifrado en reposo](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obtener más ejemplos y para información sobre los mecanismos de cifrado de clave integrada.

Para configurar el sistema para usar una vigencia de clave del valor predeterminado de 14 días en lugar de 90 días, considere el ejemplo siguiente:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

De forma predeterminada el sistema de protección de datos aísla las aplicaciones entre sí, incluso si comparte el mismo repositorio clave físico. Esto evita que las aplicaciones de la descripción de todas las demás cargas protegido. Para compartir protegidos cargas entre dos aplicaciones diferentes, configurar el sistema pasa en el mismo nombre de aplicación para las aplicaciones como en el ejemplo siguiente:

<a name="data-protection-code-sample-application-name"></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name="data-protection-configuring-disable-automatic-key-generation"></a>

Por último, puede que tenga un escenario donde no desea que una aplicación revertir automáticamente las claves de medida que se acercan expiración. Un ejemplo de esto podría ser aplicaciones configuradas en una relación primaria / secundaria, donde solo la aplicación principal es responsable de problemas de administración de claves y todas las aplicaciones secundarias solo tienen una vista de solo lectura del anillo de clave. Las aplicaciones secundarias pueden configurarse para que trate el anillo de clave como de solo lectura mediante la configuración del sistema como sigue:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name="data-protection-configuration-per-app-isolation"></a>

## <a name="per-application-isolation"></a>Aislamiento por aplicación

Cuando el sistema de protección de datos se proporciona un host de ASP.NET Core, automáticamente se aislar aplicaciones entre sí, incluso si esas aplicaciones se ejecuten en la misma cuenta de proceso de trabajo y están usando el mismo material de clave maestro. Esto es similar al modificador IsolateApps de System.Web <machineKey> elemento.

El mecanismo de aislamiento funciona teniendo en cuenta cada aplicación en el equipo local como un único inquilino, por lo tanto la IDataProtector automáticamente la raíz para cualquier aplicación determinada incluye el identificador de aplicación como un discriminador. Identificador único de la aplicación procede de uno de estos dos lugares.

1. Si la aplicación se hospeda en IIS, el identificador único es la ruta de acceso de configuración de la aplicación. Si una aplicación se implementa en un entorno de granja de servidores, este valor debería ser estable suponiendo que los entornos de IIS están configurados de forma similar en todos los equipos de la granja.

2. Si la aplicación no se hospeda en IIS, el identificador único es la ruta de acceso física de la aplicación.

El identificador único está diseñado para sobrevivir restablece - tanto de la aplicación individual de la propia máquina.

Este mecanismo de aislamiento se da por supuesto que las aplicaciones no son malintencionadas. Una aplicación malintencionada siempre puede afectar a cualquier otra aplicación que se ejecuta en la misma cuenta de proceso de trabajo. En un entorno de hospedaje compartido donde las aplicaciones no son de confianza mutua, el proveedor de hospedaje debe tomar medidas para garantizar el aislamiento de nivel de sistema operativo entre aplicaciones, incluida la separación de las aplicaciones subyacentes repositorios de clave.

Si el sistema de protección de datos no se proporciona un host de ASP.NET Core (por ejemplo, si el desarrollador crea instancias de él a sí mismo a través del tipo concreto de DataProtectionProvider), aislamiento de la aplicación está deshabilitado de forma predeterminada y todas las aplicaciones respaldado por la misma clave material puede compartir cargas siempre que proporcionan los fines adecuados. Para proporcionar aislamiento de aplicaciones en este entorno, llame al método SetApplicationName en el objeto de configuración, consulte la [ejemplo de código](#data-protection-code-sample-application-name) anteriormente.

<a name="data-protection-changing-algorithms"></a>

## <a name="changing-algorithms"></a>Cambio de algoritmos

La pila de protección de datos permite cambiar el algoritmo predeterminado usado por claves recién generado. La manera más sencilla de hacerlo consiste en llamar a UseCryptographicAlgorithms desde la devolución de llamada de configuración, como en el ejemplo siguiente.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

El algoritmo de cifrado predeterminado y ValidationAlgorithm son AES-256-CBC y HMACSHA256, respectivamente. La directiva predeterminada se puede establecer un administrador del sistema a través de [directiva para todo el equipo](machine-wide-policy.md), pero una llamada explícita a UseCryptographicAlgorithms invalidará la directiva predeterminada.

Al llamar a UseCryptographicAlgorithms permitirá al programador especificar el algoritmo deseado (de una lista predefinida de integradas), y el programador no tiene que preocuparse sobre la implementación del algoritmo. Por ejemplo, en el escenario anterior, el sistema de protección de datos intentará usar la implementación de CNG de AES si se ejecuta en Windows, en caso contrario, se revertirá a la clase System.Security.Cryptography.Aes administrada.

El desarrollador puede especificar una implementación manualmente si lo desea mediante una llamada a UseCustomCryptographicAlgorithms, como se muestra en los ejemplos a continuación.

>[!TIP]
> Algoritmos de cambio no afecta a las claves existentes en el anillo de clave. Solo afecta a las claves recién generado.

<a name="data-protection-changing-algorithms-custom-managed"></a>

### <a name="specifying-custom-managed-algorithms"></a>Especificar algoritmos administrados personalizados

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar algoritmos administrados personalizados, cree una instancia de ManagedAuthenticatedEncryptorConfiguration que apunta a los tipos de implementación.

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptorConfiguration()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar algoritmos administrados personalizados, cree una instancia de ManagedAuthenticatedEncryptionSettings que apunta a los tipos de implementación.

```csharp
serviceCollection.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new ManagedAuthenticatedEncryptionSettings()
    {
        // a type that subclasses SymmetricAlgorithm
        EncryptionAlgorithmType = typeof(Aes),

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // a type that subclasses KeyedHashAlgorithm
        ValidationAlgorithmType = typeof(HMACSHA256)
    });
```

---

Por lo general el \*propiedades de tipo deben apuntar a concretas, implementaciones (a través de un constructor sin parámetros público) se pueden crear instancias de SymmetricAlgorithm y KeyedHashAlgorithm, aunque algunos valores de los casos especiales de sistema como typeof(Aes) para mayor comodidad .

> [!NOTE]
> El SymmetricAlgorithm debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 64 bits ≥ y debe admitir el cifrado del modo CBC con relleno PKCS #7. El KeyedHashAlgorithm debe tener un tamaño de síntesis de > = 128 bits, y debe ser compatible con claves de una longitud igual a la longitud de texto implícita del algoritmo de hash. El KeyedHashAlgorithm no es estrictamente necesaria para ser HMAC.

<a name="data-protection-changing-algorithms-cng"></a>

### <a name="specifying-custom-windows-cng-algorithms"></a>Especificar algoritmos personalizados de CNG de Windows

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo CBC + validación HMAC, cree una instancia de CngCbcAuthenticatedEncryptorConfiguration que contiene la información algorítmica.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo CBC + validación HMAC, cree una instancia de CngCbcAuthenticatedEncryptionSettings que contiene la información algorítmica.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngCbcAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256,

        // passed to BCryptOpenAlgorithmProvider
        HashAlgorithm = "SHA256",
        HashAlgorithmProvider = null
    });
```

---

> [!NOTE]
> El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de ≥ 64 bits y debe admitir el cifrado del modo CBC con relleno PKCS #7. El algoritmo hash debe tener un tamaño de síntesis de > = 128 bits y debe ser compatible con que se abre con la marca BCRYPT_ALG_HANDLE_HMAC_FLAG. El \*propiedades del proveedor se pueden establecer en null para utilizar el proveedor predeterminado para el algoritmo especificado. Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador + validación, cree una instancia de CngGcmAuthenticatedEncryptorConfiguration que contiene la información algorítmica.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptorConfiguration()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador + validación, cree una instancia de CngGcmAuthenticatedEncryptionSettings que contiene la información algorítmica.

```csharp
services.AddDataProtection()
    .UseCustomCryptographicAlgorithms(new CngGcmAuthenticatedEncryptionSettings()
    {
        // passed to BCryptOpenAlgorithmProvider
        EncryptionAlgorithm = "AES",
        EncryptionAlgorithmProvider = null,

        // specified in bits
        EncryptionAlgorithmKeySize = 256
    });
```

---

> [!NOTE]
> El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 128 bits exactamente, y debe ser compatible con cifrado de GCM. La propiedad EncryptionAlgorithmProvider puede establecerse en null para utilizar el proveedor predeterminado para el algoritmo especificado. Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.

### <a name="specifying-other-custom-algorithms"></a>Especificar otros algoritmos personalizados

Aunque no se expone como una API de primera clase, el sistema de protección de datos es lo suficientemente extensible para permitir la especificación de casi cualquier tipo de algoritmo. Por ejemplo, es posible mantener todas las claves contenidas dentro de un HSM y para proporcionar una implementación personalizada del núcleo de rutinas de cifrado y descifrado. Consulte IAuthenticatedEncryptorConfiguration en la sección de extensibilidad de criptografía de núcleo para obtener más información.

### <a name="see-also"></a>Vea también

* [Escenarios no compatibles con DI](non-di-scenarios.md)
* [Directiva para toda la máquina](machine-wide-policy.md)
