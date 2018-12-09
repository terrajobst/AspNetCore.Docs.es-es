---
title: Extensibilidad de administración de claves en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de la extensibilidad de administración de claves de protección de datos de ASP.NET Core.
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/24/2018
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: e5ed2a65355a1dba34af09379f2583b3e73c24d7
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121432"
---
# <a name="key-management-extensibility-in-aspnet-core"></a>Extensibilidad de administración de claves en ASP.NET Core

> [!TIP]
> Leer el [administración de claves](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) sección antes de leer esta sección, ya que explica algunos de los conceptos fundamentales de estas API.

> [!WARNING]
> Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.

## <a name="key"></a>Key

El `IKey` interfaz es la representación básica de una clave en los sistemas de cifrado. La clave de término se utiliza aquí en el sentido abstracto, no en el sentido de "material criptográfico clave" literal. Una clave tiene las siguientes propiedades:

* Fechas de expiración, la creación y activación

* Estado de revocación

* Identificador de clave (GUID)

::: moniker range=">= aspnetcore-2.0"

Además, `IKey` expone un `CreateEncryptor` método que se puede usar para crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Además, `IKey` expone un `CreateEncryptorInstance` método que se puede usar para crear un [IAuthenticatedEncryptor](xref:security/data-protection/extensibility/core-crypto#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.

::: moniker-end

> [!NOTE]
> No hay ninguna API para recuperar el material criptográfico sin procesar de un `IKey` instancia.

## <a name="ikeymanager"></a>IKeyManager

El `IKeyManager` interfaz representa un objeto responsable de almacenamiento de claves general, la recuperación y manipulación. Expone tres operaciones de alto nivel:

* Cree una nueva clave y enviarlo al almacenamiento.

* Obtener todas las claves de almacenamiento.

* Revocar una o varias claves y conservar la información de revocación en el almacenamiento.

>[!WARNING]
> Escribir un `IKeyManager` una tarea muy avanzada y la mayoría de los desarrolladores no debería intentar realizar. En su lugar, la mayoría de los desarrolladores deben sacar partido de las funciones que ofrece el [XmlKeyManager](#xmlkeymanager) clase.

## <a name="xmlkeymanager"></a>XmlKeyManager

El `XmlKeyManager` es de tipo de la implementación concreta en el cuadro de `IKeyManager`. Proporciona varias utilidades útiles, incluida la custodia de clave y el cifrado de claves en reposo. Las claves en este sistema se representan como elementos XML (en concreto, [XElement](/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager` depende de otros componentes en el transcurso de cumplimiento de sus tareas:

::: moniker range=">= aspnetcore-2.0"

* `AlgorithmConfiguration`, que dicta los algoritmos usados por las nuevas claves.

* `IXmlRepository`, que controla donde las claves se conservan en almacenamiento.

* `IXmlEncryptor` [opcional], lo que permite cifrar las claves en reposo.

* `IKeyEscrowSink` [opcional], que proporciona servicios de custodia de clave.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* `IXmlRepository`, que controla donde las claves se conservan en almacenamiento.

* `IXmlEncryptor` [opcional], lo que permite cifrar las claves en reposo.

* `IKeyEscrowSink` [opcional], que proporciona servicios de custodia de clave.

::: moniker-end

A continuación se muestran los diagramas de alto nivel que indican cómo estos componentes se conectan entre sí dentro de `XmlKeyManager`.

::: moniker range=">= aspnetcore-2.0"

![Creación de claves](key-management/_static/keycreation2.png)

*Creación de clave / CreateNewKey*

En la implementación de `CreateNewKey`, `AlgorithmConfiguration` componente se utiliza para crear un nombre único `IAuthenticatedEncryptorDescriptor`, que, a continuación, se serializa como XML. Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo. A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado. Este documento cifrada se almacena en almacenamiento a largo plazo a través de la `IXmlRepository`. (Si no hay ningún `IXmlEncryptor` está configurado, se conserva en el documento sin cifrar el `IXmlRepository`.)

![Recuperación de clave](key-management/_static/keyretrieval2.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

![Creación de claves](key-management/_static/keycreation1.png)

*Creación de clave / CreateNewKey*

En la implementación de `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` componente se utiliza para crear un nombre único `IAuthenticatedEncryptorDescriptor`, que, a continuación, se serializa como XML. Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo. A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado. Este documento cifrada se almacena en almacenamiento a largo plazo a través de la `IXmlRepository`. (Si no hay ningún `IXmlEncryptor` está configurado, se conserva en el documento sin cifrar el `IXmlRepository`.)

![Recuperación de clave](key-management/_static/keyretrieval1.png)

::: moniker-end

*Recuperación de clave / GetAllKeys*

En la implementación de `GetAllKeys`, el XML que representa claves documenta y se leen las revocaciones de subyacente `IXmlRepository`. Si estos documentos están cifrados, el sistema les descifrará automáticamente. `XmlKeyManager` crea la adecuada `IAuthenticatedEncryptorDescriptorDeserializer` instancias para deserializar los documentos de nuevo en `IAuthenticatedEncryptorDescriptor` instancias, que, a continuación, se incluyen en persona `IKey` instancias. Esta colección de `IKey` instancias se devuelve al llamador.

Encontrará más información sobre los elementos XML determinados en el [documento con formato de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-format#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

El `IXmlRepository` interfaz representa un tipo que puede conservar XML y recuperar el XML desde un almacén de respaldo. Expone dos API:

* `GetAllElements` :`IReadOnlyCollection<XElement>`

* `StoreElement(XElement element, string friendlyName)`

Las implementaciones de `IXmlRepository` no es necesario analizar el XML que se pasa a través de ellos. Deben tratar los documentos XML como opaco y permitir que los niveles superiores preocuparse de generar y analizar los documentos.

Hay cuatro tipos concretos integrados que implementan `IXmlRepository`:

::: moniker range=">= aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.stackexchangeredis.redisxmlrepository)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [FileSystemXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.filesystemxmlrepository)
* [RegistryXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.repositories.registryxmlrepository)
* [AzureStorage.AzureBlobXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.azurestorage.azureblobxmlrepository)
* [RedisXmlRepository](/dotnet/api/microsoft.aspnetcore.dataprotection.redisxmlrepository)

::: moniker-end

Consulte la [documento de proveedores de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-providers) para obtener más información.

Registrar un personalizado `IXmlRepository` es adecuado cuando se usa un almacén de respaldo diferentes (por ejemplo, Azure Table Storage).

Para cambiar el repositorio predeterminado de toda la aplicación, registrar un personalizado `IXmlRepository` instancia:

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

El `IXmlEncryptor` interfaz representa un tipo que puede cifrar un elemento XML de texto simple. Expone una única API:

* Cifrar (plaintextElement de XElement): EncryptedXmlInfo

Si un serializador `IAuthenticatedEncryptorDescriptor` contiene todos los elementos marcados como "requiere cifrado", a continuación, `XmlKeyManager` ejecutará esos elementos a través de la configurada `IXmlEncryptor`del `Encrypt` método y conservarán el elemento cifrado en lugar de elemento de texto simple a la `IXmlRepository`. La salida de la `Encrypt` método es un `EncryptedXmlInfo` objeto. Este objeto es un contenedor que contiene tanto el resultante cifrado `XElement` y el tipo que representa un `IXmlDecryptor` que puede utilizarse para descifrar el elemento correspondiente.

Hay cuatro tipos concretos integrados que implementan `IXmlEncryptor`:

* [CertificateXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.certificatexmlencryptor)
* [DpapiNGXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapingxmlencryptor)
* [DpapiXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.dpapixmlencryptor)
* [NullXmlEncryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.nullxmlencryptor)

Consulte la [cifrado de claves en el documento de rest](xref:security/data-protection/implementation/key-encryption-at-rest) para obtener más información.

Para cambiar el mecanismo predeterminado de la clave de cifrado en reposo toda la aplicación, registrar un personalizado `IXmlEncryptor` instancia:

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

El `IXmlDecryptor` interfaz representa un tipo que sabe cómo descifrar un `XElement` que se descifra mediante una `IXmlEncryptor`. Expone una única API:

* Descifrar (encryptedElement de XElement): XElement

El `Decrypt` método deshace el cifrado realizado por `IXmlEncryptor.Encrypt`. Por lo general, cada hormigón `IXmlEncryptor` implementación tendrá un hormigón correspondiente `IXmlDecryptor` implementación.

Los tipos que implementan `IXmlDecryptor` debe tener uno de los dos constructores públicos siguientes:

* .ctor(IServiceProvider)
* .ctor()

> [!NOTE]
> El `IServiceProvider` pasado al constructor puede ser null.

## <a name="ikeyescrowsink"></a>IKeyEscrowSink

El `IKeyEscrowSink` interfaz representa un tipo que puede realizar la custodia de información confidencial. Recuerde que los descriptores de serializado podrían contener información confidencial (por ejemplo, el material criptográfico) y esto es lo que llevó a la introducción de la [IXmlEncryptor](#ixmlencryptor) escriba en primer lugar. Sin embargo, los accidentes suceden y llaveros puede ser eliminados o dañados.

La interfaz de custodia proporciona un sombreado de escape de emergencia, que permite el acceso al XML serializado sin procesar antes de que se transforme alguno configurado [IXmlEncryptor](#ixmlencryptor). La interfaz expone una única API:

* Store (keyId Guid, elemento de XElement)

Es hasta el `IKeyEscrowSink` implementación para controlar el elemento proporcionado de forma segura coherente con la directiva empresarial. Una posible implementación podría ser para que el receptor de custodia cifrar el elemento XML con un certificado X.509 corporativo conocido que se ha custodiado clave privada del certificado; el `CertificateXmlEncryptor` tipo puede ayudar con esto. El `IKeyEscrowSink` implementación también es responsable de conservar el elemento proporcionado de forma adecuada.

De forma predeterminada no está habilitado ningún mecanismo de custodia, aunque los administradores de servidor pueden [configurar esto globalmente](xref:security/data-protection/configuration/machine-wide-policy). También se puede configurar mediante programación a través de la `IDataProtectionBuilder.AddKeyEscrowSink` método tal como se muestra en el ejemplo siguiente. El `AddKeyEscrowSink` reflejado sobrecargas de método la `IServiceCollection.AddSingleton` y `IServiceCollection.AddInstance` sobrecargas, como `IKeyEscrowSink` instancias están pensadas para formar singletons con estado. Si hay varios `IKeyEscrowSink` se registran las instancias, cada uno de ellos se llamará durante la generación de claves, por lo que las claves se pueden custodiar a varios mecanismos simultáneamente.

No hay ninguna API para leer el material de un `IKeyEscrowSink` instancia. Esto es coherente con la teoría del diseño del mecanismo de custodia: se ha diseñado para que el material de clave sea accesible a una autoridad de confianza y, puesto que la aplicación propia no es una autoridad de confianza, no debería tener acceso a su propio material custodiada.

Ejemplo de código siguiente muestra cómo crear y registrar un `IKeyEscrowSink` donde se puede custodiar claves tal que solo los miembros del "CONTOSODomain Admins" pueden recuperarlos.

> [!NOTE]
> Para ejecutar este ejemplo, debe estar en un dominio de Windows 8 / máquina con Windows Server 2012 y el controlador de dominio deben ser Windows Server 2012 o posterior.

[!code-csharp[](key-management/samples/key-management-extensibility.cs)]
