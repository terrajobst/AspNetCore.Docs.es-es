---
title: Inmutabilidad de claves y valores de clave en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación de la inmutabilidad de claves de protección de datos de ASP.NET Core API.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 7796cb102c0f6f03809704016fd36b28c7a82438
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219308"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Inmutabilidad de claves y valores de clave en ASP.NET Core

Una vez que un objeto se conserva en el almacén de respaldo, su representación es fija para siempre. Se pueden agregar nuevos datos en el almacén de respaldo, pero nunca pueden transformarse los datos existentes. El propósito principal de este comportamiento es evitar daños en los datos.

Una consecuencia de este comportamiento es que, una vez que se escribe una clave en el almacén de respaldo, es inmutable. Su fecha de creación, activación y expiración nunca se puede cambiar, aunque pueden revocar utilizando `IKeyManager`. Además, su información algorítmico subyacente, material de claves maestra y el cifrado en las propiedades de rest también son inmutables.

Si el desarrollador cambia cualquier configuración que afecta a la persistencia de clave, dichos cambios no entran en vigor hasta la próxima vez que se genera una clave, ya sea a través de una llamada explícita a `IKeyManager.CreateNewKey` o a través de lo datos protección del sistema propio [clave automático generación](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportamiento. La configuración que afecta la persistencia de clave es los siguientes:

* [Duración de la clave predeterminada](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [El cifrado de claves en el mecanismo de rest](xref:security/data-protection/implementation/key-encryption-at-rest)

* [La información algorítmica contenida dentro de la clave](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Si necesita esta configuración se activará anteriores a la siguiente clave automática gradual tiempo, considere la posibilidad de realizar una llamada explícita a `IKeyManager.CreateNewKey` para forzar la creación de una nueva clave. Recuerde que debe proporcionar una fecha de activación explícita ({ahora + 2 días} es una buena regla general para dejar tiempo propagar el cambio) y la fecha de expiración en la llamada.

>[!TIP]
> Todas las aplicaciones que tocar el repositorio deben especificar la misma configuración con el `IDataProtectionBuilder` métodos de extensión. En caso contrario, las propiedades de la clave almacenada será dependientes de la aplicación específica que invoca las rutinas de generación de claves.
