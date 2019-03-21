---
title: Desprotección de cargas cuyas claves se han revocado en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo desproteger los datos, protegidos con las claves que desde entonces han sido revocadas, en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208036"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Desprotección de cargas cuyas claves se han revocado en ASP.NET Core

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Las API de protección de datos de ASP.NET Core no están pensados principalmente para la persistencia indefinido de cargas confidenciales. Otras tecnologías como [DPAPI de Windows CNG](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) y [Azure Rights Management](/rights-management/) son más adecuados para el escenario de almacenamiento ilimitada, y tienen capacidades de administración de claves seguro según corresponda. Es decir, no hay nada que prohíben un desarrollador del uso de las API de protección de datos de ASP.NET Core para la protección a largo plazo de los datos confidenciales. Las claves no se quitan nunca de conjunto de claves, por lo que `IDataProtector.Unprotect` siempre se puede recuperar las cargas existentes siempre que las claves están disponibles y son válidos.

Sin embargo, un problema se produce cuando el desarrollador intenta desproteger los datos que se ha protegido con una clave revocada, como `IDataProtector.Unprotect` se iniciará una excepción en este caso. Esto podría ser interesante cargas breves o temporales (por ejemplo, tokens de autenticación), que fácilmente se pueden volver a crear estos tipos de cargas por el sistema y, en el peor caso el visitante del sitio podría ser necesario volver a iniciar sesión. Pero para las cargas persistentes, tener `Unprotect` throw podría provocar la pérdida de datos inaceptable.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Para admitir el escenario de permitir que las cargas que se debe desproteger aunque se produzcan revocadas claves, el sistema de protección de datos contiene un `IPersistedDataProtector` tipo. Para obtener una instancia de `IPersistedDataProtector`, obtener simplemente una instancia de `IDataProtector` en la forma normal y la conversión de try el `IDataProtector` a `IPersistedDataProtector`.

> [!NOTE]
> No todos los `IDataProtector` instancias se pueden convertir en `IPersistedDataProtector`. Los desarrolladores deben usar la C# como operador o similar evitar las excepciones en tiempo de ejecución causados por las conversiones no válidas, y deben estar preparados para controlar el caso de error adecuadamente.

`IPersistedDataProtector` expone la superficie de API siguiente:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Esta API toma la carga protegida (como una matriz de bytes) y devuelve la carga no protegida. No hay ninguna sobrecarga basada en cadena. Los dos parámetros de salida son los siguientes.

* `requiresMigration`: se establece en true si la clave utilizada para proteger esta carga ya no es la clave activa de forma predeterminada, por ejemplo, la clave utilizada para proteger esta carga es antigua y tiene una clave de revertir la operación desde tendrán lugar. El llamador que desee considerar volver a proteger la carga según sus necesidades empresariales.

* `wasRevoked`: se establecerá en true si la clave utilizada para proteger esta carga se ha revocado.

>[!WARNING]
> Debe extremar las precauciones al pasar `ignoreRevocationErrors: true` a la `DangerousUnprotect` método. Si después de llamar a este método la `wasRevoked` valor es true, a continuación, la clave utilizada para proteger esta carga se ha revocado y autenticidad de la carga se debe tratar como sospechosa. En este caso, sólo seguir funcionando en la carga no protegida si tiene cierta seguridad independiente que es auténtico, por ejemplo, que procede de una base de datos seguro en lugar de que se envían por un cliente web de confianza.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
