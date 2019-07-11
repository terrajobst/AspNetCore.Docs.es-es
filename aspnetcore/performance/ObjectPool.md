---
title: Reutilización de los objetos con ObjectPool en ASP.NET Core
author: rick-anderson
description: Sugerencias para aumentar el rendimiento en aplicaciones de ASP.NET Core mediante ObjectPool.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
uid: performance/ObjectPool
ms.openlocfilehash: 771f19e54a908b8b2cd85ff72f368f16e94a2310
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815525"
---
# <a name="object-reuse-with-objectpool-in-aspnet-core"></a>Reutilización de los objetos con ObjectPool en ASP.NET Core

Por [Steve Gordon](https://twitter.com/stevejgordon), [Ryan Nowak](https://github.com/rynowak), y [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.ObjectPool> forma parte de la infraestructura de ASP.NET Core que admita mantener un grupo de objetos en memoria para su reutilización en lugar de permitir que los objetos de elementos no utilizados recopilados.

Es posible que desea utilizar el grupo de objetos si los objetos que se van a administrar son:

- Asignar o inicializar resulta caro.
- Representan algunos recursos limitados.
- Usa de forma predecible y con frecuencia.

Por ejemplo, el marco de ASP.NET Core usa el grupo de objetos en algunos lugares reutilizar <xref:System.Text.StringBuilder> instancias. `StringBuilder` asigna y administra su propia búferes para almacenar datos de caracteres. ASP.NET Core se usa con frecuencia `StringBuilder` para implementar las características y reutilizarlos mejora el rendimiento.

Agrupación de objetos siempre no mejora el rendimiento:

- A menos que el costo de inicialización de un objeto es alto, es normalmente más lento obtener el objeto del grupo.
- Objetos administrados por el grupo no se anulará la asignación hasta que se anule la asignación del grupo.

Usar agrupación de objetos solo después de recopilar datos de rendimiento mediante escenarios realistas para su aplicación o biblioteca.

**ADVERTENCIA: El `ObjectPool` no implementa `IDisposable`. No se recomienda utilizarlo con tipos que necesitan la eliminación.**

**NOTA: El ObjectPool no pone un límite en el número de objetos que va a asignar, impone un límite en el número de objetos que conservarán.**

## <a name="concepts"></a>Conceptos

<xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> -la abstracción del grupo de objetos básicos. Se utiliza para obtener y devolver objetos.

<xref:Microsoft.Extensions.ObjectPool.PooledObjectPolicy%601> -implementar esto para personalizar cómo se crea un objeto y cómo es *restablecer* cuando regrese al grupo. Esto se puede pasar a un grupo de objetos que se construye directamente... OR

<xref:Microsoft.Extensions.ObjectPool.ObjectPoolProvider.Create*> actúa como un generador para crear los grupos de objetos.
<!-- REview, there is no ObjectPoolProvider<T> -->

El ObjectPool puede usarse en una aplicación de varias maneras:

* Crear instancias de un grupo.
* Registrar un grupo en [inserción de dependencias](xref:fundamentals/dependency-injection) (DI) como una instancia.
* Registrar el `ObjectPoolProvider<>` en DI y usarlo como una fábrica.

## <a name="how-to-use-objectpool"></a>Cómo usar ObjectPool

Llame a <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1> para obtener un objeto y <xref:Microsoft.Extensions.ObjectPool.ObjectPool`1.Return*> para devolver el objeto.  No hay ningún requisito que devuelva todos los objetos. Si no devuelve un objeto, será recolectado.

## <a name="objectpool-sample"></a>Ejemplo de ObjectPool

El código siguiente:

* Agrega `ObjectPoolProvider` a la [inserción de dependencias](xref:fundamentals/dependency-injection) contenedor (DI).
* Agrega y configura `ObjectPool<StringBuilder>` para el contenedor de DI.
* Agrega el `BirthdayMiddleware`.

[!code-csharp[](ObjectPool/ObjectPoolSample/Startup.cs?name=snippet)]

El código siguiente implementa `BirthdayMiddleware`

[!code-csharp[](ObjectPool/ObjectPoolSample/BirthdayMiddleware.cs?name=snippet)]
