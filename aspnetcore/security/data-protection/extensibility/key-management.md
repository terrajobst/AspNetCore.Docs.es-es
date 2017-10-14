---
title: "Extensibilidad de administración de claves"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3606b251-8324-4485-8d52-582a2cd5cffb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: ce23931e72404347ebc17c69ae90e70cd15328bc
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-extensibility"></a><span data-ttu-id="20156-103">Extensibilidad de administración de claves</span><span class="sxs-lookup"><span data-stu-id="20156-103">Key management extensibility</span></span>

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> <span data-ttu-id="20156-104">Leer la [administración de claves](../implementation/key-management.md#data-protection-implementation-key-management) sección antes de leer esta sección, tal y como se explican algunos de los conceptos fundamentales de estas API.</span><span class="sxs-lookup"><span data-stu-id="20156-104">Read the [key management](../implementation/key-management.md#data-protection-implementation-key-management) section before reading this section, as it explains some of the fundamental concepts behind these APIs.</span></span>

>[!WARNING]
> <span data-ttu-id="20156-105">Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.</span><span class="sxs-lookup"><span data-stu-id="20156-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="key"></a><span data-ttu-id="20156-106">Key</span><span class="sxs-lookup"><span data-stu-id="20156-106">Key</span></span>

<span data-ttu-id="20156-107">La interfaz de IKey es la representación básica de una clave en el sistema de cifrado.</span><span class="sxs-lookup"><span data-stu-id="20156-107">The IKey interface is the basic representation of a key in cryptosystem.</span></span> <span data-ttu-id="20156-108">La clave de término se utiliza aquí en el sentido abstracto, no en el sentido de "material clave criptográfico" literal.</span><span class="sxs-lookup"><span data-stu-id="20156-108">The term key is used here in the abstract sense, not in the literal sense of "cryptographic key material".</span></span> <span data-ttu-id="20156-109">Una clave tiene las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="20156-109">A key has the following properties:</span></span>

* <span data-ttu-id="20156-110">Fechas de expiración, la creación y la activación</span><span class="sxs-lookup"><span data-stu-id="20156-110">Activation, creation, and expiration dates</span></span>

* <span data-ttu-id="20156-111">Estado de revocación</span><span class="sxs-lookup"><span data-stu-id="20156-111">Revocation status</span></span>

* <span data-ttu-id="20156-112">Identificador de clave (GUID)</span><span class="sxs-lookup"><span data-stu-id="20156-112">Key identifier (a GUID)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="20156-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="20156-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="20156-114">Además, IKey expone un método CreateEncryptor que se puede usar para crear un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.</span><span class="sxs-lookup"><span data-stu-id="20156-114">Additionally, IKey exposes a CreateEncryptor method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="20156-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="20156-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="20156-116">Además, IKey expone un método CreateEncryptorInstance que se puede usar para crear un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.</span><span class="sxs-lookup"><span data-stu-id="20156-116">Additionally, IKey exposes a CreateEncryptorInstance method which can be used to create an [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance tied to this key.</span></span>

---

> [!NOTE]
> <span data-ttu-id="20156-117">No hay ninguna API para recuperar el material criptográfico sin procesar de una instancia de IKey.</span><span class="sxs-lookup"><span data-stu-id="20156-117">There is no API to retrieve the raw cryptographic material from an IKey instance.</span></span>

## <a name="ikeymanager"></a><span data-ttu-id="20156-118">IKeyManager</span><span class="sxs-lookup"><span data-stu-id="20156-118">IKeyManager</span></span>

<span data-ttu-id="20156-119">La interfaz IKeyManager representa un objeto responsable de la manipulación, recuperación y almacenamiento de claves general.</span><span class="sxs-lookup"><span data-stu-id="20156-119">The IKeyManager interface represents an object responsible for general key storage, retrieval, and manipulation.</span></span> <span data-ttu-id="20156-120">Expone tres operaciones de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="20156-120">It exposes three high-level operations:</span></span>

* <span data-ttu-id="20156-121">Cree una nueva clave y almacenar los datos en almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="20156-121">Create a new key and persist it to storage.</span></span>

* <span data-ttu-id="20156-122">Obtener todas las claves de almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="20156-122">Get all keys from storage.</span></span>

* <span data-ttu-id="20156-123">Revocar una o varias claves y conservar la información de revocación en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="20156-123">Revoke one or more keys and persist the revocation information to storage.</span></span>

>[!WARNING]
> <span data-ttu-id="20156-124">Escribir una IKeyManager es una tarea muy avanzada y la mayoría de los desarrolladores no debería intentar.</span><span class="sxs-lookup"><span data-stu-id="20156-124">Writing an IKeyManager is a very advanced task, and the majority of developers should not attempt it.</span></span> <span data-ttu-id="20156-125">En su lugar, la mayoría de los desarrolladores deben aprovechar las ventajas de las funciones que ofrece el [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) clase.</span><span class="sxs-lookup"><span data-stu-id="20156-125">Instead, most developers should take advantage of the facilities offered by the [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) class.</span></span>

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a><span data-ttu-id="20156-126">XmlKeyManager</span><span class="sxs-lookup"><span data-stu-id="20156-126">XmlKeyManager</span></span>

<span data-ttu-id="20156-127">El tipo de XmlKeyManager es la implementación concreta en el cuadro de IKeyManager.</span><span class="sxs-lookup"><span data-stu-id="20156-127">The XmlKeyManager type is the in-box concrete implementation of IKeyManager.</span></span> <span data-ttu-id="20156-128">Proporciona varias funciones útiles, incluidas la custodia de clave y el cifrado de claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="20156-128">It provides several useful facilities, including key escrow and encryption of keys at rest.</span></span> <span data-ttu-id="20156-129">Las claves en este sistema se representan como elementos XML (en concreto, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).</span><span class="sxs-lookup"><span data-stu-id="20156-129">Keys in this system are represented as XML elements (specifically, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview).</span></span>

<span data-ttu-id="20156-130">XmlKeyManager depende de otros componentes en el curso de cumplir sus tareas:</span><span class="sxs-lookup"><span data-stu-id="20156-130">XmlKeyManager depends on several other components in the course of fulfilling its tasks:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="20156-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="20156-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="20156-132">AlgorithmConfiguration, que determina los algoritmos utilizados por las nuevas claves.</span><span class="sxs-lookup"><span data-stu-id="20156-132">AlgorithmConfiguration, which dictates the algorithms used by new keys.</span></span>

* <span data-ttu-id="20156-133">IXmlRepository, qué controles donde las claves se conservan en almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="20156-133">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="20156-134">IXmlEncryptor [opcional], que permite cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="20156-134">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="20156-135">IKeyEscrowSink [opcional], que proporciona servicios de custodia de clave.</span><span class="sxs-lookup"><span data-stu-id="20156-135">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="20156-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="20156-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="20156-137">IXmlRepository, qué controles donde las claves se conservan en almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="20156-137">IXmlRepository, which controls where keys are persisted in storage.</span></span>

* <span data-ttu-id="20156-138">IXmlEncryptor [opcional], que permite cifrar las claves en reposo.</span><span class="sxs-lookup"><span data-stu-id="20156-138">IXmlEncryptor [optional], which allows encrypting keys at rest.</span></span>

* <span data-ttu-id="20156-139">IKeyEscrowSink [opcional], que proporciona servicios de custodia de clave.</span><span class="sxs-lookup"><span data-stu-id="20156-139">IKeyEscrowSink [optional], which provides key escrow services.</span></span>

---

<span data-ttu-id="20156-140">A continuación se muestran los diagramas de alto nivel que indican cómo se conectan juntos estos componentes en XmlKeyManager.</span><span class="sxs-lookup"><span data-stu-id="20156-140">Below are high-level diagrams which indicate how these components are wired together within XmlKeyManager.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="20156-141">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="20156-141">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Creación de claves](key-management/_static/keycreation2.png)

   <span data-ttu-id="20156-143">*Creación de clave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="20156-143">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="20156-144">En la implementación de CreateNewKey, el componente AlgorithmConfiguration se utiliza para crear un IAuthenticatedEncryptorDescriptor único, que, a continuación, se serializa como XML.</span><span class="sxs-lookup"><span data-stu-id="20156-144">In the implementation of CreateNewKey, the AlgorithmConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="20156-145">Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="20156-145">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="20156-146">El XML sin cifrar se ejecuta a través de un IXmlEncryptor (si es necesario) para generar el documento XML cifrado.</span><span class="sxs-lookup"><span data-stu-id="20156-146">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="20156-147">Este documento cifrada se almacena en un almacenamiento a largo plazo a través de la IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="20156-147">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="20156-148">(Si no se ha configurado ningún IXmlEncryptor, el documento sin cifrar se conserva en el IXmlRepository).</span><span class="sxs-lookup"><span data-stu-id="20156-148">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="20156-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="20156-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Creación de claves](key-management/_static/keycreation1.png)

   <span data-ttu-id="20156-151">*Creación de clave / CreateNewKey*</span><span class="sxs-lookup"><span data-stu-id="20156-151">*Key Creation / CreateNewKey*</span></span>

<span data-ttu-id="20156-152">En la implementación de CreateNewKey, el componente IAuthenticatedEncryptorConfiguration se utiliza para crear un IAuthenticatedEncryptorDescriptor único, que, a continuación, se serializa como XML.</span><span class="sxs-lookup"><span data-stu-id="20156-152">In the implementation of CreateNewKey, the IAuthenticatedEncryptorConfiguration component is used to create a unique IAuthenticatedEncryptorDescriptor, which is then serialized as XML.</span></span> <span data-ttu-id="20156-153">Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo.</span><span class="sxs-lookup"><span data-stu-id="20156-153">If a key escrow sink is present, the raw (unencrypted) XML is provided to the sink for long-term storage.</span></span> <span data-ttu-id="20156-154">El XML sin cifrar se ejecuta a través de un IXmlEncryptor (si es necesario) para generar el documento XML cifrado.</span><span class="sxs-lookup"><span data-stu-id="20156-154">The unencrypted XML is then run through an IXmlEncryptor (if required) to generate the encrypted XML document.</span></span> <span data-ttu-id="20156-155">Este documento cifrada se almacena en un almacenamiento a largo plazo a través de la IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="20156-155">This encrypted document is persisted to long-term storage via the IXmlRepository.</span></span> <span data-ttu-id="20156-156">(Si no se ha configurado ningún IXmlEncryptor, el documento sin cifrar se conserva en el IXmlRepository).</span><span class="sxs-lookup"><span data-stu-id="20156-156">(If no IXmlEncryptor is configured, the unencrypted document is persisted in the IXmlRepository.)</span></span>

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="20156-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="20156-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

   ![Recuperación de claves](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="20156-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="20156-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

   ![Recuperación de claves](key-management/_static/keyretrieval1.png)

---

   <span data-ttu-id="20156-161">*Recuperación de clave / GetAllKeys*</span><span class="sxs-lookup"><span data-stu-id="20156-161">*Key Retrieval / GetAllKeys*</span></span>

<span data-ttu-id="20156-162">En la implementación de GetAllKeys, los documentos XML que representa las claves y revocaciones se leen desde el IXmlRepository subyacente.</span><span class="sxs-lookup"><span data-stu-id="20156-162">In the implementation of GetAllKeys, the XML documents representing keys and revocations are read from the underlying IXmlRepository.</span></span> <span data-ttu-id="20156-163">Si estos documentos están cifrados, el sistema les descifrará automáticamente.</span><span class="sxs-lookup"><span data-stu-id="20156-163">If these documents are encrypted, the system will automatically decrypt them.</span></span> <span data-ttu-id="20156-164">XmlKeyManager crea las instancias de IAuthenticatedEncryptorDescriptorDeserializer adecuadas para deserializar los documentos en instancias de IAuthenticatedEncryptorDescriptor, que, a continuación, se incluyen en las instancias individuales de IKey.</span><span class="sxs-lookup"><span data-stu-id="20156-164">XmlKeyManager creates the appropriate IAuthenticatedEncryptorDescriptorDeserializer instances to deserialize the documents back into IAuthenticatedEncryptorDescriptor instances, which are then wrapped in individual IKey instances.</span></span> <span data-ttu-id="20156-165">Esta colección de instancias de IKey se devuelve al llamador.</span><span class="sxs-lookup"><span data-stu-id="20156-165">This collection of IKey instances is returned to the caller.</span></span>

<span data-ttu-id="20156-166">Encontrará más información sobre los elementos XML determinados en el [documento de formato de almacenamiento de claves](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span><span class="sxs-lookup"><span data-stu-id="20156-166">Further information on the particular XML elements can be found in the [key storage format document](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).</span></span>

## <a name="ixmlrepository"></a><span data-ttu-id="20156-167">IXmlRepository</span><span class="sxs-lookup"><span data-stu-id="20156-167">IXmlRepository</span></span>

<span data-ttu-id="20156-168">La interfaz IXmlRepository representa un tipo que pueda persista código XML en y recuperar el XML de un almacén de copia de seguridad.</span><span class="sxs-lookup"><span data-stu-id="20156-168">The IXmlRepository interface represents a type that can persist XML to and retrieve XML from a backing store.</span></span> <span data-ttu-id="20156-169">Expone dos API:</span><span class="sxs-lookup"><span data-stu-id="20156-169">It exposes two APIs:</span></span>

* <span data-ttu-id="20156-170">GetAllElements(): IReadOnlyCollection<XElement></span><span class="sxs-lookup"><span data-stu-id="20156-170">GetAllElements() : IReadOnlyCollection<XElement></span></span>

* <span data-ttu-id="20156-171">StoreElement (elemento XElement, cadena friendlyName)</span><span class="sxs-lookup"><span data-stu-id="20156-171">StoreElement(XElement element, string friendlyName)</span></span>

<span data-ttu-id="20156-172">Las implementaciones de IXmlRepository no es necesario analizar el XML que se pasan a través de ellos.</span><span class="sxs-lookup"><span data-stu-id="20156-172">Implementations of IXmlRepository don't need to parse the XML passing through them.</span></span> <span data-ttu-id="20156-173">Debe tratar los documentos XML como opaco y permitir que los niveles superiores a preocuparse sobre cómo generar y analizar los documentos.</span><span class="sxs-lookup"><span data-stu-id="20156-173">They should treat the XML documents as opaque and let higher layers worry about generating and parsing the documents.</span></span>

<span data-ttu-id="20156-174">Hay dos tipos integrados concretos que implementan IXmlRepository: FileSystemXmlRepository y RegistryXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="20156-174">There are two built-in concrete types which implement IXmlRepository: FileSystemXmlRepository and RegistryXmlRepository.</span></span> <span data-ttu-id="20156-175">Consulte la [documento de proveedores de almacenamiento de claves](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="20156-175">See the [key storage providers document](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) for more information.</span></span> <span data-ttu-id="20156-176">Registrar un IXmlRepository personalizado podría ser la manera adecuada para utilizar otra memoria auxiliar, p. ej., el almacenamiento de blobs de Azure.</span><span class="sxs-lookup"><span data-stu-id="20156-176">Registering a custom IXmlRepository would be the appropriate manner to use a different backing store, e.g., Azure Blob Storage.</span></span> <span data-ttu-id="20156-177">Para cambiar el valor predeterminado repositorio-toda la aplicación, registrar un singleton personalizado IXmlRepository en el proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="20156-177">To change the default repository application-wide, register a custom singleton IXmlRepository in the service provider.</span></span>

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a><span data-ttu-id="20156-178">IXmlEncryptor</span><span class="sxs-lookup"><span data-stu-id="20156-178">IXmlEncryptor</span></span>

<span data-ttu-id="20156-179">La interfaz IXmlEncryptor representa un tipo que puede cifrar un elemento XML de texto simple.</span><span class="sxs-lookup"><span data-stu-id="20156-179">The IXmlEncryptor interface represents a type that can encrypt a plaintext XML element.</span></span> <span data-ttu-id="20156-180">Expone una única API:</span><span class="sxs-lookup"><span data-stu-id="20156-180">It exposes a single API:</span></span>

* <span data-ttu-id="20156-181">Cifrar (plaintextElement de XElement): EncryptedXmlInfo</span><span class="sxs-lookup"><span data-stu-id="20156-181">Encrypt(XElement plaintextElement) : EncryptedXmlInfo</span></span>

<span data-ttu-id="20156-182">Si un IAuthenticatedEncryptorDescriptor serializado contiene elementos marcados como "requiere cifrado", a continuación, XmlKeyManager se ejecutarán esos elementos a través del método de cifrado del IXmlEncryptor configurado y conservará el elemento descifra en su lugar que el elemento de texto simple para el IXmlRepository.</span><span class="sxs-lookup"><span data-stu-id="20156-182">If a serialized IAuthenticatedEncryptorDescriptor contains any elements marked as "requires encryption", then XmlKeyManager will run those elements through the configured IXmlEncryptor's Encrypt method, and it will persist the enciphered element rather than the plaintext element to the IXmlRepository.</span></span> <span data-ttu-id="20156-183">La salida del método de cifrado es un objeto EncryptedXmlInfo.</span><span class="sxs-lookup"><span data-stu-id="20156-183">The output of the Encrypt method is an EncryptedXmlInfo object.</span></span> <span data-ttu-id="20156-184">Este objeto es un contenedor que contiene el XElement descifra resultante y el tipo que representa un IXmlDecryptor que puede usarse para descifrar el elemento correspondiente.</span><span class="sxs-lookup"><span data-stu-id="20156-184">This object is a wrapper which contains both the resultant enciphered XElement and the Type which represents an IXmlDecryptor which can be used to decipher the corresponding element.</span></span>

<span data-ttu-id="20156-185">Hay cuatro tipos integrados concretos que implementan IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor y NullXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="20156-185">There are four built-in concrete types which implement IXmlEncryptor: CertificateXmlEncryptor, DpapiNGXmlEncryptor, DpapiXmlEncryptor, and NullXmlEncryptor.</span></span> <span data-ttu-id="20156-186">Consulte la [cifrado de clave en el documento de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="20156-186">See the [key encryption at rest document](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) for more information.</span></span> <span data-ttu-id="20156-187">Para cambiar el mecanismo de cifrado de clave en el resto de predeterminado de toda la aplicación, registrar un singleton personalizado IXmlEncryptor en el proveedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="20156-187">To change the default key-encryption-at-rest mechanism application-wide, register a custom singleton IXmlEncryptor in the service provider.</span></span>

## <a name="ixmldecryptor"></a><span data-ttu-id="20156-188">IXmlDecryptor</span><span class="sxs-lookup"><span data-stu-id="20156-188">IXmlDecryptor</span></span>

<span data-ttu-id="20156-189">La interfaz IXmlDecryptor representa un tipo que sabe cómo descifrar un XElement que se descifra mediante una IXmlEncryptor.</span><span class="sxs-lookup"><span data-stu-id="20156-189">The IXmlDecryptor interface represents a type that knows how to decrypt an XElement that was enciphered via an IXmlEncryptor.</span></span> <span data-ttu-id="20156-190">Expone una única API:</span><span class="sxs-lookup"><span data-stu-id="20156-190">It exposes a single API:</span></span>

* <span data-ttu-id="20156-191">Descifrar (encryptedElement de XElement): XElement</span><span class="sxs-lookup"><span data-stu-id="20156-191">Decrypt(XElement encryptedElement) : XElement</span></span>

<span data-ttu-id="20156-192">El método Decrypt deshace el cifrado realizado por IXmlEncryptor.Encrypt.</span><span class="sxs-lookup"><span data-stu-id="20156-192">The Decrypt method undoes the encryption performed by IXmlEncryptor.Encrypt.</span></span> <span data-ttu-id="20156-193">Por lo general cada implementación concreta de IXmlEncryptor tendrá una implementación concreta en IXmlDecryptor correspondiente.</span><span class="sxs-lookup"><span data-stu-id="20156-193">Generally each concrete IXmlEncryptor implementation will have a corresponding concrete IXmlDecryptor implementation.</span></span>

<span data-ttu-id="20156-194">Tipos que implementan IXmlDecryptor deben tener uno de los dos constructores públicos siguientes:</span><span class="sxs-lookup"><span data-stu-id="20156-194">Types which implement IXmlDecryptor should have one of the following two public constructors:</span></span>

* <span data-ttu-id="20156-195">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="20156-195">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="20156-196">.ctor()</span><span class="sxs-lookup"><span data-stu-id="20156-196">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="20156-197">IServiceProvider pasado al constructor puede ser null.</span><span class="sxs-lookup"><span data-stu-id="20156-197">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="ikeyescrowsink"></a><span data-ttu-id="20156-198">IKeyEscrowSink</span><span class="sxs-lookup"><span data-stu-id="20156-198">IKeyEscrowSink</span></span>

<span data-ttu-id="20156-199">La interfaz IKeyEscrowSink representa un tipo que puede realizar la custodia de la información confidencial.</span><span class="sxs-lookup"><span data-stu-id="20156-199">The IKeyEscrowSink interface represents a type that can perform escrow of sensitive information.</span></span> <span data-ttu-id="20156-200">Recuerde que descriptores serializados podrían contener información confidencial (por ejemplo, el material criptográfico) y esto es lo que ha provocado la introducción de la [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) escriba en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="20156-200">Recall that serialized descriptors might contain sensitive information (such as cryptographic material), and this is what led to the introduction of the [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) type in the first place.</span></span> <span data-ttu-id="20156-201">Sin embargo, los accidentes y keyrings pueden eliminarse o están dañados.</span><span class="sxs-lookup"><span data-stu-id="20156-201">However, accidents happen, and keyrings can be deleted or become corrupted.</span></span>

<span data-ttu-id="20156-202">La interfaz de custodia proporciona una trama de escape de emergencia, permitir el acceso al XML serializado sin procesar antes de que se transforme en ninguno configurado [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span><span class="sxs-lookup"><span data-stu-id="20156-202">The escrow interface provides an emergency escape hatch, allowing access to the raw serialized XML before it is transformed by any configured [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor).</span></span> <span data-ttu-id="20156-203">La interfaz expone una única API:</span><span class="sxs-lookup"><span data-stu-id="20156-203">The interface exposes a single API:</span></span>

* <span data-ttu-id="20156-204">Almacén (keyId de Guid, elemento de XElement)</span><span class="sxs-lookup"><span data-stu-id="20156-204">Store(Guid keyId, XElement element)</span></span>

<span data-ttu-id="20156-205">Depende de la implementación de IKeyEscrowSink para controlar el elemento proporcionado de forma segura coherente con la directiva empresarial.</span><span class="sxs-lookup"><span data-stu-id="20156-205">It is up to the IKeyEscrowSink implementation to handle the provided element in a secure manner consistent with business policy.</span></span> <span data-ttu-id="20156-206">Una posible implementación podría ser para que el receptor de custodia cifrar el elemento XML mediante un certificado X.509 corporativo conocido donde se ha custodiado clave privada del certificado; el tipo de CertificateXmlEncryptor puede ayudarle con esto.</span><span class="sxs-lookup"><span data-stu-id="20156-206">One possible implementation could be for the escrow sink to encrypt the XML element using a known corporate X.509 certificate where the certificate's private key has been escrowed; the CertificateXmlEncryptor type can assist with this.</span></span> <span data-ttu-id="20156-207">La implementación de IKeyEscrowSink también es responsable de conservar el elemento proporcionado de forma adecuada.</span><span class="sxs-lookup"><span data-stu-id="20156-207">The IKeyEscrowSink implementation is also responsible for persisting the provided element appropriately.</span></span>

<span data-ttu-id="20156-208">De forma predeterminada ningún mecanismo de custodia está habilitado, aunque los administradores de servidor pueden [configurarlo global](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span><span class="sxs-lookup"><span data-stu-id="20156-208">By default no escrow mechanism is enabled, though server administrators can [configure this globally](../configuration/machine-wide-policy.md#data-protection-configuration-machinewidepolicy).</span></span> <span data-ttu-id="20156-209">También puede configurar mediante programación a través de la *IDataProtectionBuilder.AddKeyEscrowSink* método tal como se muestra en el ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="20156-209">It can also be configured programmatically via the *IDataProtectionBuilder.AddKeyEscrowSink* method as shown in the sample below.</span></span> <span data-ttu-id="20156-210">El *AddKeyEscrowSink* reflejado de las sobrecargas de método la *IServiceCollection.AddSingleton* y *IServiceCollection.AddInstance* sobrecargas, como IKeyEscrowSink instancias están diseñadas para ser singletons.</span><span class="sxs-lookup"><span data-stu-id="20156-210">The *AddKeyEscrowSink* method overloads mirror the *IServiceCollection.AddSingleton* and *IServiceCollection.AddInstance* overloads, as IKeyEscrowSink instances are intended to be singletons.</span></span> <span data-ttu-id="20156-211">Si se registran varias instancias de IKeyEscrowSink, se llamará cada uno de ellos durante la generación de claves, por lo que las claves se pueden custodiar a varios mecanismos simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="20156-211">If multiple IKeyEscrowSink instances are registered, each one will be called during key generation, so keys can be escrowed to multiple mechanisms simultaneously.</span></span>

<span data-ttu-id="20156-212">No hay ninguna API para leer el material de una instancia de IKeyEscrowSink.</span><span class="sxs-lookup"><span data-stu-id="20156-212">There is no API to read material from an IKeyEscrowSink instance.</span></span> <span data-ttu-id="20156-213">Esto es coherente con la teoría del diseño del mecanismo de custodia: se ha diseñado para hacer que el material de clave sea accesible a una autoridad de confianza y, puesto que la aplicación de sí mismo no es una entidad de confianza, no debería tener acceso a su propio material custodiada.</span><span class="sxs-lookup"><span data-stu-id="20156-213">This is consistent with the design theory of the escrow mechanism: it's intended to make the key material accessible to a trusted authority, and since the application is itself not a trusted authority, it shouldn't have access to its own escrowed material.</span></span>

<span data-ttu-id="20156-214">El código de ejemplo siguiente muestra cómo crear y registrar un IKeyEscrowSink donde se custodiar claves de modo que solo los miembros del "CONTOSODomain Admins" pueden recuperarlos.</span><span class="sxs-lookup"><span data-stu-id="20156-214">The following sample code demonstrates creating and registering an IKeyEscrowSink where keys are escrowed such that only members of "CONTOSODomain Admins" can recover them.</span></span>

> [!NOTE]
> <span data-ttu-id="20156-215">Para ejecutar este ejemplo, debe ser en un equipo con Windows 8 Unidos a un dominio / máquina con Windows Server 2012 y el controlador de dominio deben ser Windows Server 2012 o posterior.</span><span class="sxs-lookup"><span data-stu-id="20156-215">To run this sample, you must be on a domain-joined Windows 8 / Windows Server 2012 machine, and the domain controller must be Windows Server 2012 or later.</span></span>

[!code-none[Main](key-management/samples/key-management-extensibility.cs)]
