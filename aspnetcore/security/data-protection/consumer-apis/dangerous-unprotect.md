---
title: Desprotección de cargas cuyas claves se han revocado en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo desproteger datos, protegidos con claves que se han revocado, en una aplicación ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 26061d048dcd9c1e3d8909e9388d8b565376fa2f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654599"
---
# <a name="unprotect-payloads-whose-keys-have-been-revoked-in-aspnet-core"></a>Desprotección de cargas cuyas claves se han revocado en ASP.NET Core

<a name="data-protection-consumer-apis-dangerous-unprotect"></a>

Las API de protección de datos de ASP.NET Core no están pensadas principalmente para la persistencia indefinida de cargas confidenciales. Otras tecnologías, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) y [Azure Rights Management](/rights-management/) , son más adecuadas para el escenario de almacenamiento indefinido, y tienen una sólida funcionalidad de administración de claves. Dicho esto, no hay nada que prohíba a un desarrollador usar las API de protección de datos ASP.NET Core para la protección a largo plazo de datos confidenciales. Las claves nunca se quitan del anillo de claves, por lo que `IDataProtector.Unprotect` puede recuperar siempre las cargas existentes, siempre y cuando las claves estén disponibles y sean válidas.

Sin embargo, surge un problema cuando el desarrollador intenta desproteger los datos que se han protegido con una clave revocada, ya que `IDataProtector.Unprotect` producirá una excepción en este caso. Esto puede ser adecuado para cargas de corta duración o transitorias (como tokens de autenticación), ya que el sistema puede volver a crear fácilmente estos tipos de cargas y, en el peor de los demás, el visitante del sitio podría requerir que vuelva a iniciar sesión. Pero en el caso de las cargas persistentes, tener `Unprotect` Throw podría provocar una pérdida de datos inaceptable.

## <a name="ipersisteddataprotector"></a>IPersistedDataProtector

Para admitir el escenario de permitir que las cargas se desprotejan incluso en el caso de las claves revocadas, el sistema de protección de datos contiene un tipo de `IPersistedDataProtector`. Para obtener una instancia de `IPersistedDataProtector`, simplemente obtenga una instancia de `IDataProtector` de la manera normal e intente convertir el `IDataProtector` en `IPersistedDataProtector`.

> [!NOTE]
> No todas las instancias de `IDataProtector` se pueden convertir en `IPersistedDataProtector`. Los desarrolladores deben usar C# el operador as o similar para evitar las excepciones en tiempo de ejecución causadas por conversiones no válidas y deben estar preparadas para controlar el caso de error adecuadamente.

`IPersistedDataProtector` expone la siguiente superficie de API:

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
```

Esta API toma la carga protegida (como una matriz de bytes) y devuelve la carga desprotegida. No hay ninguna sobrecarga basada en cadena. Los dos parámetros out son los siguientes.

* `requiresMigration`: se establecerá en true si la clave que se usa para proteger esta carga ya no es la clave predeterminada activa; por ejemplo, la clave usada para proteger esta carga es antigua y se ha realizado una operación de desplazamiento de claves. Es posible que el autor de la llamada desee considerar la posibilidad de volver a proteger la carga en función de sus necesidades empresariales.

* `wasRevoked`: se establecerá en true si se revocó la clave usada para proteger esta carga.

>[!WARNING]
> Tenga mucho cuidado al pasar `ignoreRevocationErrors: true` al método `DangerousUnprotect`. Si después de llamar a este método el valor de `wasRevoked` es true, la clave usada para proteger esta carga se ha revocado y la autenticidad de la carga debe tratarse como sospechosa. En este caso, siga usando solo la carga desprotegida si tiene una garantía independiente de que es auténtica, por ejemplo, que proviene de una base de datos segura en lugar de ser enviada por un cliente web que no es de confianza.

[!code-csharp[](dangerous-unprotect/samples/dangerous-unprotect.cs)]
