---
title: Extensibilidad de criptografía de núcleo de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer y el generador de nivel superior.
manager: wpickett
ms.author: riande
ms.date: 8/11/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: b5a0dbc9120a8032dbb8d8eee74684495a982ac1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896831"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a><span data-ttu-id="43e3e-103">Extensibilidad de criptografía de núcleo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="43e3e-103">Core cryptography extensibility in ASP.NET Core</span></span>

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> <span data-ttu-id="43e3e-104">Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.</span><span class="sxs-lookup"><span data-stu-id="43e3e-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a><span data-ttu-id="43e3e-105">IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="43e3e-105">IAuthenticatedEncryptor</span></span>

<span data-ttu-id="43e3e-106">El **IAuthenticatedEncryptor** interfaz es el bloque de creación básico del subsistema criptográfico.</span><span class="sxs-lookup"><span data-stu-id="43e3e-106">The **IAuthenticatedEncryptor** interface is the basic building block of the cryptographic subsystem.</span></span> <span data-ttu-id="43e3e-107">Por lo general, hay un IAuthenticatedEncryptor por clave y la instancia de IAuthenticatedEncryptor contiene todos los material de clave de cifrado y algoritmo información necesaria para realizar operaciones criptográficas.</span><span class="sxs-lookup"><span data-stu-id="43e3e-107">There's generally one IAuthenticatedEncryptor per key, and the IAuthenticatedEncryptor instance wraps all cryptographic key material and algorithmic information necessary to perform cryptographic operations.</span></span>

<span data-ttu-id="43e3e-108">Como sugiere su nombre, el tipo es responsable de proporcionar servicios de cifrado y descifrado autenticados.</span><span class="sxs-lookup"><span data-stu-id="43e3e-108">As its name suggests, the type is responsible for providing authenticated encryption and decryption services.</span></span> <span data-ttu-id="43e3e-109">Expone las siguientes API de dos.</span><span class="sxs-lookup"><span data-stu-id="43e3e-109">It exposes the following two APIs.</span></span>

* <span data-ttu-id="43e3e-110">Descifrar (ArraySegment<byte> texto cifrado, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="43e3e-110">Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

* <span data-ttu-id="43e3e-111">Cifrar (ArraySegment<byte> texto simple, ArraySegment<byte> additionalAuthenticatedData): byte]</span><span class="sxs-lookup"><span data-stu-id="43e3e-111">Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]</span></span>

<span data-ttu-id="43e3e-112">El método Encrypt devuelve un blob que incluye el texto sin formato descifra y una etiqueta de autenticación.</span><span class="sxs-lookup"><span data-stu-id="43e3e-112">The Encrypt method returns a blob that includes the enciphered plaintext and an authentication tag.</span></span> <span data-ttu-id="43e3e-113">La etiqueta de autenticación debe incluir los datos adicionales autenticados (AAD), aunque el AAD propio no necesita poder recuperarse desde la carga final.</span><span class="sxs-lookup"><span data-stu-id="43e3e-113">The authentication tag must encompass the additional authenticated data (AAD), though the AAD itself need not be recoverable from the final payload.</span></span> <span data-ttu-id="43e3e-114">El método Decrypt valida la etiqueta de autenticación y devuelve la carga deciphered.</span><span class="sxs-lookup"><span data-stu-id="43e3e-114">The Decrypt method validates the authentication tag and returns the deciphered payload.</span></span> <span data-ttu-id="43e3e-115">Todos los errores (excepto ArgumentNullException y similar) deben homogeneizarse a CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="43e3e-115">All failures (except ArgumentNullException and similar) should be homogenized to CryptographicException.</span></span>

> [!NOTE]
> <span data-ttu-id="43e3e-116">La propia instancia IAuthenticatedEncryptor realmente no debe contener el material de clave.</span><span class="sxs-lookup"><span data-stu-id="43e3e-116">The IAuthenticatedEncryptor instance itself doesn't actually need to contain the key material.</span></span> <span data-ttu-id="43e3e-117">Por ejemplo, la implementación puede delegar a un HSM para todas las operaciones.</span><span class="sxs-lookup"><span data-stu-id="43e3e-117">For example, the implementation could delegate to an HSM for all operations.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a><span data-ttu-id="43e3e-118">Cómo crear un IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="43e3e-118">How to create an IAuthenticatedEncryptor</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43e3e-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="43e3e-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="43e3e-120">El **IAuthenticatedEncryptorFactory** interfaz representa un tipo que sabe cómo crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia.</span><span class="sxs-lookup"><span data-stu-id="43e3e-120">The **IAuthenticatedEncryptorFactory** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="43e3e-121">La API es como sigue.</span><span class="sxs-lookup"><span data-stu-id="43e3e-121">Its API is as follows.</span></span>

* <span data-ttu-id="43e3e-122">CreateEncryptorInstance (clave IKey): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="43e3e-122">CreateEncryptorInstance(IKey key) : IAuthenticatedEncryptor</span></span>

<span data-ttu-id="43e3e-123">Para cualquier instancia de IKey determinada, los sistemas de cifrado autenticados creados por el método CreateEncryptorInstance deben considerarse equivalentes, como en el ejemplo de código siguiente.</span><span class="sxs-lookup"><span data-stu-id="43e3e-123">For any given IKey instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorFactory instance and an IKey instance
IAuthenticatedEncryptorFactory factory = ...;
IKey key = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = factory.CreateEncryptorInstance(key);
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = factory.CreateEncryptorInstance(key);
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43e3e-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43e3e-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="43e3e-125">El **IAuthenticatedEncryptorDescriptor** interfaz representa un tipo que sabe cómo crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia.</span><span class="sxs-lookup"><span data-stu-id="43e3e-125">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to create an [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instance.</span></span> <span data-ttu-id="43e3e-126">La API es como sigue.</span><span class="sxs-lookup"><span data-stu-id="43e3e-126">Its API is as follows.</span></span>

* <span data-ttu-id="43e3e-127">CreateEncryptorInstance(): IAuthenticatedEncryptor</span><span class="sxs-lookup"><span data-stu-id="43e3e-127">CreateEncryptorInstance() : IAuthenticatedEncryptor</span></span>

* <span data-ttu-id="43e3e-128">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="43e3e-128">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

<span data-ttu-id="43e3e-129">Al igual que IAuthenticatedEncryptor, una instancia de IAuthenticatedEncryptorDescriptor se supone que va a contener una clave específica.</span><span class="sxs-lookup"><span data-stu-id="43e3e-129">Like IAuthenticatedEncryptor, an instance of IAuthenticatedEncryptorDescriptor is assumed to wrap one specific key.</span></span> <span data-ttu-id="43e3e-130">Esto significa que para cualquier instancia de IAuthenticatedEncryptorDescriptor determinada, los sistemas de cifrado autenticados creados por el método CreateEncryptorInstance deben considerarse equivalentes, como en el ejemplo de código siguiente.</span><span class="sxs-lookup"><span data-stu-id="43e3e-130">This means that for any given IAuthenticatedEncryptorDescriptor instance, any authenticated encryptors created by its CreateEncryptorInstance method should be considered equivalent, as in the below code sample.</span></span>

```csharp
// we have an IAuthenticatedEncryptorDescriptor instance
IAuthenticatedEncryptorDescriptor descriptor = ...;

// get an encryptor instance and perform an authenticated encryption operation
ArraySegment<byte> plaintext = new ArraySegment<byte>(Encoding.UTF8.GetBytes("plaintext"));
ArraySegment<byte> aad = new ArraySegment<byte>(Encoding.UTF8.GetBytes("AAD"));
var encryptor1 = descriptor.CreateEncryptorInstance();
byte[] ciphertext = encryptor1.Encrypt(plaintext, aad);

// get another encryptor instance and perform an authenticated decryption operation
var encryptor2 = descriptor.CreateEncryptorInstance();
byte[] roundTripped = encryptor2.Decrypt(new ArraySegment<byte>(ciphertext), aad);


// the 'roundTripped' and 'plaintext' buffers should be equivalent
```

---

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a><span data-ttu-id="43e3e-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core solo 2.x)</span><span class="sxs-lookup"><span data-stu-id="43e3e-131">IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x only)</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43e3e-132">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="43e3e-132">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="43e3e-133">El **IAuthenticatedEncryptorDescriptor** interfaz representa un tipo que sabe cómo exportar a XML.</span><span class="sxs-lookup"><span data-stu-id="43e3e-133">The **IAuthenticatedEncryptorDescriptor** interface represents a type that knows how to export itself to XML.</span></span> <span data-ttu-id="43e3e-134">La API es como sigue.</span><span class="sxs-lookup"><span data-stu-id="43e3e-134">Its API is as follows.</span></span>

* <span data-ttu-id="43e3e-135">ExportToXml(): XmlSerializedDescriptorInfo</span><span class="sxs-lookup"><span data-stu-id="43e3e-135">ExportToXml() : XmlSerializedDescriptorInfo</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43e3e-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43e3e-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a><span data-ttu-id="43e3e-137">Serialización XML</span><span class="sxs-lookup"><span data-stu-id="43e3e-137">XML Serialization</span></span>

<span data-ttu-id="43e3e-138">La diferencia principal entre IAuthenticatedEncryptor y IAuthenticatedEncryptorDescriptor es que el descriptor sabe cómo crear el sistema de cifrado y proporcionarle argumentos válidos.</span><span class="sxs-lookup"><span data-stu-id="43e3e-138">The primary difference between IAuthenticatedEncryptor and IAuthenticatedEncryptorDescriptor is that the descriptor knows how to create the encryptor and supply it with valid arguments.</span></span> <span data-ttu-id="43e3e-139">Tenga en cuenta un IAuthenticatedEncryptor cuya implementación se basa en SymmetricAlgorithm y KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="43e3e-139">Consider an IAuthenticatedEncryptor whose implementation relies on SymmetricAlgorithm and KeyedHashAlgorithm.</span></span> <span data-ttu-id="43e3e-140">Trabajo del sistema de cifrado es consumen estos tipos, pero no conoce necesariamente estos tipos de proceden, por lo que realmente no se puede escribir una descripción de cómo volver a sí mismo si se reinicia la aplicación adecuada.</span><span class="sxs-lookup"><span data-stu-id="43e3e-140">The encryptor's job is to consume these types, but it doesn't necessarily know where these types came from, so it can't really write out a proper description of how to recreate itself if the application restarts.</span></span> <span data-ttu-id="43e3e-141">El descriptor de actúa como un nivel más alto a partir de esto.</span><span class="sxs-lookup"><span data-stu-id="43e3e-141">The descriptor acts as a higher level on top of this.</span></span> <span data-ttu-id="43e3e-142">Puesto que el descriptor sabe cómo crear la instancia de sistema de cifrado (p. ej., sabe cómo crear los algoritmos necesarios), puede serializar esa información en forma de XML para que la instancia de sistema de cifrado se puede volver a crear después de restablece una aplicación.</span><span class="sxs-lookup"><span data-stu-id="43e3e-142">Since the descriptor knows how to create the encryptor instance (e.g., it knows how to create the required algorithms), it can serialize that knowledge in XML form so that the encryptor instance can be recreated after an application reset.</span></span>

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

<span data-ttu-id="43e3e-143">El descriptor de se puede serializar a través de su rutina de ExportToXml.</span><span class="sxs-lookup"><span data-stu-id="43e3e-143">The descriptor can be serialized via its ExportToXml routine.</span></span> <span data-ttu-id="43e3e-144">Esta rutina devuelve un XmlSerializedDescriptorInfo que contiene dos propiedades: la representación de XElement de descriptor y el tipo que representa un [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) que puede ser se usa para restablecerse este descriptor dada la XElement correspondiente.</span><span class="sxs-lookup"><span data-stu-id="43e3e-144">This routine returns an XmlSerializedDescriptorInfo which contains two properties: the XElement representation of the descriptor and the Type which represents an [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) which can be used to resurrect this descriptor given the corresponding XElement.</span></span>

<span data-ttu-id="43e3e-145">El descriptor serializado puede contener información confidencial como material de clave de cifrado.</span><span class="sxs-lookup"><span data-stu-id="43e3e-145">The serialized descriptor may contain sensitive information such as cryptographic key material.</span></span> <span data-ttu-id="43e3e-146">El sistema de protección de datos tiene compatibilidad integrada para cifrar la información antes de que se conservan en el almacenamiento.</span><span class="sxs-lookup"><span data-stu-id="43e3e-146">The data protection system has built-in support for encrypting information before it's persisted to storage.</span></span> <span data-ttu-id="43e3e-147">Para aprovechar estas características, el descriptor debería marcar el elemento que contiene información confidencial con el nombre de atributo "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), valor "true".</span><span class="sxs-lookup"><span data-stu-id="43e3e-147">To take advantage of this, the descriptor should mark the element which contains sensitive information with the attribute name "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), value "true".</span></span>

>[!TIP]
> <span data-ttu-id="43e3e-148">Hay una API auxiliar para establecer este atributo.</span><span class="sxs-lookup"><span data-stu-id="43e3e-148">There's a helper API for setting this attribute.</span></span> <span data-ttu-id="43e3e-149">Llame al método de extensión que XElement.markasrequiresencryption() ubicado en el espacio de nombres Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span><span class="sxs-lookup"><span data-stu-id="43e3e-149">Call the extension method XElement.MarkAsRequiresEncryption() located in namespace Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.</span></span>

<span data-ttu-id="43e3e-150">También puede haber casos donde el descriptor serializado no contiene información confidencial.</span><span class="sxs-lookup"><span data-stu-id="43e3e-150">There can also be cases where the serialized descriptor doesn't contain sensitive information.</span></span> <span data-ttu-id="43e3e-151">Considere la posibilidad de nuevo el caso de una clave criptográfica que se almacenan en un HSM.</span><span class="sxs-lookup"><span data-stu-id="43e3e-151">Consider again the case of a cryptographic key stored in an HSM.</span></span> <span data-ttu-id="43e3e-152">El material de clave no se puede escribir el descriptor al serializar a sí mismo porque el HSM no expone el material en formato de texto simple.</span><span class="sxs-lookup"><span data-stu-id="43e3e-152">The descriptor cannot write out the key material when serializing itself since the HSM won't expose the material in plaintext form.</span></span> <span data-ttu-id="43e3e-153">En su lugar, puede escribir el descriptor de la versión de clave ajusta de la clave (si el HSM permite la exportación de este modo) o el identificador único del HSM para la clave.</span><span class="sxs-lookup"><span data-stu-id="43e3e-153">Instead, the descriptor might write out the key-wrapped version of the key (if the HSM allows export in this fashion) or the HSM's own unique identifier for the key.</span></span>

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a><span data-ttu-id="43e3e-154">IAuthenticatedEncryptorDescriptorDeserializer</span><span class="sxs-lookup"><span data-stu-id="43e3e-154">IAuthenticatedEncryptorDescriptorDeserializer</span></span>

<span data-ttu-id="43e3e-155">El **IAuthenticatedEncryptorDescriptorDeserializer** interfaz representa un tipo que sabe cómo deserializar una instancia de IAuthenticatedEncryptorDescriptor desde un XElement.</span><span class="sxs-lookup"><span data-stu-id="43e3e-155">The **IAuthenticatedEncryptorDescriptorDeserializer** interface represents a type that knows how to deserialize an IAuthenticatedEncryptorDescriptor instance from an XElement.</span></span> <span data-ttu-id="43e3e-156">Expone un único método:</span><span class="sxs-lookup"><span data-stu-id="43e3e-156">It exposes a single method:</span></span>

* <span data-ttu-id="43e3e-157">ImportFromXml (elemento de XElement): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="43e3e-157">ImportFromXml(XElement element) : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="43e3e-158">El método ImportFromXml toma el XElement que devolvió [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) y crea un equivalente de la IAuthenticatedEncryptorDescriptor original.</span><span class="sxs-lookup"><span data-stu-id="43e3e-158">The ImportFromXml method takes the XElement that was returned by [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) and creates an equivalent of the original IAuthenticatedEncryptorDescriptor.</span></span>

<span data-ttu-id="43e3e-159">Tipos que implementan IAuthenticatedEncryptorDescriptorDeserializer deben tener uno de los dos constructores públicos siguientes:</span><span class="sxs-lookup"><span data-stu-id="43e3e-159">Types which implement IAuthenticatedEncryptorDescriptorDeserializer should have one of the following two public constructors:</span></span>

* <span data-ttu-id="43e3e-160">.ctor(IServiceProvider)</span><span class="sxs-lookup"><span data-stu-id="43e3e-160">.ctor(IServiceProvider)</span></span>

* <span data-ttu-id="43e3e-161">.ctor()</span><span class="sxs-lookup"><span data-stu-id="43e3e-161">.ctor()</span></span>

> [!NOTE]
> <span data-ttu-id="43e3e-162">IServiceProvider pasado al constructor puede ser null.</span><span class="sxs-lookup"><span data-stu-id="43e3e-162">The IServiceProvider passed to the constructor may be null.</span></span>

## <a name="the-top-level-factory"></a><span data-ttu-id="43e3e-163">El generador de nivel superior</span><span class="sxs-lookup"><span data-stu-id="43e3e-163">The top-level factory</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="43e3e-164">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="43e3e-164">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="43e3e-165">El **AlgorithmConfiguration** clase representa un tipo que sabe cómo crear [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instancias.</span><span class="sxs-lookup"><span data-stu-id="43e3e-165">The **AlgorithmConfiguration** class represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="43e3e-166">Expone una sola API.</span><span class="sxs-lookup"><span data-stu-id="43e3e-166">It exposes a single API.</span></span>

* <span data-ttu-id="43e3e-167">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="43e3e-167">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="43e3e-168">Considerar AlgorithmConfiguration como el generador de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="43e3e-168">Think of AlgorithmConfiguration as the top-level factory.</span></span> <span data-ttu-id="43e3e-169">La configuración actúa como una plantilla.</span><span class="sxs-lookup"><span data-stu-id="43e3e-169">The configuration serves as a template.</span></span> <span data-ttu-id="43e3e-170">Encapsula información algorítmica (p. ej., esta configuración produce descriptores con una clave maestra de AES-128-GCM), pero aún no está asociada a una clave específica.</span><span class="sxs-lookup"><span data-stu-id="43e3e-170">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="43e3e-171">Cuando se llama a CreateNewDescriptor, material de clave nueva se crea únicamente para esta llamada y se genera un nuevo IAuthenticatedEncryptorDescriptor que ajusta este material de clave y la información algorítmica necesarios para consumir el material.</span><span class="sxs-lookup"><span data-stu-id="43e3e-171">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="43e3e-172">El material de clave podría creó en software (y se mantienen en la memoria), podría ser crea y mantiene dentro de un HSM y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="43e3e-172">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="43e3e-173">El punto fundamental es que las dos llamadas a CreateNewDescriptor nunca deben crearse instancias de IAuthenticatedEncryptorDescriptor equivalente.</span><span class="sxs-lookup"><span data-stu-id="43e3e-173">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="43e3e-174">El tipo de AlgorithmConfiguration actúa como punto de entrada para las rutinas de creación de claves como [reversión de clave automática](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="43e3e-174">The AlgorithmConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="43e3e-175">Para cambiar la implementación de todas las claves futuras, establezca la propiedad AuthenticatedEncryptorConfiguration en KeyManagementOptions.</span><span class="sxs-lookup"><span data-stu-id="43e3e-175">To change the implementation for all future keys, set the AuthenticatedEncryptorConfiguration property in KeyManagementOptions.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="43e3e-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="43e3e-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="43e3e-177">El **IAuthenticatedEncryptorConfiguration** interfaz representa un tipo que sabe cómo crear [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instancias.</span><span class="sxs-lookup"><span data-stu-id="43e3e-177">The **IAuthenticatedEncryptorConfiguration** interface represents a type which knows how to create [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instances.</span></span> <span data-ttu-id="43e3e-178">Expone una sola API.</span><span class="sxs-lookup"><span data-stu-id="43e3e-178">It exposes a single API.</span></span>

* <span data-ttu-id="43e3e-179">CreateNewDescriptor(): IAuthenticatedEncryptorDescriptor</span><span class="sxs-lookup"><span data-stu-id="43e3e-179">CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor</span></span>

<span data-ttu-id="43e3e-180">Considerar IAuthenticatedEncryptorConfiguration como el generador de nivel superior.</span><span class="sxs-lookup"><span data-stu-id="43e3e-180">Think of IAuthenticatedEncryptorConfiguration as the top-level factory.</span></span> <span data-ttu-id="43e3e-181">La configuración actúa como una plantilla.</span><span class="sxs-lookup"><span data-stu-id="43e3e-181">The configuration serves as a template.</span></span> <span data-ttu-id="43e3e-182">Encapsula información algorítmica (p. ej., esta configuración produce descriptores con una clave maestra de AES-128-GCM), pero aún no está asociada a una clave específica.</span><span class="sxs-lookup"><span data-stu-id="43e3e-182">It wraps algorithmic information (e.g., this configuration produces descriptors with an AES-128-GCM master key), but it's not yet associated with a specific key.</span></span>

<span data-ttu-id="43e3e-183">Cuando se llama a CreateNewDescriptor, material de clave nueva se crea únicamente para esta llamada y se genera un nuevo IAuthenticatedEncryptorDescriptor que ajusta este material de clave y la información algorítmica necesarios para consumir el material.</span><span class="sxs-lookup"><span data-stu-id="43e3e-183">When CreateNewDescriptor is called, fresh key material is created solely for this call, and a new IAuthenticatedEncryptorDescriptor is produced which wraps this key material and the algorithmic information required to consume the material.</span></span> <span data-ttu-id="43e3e-184">El material de clave podría creó en software (y se mantienen en la memoria), podría ser crea y mantiene dentro de un HSM y así sucesivamente.</span><span class="sxs-lookup"><span data-stu-id="43e3e-184">The key material could be created in software (and held in memory), it could be created and held within an HSM, and so on.</span></span> <span data-ttu-id="43e3e-185">El punto fundamental es que las dos llamadas a CreateNewDescriptor nunca deben crearse instancias de IAuthenticatedEncryptorDescriptor equivalente.</span><span class="sxs-lookup"><span data-stu-id="43e3e-185">The crucial point is that any two calls to CreateNewDescriptor should never create equivalent IAuthenticatedEncryptorDescriptor instances.</span></span>

<span data-ttu-id="43e3e-186">El tipo de IAuthenticatedEncryptorConfiguration actúa como punto de entrada para las rutinas de creación de claves como [reversión de clave automática](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span><span class="sxs-lookup"><span data-stu-id="43e3e-186">The IAuthenticatedEncryptorConfiguration type serves as the entry point for key creation routines such as [automatic key rolling](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling).</span></span> <span data-ttu-id="43e3e-187">Para cambiar la implementación de todas las claves futuras, registrar un singleton IAuthenticatedEncryptorConfiguration en el contenedor de servicios.</span><span class="sxs-lookup"><span data-stu-id="43e3e-187">To change the implementation for all future keys, register a singleton IAuthenticatedEncryptorConfiguration in the service container.</span></span>

---
