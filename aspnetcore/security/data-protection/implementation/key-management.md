---
title: "Administración de claves"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fb9b807a-d143-4861-9ddb-005d8796afa3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 507c00edc5bade2427151ecadfed581817e4d088
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="key-management"></a>Administración de claves

<a name=data-protection-implementation-key-management></a>

El sistema de protección de datos administra automáticamente la duración de claves maestras de usa para proteger y desproteger cargas. Cada clave puede estar en uno de cuatro fases.

* Creado: la clave existe en el anillo de clave, pero aún no se ha activado. La clave no debe utilizarse para nuevas operaciones de protección hasta que haya transcurrido el tiempo suficiente que la clave ha tenido la oportunidad de propagarse a todas las máquinas que consumen este anillo de clave.

* Active - la clave existe en el anillo de clave y debe utilizarse para todas las operaciones de proteger de nuevo.

* Ha caducado: la clave de su duración natural ha ejecutado y ya no debe usarse para nuevas operaciones de protección.

* Revocar - la clave está en peligro y no se debe utilizar para nuevas operaciones de protección.

Claves creadas, activas y caducadas pueden utilizarse para desproteger cargas entrantes. Claves revocadas de forma predeterminada no pueden usarse para desproteger cargas, pero el desarrollador de aplicaciones puede [invalidar este comportamiento](../consumer-apis/dangerous-unprotect.md#data-protection-consumer-apis-dangerous-unprotect) si es necesario.

>[!WARNING]
> El programador podría verse tentado a eliminar una clave desde el anillo de clave (p. ej., eliminando el archivo correspondiente del sistema de archivos). En ese momento, todos los datos protegidos por la clave es indescifrables permanentemente, y no hay ninguna invalidación emergencia que hay con claves revocadas. Si se elimina una clave es un comportamiento destructivo realmente y, por consiguiente, el sistema de protección de datos no expone ninguna API de primera clase para realizar esta operación.

## <a name="default-key-selection"></a>Selección de clave predeterminada

Cuando el sistema de protección de datos lee el anillo de clave desde el repositorio de respaldo, intentará encontrar una clave de "default" desde el anillo de clave. La clave predeterminada se usa para operaciones de proteger de nuevo.

La heurística general es que el sistema de protección de datos elige la clave con la fecha de activación más reciente que la clave predeterminada. (No hay un factor de aglutinante pequeño para permitir el reloj del servidor a servidor sesgo). Si la clave expiró o se revocó, generación de claves y si la aplicación no ha deshabilitado automática, se generará una nueva clave con la activación inmediata por la [clave de expiración y las sucesivas](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) directiva siguiente.

El motivo por el sistema de protección de datos genera una nueva clave inmediatamente en lugar de usar una clave diferente es que la nueva generación de claves debe tratarse como una expiración implícita de todas las claves que se activaron antes de la nueva clave. La idea general es que pueden haber sido configuradas nuevas claves con algoritmos diferentes o mecanismos de cifrado en el resto de las claves antiguas, y el sistema debe preferir al usar la configuración actual.

Hay una excepción. Si el desarrollador de aplicaciones tiene [deshabilita la generación automática de claves](../configuration/overview.md#data-protection-configuring-disable-automatic-key-generation), a continuación, el sistema de protección de datos debe elegir algo como la clave predeterminada. En este escenario de reserva, el sistema elegirá la clave no revocados con la fecha de activación más reciente, con preferencia otorgado a las claves que hayan tenido tiempo para propagar a otros equipos del clúster. El sistema de reserva puede acabar elegir una clave predeterminada expiradas como resultado. El sistema de reserva no elegirá nunca una clave revocada como la clave predeterminada y si el anillo de clave está vacío o todas las claves se ha revocado el sistema generará un error en la inicialización.

<a name=data-protection-implementation-key-management-expiration></a>

## <a name="key-expiration-and-rolling"></a>Expiración de la clave y gradual

Cuando se crea una clave, que se genera automáticamente una fecha de activación de {now + 2 días} y una fecha de expiración de {now + 90 días}. El retraso de 2 días antes de la activación le ofrece la key time en propagarse a través del sistema. Es decir, permite que otras aplicaciones que apunta a la memoria auxiliar observar la clave en el siguiente período de actualización automática, lo que maximiza las posibilidades de que cuando la clave de anillo activo hace que se convierten en que se propague a todas las aplicaciones que pueden necesitar para utilizan.

Si la clave predeterminada expirará dentro de 2 días y el anillo de clave aún no tiene una clave que se activará tras la expiración de la clave de forma predeterminada, el sistema de protección de datos conservará automáticamente una nueva clave para el anillo de clave. Esta nueva clave tiene una fecha de activación de {fecha de expiración de la clave predeterminada} y una fecha de expiración de {now + 90 días}. Esto permite al sistema poner automáticamente las claves de forma regular con ninguna interrupción del servicio.

Puede haber circunstancias donde se creará una clave con la activación inmediata. Un ejemplo sería cuando la aplicación no se haya ejecutado durante un tiempo y todas las claves en el anillo de clave se ha caducado. Cuando esto ocurre, la clave se genera una fecha de activación de {ahora} sin el retardo de activación normal de 2 días.

La vigencia de la clave predeterminada es 90 días, aunque esto es configurable como en el ejemplo siguiente.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
   ```

Un administrador también puede cambiar el valor predeterminado para todo el sistema, aunque una llamada explícita a SetDefaultKeyLifetime invalidará cualquier directiva de todo el sistema. La vigencia de la clave predeterminada no puede ser inferior a 7 días.

## <a name="automatic-keyring-refresh"></a>Actualización automática del conjunto de claves

Cuando se inicializa el sistema de protección de datos, lee el anillo de clave desde el repositorio subyacente y lo almacena en caché en memoria. Esta memoria caché permite proteger y desproteger operaciones podrán continuar sin alcanzar el almacén de copia de seguridad. El sistema comprobará automáticamente la memoria auxiliar para cambios aproximadamente cada 24 horas o cuando caduca la clave predeterminada actual, lo que ocurra primero.

>[!WARNING]
> Los desarrolladores rara vez deberían (si alguna vez) que deba usar las API de administración clave directamente. El sistema de protección de datos llevará a cabo la administración automática de claves como se describió anteriormente.

El sistema de protección de datos expone una interfaz IKeyManager que puede utilizar para inspeccionar y realizar cambios en el anillo de clave. El sistema DI que proporciona la instancia de IDataProtectionProvider también puede proporcionar una instancia de IKeyManager para su consumo. Como alternativa, puede insertar el IKeyManager recta desde IServiceProvider como en el ejemplo siguiente.

Cualquier operación que modifica el anillo de clave (crear una nueva clave explícitamente o realizar una revocación) invalidará la memoria caché en memoria. La siguiente llamada a proteger o desproteger hará que el sistema de protección de datos leer el anillo de clave y volver a crear la memoria caché.

El ejemplo siguiente muestra cómo utilizar la interfaz IKeyManager para inspeccionar y manipular el anillo de clave, incluida la revocación de claves existentes y generar una nueva clave manualmente.

[!code-none[Main](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Almacenamiento de claves

El sistema de protección de datos tiene una heurística mediante el cual intenta deducir automáticamente una ubicación de almacenamiento de claves adecuado y el cifrado en el mecanismo de rest. Esto también es configurable por el desarrollador de aplicaciones. Los documentos siguientes explican las implementaciones de forma predeterminada de estos mecanismos:

* [Proveedores de almacenamiento de claves de forma predeterminada](key-storage-providers.md#data-protection-implementation-key-storage-providers)

* [Cifrado de claves de forma predeterminada en proveedores de rest](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest-providers)
