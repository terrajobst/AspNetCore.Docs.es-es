---
title: API de protección de datos de varios núcleos de ASP.NET
author: rick-anderson
description: Obtenga información acerca de la interfaz ISecret de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896622"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>API de protección de datos de varios núcleos de ASP.NET

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.

## <a name="isecret"></a>ISecret

El `ISecret` interfaz representa un valor de secreto, como material de clave criptográfica. Contiene la superficie de API siguiente:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

El `WriteSecretIntoBuffer` método rellena el búfer proporcionado con el valor del secreto sin procesar. El motivo de esta API toma el búfer como un parámetro en lugar de devolver un `byte[]` directamente es que esto proporciona el llamador la oportunidad para anclar el objeto de búfer, limitar la exposición secreta para el recolector de elementos no utilizados administrado.

El `Secret` tipo es una implementación concreta de `ISecret` donde el valor del secreto se almacena en memoria en proceso. En las plataformas Windows, el valor del secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
