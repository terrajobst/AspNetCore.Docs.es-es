---
title: Otras API
author: rick-anderson
description: "Este documento describe la interfaz de ISecret de protección de datos de ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 26474de3a1c681a8c7f8b7812e9ef859f5d448bb
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="miscellaneous-apis"></a>Otras API

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.

## <a name="isecret"></a>ISecret

El `ISecret` interfaz representa un valor secreto, como material de clave de cifrado. Contiene la superficie de API siguiente:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

El `WriteSecretIntoBuffer` método rellena el búfer proporcionado con el valor sin formato del secreto. El motivo de esta API toma el búfer como un parámetro en lugar de devolver un `byte[]` directamente es esto da al llamador la oportunidad para anclar el objeto de búfer, limitar la exposición de secreto para el recolector de elementos no utilizados administrado.

El `Secret` tipo es una implementación concreta de `ISecret` donde el valor secreto se almacena en memoria en el proceso. En plataformas de Windows, el valor secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
