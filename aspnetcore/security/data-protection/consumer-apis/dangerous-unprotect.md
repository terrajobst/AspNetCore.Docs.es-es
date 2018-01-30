---
title: Desproteger cargas cuyas claves se han revocado
author: rick-anderson
description: "Este documento explica cómo desproteger los datos, protegidos con claves que ya han sido revocadas, en una aplicación de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 584dbb545c15add4401086b9160d4bf30caf41b5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a>Desproteger cargas cuyas claves se han revocado

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

La API de protección de datos de ASP.NET Core no están pensados principalmente para la persistencia indefinido de cargas confidenciales. Otras tecnologías, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) y [Azure Rights Management](https://docs.microsoft.com/rights-management/) son más adecuados para el escenario de almacenamiento indefinido, y tienen capacidades de administración de claves seguro según corresponda. Es decir, no hay nada prohibir a un desarrollador usa las API de protección de datos de ASP.NET Core para la protección a largo plazo de información confidencial. Claves nunca se eliminan desde el anillo de clave, por lo que `IDataProtector.Unprotect` siempre se puede recuperar cargas existentes siempre que las claves sean disponible y es válido.

Sin embargo, un problema se produce cuando el programador intenta desproteger los datos que se ha protegido con una clave revocada, como `IDataProtector.Unprotect` se iniciará una excepción en este caso. Esto podría ser bien para las cargas breves o temporales (por ejemplo, tokens de autenticación), tal y como fácilmente se pueden volver a crear estos tipos de cargas por el sistema y en el peor el visitante del sitio podría ser necesario volver a iniciar sesión. Pero para cargas persistentes, tener `Unprotect` throw podría provocar la pérdida de datos aceptable.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Para admitir el escenario de permitir cargas que se debe desproteger incluso de cara a revocados claves, el sistema de protección de datos contiene un `IPersistedDataProtector` tipo. Para obtener una instancia de `IPersistedDataProtector`, simplemente obtener una instancia de `IDataProtector` en el modo normal y conversión de try el `IDataProtector` a `IPersistedDataProtector`.

> [!NOTE]
> No todos los `IDataProtector` instancias pueden convertirse a `IPersistedDataProtector`. Los programadores deben utilizar el C# como operador o similar evitar las excepciones en tiempo de ejecución deberse a conversiones no válidas y que deben estar preparado para controlar el caso de fallo adecuadamente.

`IPersistedDataProtector`expone la superficie de API siguiente:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Esta API toma la carga protegida (como una matriz de bytes) y devuelve la carga sin protección. No hay ninguna sobrecarga basada en cadena. Los dos parámetros de salida son los siguientes.

* `requiresMigration`: se establece en true si la clave utilizada para proteger esta carga ya no es la clave predeterminada activa, por ejemplo, la clave utilizada para proteger esta carga es antigua y tiene una operación de reversión de clave desde tendrán lugar. El llamador puede llamar a considere la posibilidad de volver a proteger la carga de la función de sus necesidades empresariales.

* `wasRevoked`: se establecerá en true si la clave utilizada para proteger esta carga se ha revocado.

>[!WARNING]
> Debe extremar las precauciones al pasar `ignoreRevocationErrors: true` a la `DangerousUnprotect` método. Si después de llamar a este método la `wasRevoked` valor es true, a continuación, la clave utilizada para proteger esta carga ha sido revocada y autenticidad de la carga debe tratarse como sospechosa. En este caso, solo seguir funcionando en la carga sin protección si tienen seguridad independiente que es auténtica, por ejemplo, que procede de una base de datos segura en lugar de enviarse por un cliente web no es de confianza.

[!code-csharp[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]
