---
title: Otras API
author: rick-anderson
description: "Este documento describe la interfaz de ISecret de protección de datos de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 88a08a25abf4c4e1ba0746087b05b1cc8fa13024
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="80465-103">Otras API</span><span class="sxs-lookup"><span data-stu-id="80465-103">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="80465-104">Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.</span><span class="sxs-lookup"><span data-stu-id="80465-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="80465-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="80465-105">ISecret</span></span>

<span data-ttu-id="80465-106">El `ISecret` interfaz representa un valor secreto, como material de clave de cifrado.</span><span class="sxs-lookup"><span data-stu-id="80465-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="80465-107">Contiene la superficie de API siguiente:</span><span class="sxs-lookup"><span data-stu-id="80465-107">It contains the following API surface:</span></span>

* <span data-ttu-id="80465-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="80465-108">`Length`: `int`</span></span>

* <span data-ttu-id="80465-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="80465-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="80465-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="80465-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="80465-111">El `WriteSecretIntoBuffer` método rellena el búfer proporcionado con el valor sin formato del secreto.</span><span class="sxs-lookup"><span data-stu-id="80465-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="80465-112">El motivo de esta API toma el búfer como un parámetro en lugar de devolver un `byte[]` directamente es esto da al llamador la oportunidad para anclar el objeto de búfer, limitar la exposición de secreto para el recolector de elementos no utilizados administrado.</span><span class="sxs-lookup"><span data-stu-id="80465-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="80465-113">El `Secret` tipo es una implementación concreta de `ISecret` donde el valor secreto se almacena en memoria en el proceso.</span><span class="sxs-lookup"><span data-stu-id="80465-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="80465-114">En plataformas de Windows, el valor secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="80465-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
