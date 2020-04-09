---
title: Formato de almacenamiento clave en ASP.NET Core
author: rick-anderson
description: Conozca los detalles de implementación del formato de almacenamiento de claves ASP.NET Core Data Protection.
ms.author: riande
ms.date: 04/08/2020
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 3072c673791b589027a910b80eaba52052eb9311
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80976942"
---
# <a name="key-storage-format-in-aspnet-core"></a>Formato de almacenamiento clave en ASP.NET Core

<a name="data-protection-implementation-key-storage-format"></a>

Los objetos se almacenan en reposo en la representación XML. El directorio predeterminado para el almacenamiento de claves es:

* Windows: *%LOCALAPPDATA%-ASP.NET-DataProtection-Keys\*
* macOS / Linux: *$HOME/.aspnet/DataProtection-Keys*

## <a name="the-key-element"></a>El \<elemento clave>

Las claves existen como objetos de nivel superior en el repositorio de claves. Por las claves de convención tienen la clave de nombre de **archivo-- .xml**, donde el identificador de la clave es el identificador. Cada archivo contiene una sola clave. El formato del archivo es el siguiente.

```xml
<?xml version="1.0" encoding="utf-8"?>
<key id="80732141-ec8f-4b80-af9c-c4d2d1ff8901" version="1">
  <creationDate>2015-03-19T23:32:02.3949887Z</creationDate>
  <activationDate>2015-03-19T23:32:02.3839429Z</activationDate>
  <expirationDate>2015-06-17T23:32:02.3839429Z</expirationDate>
  <descriptor deserializerType="{deserializerType}">
    <descriptor>
      <encryption algorithm="AES_256_CBC" />
      <validation algorithm="HMACSHA256" />
      <enc:encryptedSecret decryptorType="{decryptorType}" xmlns:enc="...">
        <encryptedKey>
          <!-- This key is encrypted with Windows DPAPI. -->
          <value>AQAAANCM...8/zeP8lcwAg==</value>
        </encryptedKey>
      </enc:encryptedSecret>
    </descriptor>
  </descriptor>
</key>
```

El \<elemento> contiene los siguientes atributos y elementos secundarios:

* La identificación de la llave. Este valor se trata como autoritativo; el nombre de archivo es simplemente una amabilidad para la legibilidad humana.

* La versión \<del elemento de> clave, actualmente fija da 1.

* Fechas de creación, activación y vencimiento de la clave.

* Un \<descriptor> elemento, que contiene información sobre la implementación de cifrado autenticada contenida en esta clave.

En el ejemplo anterior, el identificador de la clave es 80732141-ec8f-4b80-af9c-c4d2d1ff8901, se creó y activó el 19 de marzo de 2015, y tiene una vida útil de 90 días. (Ocasionalmente, la fecha de activación puede ser ligeramente anterior a la fecha de creación como en este ejemplo. Esto se debe a una nit en el funcionamiento de las API y es inofensivo en la práctica.)

## <a name="the-descriptor-element"></a>El \<descriptor> elemento

El \<descriptor externo> elemento contiene un atributo deserializerType, que es el nombre calificado del ensamblado de un tipo que implementa IAuthenticatedEncryptorDescriptorDeserializer. Este tipo es responsable \<de leer el descriptor interno> elemento y de analizar la información contenida en.

El formato concreto \<del descriptor> elemento depende de la implementación del cifrador autenticado encapsulado por la clave, y cada tipo de deserializador espera un formato ligeramente diferente para esto. En general, sin embargo, este elemento contendrá información algorítmica (nombres, tipos, OID o similares) y material de clave secreta. En el ejemplo anterior, el descriptor especifica que esta clave ajusta el cifrado AES-256-CBC + validación HMACSHA256.

## <a name="the-encryptedsecret-element"></a>El \<elemento de> encryptedSecret

Un ** &lt;elemento&gt; encryptedSecret** que contiene la forma cifrada del material de clave secreta puede estar presente si [el cifrado de secretos en reposo está habilitado.](xref:security/data-protection/implementation/key-encryption-at-rest) El `decryptorType` atributo es el nombre completo del ensamblado de un tipo que implementa [IXmlDecryptor](/dotnet/api/microsoft.aspnetcore.dataprotection.xmlencryption.ixmldecryptor). Este tipo es responsable de leer el elemento ** &lt;cifrado&gt; ** cifrado interno y descifrarlo para recuperar el texto sin formato original.

Al `<descriptor>`igual que con `<encryptedSecret>` , el formato particular del elemento depende del mecanismo de cifrado en reposo en uso. En el ejemplo anterior, la clave maestra se cifra mediante DPAPI de Windows por el comentario.

## <a name="the-revocation-element"></a>El \<elemento de revocación>

Las revocaciones existen como objetos de nivel superior en el repositorio de claves. Por convención, las revocaciones tienen el nombre de archivo **revocation--timestamp-.xml** (para revocar todas las claves antes de una fecha específica) o **la revocación-- .xml** (para revocar una clave específica). Cada archivo contiene \<un único elemento de revocación>.

Para las revocaciones de claves individuales, el contenido del archivo será el siguiente.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

En este caso, solo se revoca la clave especificada. Sin embargo, si el identificador de clave es "*", como en el ejemplo siguiente, se revocaron todas las claves cuya fecha de creación sea anterior a la fecha de revocación especificada.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

El \<sistema nunca lee> elemento. Es simplemente un lugar conveniente para almacenar una razón legible para la revocación.
