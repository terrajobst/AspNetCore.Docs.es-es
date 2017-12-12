---
title: Otras API
author: rick-anderson
description: "Este documento describe la interfaz de ISecret de protección de datos de ASP.NET Core."
keywords: "Núcleo de ASP.NET, protección de datos, ISecret"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: f5d6920f9f229bd480a76c952dab30efb7d9eff5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="6ef91-104">Otras API</span><span class="sxs-lookup"><span data-stu-id="6ef91-104">Miscellaneous APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="6ef91-105">Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.</span><span class="sxs-lookup"><span data-stu-id="6ef91-105">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="6ef91-106">ISecret</span><span class="sxs-lookup"><span data-stu-id="6ef91-106">ISecret</span></span>

<span data-ttu-id="6ef91-107">El `ISecret` interfaz representa un valor secreto, como material de clave de cifrado.</span><span class="sxs-lookup"><span data-stu-id="6ef91-107">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="6ef91-108">Contiene la superficie de API siguiente:</span><span class="sxs-lookup"><span data-stu-id="6ef91-108">It contains the following API surface:</span></span>

* <span data-ttu-id="6ef91-109">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="6ef91-109">`Length`: `int`</span></span>

* <span data-ttu-id="6ef91-110">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="6ef91-110">`Dispose()`: `void`</span></span>

* <span data-ttu-id="6ef91-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="6ef91-111">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="6ef91-112">El `WriteSecretIntoBuffer` método rellena el búfer proporcionado con el valor sin formato del secreto.</span><span class="sxs-lookup"><span data-stu-id="6ef91-112">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="6ef91-113">El motivo de esta API toma el búfer como un parámetro en lugar de devolver un `byte[]` directamente es esto da al llamador la oportunidad para anclar el objeto de búfer, limitar la exposición de secreto para el recolector de elementos no utilizados administrado.</span><span class="sxs-lookup"><span data-stu-id="6ef91-113">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="6ef91-114">El `Secret` tipo es una implementación concreta de `ISecret` donde el valor secreto se almacena en memoria en el proceso.</span><span class="sxs-lookup"><span data-stu-id="6ef91-114">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="6ef91-115">En plataformas de Windows, el valor secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="6ef91-115">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
