---
title: "Inmutabilidad de clave y cambiar la configuración"
author: rick-anderson
description: "Este documento describen los detalles de implementación de la inmutabilidad de clave de protección las API de datos ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 8e46e634266fa5f082c47f3be306009eb54bcbcc
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="key-immutability-and-changing-settings"></a>Inmutabilidad de clave y cambiar la configuración

Una vez que un objeto se mantiene en la memoria auxiliar, su representación de infinito es fijo. Se pueden agregar datos nuevos a la memoria auxiliar, pero nunca pueden transformarse los datos existentes. El propósito principal de este comportamiento es evitar daños en los datos.

Una consecuencia de este comportamiento es que, cuando una clave se escribe en la memoria auxiliar, es inmutable. Su fecha de creación, activación y expiración nunca se puede cambiar, aunque puede revocar utilizando `IKeyManager`. Además, su información algorítmica subyacente, el material de creación de claves maestras y el cifrado en Propiedades de rest también son inmutables.

Si el desarrollador cambia cualquier configuración que afecta a la persistencia de clave, esos cambios no entrará en vigor hasta la próxima vez que se genere una clave, ya sea a través de una llamada explícita a `IKeyManager.CreateNewKey` o a través de lo datos protección del sistema propio [clave automática generación](key-management.md#data-protection-implementation-key-management) comportamiento. La configuración que afecta a la persistencia de clave es los siguientes:

* [La vigencia de clave predeterminada](key-management.md#data-protection-implementation-key-management)

* [El cifrado de claves en el mecanismo de rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [La información algorítmica contenida dentro de la clave](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

Si necesita estas opciones para iniciar anteriores a la siguiente tecla automática gradual tiempo, considere la posibilidad de realizar una llamada explícita a `IKeyManager.CreateNewKey` para forzar la creación de una nueva clave. Recuerde que debe proporcionar una fecha de activación explícita ({ahora + 2 días} es una buena regla general para dejar tiempo propagar el cambio) y la fecha de expiración en la llamada.

>[!TIP]
> Todas las aplicaciones tocar el repositorio deben especificar la misma configuración con el `IDataProtectionBuilder` métodos de extensión. De lo contrario, las propiedades de la clave persistente será depende de la aplicación en particular que invoca las rutinas de generación de claves.
