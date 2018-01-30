---
title: Formato de almacenamiento de claves
author: tdykstra
description: "Este documento explica los detalles de implementación del formato de almacenamiento de claves de protección de datos de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-format
ms.openlocfilehash: 66783eb7264a4551eafdd9d5c7d99b014701a6de
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="key-storage-format"></a>Formato de almacenamiento de claves

<a name="data-protection-implementation-key-storage-format"></a>

Objetos se almacenan en reposo en la representación XML. El directorio predeterminado para el almacenamiento de claves es % LOCALAPPDATA%\ASP.NET\DataProtection-Keys\.

## <a name="the-key-element"></a>El \<clave > elemento

Las claves existen como objetos de nivel superior en el repositorio de clave. Por convención, las claves tienen el nombre de archivo **clave-{guid} .xml**, donde {guid} es el identificador de la clave. Cada archivo de este tipo contiene una clave única. El formato del archivo es como sigue.

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

El \<clave > elemento contiene los siguientes atributos y elementos secundarios:

* El identificador de clave. Este valor se trata como autorizados; el nombre de archivo es simplemente una nicety más legible.

* La versión de la \<clave > elemento, que actualmente se fija en 1.

* Fecha de creación, activación y expiración de la clave.

* Un \<descriptor > elemento, que contiene información sobre la implementación del cifrado autenticado dentro de esta clave.

En el ejemplo anterior, Id. de la clave es {80732141-ec8f-4b80-af9c-c4d2d1ff8901}, que se creó o se activa en el 19 de marzo de 2015 y tiene una duración de 90 días. (En ocasiones, la fecha de activación puede estar ligeramente antes de la fecha de creación como en este ejemplo. Esto es debido a un nit en la forma en que las API de trabajo y es inofensivas en la práctica).

## <a name="the-descriptor-element"></a>El \<descriptor > elemento

El exterior \<descriptor > elemento contiene un deserializerType de atributo, que es el nombre calificado con el ensamblado de un tipo que implementa IAuthenticatedEncryptorDescriptorDeserializer. Este tipo es responsable de leer interna \<descriptor > elemento y para analizar la información incluida en.

El formato en cuestión de la \<descriptor > elemento depende de la implementación de sistema de cifrado autenticado encapsulada por la clave y cada tipo de deserializador espera un formato ligeramente diferente para este. En general, no obstante, este elemento contendrá información algorítmica (nombres, tipos, OID, o similar) y material de clave secreta. En el ejemplo anterior, el descriptor especifica que ajuste esta clave de cifrado de AES-256-CBC + HMACSHA256 validación.

## <a name="the-encryptedsecret-element"></a>El \<encryptedSecret > elemento

Un <encryptedSecret> elemento que contiene la forma cifrada del material de clave secreta puede estar presente si [está habilitado el cifrado de secretos en reposo](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest). El atributo decryptorType será el nombre calificado con el ensamblado de un tipo que implementa IXmlDecryptor. Este tipo es responsable de leer interna <encryptedKey> elemento y el descifrado para recuperar el texto sin formato original.

Al igual que con \<descriptor >, el formato en cuestión de la <encryptedSecret> elemento depende el mecanismo de cifrado en reposo en uso. En el ejemplo anterior, la clave maestra se cifra mediante DPAPI de Windows por el comentario.

## <a name="the-revocation-element"></a>El \<revocación > elemento

Revocaciones existen como objetos de nivel superior en el repositorio de clave. Por convención, las revocaciones tienen el nombre de archivo **revocación-{timestamp} .xml** (para revocar todas las claves antes de una fecha concreta) o **revocación-{guid} .xml** (para revocar una clave específica). Cada archivo contiene un único \<revocación > elemento.

Para las revocaciones de las claves individuales, será el contenido del archivo a la siguiente.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T22:45:30.2616742Z</revocationDate>
  <key id="eb4fc299-8808-409d-8a34-23fc83d026c9" />
  <reason>human-readable reason</reason>
</revocation>
```

En este caso, solo la clave especificada se ha revocado. Si el identificador de clave es "*", sin embargo, como en el ejemplo siguiente, se revocan todas las claves cuya fecha de creación es anterior a la fecha de revocación especificada.

```xml
<?xml version="1.0" encoding="utf-8"?>
<revocation version="1">
  <revocationDate>2015-03-20T15:45:45.7366491-07:00</revocationDate>
  <!-- All keys created before the revocation date are revoked. -->
  <key id="*" />
  <reason>human-readable reason</reason>
</revocation>
```

El \<motivo > elemento nunca se lee por el sistema. Es simplemente un lugar conveniente para almacenar un motivo de revocación legible.
