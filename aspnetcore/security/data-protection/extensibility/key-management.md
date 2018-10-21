---
title: Extensibilidad de administración de claves en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de la extensibilidad de administración de claves de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 11/22/2017
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: b52212ff3462748a5c64f21e1b7854673e5fcadc
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477467"
---
# <a name="key-management-extensibility-in-aspnet-core"></a><span data-ttu-id="44102-103">Extensibilidad de administración de claves en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="44102-103">Key management extensibility in ASP.NET Core</span></span>

> [!TIP]
> <span data-ttu-id="44102-104">Leer el [administración de claves](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sección antes de leer esta sección, ya que explica algunos de los conceptos fundamentales de estas API.</span><span class="sxs-lookup"><span data-stu-id="44102-104">Read the [key management](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

> [!WARNING]
> <span data-ttu-id="44102-105">Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.</span><span class="sxs-lookup"><span data-stu-id="44102-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="44102-106">Key</span><span class="sxs-lookup"><span data-stu-id="44102-106">Key</span></span>

<span data-ttu-id="44102-107">El `IKey` interfaz es la representación básica de una clave en los sistemas de cifrado.</span><span class="sxs-lookup"><span data-stu-id="44102-107">The `IKey` interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="44102-108">La clave de término se utiliza aquí en el sentido abstracto, no en el sentido de "material criptográfico clave" literal.</span><span class="sxs-lookup"><span data-stu-id="44102-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="44102-109">Una clave tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="44102-109">A key has the following properties:</span></span>

* <span data-ttu-id="44102-110">Fechas de expiración, la creación y activación</span><span class="sxs-lookup"><span data-stu-id="44102-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="44102-111">Estado de revocación</span><span class="sxs-lookup"><span data-stu-id="44102-111">Revocation status</span></span>

* <span data-ttu-id="44102-112">Identificador de clave (GUID)</span><span class="sxs-lookup"><span data-stu-id="44102-112">Key identifier (a GUID)</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="44102-113">Además, `IKey` expone un `CreateEncryptor` método que se puede usar para crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.</span><span class="sxs-lookup"><span data-stu-id="44102-113">Additionally, `IKey` exposes a `CreateEncryptor` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="44102-114">Además, `IKey` expone un `CreateEncryptorInstance` método que se puede usar para crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.</span><span class="sxs-lookup"><span data-stu-id="44102-114">Additionally, `IKey` exposes a `CreateEncryptorInstance` method which can be used to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

::: moniker-end

> [!NOTE]
> <span data-ttu-id="44102-115">No hay ninguna API para recuperar el material criptográfico sin procesar de un `IKey` instancia.</span><span class="sxs-lookup"><span data-stu-id="44102-115">There's no API to retrieve the raw cryptographic material from an `IKey` instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="44102-116">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="44102-116">IKeyManager</span></span>

<span data-ttu-id="44102-117">El `IKeyManager` interfaz representa un objeto responsable de almacenamiento de claves general, la recuperación y manipulación.</span><span class="sxs-lookup"><span data-stu-id="44102-117">The `IKeyManager` interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="44102-118">Expone tres operaciones de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="44102-118">It exposes three high-level operations:</span></span>

* <span data-ttu-id="44102-119">Cree una nueva clave y enviarlo al almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="44102-119">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="44102-120">Obtener todas las claves de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="44102-120">Get all keys from storage.</span></span>

* <span data-ttu-id="44102-121">Revocar una o varias claves y conservar la información de revocación en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="44102-121">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="44102-122">Escribir un `IKeyManager` una tarea muy avanzada y la mayoría de los desarrolladores no debería intentar realizar.</span><span class="sxs-lookup"><span data-stu-id="44102-122">Writing an `IKeyManager` is a very advanced task, and the majority of developers shouldn't attempt it.</span></span> <span data-ttu-id="44102-123">En su lugar, la mayoría de los desarrolladores deben sacar partido de las funciones que ofrece el [XmlKeyManager](#xmlkeymanager) clase.</span><span class="sxs-lookup"><span data-stu-id="44102-123">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](#xmlkeymanager) class.</span></span>

## <a name="xmlkeymanager"></a><span data-ttu-id="44102-124">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="44102-124">XmlKeyManager</span></span>

<span data-ttu-id="44102-125">El `XmlKeyManager` es de tipo de la implementación concreta en el cuadro de `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="44102-125">The `XmlKeyManager` type is the in-box concrete implementation of `IKeyManager`.</span></span> <span data-ttu-id="44102-126">Proporciona varias utilidades útiles, incluida la custodia de clave y el cifrado de claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="44102-126">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="44102-127">Las claves en este sistema se representan como elementos XML (en concreto, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span><span class="sxs-lookup"><span data-stu-id="44102-127">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).</span></span>

<span data-ttu-id="44102-128">`XmlKeyManager` depende de otros componentes en el transcurso de cumplimiento de sus tareas:</span><span class="sxs-lookup"><span data-stu-id="44102-128">`XmlKeyManager` depends on several other components in the course of fulfilling its tasks:</span></span>

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="44102-129">`AlgorithmConfiguration`, que dicta los algoritmos usados por las nuevas claves.</span><span class="sxs-lookup"><span data-stu-id="44102-129">`AlgorithmConfiguration`, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="44102-130">`IXmlRepository`, que controla donde las claves se conservan en almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="44102-130">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="44102-131">`IXmlEncryptor` [opcional], lo que permite cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="44102-131">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="44102-132">`IKeyEscrowSink` [opcional], que proporciona servicios de custodia de clave.</span><span class="sxs-lookup"><span data-stu-id="44102-132">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="44102-133">`IXmlRepository`, que controla donde las claves se conservan en almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="44102-133">`IXmlRepository`, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="44102-134">`IXmlEncryptor` [opcional], lo que permite cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="44102-134">`IXmlEncryptor` [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="44102-135">`IKeyEscrowSink` [opcional], que proporciona servicios de custodia de clave.</span><span class="sxs-lookup"><span data-stu-id="44102-135">`IKeyEscrowSink` [optional], which provides key escrow services.</span></span>

::: moniker-end

<span data-ttu-id="44102-136">A continuación se muestran los diagramas de alto nivel que indican cómo estos componentes se conectan entre sí dentro de `XmlKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="44102-136">Below are high-level diagrams which indicate how these components are wired together within `XmlKeyManager`.</span></span>

::: moniker range=">= aspnetcore-2.0"

![Creación de claves](key-management/_static/keycreation2.png)

<span data-ttu-id="44102-138">*Creación de clave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="44102-138">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="44102-139">En la implementación de `CreateNewKey`, `AlgorithmConfiguration` componente se utiliza para crear un nombre único `IAuthenticatedEncryptorDescriptor`, que, a continuación, se serializa como XML.</span><span class="sxs-lookup"><span data-stu-id="44102-139">In the implementation of `CreateNewKey`, the `AlgorithmConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="44102-140">Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="44102-140">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="44102-141">A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado.</span><span class="sxs-lookup"><span data-stu-id="44102-141">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="44102-142">Este documento cifrada se almacena en almacenamiento a largo plazo a través de la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="44102-142">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="44102-143">(Si no hay ningún `IXmlEncryptor` está configurado, se conserva en el documento sin cifrar el `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="44102-143">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Recuperación de clave](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Creación de claves](key-management/_static/keycreation1.png)

<span data-ttu-id="44102-146">*Creación de clave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="44102-146">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="44102-147">En la implementación de `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` componente se utiliza para crear un nombre único `IAuthenticatedEncryptorDescriptor`, que, a continuación, se serializa como XML.</span><span class="sxs-lookup"><span data-stu-id="44102-147">In the implementation of `CreateNewKey`, the `IAuthenticatedEncryptorConfiguration` component is used to create a unique `IAuthenticatedEncryptorDescriptor`, which is then serialized as XML.</span></span> <span data-ttu-id="44102-148">Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="44102-148">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="44102-149">A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado.</span><span class="sxs-lookup"><span data-stu-id="44102-149">The unencrypted XML is then run through an `IXmlEncryptor` (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="44102-150">Este documento cifrada se almacena en almacenamiento a largo plazo a través de la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="44102-150">This encrypted document is persisted to long-term storage via the `IXmlRepository`.</span></span> <span data-ttu-id="44102-151">(Si no hay ningún `IXmlEncryptor` está configurado, se conserva en el documento sin cifrar el `IXmlRepository`.)</span><span class="sxs-lookup"><span data-stu-id="44102-151">(If no `IXmlEncryptor` is configured, the unencrypted document is persisted in the `IXmlRepository`.)</span></span>

![Recuperación de clave](key-management/_static/keyretrieval1.png)

::: moniker-end

<span data-ttu-id="44102-153">*Recuperación de clave / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="44102-153">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="44102-154">En la implementación de `GetAllKeys`, el XML que representa claves documenta y se leen las revocaciones de subyacente `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="44102-154">In the implementation of `GetAllKeys`, the XML documents representing keys and revocations are read from the underlying `IXmlRepository`.</span></span> <span data-ttu-id="44102-155">Si estos documentos están cifrados, el sistema les descifrará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="44102-155">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="44102-156">`XmlKeyManager` crea la adecuada `IAuthenticatedEncryptorDescriptorDeserializer` instancias para deserializar los documentos de nuevo en `IAuthenticatedEncryptorDescriptor` instancias, que, a continuación, se incluyen en persona `IKey` instancias.</span><span class="sxs-lookup"><span data-stu-id="44102-156">`XmlKeyManager` creates the appropriate `IAuthenticatedEncryptorDescriptorDeserializer` instances to deserialize the documents back into `IAuthenticatedEncryptorDescriptor` instances, which are then wrapped in individual `IKey` instances.</span></span> <span data-ttu-id="44102-157">Esta colección de `IKey` instancias se devuelve al llamador.</span><span class="sxs-lookup"><span data-stu-id="44102-157">This collection of `IKey` instances is returned to the caller.</span></span>

<span data-ttu-id="44102-158">Encontrará más información sobre los elementos XML determinados en el [documento con formato de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="44102-158">Further information on the particular XML elements can be found in the [key storage format document](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="44102-159">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-159">IXmlRepository</span></span>

<span data-ttu-id="44102-160">El `IXmlRepository` interfaz representa un tipo que puede conservar XML y recuperar el XML desde un almacén de respaldo.</span><span class="sxs-lookup"><span data-stu-id="44102-160">The `IXmlRepository` interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="44102-161">Expone dos API:</span><span class="sxs-lookup"><span data-stu-id="44102-161">It exposes two APIs:</span></span>

* <span data-ttu-id="44102-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span><span class="sxs-lookup"><span data-stu-id="44102-162">`GetAllElements` :`IReadOnlyCollection<XElement>`</span></span>

* `StoreElement(XElement element, string friendlyName)`

<span data-ttu-id="44102-163">Las implementaciones de `IXmlRepository` no es necesario analizar el XML que se pasa a través de ellos.</span><span class="sxs-lookup"><span data-stu-id="44102-163">Implementations of `IXmlRepository` don't need to parse the XML passing through them.</span></span> <span data-ttu-id="44102-164">Deben tratar los documentos XML como opaco y permitir que los niveles superiores preocuparse de generar y analizar los documentos.</span><span class="sxs-lookup"><span data-stu-id="44102-164">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="44102-165">Hay cuatro tipos concretos integrados que implementan `IXmlRepository`:</span><span class="sxs-lookup"><span data-stu-id="44102-165">There are four built-in concrete types which implement `IXmlRepository`:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="44102-166">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-166">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="44102-167">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-167">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="44102-168">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-168">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="44102-169">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-169">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="44102-170">FileSystemXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-170">FileSystemXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [<span data-ttu-id="44102-171">RegistryXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-171">RegistryXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [<span data-ttu-id="44102-172">AzureStorage.AzureBlobXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-172">AzureStorage.AzureBlobXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [<span data-ttu-id="44102-173">RedisXmlRepository</span><span class="sxs-lookup"><span data-stu-id="44102-173">RedisXmlRepository</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

<span data-ttu-id="44102-174">Consulte la [documento de proveedores de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-providers) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="44102-174">See the [key storage providers document](xref:security/data-protection/implementation/key-storage-providers) for more information.</span></span>

<span data-ttu-id="44102-175">Registrar un personalizado `IXmlRepository` es adecuado cuando se usa un almacén de respaldo diferentes (por ejemplo, Azure Table Storage).</span><span class="sxs-lookup"><span data-stu-id="44102-175">Registering a custom `IXmlRepository` is appropriate when using a different backing store (for example, Azure Table Storage).</span></span>

<span data-ttu-id="44102-176">Para cambiar el repositorio predeterminado de toda la aplicación, registrar un personalizado `IXmlRepository` instancia:</span><span class="sxs-lookup"><span data-stu-id="44102-176">To change the default repository application-wide, register a custom `IXmlRepository` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
```

::: moniker-end

## <a name="ixmlencryptor"></a><span data-ttu-id="44102-177">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="44102-177">IXmlEncryptor</span></span>

<span data-ttu-id="44102-178">El `IXmlEncryptor` interfaz representa un tipo que puede cifrar un elemento XML de texto simple.</span><span class="sxs-lookup"><span data-stu-id="44102-178">The `IXmlEncryptor` interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="44102-179">Expone una única API:</span><span class="sxs-lookup"><span data-stu-id="44102-179">It exposes a single API:</span></span>

* <span data-ttu-id="44102-180">Cifrar (plaintextElement de XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="44102-180">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="44102-181">Si un serializador `IAuthenticatedEncryptorDescriptor` contiene todos los elementos marcados como "requiere cifrado", a continuación, `XmlKeyManager` ejecutará esos elementos a través de la configurada `IXmlEncryptor`del `Encrypt` método y conservarán el elemento cifrado en lugar de elemento de texto simple a la `IXmlRepository`.</span><span class="sxs-lookup"><span data-stu-id="44102-181">If a serialized `IAuthenticatedEncryptorDescriptor` contains any elements marked as "requires encryption", then `XmlKeyManager` will run those elements through the configured `IXmlEncryptor`'s `Encrypt` method, and it will persist the enciphered element rather than the plaintext element to the `IXmlRepository`.</span></span> <span data-ttu-id="44102-182">La salida de la `Encrypt` método es un `EncryptedXmlInfo` objeto.</span><span class="sxs-lookup"><span data-stu-id="44102-182">The output of the `Encrypt` method is an `EncryptedXmlInfo` object.</span></span> <span data-ttu-id="44102-183">Este objeto es un contenedor que contiene tanto el resultante cifrado `XElement` y el tipo que representa un `IXmlDecryptor` que puede utilizarse para descifrar el elemento correspondiente.</span><span class="sxs-lookup"><span data-stu-id="44102-183">This object is a wrapper which contains both the resultant enciphered `XElement` and the Type which represents an `IXmlDecryptor` which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="44102-184">Hay cuatro tipos concretos integrados que implementan `IXmlEncryptor`:</span><span class="sxs-lookup"><span data-stu-id="44102-184">There are four built-in concrete types which implement `IXmlEncryptor`:</span></span>

* [<span data-ttu-id="44102-185">CertificateXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="44102-185">CertificateXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [<span data-ttu-id="44102-186">DpapiNGXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="44102-186">DpapiNGXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [<span data-ttu-id="44102-187">DpapiXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="44102-187">DpapiXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [<span data-ttu-id="44102-188">NullXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="44102-188">NullXmlEncryptor</span></span>](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

<span data-ttu-id="44102-189">Consulte la [cifrado de claves en el documento de rest](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="44102-189">See the [key encryption at rest document](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

<span data-ttu-id="44102-190">Para cambiar el mecanismo predeterminado de la clave de cifrado en reposo toda la aplicación, registrar un personalizado `IXmlEncryptor` instancia:</span><span class="sxs-lookup"><span data-stu-id="44102-190">To change the default key-encryption-at-rest mechanism application-wide, register a custom `IXmlEncryptor` instance:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
```

::: moniker-end

## <a name="ixmldecryptor"></a><span data-ttu-id="44102-191">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="44102-191">IXmlDecryptor</span></span>

<span data-ttu-id="44102-192">El `IXmlDecryptor` interfaz representa un tipo que sabe cómo descifrar un `XElement` que se descifra mediante una `IXmlEncryptor`.</span><span class="sxs-lookup"><span data-stu-id="44102-192">The `IXmlDecryptor` interface represents a type that knows how to decrypt an `XElement` that was enciphered via an `IXmlEncryptor`.</span></span> <span data-ttu-id="44102-193">Expone una única API:</span><span class="sxs-lookup"><span data-stu-id="44102-193">It exposes a single API:</span></span>

* <span data-ttu-id="44102-194">Descifrar (encryptedElement de XElement): XElement</span><span class="sxs-lookup"><span data-stu-id="44102-194">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="44102-195">El `Decrypt` método deshace el cifrado realizado por `IXmlEncryptor.Encrypt`.</span><span class="sxs-lookup"><span data-stu-id="44102-195">The `Decrypt` method undoes the encryption performed by `IXmlEncryptor.Encrypt`.</span></span> <span data-ttu-id="44102-196">Por lo general, cada hormigón `IXmlEncryptor` implementación tendrá un hormigón correspondiente `IXmlDecryptor` implementación.</span><span class="sxs-lookup"><span data-stu-id="44102-196">Generally, each concrete `IXmlEncryptor` implementation will have a corresponding concrete `IXmlDecryptor` implementation.</span></span>

<span data-ttu-id="44102-197">Los tipos que implementan `IXmlDecryptor` debe tener uno de los dos constructores públicos siguientes:</span><span class="sxs-lookup"><span data-stu-id="44102-197">Types which implement `IXmlDecryptor` should have one of the following two public constructors:</span></span>

* <span data-ttu-id="44102-198">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="44102-198">.ctor(IServiceProvider)</span></span>
* <span data-ttu-id="44102-199">.ctor()</span><span class="sxs-lookup"><span data-stu-id="44102-199">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="44102-200">El `IServiceProvider` pasado al constructor puede ser null.</span><span class="sxs-lookup"><span data-stu-id="44102-200">The `IServiceProvider` passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="44102-201">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="44102-201">IKeyEscrowSink</span></span>

<span data-ttu-id="44102-202">El `IKeyEscrowSink` interfaz representa un tipo que puede realizar la custodia de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="44102-202">The `IKeyEscrowSink` interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="44102-203">Recuerde que los descriptores de serializado podrían contener información confidencial (por ejemplo, el material criptográfico) y esto es lo que llevó a la introducción de la [IXmlEncryptor](#ixmlencryptor) escriba en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="44102-203">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](#ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="44102-204">Sin embargo, los accidentes suceden y llaveros puede ser eliminados o dañados.</span><span class="sxs-lookup"><span data-stu-id="44102-204">However, accidents happen, and key rings can be deleted or become corrupted.</span></span>

<span data-ttu-id="44102-205">La interfaz de custodia proporciona un sombreado de escape de emergencia, que permite el acceso al XML serializado sin procesar antes de que se transforme alguno configurado [IXmlEncryptor](#ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="44102-205">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it's transformed by any configured [IXmlEncryptor](#ixmlencryptor).</span></span> <span data-ttu-id="44102-206">La interfaz expone una única API:</span><span class="sxs-lookup"><span data-stu-id="44102-206">The interface exposes a single API:</span></span>

* <span data-ttu-id="44102-207">Store (keyId Guid, elemento de XElement)</span><span class="sxs-lookup"><span data-stu-id="44102-207">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="44102-208">Es hasta el `IKeyEscrowSink` implementación para controlar el elemento proporcionado de forma segura coherente con la directiva empresarial.</span><span class="sxs-lookup"><span data-stu-id="44102-208">It's up to the `IKeyEscrowSink` implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="44102-209">Una posible implementación podría ser para que el receptor de custodia cifrar el elemento XML con un certificado X.509 corporativo conocido que se ha custodiado clave privada del certificado; el `CertificateXmlEncryptor` tipo puede ayudar con esto.</span><span class="sxs-lookup"><span data-stu-id="44102-209">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the `CertificateXmlEncryptor` type can assist with this.</span></span> <span data-ttu-id="44102-210">El `IKeyEscrowSink` implementación también es responsable de conservar el elemento proporcionado de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="44102-210">The `IKeyEscrowSink` implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="44102-211">De forma predeterminada no está habilitado ningún mecanismo de custodia, aunque los administradores de servidor pueden [configurar esto globalmente](xref:security/data-protection/configuration/machine-wide-policy).</span><span class="sxs-lookup"><span data-stu-id="44102-211">By default no escrow mechanism is enabled, though server administrators can [configure this globally](xref:security/data-protection/configuration/machine-wide-policy).</span></span> <span data-ttu-id="44102-212">También se puede configurar mediante programación a través de la `IDataProtectionBuilder.AddKeyEscrowSink` método tal como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="44102-212">It can also be configured programmatically via the `IDataProtectionBuilder.AddKeyEscrowSink` method as shown in the sample below.</span></span> <span data-ttu-id="44102-213">El `AddKeyEscrowSink` reflejado sobrecargas de método la `IServiceCollection.AddSingleton` y `IServiceCollection.AddInstance` sobrecargas, como `IKeyEscrowSink` instancias están pensadas para formar singletons con estado.</span><span class="sxs-lookup"><span data-stu-id="44102-213">The `AddKeyEscrowSink` method overloads mirror the `IServiceCollection.AddSingleton` and `IServiceCollection.AddInstance` overloads, as `IKeyEscrowSink` instances are intended to be singletons.</span></span> <span data-ttu-id="44102-214">Si hay varios `IKeyEscrowSink` se registran las instancias, cada uno de ellos se llamará durante la generación de claves, por lo que las claves se pueden custodiar a varios mecanismos simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="44102-214">If multiple `IKeyEscrowSink` instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="44102-215">No hay ninguna API para leer el material de un `IKeyEscrowSink` instancia.</span><span class="sxs-lookup"><span data-stu-id="44102-215">There's no API to read material from an `IKeyEscrowSink` instance.</span></span> <span data-ttu-id="44102-216">Esto es coherente con la teoría del diseño del mecanismo de custodia: se ha diseñado para que el material de clave sea accesible a una autoridad de confianza y, puesto que la aplicación propia no es una autoridad de confianza, no debería tener acceso a su propio material custodiada.</span><span class="sxs-lookup"><span data-stu-id="44102-216">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="44102-217">Ejemplo de código siguiente muestra cómo crear y registrar un `IKeyEscrowSink` donde se puede custodiar claves tal que solo los miembros del "CONTOSODomain Admins" pueden recuperarlos.</span><span class="sxs-lookup"><span data-stu-id="44102-217">The following sample code demonstrates creating and registering an `IKeyEscrowSink` where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="44102-218">Para ejecutar este ejemplo, debe estar en un dominio de Windows 8 / máquina con Windows Server 2012 y el controlador de dominio deben ser Windows Server 2012 o posterior.</span><span class="sxs-lookup"><span data-stu-id="44102-218">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
