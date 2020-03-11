---
title: Varias API de protección de datos ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la ASP.NET Core interfaz de ISecret de protección de datos.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654359"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a>Varias API de protección de datos ASP.NET Core

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.

## <a name="isecret"></a>ISecret

La interfaz de `ISecret` representa un valor secreto, como material de clave criptográfica. Contiene la siguiente superficie de API:

* `Length`: `int`

* `Dispose()`: `void`

* `WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`

El método `WriteSecretIntoBuffer` rellena el búfer proporcionado con el valor de secreto sin formato. La razón por la que esta API toma el búfer como parámetro en lugar de devolver un `byte[]` directamente es que permite al llamador la oportunidad de anclar el objeto de búfer, lo que limita la exposición secreta al recolector de elementos no utilizados administrado.

El tipo de `Secret` es una implementación concreta de `ISecret` en la que el valor de secreto se almacena en memoria en proceso. En las plataformas de Windows, el valor secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).
