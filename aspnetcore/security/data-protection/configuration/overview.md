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
ms.openlocfilehash: 9361dcec89a0f35067181523cc56637d629614ff
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="configuring-data-protection"></a><span data-ttu-id="9bdb5-103">Configurar la protección de datos</span><span class="sxs-lookup"><span data-stu-id="9bdb5-103">Configuring data protection</span></span>

<a name=data-protection-configuring></a>

<span data-ttu-id="9bdb5-104">Cuando se inicializa el sistema de protección de datos aplica algunos [configuración predeterminada](default-settings.md#data-protection-default-settings) basado en el entorno operativo.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-104">When the data protection system is initialized it applies some [default settings](default-settings.md#data-protection-default-settings) based on the operational environment.</span></span> <span data-ttu-id="9bdb5-105">Esta configuración es normalmente adecuada para aplicaciones que se ejecutan en un único equipo.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-105">These settings are generally good for applications running on a single machine.</span></span> <span data-ttu-id="9bdb5-106">Hay algunos casos donde un programador podría desear cambiar estas (quizás porque su aplicación se distribuye entre varias máquinas o por motivos de cumplimiento), y para estos escenarios, el sistema de protección de datos ofrece una API de configuración completo.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-106">There are some cases where a developer may want to change these (perhaps because their application is spread across multiple machines or for compliance reasons), and for these scenarios the data protection system offers a rich configuration API.</span></span>

<a name=data-protection-configuration-callback></a>

<span data-ttu-id="9bdb5-107">Hay un método de extensión AddDataProtection que devuelve un IDataProtectionBuilder que a su vez expone métodos de extensión que se pueden encadenar juntos para configurar la protección de datos de varias opciones.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-107">There is an extension method AddDataProtection which returns an IDataProtectionBuilder which itself exposes extension methods that you can chain together to configure various data protection options.</span></span> <span data-ttu-id="9bdb5-108">Por ejemplo, para almacenar las claves en un recurso compartido UNC en lugar de % LOCALAPPDATA % (predeterminado), configure el sistema de la manera siguiente:</span><span class="sxs-lookup"><span data-stu-id="9bdb5-108">For instance, to store keys at a UNC share instead of %LOCALAPPDATA% (the default), configure the system as follows:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"));
}
```

>[!WARNING]
> <span data-ttu-id="9bdb5-109">Si cambia la ubicación de la clave de persistencia, el sistema, ya no se cifrarán automáticamente claves en reposo ya que no sabe si DPAPI es un mecanismo de cifrado adecuado.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-109">If you change the key persistence location, the system will no longer automatically encrypt keys at rest since it doesn't know whether DPAPI is an appropriate encryption mechanism.</span></span>

<a name=configuring-x509-certificate></a>

<span data-ttu-id="9bdb5-110">Puede configurar el sistema para proteger las claves en reposo mediante una llamada a cualquiera de lo ProtectKeysWith\* API de configuración.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-110">You can configure the system to protect keys at rest by calling any of the ProtectKeysWith\* configuration APIs.</span></span> <span data-ttu-id="9bdb5-111">Considere el ejemplo siguiente, que almacena las claves en un recurso compartido UNC y cifra estas claves en reposo con un certificado X.509 concreto.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-111">Consider the example below, which stores keys at a UNC share and encrypts those keys at rest with a specific X.509 certificate.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\directory\"))
        .ProtectKeysWithCertificate("thumbprint");
}
```

<span data-ttu-id="9bdb5-112">Vea [clave cifrado en reposo](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obtener más ejemplos y para información sobre los mecanismos de cifrado de clave integrada.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-112">See [key encryption at rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more examples and for discussion on the built-in key encryption mechanisms.</span></span>

<span data-ttu-id="9bdb5-113">Para configurar el sistema para usar una vigencia de clave del valor predeterminado de 14 días en lugar de 90 días, considere el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9bdb5-113">To configure the system to use a default key lifetime of 14 days instead of 90 days, consider the following example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
}
```

<span data-ttu-id="9bdb5-114">De forma predeterminada el sistema de protección de datos aísla las aplicaciones entre sí, incluso si comparte el mismo repositorio clave físico.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-114">By default the data protection system isolates applications from one another, even if they're sharing the same physical key repository.</span></span> <span data-ttu-id="9bdb5-115">Esto evita que las aplicaciones de la descripción de todas las demás cargas protegido.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-115">This prevents the applications from understanding each other's protected payloads.</span></span> <span data-ttu-id="9bdb5-116">Para compartir protegidos cargas entre dos aplicaciones diferentes, configurar el sistema pasa en el mismo nombre de aplicación para las aplicaciones como en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="9bdb5-116">To share protected payloads between two different applications, configure the system passing in the same application name for both applications as in the below example:</span></span>

<a name=data-protection-code-sample-application-name></a>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .SetApplicationName("my application");
}
```

<a name=data-protection-configuring-disable-automatic-key-generation></a>

<span data-ttu-id="9bdb5-117">Por último, puede que tenga un escenario donde no desea que una aplicación revertir automáticamente las claves de medida que se acercan expiración.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-117">Finally, you may have a scenario where you do not want an application to automatically roll keys as they approach expiration.</span></span> <span data-ttu-id="9bdb5-118">Un ejemplo de esto podría ser aplicaciones configuradas en una relación primaria / secundaria, donde solo la aplicación principal es responsable de problemas de administración de claves y todas las aplicaciones secundarias solo tienen una vista de solo lectura del anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-118">One example of this might be applications set up in a primary / secondary relationship, where only the primary application is responsible for key management concerns, and all secondary applications simply have a read-only view of the key ring.</span></span> <span data-ttu-id="9bdb5-119">Las aplicaciones secundarias pueden configurarse para que trate el anillo de clave como de solo lectura mediante la configuración del sistema como sigue:</span><span class="sxs-lookup"><span data-stu-id="9bdb5-119">The secondary applications can be configured to treat the key ring as read-only by configuring the system as below:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDataProtection()
        .DisableAutomaticKeyGeneration();
}
```

<a name=data-protection-configuration-per-app-isolation></a>

## <a name="per-application-isolation"></a><span data-ttu-id="9bdb5-120">Aislamiento por aplicación</span><span class="sxs-lookup"><span data-stu-id="9bdb5-120">Per-application isolation</span></span>

<span data-ttu-id="9bdb5-121">Cuando el sistema de protección de datos se proporciona un host de ASP.NET Core, automáticamente se aislar aplicaciones entre sí, incluso si esas aplicaciones se ejecuten en la misma cuenta de proceso de trabajo y están usando el mismo material de clave maestro.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-121">When the data protection system is provided by an ASP.NET Core host, it will automatically isolate applications from one another, even if those applications are running under the same worker process account and are using the same master keying material.</span></span> <span data-ttu-id="9bdb5-122">Esto es similar al modificador IsolateApps de System.Web <machineKey> elemento.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-122">This is somewhat similar to the IsolateApps modifier from System.Web's <machineKey> element.</span></span>

<span data-ttu-id="9bdb5-123">El mecanismo de aislamiento funciona teniendo en cuenta cada aplicación en el equipo local como un único inquilino, por lo tanto la IDataProtector automáticamente la raíz para cualquier aplicación determinada incluye el identificador de aplicación como un discriminador.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-123">The isolation mechanism works by considering each application on the local machine as a unique tenant, thus the IDataProtector rooted for any given application automatically includes the application ID as a discriminator.</span></span> <span data-ttu-id="9bdb5-124">Identificador único de la aplicación procede de uno de estos dos lugares.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-124">The application's unique ID comes from one of two places.</span></span>

1. <span data-ttu-id="9bdb5-125">Si la aplicación se hospeda en IIS, el identificador único es la ruta de acceso de configuración de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-125">If the application is hosted in IIS, the unique identifier is the application's configuration path.</span></span> <span data-ttu-id="9bdb5-126">Si una aplicación se implementa en un entorno de granja de servidores, este valor debería ser estable suponiendo que los entornos de IIS están configurados de forma similar en todos los equipos de la granja.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-126">If an application is deployed in a farm environment, this value should be stable assuming that the IIS environments are configured similarly across all machines in the farm.</span></span>

2. <span data-ttu-id="9bdb5-127">Si la aplicación no se hospeda en IIS, el identificador único es la ruta de acceso física de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-127">If the application is not hosted in IIS, the unique identifier is the physical path of the application.</span></span>

<span data-ttu-id="9bdb5-128">El identificador único está diseñado para sobrevivir restablece - tanto de la aplicación individual de la propia máquina.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-128">The unique identifier is designed to survive resets - both of the individual application and of the machine itself.</span></span>

<span data-ttu-id="9bdb5-129">Este mecanismo de aislamiento se da por supuesto que las aplicaciones no son malintencionadas.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-129">This isolation mechanism assumes that the applications are not malicious.</span></span> <span data-ttu-id="9bdb5-130">Una aplicación malintencionada siempre puede afectar a cualquier otra aplicación que se ejecuta en la misma cuenta de proceso de trabajo.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-130">A malicious application can always impact any other application running under the same worker process account.</span></span> <span data-ttu-id="9bdb5-131">En un entorno de hospedaje compartido donde las aplicaciones no son de confianza mutua, el proveedor de hospedaje debe tomar medidas para garantizar el aislamiento de nivel de sistema operativo entre aplicaciones, incluida la separación de las aplicaciones subyacentes repositorios de clave.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-131">In a shared hosting environment where applications are mutually untrusted, the hosting provider should take steps to ensure OS-level isolation between applications, including separating the applications' underlying key repositories.</span></span>

<span data-ttu-id="9bdb5-132">Si el sistema de protección de datos no se proporciona un host de ASP.NET Core (por ejemplo, si el desarrollador crea instancias de él a sí mismo a través del tipo concreto de DataProtectionProvider), aislamiento de la aplicación está deshabilitado de forma predeterminada y todas las aplicaciones respaldado por la misma clave material puede compartir cargas siempre que proporcionan los fines adecuados.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-132">If the data protection system is not provided by an ASP.NET Core host (e.g., if the developer instantiates it himself via the DataProtectionProvider concrete type), application isolation is disabled by default, and all applications backed by the same keying material can share payloads as long as they provide the appropriate purposes.</span></span> <span data-ttu-id="9bdb5-133">Para proporcionar aislamiento de aplicaciones en este entorno, llame al método SetApplicationName en el objeto de configuración, consulte la [ejemplo de código](#data-protection-code-sample-application-name) anteriormente.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-133">To provide application isolation in this environment, call the SetApplicationName method on the configuration object, see the [code sample](#data-protection-code-sample-application-name) above.</span></span>

<a name=data-protection-changing-algorithms></a>

## <a name="changing-algorithms"></a><span data-ttu-id="9bdb5-134">Cambio de algoritmos</span><span class="sxs-lookup"><span data-stu-id="9bdb5-134">Changing algorithms</span></span>

<span data-ttu-id="9bdb5-135">La pila de protección de datos permite cambiar el algoritmo predeterminado usado por claves recién generado.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-135">The data protection stack allows changing the default algorithm used by newly-generated keys.</span></span> <span data-ttu-id="9bdb5-136">La manera más sencilla de hacerlo consiste en llamar a UseCryptographicAlgorithms desde la devolución de llamada de configuración, como en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-136">The simplest way to do this is to call UseCryptographicAlgorithms from the configuration callback, as in the below example.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9bdb5-137">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-137">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptorConfiguration()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9bdb5-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
services.AddDataProtection()
    .UseCryptographicAlgorithms(new AuthenticatedEncryptionSettings()
    {
        EncryptionAlgorithm = EncryptionAlgorithm.AES_256_CBC,
        ValidationAlgorithm = ValidationAlgorithm.HMACSHA256
    });
```

---

<span data-ttu-id="9bdb5-139">El algoritmo de cifrado predeterminado y ValidationAlgorithm son AES-256-CBC y HMACSHA256, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-139">The default EncryptionAlgorithm and ValidationAlgorithm are AES-256-CBC and HMACSHA256, respectively.</span></span> <span data-ttu-id="9bdb5-140">La directiva predeterminada se puede establecer un administrador del sistema a través de [directiva para todo el equipo](machine-wide-policy.md), pero una llamada explícita a UseCryptographicAlgorithms invalidará la directiva predeterminada.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-140">The default policy can be set by a system administrator via [Machine Wide Policy](machine-wide-policy.md), but an explicit call to UseCryptographicAlgorithms will override the default policy.</span></span>

<span data-ttu-id="9bdb5-141">Al llamar a UseCryptographicAlgorithms permitirá al programador especificar el algoritmo deseado (de una lista predefinida de integradas), y el programador no tiene que preocuparse sobre la implementación del algoritmo.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-141">Calling UseCryptographicAlgorithms will allow the developer to specify the desired algorithm (from a predefined built-in list), and the developer does not need to worry about the implementation of the algorithm.</span></span> <span data-ttu-id="9bdb5-142">Por ejemplo, en el escenario anterior, el sistema de protección de datos intentará usar la implementación de CNG de AES si se ejecuta en Windows, en caso contrario, se revertirá a la clase System.Security.Cryptography.Aes administrada.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-142">For instance, in the scenario above the data protection system will attempt to use the CNG implementation of AES if running on Windows, otherwise it will fall back to the managed System.Security.Cryptography.Aes class.</span></span>

<span data-ttu-id="9bdb5-143">El desarrollador puede especificar una implementación manualmente si lo desea mediante una llamada a UseCustomCryptographicAlgorithms, como se muestra en los ejemplos a continuación.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-143">The developer can manually specify an implementation if desired via a call to UseCustomCryptographicAlgorithms, as show in the below examples.</span></span>

>[!TIP]
> <span data-ttu-id="9bdb5-144">Algoritmos de cambio no afecta a las claves existentes en el anillo de clave.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-144">Changing algorithms does not affect existing keys in the key ring.</span></span> <span data-ttu-id="9bdb5-145">Solo afecta a las claves recién generado.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-145">It only affects newly-generated keys.</span></span>

<a name=data-protection-changing-algorithms-custom-managed></a>

### <a name="specifying-custom-managed-algorithms"></a><span data-ttu-id="9bdb5-146">Especificar algoritmos administrados personalizados</span><span class="sxs-lookup"><span data-stu-id="9bdb5-146">Specifying custom managed algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9bdb5-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9bdb5-148">Para especificar algoritmos administrados personalizados, cree una instancia de ManagedAuthenticatedEncryptorConfiguration que apunta a los tipos de implementación.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-148">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptorConfiguration instance that points to the implementation types.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9bdb5-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9bdb5-150">Para especificar algoritmos administrados personalizados, cree una instancia de ManagedAuthenticatedEncryptionSettings que apunta a los tipos de implementación.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-150">To specify custom managed algorithms, create a ManagedAuthenticatedEncryptionSettings instance that points to the implementation types.</span></span>

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

<span data-ttu-id="9bdb5-151">Por lo general el \*propiedades de tipo deben apuntar a concretas, implementaciones (a través de un constructor sin parámetros público) se pueden crear instancias de SymmetricAlgorithm y KeyedHashAlgorithm, aunque algunos valores de los casos especiales de sistema como typeof(Aes) para mayor comodidad .</span><span class="sxs-lookup"><span data-stu-id="9bdb5-151">Generally the \*Type properties must point to concrete, instantiable (via a public parameterless ctor) implementations of SymmetricAlgorithm and KeyedHashAlgorithm, though the system special-cases some values like typeof(Aes) for convenience.</span></span>

> [!NOTE]
> <span data-ttu-id="9bdb5-152">El SymmetricAlgorithm debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 64 bits ≥ y debe admitir el cifrado del modo CBC con relleno PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-152">The SymmetricAlgorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="9bdb5-153">El KeyedHashAlgorithm debe tener un tamaño de síntesis de > = 128 bits, y debe ser compatible con claves de una longitud igual a la longitud de texto implícita del algoritmo de hash.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-153">The KeyedHashAlgorithm must have a digest size of >= 128 bits, and it must support keys of length equal to the hash algorithm's digest length.</span></span> <span data-ttu-id="9bdb5-154">El KeyedHashAlgorithm no es estrictamente necesaria para ser HMAC.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-154">The KeyedHashAlgorithm is not strictly required to be HMAC.</span></span>

<a name=data-protection-changing-algorithms-cng></a>

### <a name="specifying-custom-windows-cng-algorithms"></a><span data-ttu-id="9bdb5-155">Especificar algoritmos personalizados de CNG de Windows</span><span class="sxs-lookup"><span data-stu-id="9bdb5-155">Specifying custom Windows CNG algorithms</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9bdb5-156">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-156">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9bdb5-157">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo CBC + validación HMAC, cree una instancia de CngCbcAuthenticatedEncryptorConfiguration que contiene la información algorítmica.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-157">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9bdb5-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9bdb5-159">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo CBC + validación HMAC, cree una instancia de CngCbcAuthenticatedEncryptionSettings que contiene la información algorítmica.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-159">To specify a custom Windows CNG algorithm using CBC-mode encryption + HMAC validation, create a CngCbcAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="9bdb5-160">El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de ≥ 64 bits y debe admitir el cifrado del modo CBC con relleno PKCS #7.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-160">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of ≥ 64 bits, and it must support CBC-mode encryption with PKCS #7 padding.</span></span> <span data-ttu-id="9bdb5-161">El algoritmo hash debe tener un tamaño de síntesis de > = 128 bits y debe ser compatible con que se abre con la marca BCRYPT_ALG_HANDLE_HMAC_FLAG.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-161">The hash algorithm must have a digest size of >= 128 bits and must support being opened with the BCRYPT_ALG_HANDLE_HMAC_FLAG flag.</span></span> <span data-ttu-id="9bdb5-162">El \*propiedades del proveedor se pueden establecer en null para utilizar el proveedor predeterminado para el algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-162">The \*Provider properties can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="9bdb5-163">Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-163">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9bdb5-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9bdb5-165">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador + validación, cree una instancia de CngGcmAuthenticatedEncryptorConfiguration que contiene la información algorítmica.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-165">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptorConfiguration instance that contains the algorithmic information.</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9bdb5-166">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9bdb5-166">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9bdb5-167">Para especificar un algoritmo CNG de Windows personalizado mediante el cifrado del modo de Galois/contador + validación, cree una instancia de CngGcmAuthenticatedEncryptionSettings que contiene la información algorítmica.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-167">To specify a custom Windows CNG algorithm using Galois/Counter Mode encryption + validation, create a CngGcmAuthenticatedEncryptionSettings instance that contains the algorithmic information.</span></span>

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
> <span data-ttu-id="9bdb5-168">El algoritmo de cifrado de bloques simétrico debe tener una longitud de clave de 128 bits ≥ y un tamaño de bloque de 128 bits exactamente, y debe ser compatible con cifrado de GCM.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-168">The symmetric block cipher algorithm must have a key length of ≥ 128 bits and a block size of exactly 128 bits, and it must support GCM encryption.</span></span> <span data-ttu-id="9bdb5-169">La propiedad EncryptionAlgorithmProvider puede establecerse en null para utilizar el proveedor predeterminado para el algoritmo especificado.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-169">The EncryptionAlgorithmProvider property can be set to null to use the default provider for the specified algorithm.</span></span> <span data-ttu-id="9bdb5-170">Consulte la [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentación para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-170">See the [BCryptOpenAlgorithmProvider](https://msdn.microsoft.com/library/windows/desktop/aa375479(v=vs.85).aspx) documentation for more information.</span></span>

### <a name="specifying-other-custom-algorithms"></a><span data-ttu-id="9bdb5-171">Especificar otros algoritmos personalizados</span><span class="sxs-lookup"><span data-stu-id="9bdb5-171">Specifying other custom algorithms</span></span>

<span data-ttu-id="9bdb5-172">Aunque no se expone como una API de primera clase, el sistema de protección de datos es lo suficientemente extensible para permitir la especificación de casi cualquier tipo de algoritmo.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-172">Though not exposed as a first-class API, the data protection system is extensible enough to allow specifying almost any kind of algorithm.</span></span> <span data-ttu-id="9bdb5-173">Por ejemplo, es posible mantener todas las claves contenidas dentro de un HSM y para proporcionar una implementación personalizada del núcleo de rutinas de cifrado y descifrado.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-173">For example, it is possible to keep all keys contained within an HSM and to provide a custom implementation of the core encryption and decryption routines.</span></span> <span data-ttu-id="9bdb5-174">Consulte IAuthenticatedEncryptorConfiguration en la sección de extensibilidad de criptografía de núcleo para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="9bdb5-174">See IAuthenticatedEncryptorConfiguration in the core cryptography extensibility section for more information.</span></span>

### <a name="see-also"></a><span data-ttu-id="9bdb5-175">Vea también</span><span class="sxs-lookup"><span data-stu-id="9bdb5-175">See also</span></span>

* [<span data-ttu-id="9bdb5-176">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="9bdb5-176">Non DI Aware Scenarios</span></span>](non-di-scenarios.md)
* [<span data-ttu-id="9bdb5-177">Directiva para toda la máquina</span><span class="sxs-lookup"><span data-stu-id="9bdb5-177">Machine Wide Policy</span></span>](machine-wide-policy.md)
