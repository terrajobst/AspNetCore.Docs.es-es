---
title: Admite la directiva de todo el equipo de protección de datos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la compatibilidad para configurar una directiva de todo el equipo de forma predeterminada para todas las aplicaciones que utilizan protección de datos de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: c2d5760cd18f4e3ecaf0261f36414c9298e3f4c5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Admite la directiva de todo el equipo de protección de datos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Cuando se ejecuta en Windows, el sistema de protección de datos tiene compatibilidad limitada para establecer una directiva de todo el equipo de forma predeterminada para todas las aplicaciones que utilizan protección de datos de ASP.NET Core. La idea general es que un administrador puede que desee cambiar un valor predeterminado, como los algoritmos utilizan o la vigencia de la clave, sin necesidad de actualizar manualmente cada aplicación en el equipo.

> [!WARNING]
> El administrador del sistema puede establecer la directiva predeterminada, pero no puede aplicarla. El desarrollador de aplicaciones siempre puede reemplazar cualquier valor con uno de su propia elección. La directiva predeterminada sólo afecta a las aplicaciones que el desarrollador no ha especificado un valor explícito para una configuración.

## <a name="setting-default-policy"></a>Establecer la directiva predeterminada

Para establecer una directiva de forma predeterminada, un administrador puede establecer los valores conocidos en el registro del sistema en la siguiente clave del registro:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Si está en un sistema operativo de 64 bits y desea afectan al comportamiento de las aplicaciones de 32 bits, recuerde que debe configurar el equivalente de Wow6432Node de la clave anterior.

Los valores admitidos se muestran a continuación.

| Valor              | Tipo   | Descripción |
| ------------------ | :----: | ----------- |
| EncryptionType     | cadena | Especifica los algoritmos que se deben usar para la protección de datos. El valor debe ser CBC de CNG, GCM CNG o administrado y se describe con más detalle a continuación. |
| DefaultKeyLifetime | DWORD  | Especifica la duración de claves recién generado. El valor se especifica en días y debe ser > = 7. |
| KeyEscrowSinks     | cadena | Especifica los tipos que se usan para la custodia de clave. El valor es una lista delimitada por punto y coma de receptores de custodia de clave, donde cada elemento de la lista es el nombre de ensamblado de un tipo que implementa [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Tipos de cifrado

Si EncryptionType es CBC de CNG, el sistema está configurado para utilizar un cifrado por bloques simétrico modo CBC para confidencialidad y HMAC para autenticidad con servicios proporcionados por Windows CNG (vea [especificar algoritmos personalizados de Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) para más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de CngCbcAuthenticatedEncryptionSettings.

| Valor                       | Tipo   | Descripción |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | cadena | El nombre de un algoritmo de cifrado de bloques simétrico entendido CNG. Este algoritmo se abre en modo CBC. |
| EncryptionAlgorithmProvider | cadena | El nombre de la implementación del proveedor CNG que puede generar el algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétrico. |
| HashAlgorithm               | cadena | El nombre de un algoritmo de hash entendido CNG. Este algoritmo se abre en modo HMAC. |
| HashAlgorithmProvider       | cadena | El nombre de la implementación del proveedor CNG que puede generar el algoritmo HashAlgorithm. |

Si EncryptionType es CNG GCM, el sistema está configurado para usar un cifrado por bloques simétrico modo Galois/contador para la confidencialidad y la autenticidad con servicios proporcionados por Windows CNG (vea [especificar algoritmos personalizados de Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Para obtener más información.) Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de CngGcmAuthenticatedEncryptionSettings.

| Valor                       | Tipo   | Descripción |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | cadena | El nombre de un algoritmo de cifrado de bloques simétrico entendido CNG. Este algoritmo se abre en modo de Galois/contador. |
| EncryptionAlgorithmProvider | cadena | El nombre de la implementación del proveedor CNG que puede generar el algoritmo EncryptionAlgorithm. |
| EncryptionAlgorithmKeySize  | DWORD  | La longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétrico. |

Si se administra EncryptionType, el sistema está configurado para utilizar un SymmetricAlgorithm administrado para la confidencialidad y KeyedHashAlgorithm para autenticidad (vea [especificar personalizado administrado algoritmos](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) para obtener más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de ManagedAuthenticatedEncryptionSettings.

| Valor                      | Tipo   | Descripción |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | cadena | El nombre calificado con el ensamblado de un tipo que implementa SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | La longitud (en bits) de la clave para la derivación para el algoritmo de cifrado simétrico. |
| ValidationAlgorithmType    | cadena | El nombre calificado con el ensamblado de un tipo que implementa KeyedHashAlgorithm. |

Si EncryptionType tiene cualquier otro valor distinto de null o está vacío, el sistema de protección de datos produce una excepción durante el inicio.

> [!WARNING]
> Al configurar una configuración de directiva predeterminada que afecta a los nombres de tipo (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), los tipos deben ser disponibles para la aplicación. Esto significa que para aplicaciones que se ejecutan en CLR de escritorio, los ensamblados que contienen estos tipos deben estar presentes en la caché de ensamblados Global (GAC). Para aplicaciones de ASP.NET Core que se ejecutan en .NET Core, deben instalarse los paquetes que contienen estos tipos.
