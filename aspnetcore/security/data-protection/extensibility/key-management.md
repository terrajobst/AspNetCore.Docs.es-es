---
title: "Extensibilidad de administración de claves"
author: rick-anderson
description: "Este documento describen la extensibilidad de administración de claves de protección de datos de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 11/22/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/key-management
ms.openlocfilehash: d748ee9d3edf9eed4285fab447d5b379dfcd937c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
# <a name="key-management-extensibility"></a>Extensibilidad de administración de claves

<a name="data-protection-extensibility-key-management"></a>

>[!TIP]
> Leer la [administración de claves](../implementation/key-management.md#data-protection-implementation-key-management) sección antes de leer esta sección, tal y como se explican algunos de los conceptos fundamentales de estas API.

>[!WARNING]
> Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.

## <a name="key"></a>Key

La `IKey` interfaz es la representación básica de una clave en el sistema de cifrado. La clave de término se utiliza aquí en el sentido abstracto, no en el sentido de "material clave criptográfico" literal. Una clave tiene las siguientes propiedades:

* Fechas de expiración, la creación y la activación

* Estado de revocación

* Identificador de clave (GUID)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Además, `IKey` expone un `CreateEncryptor` método que se puede usar para crear un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Además, `IKey` expone un `CreateEncryptorInstance` método que se puede usar para crear un [IAuthenticatedEncryptor](core-crypto.md#data-protection-extensibility-core-crypto-iauthenticatedencryptor) instancia asociado a esta clave.

---

> [!NOTE]
> No hay ninguna API para recuperar el material criptográfico sin formato de un `IKey` instancia.

## <a name="ikeymanager"></a>IKeyManager

El `IKeyManager` interfaz representa un objeto responsable de almacenamiento de claves general, la recuperación y la manipulación. Expone tres operaciones de alto nivel:

* Cree una nueva clave y almacenar los datos en almacenamiento.

* Obtener todas las claves de almacenamiento.

* Revocar una o varias claves y conservar la información de revocación en el almacenamiento.

>[!WARNING]
> Escribir una `IKeyManager` una tarea muy avanzada y la mayoría de los desarrolladores no debe intentarlo. En su lugar, la mayoría de los desarrolladores deben aprovechar las ventajas de las funciones que ofrece el [XmlKeyManager](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-xmlkeymanager) clase.

<a name="data-protection-extensibility-key-management-xmlkeymanager"></a>

## <a name="xmlkeymanager"></a>XmlKeyManager

El `XmlKeyManager` tipo es la implementación concreta en el cuadro de `IKeyManager`. Proporciona varias funciones útiles, incluidas la custodia de clave y el cifrado de claves en reposo. Las claves en este sistema se representan como elementos XML (en concreto, [XElement](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/linq/xelement-class-overview)).

`XmlKeyManager`depende de otros componentes en el curso de cumplir sus tareas:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

* `AlgorithmConfiguration`, que determina los algoritmos utilizados por las nuevas claves.

* `IXmlRepository`, que controla donde las claves se conservan en almacenamiento.

* `IXmlEncryptor`[opcional], que permite cifrar las claves en reposo.

* `IKeyEscrowSink`[opcional], que proporciona servicios de custodia de clave.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `IXmlRepository`, que controla donde las claves se conservan en almacenamiento.

* `IXmlEncryptor`[opcional], que permite cifrar las claves en reposo.

* `IKeyEscrowSink`[opcional], que proporciona servicios de custodia de clave.

---

A continuación se muestran los diagramas de alto nivel que indican cómo se conectan juntos estos componentes en `XmlKeyManager`.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Creación de claves](key-management/_static/keycreation2.png)

   *Creación de clave / CreateNewKey*

En la implementación de `CreateNewKey`, `AlgorithmConfiguration` componente se utiliza para crear un nombre único `IAuthenticatedEncryptorDescriptor`, que, a continuación, se serializa como XML. Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo. A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado. Este documento cifrada se almacena en almacenamiento a largo plazo a través de la `IXmlRepository`. (Si no hay ningún `IXmlEncryptor` está configurado, se guarda el documento sin cifrar en la `IXmlRepository`.)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Creación de claves](key-management/_static/keycreation1.png)

   *Creación de clave / CreateNewKey*

En la implementación de `CreateNewKey`, `IAuthenticatedEncryptorConfiguration` componente se utiliza para crear un nombre único `IAuthenticatedEncryptorDescriptor`, que, a continuación, se serializa como XML. Si un receptor de custodia de clave está presente, el XML sin formato (sin cifrar) se proporciona al receptor de almacenamiento a largo plazo. A continuación, se ejecuta el XML sin cifrar a través de un `IXmlEncryptor` (si es necesario) para generar el documento XML cifrado. Este documento cifrada se almacena en almacenamiento a largo plazo a través de la `IXmlRepository`. (Si no hay ningún `IXmlEncryptor` está configurado, se guarda el documento sin cifrar en la `IXmlRepository`.)

---

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ![Recuperación de claves](key-management/_static/keyretrieval2.png)
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ![Recuperación de claves](key-management/_static/keyretrieval1.png)

---

   *Recuperación de clave / GetAllKeys*

En la implementación de `GetAllKeys`, el XML documenta las claves que representan y se leen las revocaciones de subyacente `IXmlRepository`. Si estos documentos están cifrados, el sistema les descifrará automáticamente. `XmlKeyManager`crea la correspondiente `IAuthenticatedEncryptorDescriptorDeserializer` instancias para deserializar los documentos de nuevo en `IAuthenticatedEncryptorDescriptor` instancias, que, a continuación, se incluyen en persona `IKey` instancias. Esta colección de `IKey` instancias se devuelve al llamador.

Encontrará más información sobre los elementos XML determinados en el [documento de formato de almacenamiento de claves](../implementation/key-storage-format.md#data-protection-implementation-key-storage-format).

## <a name="ixmlrepository"></a>IXmlRepository

El `IXmlRepository` interfaz representa un tipo que pueda persista código XML en y recuperar el XML de un almacén de copia de seguridad. Expone dos API:

* GetAllElements() : IReadOnlyCollection<XElement>

* StoreElement (elemento XElement, cadena friendlyName)

Las implementaciones de `IXmlRepository` no es necesario analizar el XML que se pasan a través de ellos. Debe tratar los documentos XML como opaco y permitir que los niveles superiores a preocuparse sobre cómo generar y analizar los documentos.

Hay dos tipos integrados concretos que implementan `IXmlRepository`: `FileSystemXmlRepository` y `RegistryXmlRepository`. Consulte la [documento de proveedores de almacenamiento de claves](../implementation/key-storage-providers.md#data-protection-implementation-key-storage-providers) para obtener más información. Registrar un personalizado `IXmlRepository` sería la manera adecuada para usar un almacén de respaldo diferentes, por ejemplo, el almacenamiento de blobs de Azure.

Para cambiar el repositorio predeterminado de toda la aplicación, registrar un personalizado `IXmlRepository` instancia:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlRepository = new MyCustomXmlRepository());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlRepository>(new MyCustomXmlRepository());
   ```

---

<a name="data-protection-extensibility-key-management-ixmlencryptor"></a>

## <a name="ixmlencryptor"></a>IXmlEncryptor

El `IXmlEncryptor` interfaz representa un tipo que puede cifrar un elemento XML de texto simple. Expone una única API:

* Cifrar (plaintextElement de XElement): EncryptedXmlInfo

Si un número de serie `IAuthenticatedEncryptorDescriptor` contiene elementos marcados como "requiere cifrado", a continuación, `XmlKeyManager` ejecutará esos elementos a través de la configurada `IXmlEncryptor`del `Encrypt` método y se conservará el elemento descifra en lugar de la elemento de texto simple para el `IXmlRepository`. El resultado de la `Encrypt` método es un `EncryptedXmlInfo` objeto. Este objeto es un contenedor que contiene tanto el resultante descifra `XElement` y el tipo que representa un `IXmlDecryptor` que puede utilizarse para descifrar el elemento correspondiente.

Hay cuatro tipos integrados concretos que implementan `IXmlEncryptor`:
* `CertificateXmlEncryptor`
* `DpapiNGXmlEncryptor`
* `DpapiXmlEncryptor`
* `NullXmlEncryptor`

Consulte la [cifrado de clave en el documento de rest](../implementation/key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest) para obtener más información.

Para cambiar el mecanismo de cifrado de clave en el resto de predeterminado de toda la aplicación, registrar un personalizado `IXmlEncryptor` instancia:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

   ```csharp
   services.Configure<KeyManagementOptions>(options => options.XmlEncryptor = new MyCustomXmlEncryptor());
   ```
   
# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

   ```csharp
   services.AddSingleton<IXmlEncryptor>(new MyCustomXmlEncryptor());
   ```

---

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

El `IKeyEscrowSink` interfaz representa un tipo que puede realizar la custodia de la información confidencial. Recuerde que descriptores serializados podrían contener información confidencial (por ejemplo, el material criptográfico) y esto es lo que ha provocado la introducción de la [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor) escriba en primer lugar. Sin embargo, los accidentes y llaveros pueden eliminarse o están dañados.

La interfaz de custodia proporciona una trama de escape de emergencia, permitir el acceso al XML serializado sin procesar antes de que se transforme en ninguno configurado [IXmlEncryptor](xref:security/data-protection/extensibility/key-management#data-protection-extensibility-key-management-ixmlencryptor). La interfaz expone una única API:

* Almacén (keyId de Guid, elemento de XElement)

Depende del `IKeyEscrowSink` implementación para controlar el elemento proporcionado de forma segura coherente con la directiva empresarial. Una posible implementación podría ser para que el receptor de custodia cifrar el elemento XML mediante un certificado X.509 corporativo conocido donde se ha custodiado clave privada del certificado; el `CertificateXmlEncryptor` tipo puede ayudarle con esto. El `IKeyEscrowSink` implementación también es responsable de conservar el elemento proporcionado de forma adecuada.

De forma predeterminada ningún mecanismo de custodia está habilitado, aunque los administradores de servidor pueden [configurarlo global](xref:security/data-protection/configuration/machine-wide-policy). También puede configurar mediante programación a través de la `IDataProtectionBuilder.AddKeyEscrowSink` método tal como se muestra en el ejemplo siguiente. El `AddKeyEscrowSink` reflejado de las sobrecargas de método la `IServiceCollection.AddSingleton` y `IServiceCollection.AddInstance` sobrecargas, como `IKeyEscrowSink` instancias están pensadas para ser singletons. Si hay varios `IKeyEscrowSink` instancias registradas, se llamará a cada uno de ellos durante la generación de claves, por lo que las claves se pueden custodiar a varios mecanismos simultáneamente.

No hay ninguna API para leer el material de un `IKeyEscrowSink` instancia. Esto es coherente con la teoría del diseño del mecanismo de custodia: se ha diseñado para hacer que el material de clave sea accesible a una autoridad de confianza y, puesto que la aplicación de sí mismo no es una entidad de confianza, no debería tener acceso a su propio material custodiada.

El código de ejemplo siguiente muestra cómo crear y registrar un `IKeyEscrowSink` donde se custodiar claves de modo que solo los miembros del "CONTOSODomain Admins" pueden recuperarlos.

> [!NOTE]
> Para ejecutar este ejemplo, debe ser en un equipo con Windows 8 Unidos a un dominio / máquina con Windows Server 2012 y el controlador de dominio deben ser Windows Server 2012 o posterior.

[!code-csharp[Main](key-management/samples/key-management-extensibility.cs)]
