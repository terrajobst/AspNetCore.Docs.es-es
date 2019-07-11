---
title: Extensibilidad de criptografía de núcleo en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre IAuthenticatedEncryptor, IAuthenticatedEncryptorDescriptor, IAuthenticatedEncryptorDescriptorDeserializer y el generador de nivel superior.
ms.author: riande
ms.date: 08/11/2017
uid: security/data-protection/extensibility/core-crypto
ms.openlocfilehash: a5f651e3313cc579b995b45905826a5bffcc241c
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814702"
---
# <a name="core-cryptography-extensibility-in-aspnet-core"></a>Extensibilidad de criptografía de núcleo en ASP.NET Core

<a name="data-protection-extensibility-core-crypto"></a>

>[!WARNING]
> Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptor"></a>

## <a name="iauthenticatedencryptor"></a>IAuthenticatedEncryptor

El **IAuthenticatedEncryptor** interfaz es el bloque de creación básico del subsistema criptográfico. Generalmente es una IAuthenticatedEncryptor por clave, y la instancia IAuthenticatedEncryptor encapsula toda la información algorítmica necesaria para realizar operaciones criptográficas y material clave criptográfico.

Como sugiere su nombre, el tipo es responsable de proporcionar servicios de cifrado y descifrado autenticados. Expone las siguientes dos API.

* `Decrypt(ArraySegment<byte> ciphertext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

* `Encrypt(ArraySegment<byte> plaintext, ArraySegment<byte> additionalAuthenticatedData) : byte[]`

El método Encrypt devuelve un blob que incluye el texto no cifrado cifrado y una etiqueta de autenticación. La etiqueta de autenticación debe incluir los datos adicionales de autenticado (AAD), aunque el propio AAD no se debe recuperar de la carga final. El método Decrypt valida la etiqueta a la autenticación y devuelve la carga deciphered. Todos los errores (excepto ArgumentNullException y similares) deben homogeneizarse a CryptographicException.

> [!NOTE]
> La propia instancia IAuthenticatedEncryptor realmente no tiene que contener el material de clave. Por ejemplo, la implementación puede delegar a un HSM para todas las operaciones.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptorfactory"></a>
<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor"></a>

## <a name="how-to-create-an-iauthenticatedencryptor"></a>Cómo crear un IAuthenticatedEncryptor

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El **IAuthenticatedEncryptorFactory** interfaz representa un tipo que sabe cómo crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia. Su API es como sigue.

* CreateEncryptorInstance (clave IKey): IAuthenticatedEncryptor

Para cualquier instancia dada de IKey, cualquier rechazarán autenticado creados por su método CreateEncryptorInstance debe considerarse equivalente, como en el ejemplo de código siguiente.

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El **IAuthenticatedEncryptorDescriptor** interfaz representa un tipo que sabe cómo crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia. Su API es como sigue.

* CreateEncryptorInstance() : IAuthenticatedEncryptor

* ExportToXml(): XmlSerializedDescriptorInfo

Al igual que IAuthenticatedEncryptor, se supone que una instancia de IAuthenticatedEncryptorDescriptor para encapsular una clave específica. Esto significa que para cualquier instancia IAuthenticatedEncryptorDescriptor, cualquier rechazarán autenticado creados por su método CreateEncryptorInstance debe considerarse equivalente, como en el ejemplo de código siguiente.

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

## <a name="iauthenticatedencryptordescriptor-aspnet-core-2x-only"></a>IAuthenticatedEncryptorDescriptor (ASP.NET Core 2.x solo)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El **IAuthenticatedEncryptorDescriptor** interfaz representa un tipo que sabe cómo exportar a XML. Su API es como sigue.

* ExportToXml(): XmlSerializedDescriptorInfo

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

---

## <a name="xml-serialization"></a>Serialización XML

La diferencia principal entre IAuthenticatedEncryptor y IAuthenticatedEncryptorDescriptor es que el descriptor sabe cómo crear el sistema de cifrado y suministrarle los argumentos válidos. Considere la posibilidad de un IAuthenticatedEncryptor cuya implementación se basa en SymmetricAlgorithm y KeyedHashAlgorithm. Trabajo del sistema de cifrado es consumen estos tipos, pero no sabe necesariamente estos tipos de proceden, por lo que realmente no se puede escribir una descripción de cómo volver a sí mismo si se reinicia la aplicación adecuada. El descriptor actúa como un nivel más alto por encima de esto. Puesto que el descriptor sabe cómo crear la instancia de sistema de cifrado (p. ej., sabe cómo crear los algoritmos necesarios), puede serializar ese conocimiento en formato XML para que la instancia de sistema de cifrado se puede volver a crear después una aplicación de restablecimiento.

<a name="data-protection-extensibility-core-crypto-exporttoxml"></a>

El descriptor se puede serializar a través de su rutina ExportToXml. Esta rutina devuelve un XmlSerializedDescriptorInfo que contiene dos propiedades: la representación de XElement de descriptor y el tipo que representa un [IAuthenticatedEncryptorDescriptorDeserializer](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer) que puede ser se usa para resucitar este descriptor dada la XElement correspondiente.

El descriptor serializado puede contener información confidencial, como material de clave criptográfica. El sistema de protección de datos tiene compatibilidad integrada para cifrar la información antes de que se conserva en el almacenamiento. Para aprovechar las ventajas de esto, el descriptor debe marcar el elemento que contiene información confidencial con el nombre de atributo "requiresEncryption" (xmlns "<http://schemas.asp.net/2015/03/dataProtection>"), valor "true".

>[!TIP]
> Hay una API auxiliar para establecer este atributo. Llame al método de extensión que XElement.markasrequiresencryption() ubicado en el espacio de nombres Microsoft.AspNetCore.DataProtection.AuthenticatedEncryption.ConfigurationModel.

También puede haber casos donde el descriptor serializado no contiene información confidencial. Considere el caso de una clave criptográfica que se almacenan en un HSM de nuevo. El descriptor no se puede escribir el material de clave al serializar propio dado que el HSM no expone el material en texto sin formato. En su lugar, puede escribir el descriptor de la versión de encapsulado de clave de la clave (si el HSM permite la exportación de este modo) o el identificador único del HSM para la clave.

<a name="data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptordeserializer"></a>

## <a name="iauthenticatedencryptordescriptordeserializer"></a>IAuthenticatedEncryptorDescriptorDeserializer

El **IAuthenticatedEncryptorDescriptorDeserializer** interfaz representa un tipo que sabe cómo deserializar una instancia de IAuthenticatedEncryptorDescriptor de un XElement. Expone un único método:

* ImportFromXml (elemento de XElement): IAuthenticatedEncryptorDescriptor

El método ImportFromXml toma el XElement que devolvió [IAuthenticatedEncryptorDescriptor.ExportToXml](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-exporttoxml) y crea un equivalente de la IAuthenticatedEncryptorDescriptor original.

Tipos que implementan IAuthenticatedEncryptorDescriptorDeserializer deben tener uno de los dos constructores públicos siguientes:

* .ctor(IServiceProvider)

* .ctor()

> [!NOTE]
> El elemento IServiceProvider pasado al constructor puede ser null.

## <a name="the-top-level-factory"></a>El generador de nivel superior

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

El **AlgorithmConfiguration** clase representa un tipo que sabe cómo crear [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instancias. Expone una sola API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Piense en AlgorithmConfiguration como el generador de nivel superior. La configuración actúa como una plantilla. Encapsula información algorítmico (p. ej., esta configuración produce descriptores con una clave maestra de AES-128-GCM), pero aún no está asociada con una clave específica.

Cuando se llama a CreateNewDescriptor, nuevo material de clave se crea únicamente para esta llamada y se genera un nuevo IAuthenticatedEncryptorDescriptor que ajusta este material de clave y la información algorítmica necesarios para consumir el material. El material de clave podría creó en software (y se mantiene en memoria), podría crearse y contenido en un HSM y así sucesivamente. El punto fundamental es que las dos llamadas a CreateNewDescriptor nunca deben crear instancias de IAuthenticatedEncryptorDescriptor equivalente.

El tipo AlgorithmConfiguration actúa como punto de entrada para las rutinas de creación de claves como [clave automatic gradual](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Para cambiar la implementación para todas las claves de futuras, establezca la propiedad AuthenticatedEncryptorConfiguration en KeyManagementOptions.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

El **IAuthenticatedEncryptorConfiguration** interfaz representa un tipo que sabe cómo crear [IAuthenticatedEncryptorDescriptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptordescriptor) instancias. Expone una sola API.

* CreateNewDescriptor() : IAuthenticatedEncryptorDescriptor

Piense en IAuthenticatedEncryptorConfiguration como el generador de nivel superior. La configuración actúa como una plantilla. Encapsula información algorítmico (p. ej., esta configuración produce descriptores con una clave maestra de AES-128-GCM), pero aún no está asociada con una clave específica.

Cuando se llama a CreateNewDescriptor, nuevo material de clave se crea únicamente para esta llamada y se genera un nuevo IAuthenticatedEncryptorDescriptor que ajusta este material de clave y la información algorítmica necesarios para consumir el material. El material de clave podría creó en software (y se mantiene en memoria), podría crearse y contenido en un HSM y así sucesivamente. El punto fundamental es que las dos llamadas a CreateNewDescriptor nunca deben crear instancias de IAuthenticatedEncryptorDescriptor equivalente.

El tipo IAuthenticatedEncryptorConfiguration actúa como punto de entrada para las rutinas de creación de claves como [clave automatic gradual](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling). Para cambiar la implementación para todas las claves futuras, registre un singleton IAuthenticatedEncryptorConfiguration en el contenedor de servicios.

---
