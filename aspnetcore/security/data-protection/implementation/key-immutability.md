---
title: "Inmutabilidad de clave y cambiar la configuración"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a>Inmutabilidad de clave y cambiar la configuración

Una vez que un objeto se mantiene en la memoria auxiliar, su representación de infinito es fijo. Se pueden agregar datos nuevos a la memoria auxiliar, pero nunca pueden transformarse los datos existentes. El propósito principal de este comportamiento es evitar daños en los datos.

Una consecuencia de este comportamiento es que, cuando una clave se escribe en la memoria auxiliar, es inmutable. Nunca se puede cambiar su fecha de creación, activación y caducidad, aunque puede revocar utilizando IKeyManager. Además, su información algorítmica subyacente, el material de creación de claves maestras y el cifrado en Propiedades de rest también son inmutables.

Si el desarrollador cambia cualquier configuración que afecta a la persistencia de clave, esos cambios no entrará en vigor hasta la próxima vez que se genere una clave, ya sea a través de una llamada explícita a IKeyManager.CreateNewKey o datos protección del sistema propio [automática generación de claves](key-management.md#data-protection-implementation-key-management) comportamiento. La configuración que afecta a la persistencia de clave es los siguientes:

* [La vigencia de clave predeterminada](key-management.md#data-protection-implementation-key-management)

* [El cifrado de claves en el mecanismo de rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [La información algorítmica contenida dentro de la clave](../configuration/overview.md#data-protection-changing-algorithms)

Si necesita estas opciones para iniciar anteriores a la siguiente tecla automática gradual tiempo, considere la posibilidad de realizar una llamada explícita a IKeyManager.CreateNewKey para forzar la creación de una nueva clave. Recuerde que debe proporcionar una fecha de activación explícita ({ahora + 2 días} es una buena regla general para dejar tiempo propagar el cambio) y la fecha de expiración en la llamada.

>[!TIP]
> Todas las aplicaciones tocar el repositorio deben especificar la misma configuración con los métodos de extensión IDataProtectionBuilder, en caso contrario, las propiedades de la clave persistente estarán depende de la aplicación en particular que invoca las rutinas de generación de claves .
