---
title: Extensibilidad de administración de claves en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de la extensibilidad de administración de claves de protección de datos de ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: 28932cbef1cc797338980f3e0de8b09caee324c0
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654263"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Extensibilidad de administración de claves en ASP.NET Core

> [!TIP]
> Lea la sección [Administración de claves](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) antes de leer esta sección, ya que se explican algunos de los conceptos fundamentales que hay detrás de estas API.

> [!WARNING]
> Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.

## <a name="key"></a>Clave

La interfaz de `IKey` es la representación básica de una clave en Cryptosystem. La clave de término se utiliza aquí en el sentido abstracto, no en el sentido de "material criptográfico clave" literal. Una clave tiene las siguientes propiedades:

* Fechas de expiración, la creación y activación

* Estado de revocación

* Identificador de clave (GUID)

::: moniker range=">= aspnetcore-2.0"

Además, `IKey` expone un método de `CreateEncryptor` que se puede utilizar para crear una instancia de [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) asociada a esta clave.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Además, `IKey` expone un método de `CreateEncryptorInstance` que se puede utilizar para crear una instancia de [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) asociada a esta clave.

::: moniker-end

> [!NOTE]
> No hay ninguna API para recuperar el material criptográfico sin formato de una instancia de `IKey`.

## <a name="ikeymanager"></a>IKeyManager

La interfaz de `IKeyManager` representa un objeto responsable del almacenamiento, la recuperación y la manipulación de claves generales. Expone tres operaciones de alto nivel:

* Cree una nueva clave y enviarlo al almacenamiento.

* Obtener todas las claves de almacenamiento.

* Revocar una o varias claves y conservar la información de revocación en el almacenamiento.

>[!WARNING]
> Escribir un `IKeyManager` es una tarea muy avanzada y la mayoría de los desarrolladores no deben intentar hacerlo. En su lugar, la mayoría de los desarrolladores deben aprovechar las ventajas de las instalaciones que ofrece la clase [XmlKeyManager](#xmlkeymanager) .

## <a name="xmlkeymanager"></a>XmlKeyManager

El tipo de `XmlKeyManager` es la implementación concreta del cuadro de `IKeyManager`. Proporciona varias utilidades útiles, incluida la custodia de clave y el cifrado de claves en reposo. Las claves de este sistema se representan como elementos XML (específicamente, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` depende de otros componentes en el transcurso de la realización de sus tareas:

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, que dicta los algoritmos usados por las nuevas claves.

* `IXmlRepository`, que controla dónde se conservan las claves en el almacenamiento.

* `IXmlEncryptor` [Optional], que permite cifrar las claves en reposo.

* `IKeyEscrowSink` [opcional], que proporciona servicios de custodia de claves.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, que controla dónde se conservan las claves en el almacenamiento.

* `IXmlEncryptor` [Optional], que permite cifrar las claves en reposo.

* `IKeyEscrowSink` [opcional], que proporciona servicios de custodia de claves.

::: moniker-end

A continuación se muestran diagramas de alto nivel que indican el modo en que estos componentes se conectan entre sí dentro de `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Creación de claves](key-management/_static/keycreation2.png)

*Creación de claves/CreateNewKey*

En la implementación de `CreateNewKey`, el componente de `AlgorithmConfiguration` se usa para crear un `IAuthenticatedEncryptorDescriptor`único, que luego se serializa como XML. Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo. A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado. Este documento cifrado se conserva en el almacenamiento a largo plazo a través de la `IXmlRepository`. (Si no se configura ningún `IXmlEncryptor`, el documento no cifrado se conserva en el `IXmlRepository`).

![Recuperación de clave](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Creación de claves](key-management/_static/keycreation1.png)

*Creación de claves/CreateNewKey*

En la implementación de `CreateNewKey`, el componente de `IAuthenticatedEncryptorConfiguration` se usa para crear un `IAuthenticatedEncryptorDescriptor`único, que luego se serializa como XML. Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo. A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado. Este documento cifrado se conserva en el almacenamiento a largo plazo a través de la `IXmlRepository`. (Si no se configura ningún `IXmlEncryptor`, el documento no cifrado se conserva en el `IXmlRepository`).

![Recuperación de clave](key-management/_static/keyretrieval1.png)

::: moniker-end

*Recuperación de clave/GetAllKeys*

En la implementación de `GetAllKeys`, los documentos XML que representan claves y revocación se leen del `IXmlRepository`subyacente. Si estos documentos están cifrados, el sistema les descifrará automáticamente. `XmlKeyManager` crea las instancias de `IAuthenticatedEncryptorDescriptorDeserializer` adecuadas para deserializar los documentos en `IAuthenticatedEncryptorDescriptor` instancias, que después se encapsulan en instancias de `IKey` individuales. Esta colección de instancias de `IKey` se devuelve al autor de la llamada.

Puede encontrar más información sobre los elementos XML concretos en el [documento formato de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

La interfaz `IXmlRepository` representa un tipo que puede conservar XML y recuperar XML de una memoria auxiliar. Expone dos API:

* `GetAllElements`:`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Las implementaciones de `IXmlRepository` no necesitan analizar el código XML que pasa a través de ellas. Deben tratar los documentos XML como opaco y permitir que los niveles superiores preocuparse de generar y analizar los documentos.

Hay cuatro tipos concretos integrados que implementan `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage. AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Consulte el [documento proveedores de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-providers) para obtener más información.

El registro de un `IXmlRepository` personalizado es adecuado cuando se usa un almacén de copia de seguridad diferente (por ejemplo, Azure Table Storage).

Para cambiar el repositorio predeterminado en toda la aplicación, registre una instancia de `IXmlRepository` personalizada:

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

## <a name="ixmlencryptor"></a>IXmlEncryptor

La interfaz `IXmlEncryptor` representa un tipo que puede cifrar un elemento XML de texto sin formato. Expone una única API:

* Cifrar (plaintextElement de XElement): EncryptedXmlInfo

Si un `IAuthenticatedEncryptorDescriptor` serializado contiene elementos marcados como "requiere cifrado", `XmlKeyManager` ejecutará esos elementos mediante el método `Encrypt` del `IXmlEncryptor`configurado y conservará el elemento cifrado en lugar del elemento de texto simple en el `IXmlRepository`. La salida del método `Encrypt` es un objeto `EncryptedXmlInfo`. Este objeto es un contenedor que contiene el `XElement` resultante cifrado y el tipo que representa una `IXmlDecryptor` que se puede utilizar para descifrar el elemento correspondiente.

Hay cuatro tipos concretos integrados que implementan `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Para obtener más información, consulte el [documento de cifrado de claves en reposo](xref:security/data-protection/implementation/key-encryption-at-rest) .

Para cambiar el mecanismo predeterminado de cifrado en reposo de la aplicación, registre una instancia de `IXmlEncryptor` personalizada:

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

## <a name="ixmldecryptor"></a>IXmlDecryptor

La interfaz de `IXmlDecryptor` representa un tipo que sabe cómo descifrar un `XElement` que se ha cifrado a través de un `IXmlEncryptor`. Expone una única API:

* Descifrar (encryptedElement de XElement): XElement

El método `Decrypt` deshace el cifrado realizado por `IXmlEncryptor.Encrypt`. Por lo general, cada implementación de `IXmlEncryptor` concreta tendrá una implementación de `IXmlDecryptor` concreta correspondiente.

Los tipos que implementan `IXmlDecryptor` deben tener uno de los dos constructores públicos siguientes:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> El `IServiceProvider` pasado al constructor puede ser null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

La interfaz de `IKeyEscrowSink` representa un tipo que puede realizar custodia de información confidencial. Recuerde que los descriptores serializados pueden contener información confidencial (por ejemplo, material criptográfico) y esto es lo que condujo a la introducción del tipo [IXmlEncryptor](#ixmlencryptor) en primer lugar. Sin embargo, los accidentes suceden y llaveros puede ser eliminados o dañados.

La interfaz de custodia proporciona un sombreado de escape de emergencia, lo que permite el acceso al XML serializado sin formato antes de que lo transforme cualquier [IXmlEncryptor](#ixmlencryptor)configurada. La interfaz expone una única API:

* Store (keyId Guid, elemento de XElement)

Depende de la implementación de `IKeyEscrowSink` controlar el elemento proporcionado de forma segura coherente con la Directiva empresarial. Una posible implementación podría ser que el receptor de custodia Cifre el elemento XML con un certificado X. 509 corporativo conocido en el que se haya custodiado la clave privada del certificado; el tipo de `CertificateXmlEncryptor` puede ayudarle. La implementación de `IKeyEscrowSink` también es responsable de conservar correctamente el elemento proporcionado.

De forma predeterminada, no hay ningún mecanismo de custodia habilitado, aunque los administradores del servidor pueden [configurar esto globalmente](xref:security/data-protection/configuration/machine-wide-policy). También se puede configurar mediante programación a través del método `IDataProtectionBuilder.AddKeyEscrowSink` como se muestra en el ejemplo siguiente. El método `AddKeyEscrowSink` sobrecarga el reflejo de las sobrecargas `IServiceCollection.AddSingleton` y `IServiceCollection.AddInstance`, ya que las instancias de `IKeyEscrowSink` están diseñadas para ser singleton. Si se registran varias instancias de `IKeyEscrowSink`, se llamará a cada una de ellas durante la generación de claves, por lo que las claves se pueden depositar en varios mecanismos simultáneamente.

No hay ninguna API para leer material de una instancia de `IKeyEscrowSink`. Esto es coherente con la teoría del diseño del mecanismo de custodia: se ha diseñado para que el material de clave sea accesible a una autoridad de confianza y, puesto que la aplicación propia no es una autoridad de confianza, no debería tener acceso a su propio material custodiada.

En el código de ejemplo siguiente se muestra cómo crear y registrar una `IKeyEscrowSink` donde las claves se guardan en custodia de forma que solo los miembros de "CONTOSODomain Admins" puedan recuperarlas.

> [!NOTE]
> Para ejecutar este ejemplo, debe estar en un dominio de Windows 8 / máquina con Windows Server 2012 y el controlador de dominio deben ser Windows Server 2012 o posterior.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
