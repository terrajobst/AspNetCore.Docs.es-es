---
title: Directiva de todo el equipo de protección de datos se admiten en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la compatibilidad para establecer una directiva de todo el equipo de forma predeterminada para todas las aplicaciones que consumen la protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 70aaca7afcd3df22cebb4466fbd9845a2277688c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897272"
---
# <a name="data-protection-machine-wide-policy-support-in-aspnet-core"></a>Directiva de todo el equipo de protección de datos se admiten en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Cuando se ejecuta en Windows, el sistema de protección de datos con compatibilidad limitada para establecer una directiva de todo el equipo de forma predeterminada para todas las aplicaciones que consumen la protección de datos de ASP.NET Core. La idea general es que un administrador que desee cambiar una configuración predeterminada, como los algoritmos utilizan o la vigencia de la clave, sin necesidad de actualizar manualmente cada aplicación en el equipo.

> [!WARNING]
> El administrador del sistema puede establecer la directiva predeterminada, pero no puede aplicarla. El desarrollador de la aplicación siempre puede reemplazar cualquier valor con uno de su propia elección. La directiva predeterminada solo afecta a las aplicaciones donde el desarrollador no ha especificado un valor explícito para una configuración.

## <a name="setting-default-policy"></a>Establecer la directiva predeterminada

Para establecer la directiva predeterminada, un administrador puede establecer los valores conocidos en el registro del sistema en la siguiente clave del registro:

**HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection**

Si está en un sistema operativo de 64 bits y desea afectar el comportamiento de las aplicaciones de 32 bits, no olvide configurar el equivalente de Wow6432Node de la clave anterior.

A continuación, se muestran los valores admitidos.

| Valor              | Tipo   | Descripción |
| ------------------ | :----: | ----------- |
| EncryptionType     | string | Especifica los algoritmos que se deben usar para la protección de datos. El valor debe ser CNG-CBC, CNG GCM o administrado y se describe más detalladamente a continuación. |
| DefaultKeyLifetime | DWORD  | Especifica la duración de las claves recién generadas. El valor se especifica en días y debe ser > = 7. |
| KeyEscrowSinks     | string | Especifica los tipos que se usan para la custodia de clave. El valor es una lista delimitada por punto y coma de receptores de custodia de clave, donde cada elemento en la lista es el nombre completo de ensamblado de un tipo que implementa [IKeyEscrowSink](/dotnet/api/microsoft.aspnetcore.dataprotection.keymanagement.ikeyescrowsink). |

## <a name="encryption-types"></a>Tipos de cifrado

Si EncryptionType es CNG-CBC, el sistema está configurado para usar un cifrado por bloques simétrico modo CBC para confidencialidad y HMAC de autenticidad con servicios proporcionados por Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) para más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad del tipo CngCbcAuthenticatedEncryptionSettings.

| Valor                       | Tipo   | Descripción |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | El nombre de un algoritmo de cifrado de bloques simétricos entendido CNG. Este algoritmo se abre en modo CBC. |
| EncryptionAlgorithmProvider | string | El nombre de la implementación del proveedor CNG que puede producir el algoritmo de cifrado del algoritmo. |
| EncryptionAlgorithmKeySize  | DWORD  | La longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétricos. |
| HashAlgorithm               | string | El nombre de un algoritmo hash entendido CNG. Este algoritmo se abre en modo HMAC. |
| HashAlgorithmProvider       | string | El nombre de la implementación del proveedor CNG que puede producir el algoritmo de HashAlgorithm. |

Si EncryptionType es CNG GCM, el sistema está configurado para usar un cifrado por bloques simétrico modo Galois/contador para la confidencialidad y la autenticidad con servicios proporcionados por Windows CNG (consulte [especificando algoritmos personalizados de Windows CNG](xref:security/data-protection/configuration/overview#specifying-custom-windows-cng-algorithms) Para obtener más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad del tipo CngGcmAuthenticatedEncryptionSettings.

| Valor                       | Tipo   | Descripción |
| --------------------------- | :----: | ----------- |
| EncryptionAlgorithm         | string | El nombre de un algoritmo de cifrado de bloques simétricos entendido CNG. Este algoritmo se abre en modo de Galois o contador. |
| EncryptionAlgorithmProvider | string | El nombre de la implementación del proveedor CNG que puede producir el algoritmo de cifrado del algoritmo. |
| EncryptionAlgorithmKeySize  | DWORD  | La longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétricos. |

Si está administrado EncryptionType, el sistema está configurado para usar un SymmetricAlgorithm administrado para la confidencialidad y KeyedHashAlgorithm para autenticidad (consulte [especificar personalizado administrado algoritmos](xref:security/data-protection/configuration/overview#specifying-custom-managed-algorithms) para obtener más detalles). Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad del tipo ManagedAuthenticatedEncryptionSettings.

| Valor                      | Tipo   | Descripción |
| -------------------------- | :----: | ----------- |
| EncryptionAlgorithmType    | string | El nombre completo de ensamblado de un tipo que implementa SymmetricAlgorithm. |
| EncryptionAlgorithmKeySize | DWORD  | La longitud (en bits) de la clave para derivar el algoritmo de cifrado simétrico. |
| ValidationAlgorithmType    | string | El nombre completo de ensamblado de un tipo que implementa KeyedHashAlgorithm. |

Si EncryptionType tiene cualquier otro valor distinto de null o está vacío, el sistema de protección de datos produce una excepción durante el inicio.

> [!WARNING]
> Al configurar una configuración de directiva predeterminada que incluye los nombres de tipo (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), los tipos deben ser disponibles para la aplicación. Esto significa que para las aplicaciones que se ejecutan en CLR de escritorio, los ensamblados que contienen estos tipos deben estar presentes en la caché de ensamblados Global (GAC). Para las aplicaciones de ASP.NET Core que se ejecutan en .NET Core, se deben instalar los paquetes que contienen estos tipos.
