---
title: Inmutabilidad de clave y la configuración de clave de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los detalles de implementación de la inmutabilidad de clave de protección de datos de ASP.NET Core API.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: e918b00562aca9821de87c38f10242177517d8a5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "30075640"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>Inmutabilidad de clave y la configuración de clave de ASP.NET Core

Una vez que un objeto se mantiene en la memoria auxiliar, su representación de infinito es fijo. Se pueden agregar datos nuevos a la memoria auxiliar, pero nunca pueden transformarse los datos existentes. El propósito principal de este comportamiento es evitar daños en los datos.

Una consecuencia de este comportamiento es que, cuando una clave se escribe en la memoria auxiliar, es inmutable. Su fecha de creación, activación y expiración nunca se puede cambiar, aunque puede revocar utilizando `IKeyManager`. Además, su información algorítmica subyacente, el material de creación de claves maestras y el cifrado en Propiedades de rest también son inmutables.

Si el desarrollador cambia cualquier configuración que afecta a la persistencia de clave, esos cambios no entran en vigor hasta la próxima vez que se genere una clave, ya sea a través de una llamada explícita a `IKeyManager.CreateNewKey` o a través de lo datos protección del sistema propio [clave automática generación](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportamiento. La configuración que afecta a la persistencia de clave es los siguientes:

* [La vigencia de clave predeterminada](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [El cifrado de claves en el mecanismo de rest](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [La información algorítmica contenida dentro de la clave](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Si necesita estas opciones para iniciar anteriores a la siguiente tecla automática gradual tiempo, considere la posibilidad de realizar una llamada explícita a `IKeyManager.CreateNewKey` para forzar la creación de una nueva clave. Recuerde que debe proporcionar una fecha de activación explícita ({ahora + 2 días} es una buena regla general para dejar tiempo propagar el cambio) y la fecha de expiración en la llamada.

>[!TIP]
> Todas las aplicaciones tocar el repositorio deben especificar la misma configuración con el `IDataProtectionBuilder` métodos de extensión. De lo contrario, las propiedades de la clave persistente será depende de la aplicación en particular que invoca las rutinas de generación de claves.
