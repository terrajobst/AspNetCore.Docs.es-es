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
# <a name="miscellaneous-aspnet-core-data-protection-apis"></a><span data-ttu-id="be395-103">Varias API de protección de datos ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be395-103">Miscellaneous ASP.NET Core Data Protection APIs</span></span>

<a name="data-protection-extensibility-mics-apis"></a>

>[!WARNING]
> <span data-ttu-id="be395-104">Tipos que implementan cualquiera de las interfaces siguientes deben ser seguro para subprocesos para varios de los llamadores.</span><span class="sxs-lookup"><span data-stu-id="be395-104">Types that implement any of the following interfaces should be thread-safe for multiple callers.</span></span>

## <a name="isecret"></a><span data-ttu-id="be395-105">ISecret</span><span class="sxs-lookup"><span data-stu-id="be395-105">ISecret</span></span>

<span data-ttu-id="be395-106">La interfaz de `ISecret` representa un valor secreto, como material de clave criptográfica.</span><span class="sxs-lookup"><span data-stu-id="be395-106">The `ISecret` interface represents a secret value, such as cryptographic key material.</span></span> <span data-ttu-id="be395-107">Contiene la siguiente superficie de API:</span><span class="sxs-lookup"><span data-stu-id="be395-107">It contains the following API surface:</span></span>

* <span data-ttu-id="be395-108">`Length`: `int`</span><span class="sxs-lookup"><span data-stu-id="be395-108">`Length`: `int`</span></span>

* <span data-ttu-id="be395-109">`Dispose()`: `void`</span><span class="sxs-lookup"><span data-stu-id="be395-109">`Dispose()`: `void`</span></span>

* <span data-ttu-id="be395-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span><span class="sxs-lookup"><span data-stu-id="be395-110">`WriteSecretIntoBuffer(ArraySegment<byte> buffer)`: `void`</span></span>

<span data-ttu-id="be395-111">El método `WriteSecretIntoBuffer` rellena el búfer proporcionado con el valor de secreto sin formato.</span><span class="sxs-lookup"><span data-stu-id="be395-111">The `WriteSecretIntoBuffer` method populates the supplied buffer with the raw secret value.</span></span> <span data-ttu-id="be395-112">La razón por la que esta API toma el búfer como parámetro en lugar de devolver un `byte[]` directamente es que permite al llamador la oportunidad de anclar el objeto de búfer, lo que limita la exposición secreta al recolector de elementos no utilizados administrado.</span><span class="sxs-lookup"><span data-stu-id="be395-112">The reason this API takes the buffer as a parameter rather than returning a `byte[]` directly is that this gives the caller the opportunity to pin the buffer object, limiting secret exposure to the managed garbage collector.</span></span>

<span data-ttu-id="be395-113">El tipo de `Secret` es una implementación concreta de `ISecret` en la que el valor de secreto se almacena en memoria en proceso.</span><span class="sxs-lookup"><span data-stu-id="be395-113">The `Secret` type is a concrete implementation of `ISecret` where the secret value is stored in in-process memory.</span></span> <span data-ttu-id="be395-114">En las plataformas de Windows, el valor secreto se cifra mediante [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="be395-114">On Windows platforms, the secret value is encrypted via [CryptProtectMemory](https://msdn.microsoft.com/library/windows/desktop/aa380262(v=vs.85).aspx).</span></span>
