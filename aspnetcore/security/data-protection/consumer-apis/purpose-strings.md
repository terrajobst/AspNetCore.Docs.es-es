---
title: Cadenas de propósito en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo se usan las cadenas de propósito en las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087546"
---
# <a name="purpose-strings-in-aspnet-core"></a>Cadenas de propósito en ASP.NET Core

<a name="data-protection-consumer-apis-purposes"></a>

Los componentes que consumen `IDataProtectionProvider` debe pasar un único *fines* parámetro para el `CreateProtector` método. Los propósitos *parámetro* es inherente a la seguridad del sistema de protección de datos, ya que proporciona aislamiento entre los consumidores de cifrado, incluso si las claves criptográficas de raíz son los mismos.

Cuando un consumidor especifica un propósito, la cadena de propósito se usa junto con las claves criptográficas de raíz para derivar subclaves criptográficas únicas a ese consumidor. Esto aísla el consumidor de todos los consumidores criptográficos en la aplicación: ningún otro componente puede leer sus cargas y no se puede leer cargas de cualquier otro componente. Este aislamiento también procesa todas las categorías no es viable de ataque contra el componente.

![Ejemplo de diagrama de propósito](purpose-strings/_static/purposes.png)

En el diagrama anterior, `IDataProtector` instancias A y B **no** leer de la otra las cargas, solo su propia.

La cadena de propósito no tiene que ser secreta. Simplemente debe ser único en el sentido de que ningún otro componente con buen comportamiento sacarán nunca proporcionará la misma cadena de propósito.

>[!TIP]
> Con el nombre de espacio de nombres y tipo del componente de consumir las API de protección de datos es una buena regla general, al igual que en la práctica, que esta información nunca estará en conflicto.
>
>Un componente creado por Contoso que se encarga de los tokens de portador de minting podría usar Contoso.Security.BearerToken como cadena de su propósito. O - incluso mejor - usaría Contoso.Security.BearerToken.v1 como cadena de su propósito. Anexa el número de versión permite que una versión futura usar Contoso.Security.BearerToken.v2 como su propósito y las distintas versiones sería completamente aisladas entre sí como cargas útiles de ir.

Desde el parámetro de fines `CreateProtector` es una matriz de cadenas, lo anterior se haya en su lugar, especificado como `[ "Contoso.Security.BearerToken", "v1" ]`. Esto permite establecer una jerarquía de propósitos y abre la posibilidad de escenarios de varios inquilinos con el sistema de protección de datos.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Componentes no deben permitir la entrada del usuario de confianza a ser la única fuente de entrada de la cadena con fines.
>
>Por ejemplo, considere la posibilidad de un componente Contoso.Messaging.SecureMessage que es responsable de almacenar mensajes seguros. Si el componente de mensajería seguro llamase a `CreateProtector([ username ])`, a continuación, un usuario malintencionado podría crear una cuenta con el nombre de usuario "Contoso.Security.BearerToken" en un intento de obtener el componente para llamar a `CreateProtector([ "Contoso.Security.BearerToken" ])`, lo que accidentalmente la mensajería segura sistema de mint cargas que se podría percibir como tokens de autenticación.
>
>Una cadena con fines mejor para el componente de mensajería sería `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, que proporciona aislamiento adecuado.

El aislamiento que ofrecen los comportamientos y `IDataProtectionProvider`, `IDataProtector`, y con fines son los siguientes:

* Para un determinado `IDataProtectionProvider` objeto, el `CreateProtector` creará un `IDataProtector` objeto vinculado de forma exclusiva a ambos el `IDataProtectionProvider` objeto que lo creó y el parámetro de efectos que se pasó al método.

* El parámetro propósito no debe ser null. (Si se especifica con fines como una matriz, esto significa que la matriz no debe ser de longitud cero y todos los elementos de la matriz deben ser distinto de null). Propósito de una cadena vacía es técnicamente válido pero en absoluto.

* Argumentos de dos objetivos son equivalentes si y solo si contienen las mismas cadenas (utilizando a un comparador ordinal) en el mismo orden. Un argumento único propósito es equivalente a la matriz de fines único elemento correspondiente.

* Dos `IDataProtector` objetos son equivalentes si y solo si se crean desde equivalente `IDataProtectionProvider` objetos con los parámetros de fines equivalente.

* Para un determinado `IDataProtector` (objeto), una llamada a `Unprotect(protectedData)` devolverá original `unprotectedData` si y solo si `protectedData := Protect(unprotectedData)` para equivalente `IDataProtector` objeto.

> [!NOTE]
> No estamos considerando el caso de que algún componente intencionadamente elige una cadena de propósito que se sabe que entran en conflicto con otro componente. Este componente básicamente se consideraría malintencionado, y este sistema no está diseñado para proporcionar garantías de seguridad en caso de que ya se está ejecutando código malintencionado dentro del proceso de trabajo.
