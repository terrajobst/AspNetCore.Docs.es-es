---
title: Administración de claves en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los detalles de implementación de la API de administración clave de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-management
ms.openlocfilehash: 431bdf2d3076c83279b78f327ddb647f69e6e584
ms.sourcegitcommit: 8f8924ce4eb9effeaf489f177fb01b66867da16f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39219256"
---
# <a name="key-management-in-aspnet-core"></a>Administración de claves en ASP.NET Core

<a name="data-protection-implementation-key-management"></a>

El sistema de protección de datos administra automáticamente la duración de las claves maestras de usa para proteger y desproteger cargas. Cada clave puede existir en uno de cuatro fases:

* Se ha creado: la clave existe en el conjunto de claves, pero aún no se ha activado. La clave no debe usarse para nuevas operaciones de protección hasta que haya transcurrido suficiente tiempo que la clave ha tenido la oportunidad de propagarse a todos los equipos que usan este conjunto de claves.

* Activo - en la clave existe en el conjunto de claves y se debe usar para todas las operaciones de proteger de nuevo.

* Ha caducado: la clave de su duración natural ha ejecutado y ya no se debe usar para las operaciones de protección nuevo.

* Revocar - la clave está en peligro y no debe usarse para nuevas operaciones de protección.

Las claves creadas, activas y caducadas pueden utilizarse para desprotección de cargas entrantes. Revocadas claves de forma predeterminada no pueden usarse para desprotección de cargas, pero el desarrollador de aplicaciones puede [invalidar este comportamiento](xref:security/data-protection/consumer-apis/dangerous-unprotect#data-protection-consumer-apis-dangerous-unprotect) si es necesario.

>[!WARNING]
> El desarrollador podría verse tentado a eliminar una clave desde el conjunto de claves (por ejemplo, eliminando el archivo correspondiente del sistema de archivos). En ese momento, todos los datos protegidos por la clave es indescifrables de forma permanente y no hay ninguna invalidación de emergencia como ocurre con las claves revocadas. Eliminación de una clave es el comportamiento realmente destructivo, y por lo tanto el sistema de protección de datos no expone ninguna API de primera clase para realizar esta operación.

## <a name="default-key-selection"></a>Selección de la clave predeterminada

Cuando el sistema de protección de datos lee el conjunto de claves desde el repositorio de respaldo, intentará encontrar una clave de "default" desde el conjunto de claves. La clave predeterminada se usa para operaciones de proteger de nuevo.

La heurística general es que el sistema de protección de datos elige la clave con la fecha de activación más reciente que la clave predeterminada. (Hay un factor aglutinante pequeño para permitir el reloj del servidor a servidor sesgo). Si se ha expirado o se revoca la clave de generación de claves y si la aplicación no ha deshabilitado automática, a continuación, se generará una nueva clave con la activación de inmediata por el [caducidad y gradual clave](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management-expiration) directiva siguiente.

El motivo por el sistema de protección de datos genera una nueva clave inmediatamente en lugar de recurrir a una clave diferente es que la nueva generación de claves debe tratarse como una fecha de expiración implícito de todas las claves que se han activado antes de la nueva clave. La idea general es que nuevas claves se han configurado con algoritmos diferentes o mecanismos de cifrado en reposo que las claves antiguas, y el sistema debería preferir al Revirtiendo la configuración actual.

Hay una excepción. Si el desarrollador de aplicaciones tiene [deshabilita la generación automática de claves](xref:security/data-protection/configuration/overview#disableautomatickeygeneration), a continuación, el sistema de protección de datos debe elegir algo como la clave predeterminada. En este escenario de reserva, el sistema elegirá la clave no revocados con la fecha de activación más reciente, preferentemente a las claves que han tenido tiempo en propagarse a otros equipos del clúster. El sistema de reserva puede acabar elegir una clave expirada predeterminada como resultado. El sistema de reserva nunca se elegirá una clave revocada como clave predeterminada y, si el conjunto de claves está vacío o todas las claves se ha revocado el sistema generará un error en la inicialización.

<a name="data-protection-implementation-key-management-expiration"></a>

## <a name="key-expiration-and-rolling"></a>Expiración de la clave y gradual

Cuando se crea una clave, automáticamente ha proporcionado una fecha de activación de {ahora + 2 días} y una fecha de expiración de {ahora + 90 días}. El retraso de 2 días antes de la activación le ofrece la key time en propagarse a través del sistema. Es decir, permite que otras aplicaciones que apunta al almacén de respaldo observar la clave en su siguiente período de actualización automática, lo que maximiza las posibilidades de que cuando la clave de anillo activa hace que se convierten en se haya propagado a todas las aplicaciones que pueden necesitar para usarlo.

Si la clave predeterminada expirará dentro de 2 días y el conjunto de claves ya no tiene una clave que se activará tras la expiración de la clave de forma predeterminada, el sistema de protección de datos conservará automáticamente una nueva clave para el conjunto de claves. Esta nueva clave tiene una fecha de activación de {fecha de expiración de la clave predeterminada} y una fecha de expiración de {ahora + 90 días}. Esto permite al sistema implementar automáticamente las claves de forma periódica se produzca ninguna interrupción del servicio.

Puede haber circunstancias donde se creará una clave con la activación de inmediata. Un ejemplo sería cuando la aplicación no se ha ejecutado durante un tiempo y todas las claves en el conjunto de claves se ha expirado. Cuando esto sucede, la clave se proporciona una fecha de activación de {ahora} sin provocar el retraso de activación de 2 días normal.

La vigencia de clave predeterminado es 90 días, aunque esto es configurable en el ejemplo siguiente.

```csharp
services.AddDataProtection()
       // use 14-day lifetime instead of 90-day lifetime
       .SetDefaultKeyLifetime(TimeSpan.FromDays(14));
```

Un administrador también puede cambiar el valor predeterminado de todo el sistema, aunque una llamada explícita a `SetDefaultKeyLifetime` invalidará cualquier directiva de todo el sistema. Duración de la clave predeterminada no puede ser inferior a 7 días.

## <a name="automatic-key-ring-refresh"></a>Actualización automática de conjunto de claves

Cuando se inicializa el sistema de protección de datos, lee el conjunto de claves desde el repositorio subyacente y lo almacena en caché en memoria. Esta caché permite proteger y desproteger operaciones podrán continuar sin llegar a la memoria auxiliar. El sistema comprobará automáticamente el almacén de respaldo para cambios aproximadamente cada 24 horas o cuando caduca la clave predeterminada actual, lo que ocurra primero.

>[!WARNING]
> Los desarrolladores deberían con poca frecuencia (si alguna vez) es necesario usar directamente las API de administración de clave. El sistema de protección de datos llevará a cabo la administración automática de claves como se describió anteriormente.

El sistema de protección de datos expone una interfaz `IKeyManager` que se puede utilizar para inspeccionar y realizar cambios en el conjunto de claves. El sistema de DI que proporciona la instancia de `IDataProtectionProvider` también puede proporcionar una instancia de `IKeyManager` su consumo. Como alternativa, puede extraer el `IKeyManager` directamente desde el `IServiceProvider` como se muestra en el ejemplo siguiente.

Cualquier operación que modifica el conjunto de claves (crear una nueva clave de forma explícita o realizar una revocación) invalidará la memoria caché en memoria. La siguiente llamada a `Protect` o `Unprotect` hará que el sistema de protección de datos vuelva a leer el conjunto de claves y volver a crear la memoria caché.

El ejemplo siguiente muestra cómo utilizar el `IKeyManager` interfaz para inspeccionar y manipular el conjunto de claves, incluida la revocación de claves existentes y generar una nueva clave manualmente.

[!code-csharp[](key-management/samples/key-management.cs)]

## <a name="key-storage"></a>Almacenamiento de claves

El sistema de protección de datos tiene un método heurístico mediante el cual intenta deducir una ubicación de almacenamiento de claves adecuado y el mecanismo de cifrado en reposo automáticamente. El mecanismo de persistencia de clave también es configurable por el desarrollador de aplicaciones. Los siguientes documentos describen las implementaciones en el cuadro de estos mecanismos:

* <xref:security/data-protection/implementation/key-storage-providers>
* <xref:security/data-protection/implementation/key-encryption-at-rest>
