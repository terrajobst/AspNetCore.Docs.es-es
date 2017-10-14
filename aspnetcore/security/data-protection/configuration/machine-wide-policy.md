---
title: Directiva para todo el equipo
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: fde8f75422c9dd84311a65b21e1e38b47fbe0306
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="machine-wide-policy"></a>Directiva para todo el equipo

<a name="data-protection-configuration-machinewidepolicy"></a>

Cuando se ejecuta en Windows, el sistema de protección de datos tiene compatibilidad limitada para establecer la directiva de equipo predeterminada para todas las aplicaciones que consumen la protección de datos. La idea general es que un administrador puede que desee cambiar algún valor predeterminado (por ejemplo, los algoritmos utilizados o clave la duración) sin tener que actualizar manualmente cada aplicación en el equipo.

>[!WARNING]
> El administrador del sistema puede establecer la directiva predeterminada, pero no puede aplicarla. Desarrollador de la aplicación siempre puede reemplazar cualquier valor con uno de su propia elección. La directiva predeterminada sólo afecta a las aplicaciones que el desarrollador ha especificado un valor explícito para algunos configuración concreta.

## <a name="setting-default-policy"></a>Establecer la directiva predeterminada

Para establecer una directiva de forma predeterminada, un administrador puede establecer los valores conocidos en el registro del sistema en la siguiente clave.

La clave del registro:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`

Si está en un sistema operativo de 64 bits y desea afectan al comportamiento de las aplicaciones de 32 bits, no olvide también configurar el equivalente de Wow6432Node de la clave anterior.

Los valores admitidos son:

* EncryptionType [cadena] - especifica los algoritmos que se deben usar para la protección de datos. Este valor debe ser "CNG-CBC", "CNG GCM" o "Administrado" y se describe con más detalle [a continuación](#data-protection-encryption-types).

* DefaultKeyLifetime [DWORD] - especifica la duración de claves recién generado. Este valor se especifica en días y debe ser ≥ 7.

* KeyEscrowSinks [cadena] - especifica los tipos que se usará para la custodia de clave. Este valor es una lista delimitada por punto y coma de receptores de custodia de clave, donde cada elemento de la lista es el nombre del ensamblado de un tipo que implementa IKeyEscrowSink.

<a name="data-protection-encryption-types"></a>

### <a name="encryption-types"></a>Tipos de cifrado

Si EncryptionType es "CNG-CBC", el sistema se configurará para utilizar un cifrado por bloques simétrico modo CBC para confidencialidad y HMAC para autenticidad con servicios proporcionados por Windows CNG (vea [especificar algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obtener más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de CngCbcAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [cadena] - el nombre de un algoritmo de cifrado de bloques simétrico entendido CNG. Este algoritmo se abrirá en modo CBC.

* EncryptionAlgorithmProvider [cadena] - el nombre de la implementación del proveedor CNG que puede generar el algoritmo EncryptionAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - la longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétrico.

* HashAlgorithm [cadena] - el nombre de un algoritmo de hash entendido CNG. Este algoritmo se abrirá en modo HMAC.

* HashAlgorithmProvider [cadena] - el nombre de la implementación del proveedor CNG que puede generar el algoritmo HashAlgorithm.

Si EncryptionType es "CNG GCM", el sistema se configurará para usar un cifrado por bloques simétrico modo Galois/contador para la confidencialidad y la autenticidad con servicios proporcionados por Windows CNG (vea [especificar algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obtener más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de CngGcmAuthenticatedEncryptionSettings:

* EncryptionAlgorithm [cadena] - el nombre de un algoritmo de cifrado de bloques simétrico entendido CNG. Este algoritmo se abrirá en modo de Galois/contador.

* EncryptionAlgorithmProvider [cadena] - el nombre de la implementación del proveedor CNG que puede generar el algoritmo EncryptionAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - la longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétrico.

Si EncryptionType está "administrado", el sistema se configurará para utilizar un SymmetricAlgorithm administrado para la confidencialidad y KeyedHashAlgorithm para autenticidad (vea [especificar personalizado administrado algoritmos](overview.md#data-protection-changing-algorithms-custom-managed) para obtener más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de ManagedAuthenticatedEncryptionSettings:

* EncryptionAlgorithmType [cadena] - el nombre calificado con el ensamblado de un tipo que implementa SymmetricAlgorithm.

* EncryptionAlgorithmKeySize [DWORD] - la longitud (en bits) de la clave para la derivación para el algoritmo de cifrado simétrico.

* ValidationAlgorithmType [cadena] - el nombre calificado con el ensamblado de un tipo que implementa KeyedHashAlgorithm.

Si EncryptionType tiene cualquier otro valor (que no sea null o vacío), el sistema de protección de datos producirá una excepción en el inicio.

>[!WARNING]
> Al configurar una configuración de directiva predeterminada que afecta a los nombres de tipo (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), los tipos deben ser disponibles para la aplicación. En la práctica, esto significa que para aplicaciones que se ejecutan en CLR de escritorio, los ensamblados que contienen estos tipos deben ser GACed. Para aplicaciones de ASP.NET Core que se ejecutan en [.NET Core](https://www.microsoft.com/net/core), se deben instalar los paquetes que contienen estos tipos.
