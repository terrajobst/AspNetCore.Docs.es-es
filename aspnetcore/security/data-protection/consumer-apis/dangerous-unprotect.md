---
title: Desproteger cargas cuyas claves se han revocado
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 5d176515792045545add66ba5aedb0358d8bdc70
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Desproteger cargas cuyas claves se han revocado

<a name=data-protection-consumer-apis-dangerous-unprotect></a>

La API de protección de datos de ASP.NET Core no están pensados principalmente para la persistencia indefinido de cargas confidenciales. Otras tecnologías, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) y [Azure Rights Management](https://docs.microsoft.com/rights-management/) son más adecuados para el escenario de almacenamiento indefinido, y tienen capacidades de administración de claves seguro según corresponda. Es decir, no hay nada prohibir a un desarrollador usa las API de protección de datos de ASP.NET Core para la protección a largo plazo de información confidencial. Claves nunca se quitan del anillo de clave, por lo que IDataProtector.Unprotect siempre puede recuperar cargas existentes, como las claves están disponibles y válido.

Sin embargo, un problema se produce cuando el programador intenta desproteger los datos que se ha protegido con una clave revocada, tal y como IDataProtector.Unprotect se iniciará una excepción en este caso. Esto podría ser bien para las cargas breves o temporales (por ejemplo, tokens de autenticación), tal y como fácilmente se pueden volver a crear estos tipos de cargas por el sistema y en el peor el visitante del sitio podría ser necesario volver a iniciar sesión. Pero para cargas persistentes, tener Unprotect throw podría provocar pérdida de datos aceptable.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Para admitir el escenario de permitir cargas que se debe desproteger incluso de cara a revocados claves, el sistema de protección de datos contiene un tipo de IPersistedDataProtector. Para obtener una instancia de IPersistedDataProtector, obtener una instancia de IDataProtector de manera normal e intente convertir el IDataProtector para IPersistedDataProtector.

> [!NOTE]
> No todas las instancias de IDataProtector pueden convertirse a IPersistedDataProtector. Los programadores deben utilizar el C# como operador o similar evitar las excepciones en tiempo de ejecución deberse a conversiones no válidas y que deben estar preparado para controlar el caso de fallo adecuadamente.

IPersistedDataProtector expone la superficie de API siguiente:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

Esta API toma la carga protegida (como una matriz de bytes) y devuelve la carga sin protección. No hay ninguna sobrecarga basada en cadena. Los dos parámetros de salida son los siguientes.

* requiresMigration: se establece en true si la clave utilizada para proteger esta carga ya no es la clave predeterminada activa, por ejemplo, la clave utilizada para proteger esta carga es antigua y tiene una operación de reversión de clave desde tendrán lugar. El llamador puede llamar a considere la posibilidad de volver a proteger la carga de la función de sus necesidades empresariales.

* wasRevoked: se establecerá en true si la clave utilizada para proteger esta carga se ha revocado.

>[!WARNING]
> Debe extremar las precauciones al pasar ignoreRevocationErrors: True si el método DangerousUnprotect. Si después de llamar a este método, el valor de wasRevoked es true, a continuación, se ha revocado la clave utilizada para proteger esta carga y autenticidad de la carga debe tratarse como sospechosa. En este caso sólo seguir funcionando en la carga sin protección si tienen seguridad independiente que es auténtico, por ejemplo, que TI del procedentes de una base de datos segura en lugar de enviarse por un cliente web no es de confianza.

[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
