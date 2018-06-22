---
title: Cadenas de propósito en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo se utilizan cadenas de propósito en las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: 4c85423f8de7e4b784ae1bb304a884541df251b6
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278770"
---
# <a name="purpose-strings-in-aspnet-core"></a>Cadenas de propósito en ASP.NET Core

<a name="data-protection-consumer-apis-purposes"></a>

Componentes que consumen `IDataProtectionProvider` debe pasar un único *fines* parámetro para el `CreateProtector` método. Los fines *parámetro* es inherente a la seguridad del sistema de protección de datos, ya que proporciona aislamiento entre los consumidores de cifrado, incluso si las claves criptográficas de raíz son los mismos.

Cuando un consumidor especifica un propósito, la cadena de fin se usa junto con las claves criptográficas de raíz para derivar criptográficas subclaves únicas para ese consumidor. Esto permite aislar el consumidor de todos los otros consumidores de cifrado en la aplicación: ningún otro componente puede leer sus cargas y no se puede leer cargas de cualquier otro componente. Este aislamiento también presenta factible todas las categorías de un ataque contra el componente.

![Ejemplo de diagrama de propósito](purpose-strings/_static/purposes.png)

En el diagrama anterior, `IDataProtector` instancias A y B **no** leer de todas las demás cargas, solo sus propios.

La cadena de fin no tiene que ser secreto. Simplemente debe ser único en el sentido de que ningún otro componente con buen comportamiento nunca proporcione la misma cadena de fin.

>[!TIP]
> Con el nombre de espacio de nombres y el tipo del componente consumir las API de protección de datos es una buena regla general, al igual que en la práctica que esta información nunca estará en conflicto.
>
>Un componente creado por Contoso que es responsable de minting tokens de portador puede usar Contoso.Security.BearerToken como cadena de su propósito. O - incluso mejor -, podría usar Contoso.Security.BearerToken.v1 como cadena de su propósito. Anexar el número de versión permite que una versión futura usar Contoso.Security.BearerToken.v2 como su propósito, y las distintas versiones sería completamente aisladas entre sí como ir de cargas.

Desde el parámetro de fines `CreateProtector` es una matriz de cadenas, los pasos anteriores se haya en su lugar especificados como `[ "Contoso.Security.BearerToken", "v1" ]`. Esto permite establecer una jerarquía de propósitos y se abre la posibilidad de escenarios de varios inquilinos con el sistema de protección de datos.

<a name="data-protection-contoso-purpose"></a>

>[!WARNING]
> Componentes no deben permitir proporcionados por el usuario de confianza como el único origen de entrada de la cadena con fines.
>
>Por ejemplo, considere la posibilidad de un componente Contoso.Messaging.SecureMessage que es responsable de almacenar mensajes seguros. Si el componente de mensajería seguro llamase a `CreateProtector([ username ])`, a continuación, un usuario malintencionado podría crear una cuenta con el nombre de usuario "Contoso.Security.BearerToken" en un intento de obtener el componente para llamar a `CreateProtector([ "Contoso.Security.BearerToken" ])`, lo que sin darse cuenta y la mensajería segura sistema que lleva cargas que se puede percibir como tokens de autenticación.
>
>Una cadena con fines mejor para el componente de mensajería sería `CreateProtector([ "Contoso.Messaging.SecureMessage", "User: username" ])`, lo que proporciona aislamiento adecuado.

El aislamiento que ofrecen y los comportamientos de `IDataProtectionProvider`, `IDataProtector`, y con fines de son los siguientes:

* Para un determinado `IDataProtectionProvider` objeto, el `CreateProtector` método creará una `IDataProtector` objeto ligada a ambos en modo exclusivo la `IDataProtectionProvider` objeto que lo creó y el parámetro de propósitos que se pasó al método.

* El parámetro de fin no debe ser null. (Si no se especifica con fines como una matriz, esto significa que la matriz no debe ser de longitud cero y todos los elementos de la matriz deben ser distinto de null). Un propósito de una cadena vacía es técnicamente válido pero en absoluto.

* Argumentos de dos objetivos son equivalentes si y solo si contienen las mismas cadenas (usando a un comparador ordinal) en el mismo orden. Un argumento único propósito es equivalente a la matriz con fines de solo elemento correspondiente.

* Dos `IDataProtector` objetos son equivalentes si y solo si se crean desde equivalente `IDataProtectionProvider` objetos con los parámetros de fines equivalente.

* Para un determinado `IDataProtector` (objeto), una llamada a `Unprotect(protectedData)` devolverá el original `unprotectedData` si y solo si `protectedData := Protect(unprotectedData)` para un equivalente `IDataProtector` objeto.

> [!NOTE]
> No estamos pensando en el caso de que algún componente intencionadamente elige una cadena de propósito que se sabe que entran en conflicto con otro componente. Esos componentes básicamente se consideraría malintencionado, y este sistema no está diseñado para proporcionar garantías de seguridad en caso de que código malintencionado ya se está ejecutando dentro del proceso de trabajo.
