---
title: Otras API
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 512c6ba7-88ec-47e4-a656-6b30350b34e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/extensibility/misc-apis
ms.openlocfilehash: 541dd721a00495632f0d633577b55933c9be03fa
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="miscellaneous-apis"></a><span data-ttu-id="c5be9-103">Otras API</span><span class="sxs-lookup"><span data-stu-id="c5be9-103">Miscellaneous APIs</span></span>

<a name=data-protection-extensibility-mics-apis></a>

>[!WARNING]
> <span data-ttu-id="c5be9-104">Los tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para distintos llamadores.</span><span class="sxs-lookup"><span data-stu-id="c5be9-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="c5be9-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="c5be9-105">ISecret</span></span>

<span data-ttu-id="c5be9-106">La interfaz ISecret representa un valor secreto, como material de clave de cifrado.</span><span class="sxs-lookup"><span data-stu-id="c5be9-106">The ISecret interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="c5be9-107">Contiene la superficie de API siguiente.</span><span class="sxs-lookup"><span data-stu-id="c5be9-107">It contains the following API surface.</span></span>

* <span data-ttu-id="c5be9-108">Longitud: int</span><span class="sxs-lookup"><span data-stu-id="c5be9-108">Length : int</span></span>

* <span data-ttu-id="c5be9-109">Dispose(): void</span><span class="sxs-lookup"><span data-stu-id="c5be9-109">Dispose() : void</span></span>

* <span data-ttu-id="c5be9-110">WriteSecretIntoBuffer (ArraySegment<byte> búfer): void</span><span class="sxs-lookup"><span data-stu-id="c5be9-110">WriteSecretIntoBuffer(ArraySegment<byte> buffer) : void</span></span>

<span data-ttu-id="c5be9-111">El método WriteSecretIntoBuffer rellena el búfer proporcionado con el valor sin formato del secreto.</span><span class="sxs-lookup"><span data-stu-id="c5be9-111">The WriteSecretIntoBuffer method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="c5be9-112">La razón de que esta API toma el búfer como un parámetro en lugar de devolver que un byte [] es directamente Esto da al llamador la oportunidad para anclar el objeto de búfer, limitar la exposición de secreto para el recolector de elementos no utilizados administrado.</span><span class="sxs-lookup"><span data-stu-id="c5be9-112">The reason this API takes the buffer as a parameter rather than returning a byte[] directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="c5be9-113">El tipo de secreto es una implementación concreta de ISecret donde el valor secreto se almacena en memoria en el proceso.</span><span class="sxs-lookup"><span data-stu-id="c5be9-113">The Secret type is a concrete implementation of ISecret where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="c5be9-114">En plataformas de Windows, el valor secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="c5be9-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
