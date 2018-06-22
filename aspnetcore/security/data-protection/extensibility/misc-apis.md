---
title: API de protección de datos de varios núcleos de ASP.NET
author: rick-anderson
description: Obtenga información acerca de la interfaz de ISecret de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 114cdd6209970e46b827e403fbe79b95692d0242
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279160"
---
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="9173a-103">API de protección de datos de varios núcleos de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9173a-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="9173a-104">Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.</span><span class="sxs-lookup"><span data-stu-id="9173a-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="9173a-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="9173a-105">ISecret</span></span>

<span data-ttu-id="9173a-106">El `ISecret` interfaz representa un valor secreto, como material de clave de cifrado.</span><span class="sxs-lookup"><span data-stu-id="9173a-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="9173a-107">Contiene la superficie de API siguiente:</span><span class="sxs-lookup"><span data-stu-id="9173a-107">It contains the following API surface:</span></span>

* <span data-ttu-id="9173a-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="9173a-108">`Length`: `int`</span></span>

* <span data-ttu-id="9173a-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="9173a-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="9173a-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="9173a-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="9173a-111">El `WriteSecretIntoBuffer` método rellena el búfer proporcionado con el valor sin formato del secreto.</span><span class="sxs-lookup"><span data-stu-id="9173a-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="9173a-112">El motivo de esta API toma el búfer como un parámetro en lugar de devolver un `byte[]` directamente es esto da al llamador la oportunidad para anclar el objeto de búfer, limitar la exposición de secreto para el recolector de elementos no utilizados administrado.</span><span class="sxs-lookup"><span data-stu-id="9173a-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="9173a-113">El `Secret` tipo es una implementación concreta de `ISecret` donde el valor secreto se almacena en memoria en el proceso.</span><span class="sxs-lookup"><span data-stu-id="9173a-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="9173a-114">En plataformas de Windows, el valor secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="9173a-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
