---
title: "Limita la duración de cargas protegidos"
author: rick-anderson
description: "Este documento explica cómo limitar la duración de una carga protegida mediante la API de protección de datos de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/limited-lifetime-payloads
ms.openlocfilehash: 144097cd1551c1d0aece5df20ce01e14146a41d1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="limiting-the-lifetime-of-protected-payloads"></a><span data-ttu-id="a755c-103">Limita la duración de cargas protegidos</span><span class="sxs-lookup"><span data-stu-id="a755c-103">Limiting the lifetime of protected payloads</span></span>

<span data-ttu-id="a755c-104">Existen escenarios donde desea que el desarrollador de aplicaciones para crear una carga protegida que expira tras un período de tiempo establecido.</span><span class="sxs-lookup"><span data-stu-id="a755c-104">There are scenarios where the application developer wants to create a protected payload that expires after a set period of time.</span></span> <span data-ttu-id="a755c-105">Por ejemplo, la carga protegida podría representar un token de restablecimiento de contraseña que solo debe ser válido durante una hora.</span><span class="sxs-lookup"><span data-stu-id="a755c-105">For instance, the protected payload might represent a password reset token that should only be valid for one hour.</span></span> <span data-ttu-id="a755c-106">Es posible para el desarrollador puede crear su propio formato de carga que contiene una fecha de expiración incrustados, y los desarrolladores avanzados que desee hacerlo de todos modos, pero para la mayoría de los desarrolladores administrar estos caducidad puede crecer tedioso.</span><span class="sxs-lookup"><span data-stu-id="a755c-106">It is certainly possible for the developer to create their own payload format that contains an embedded expiration date, and advanced developers may wish to do this anyway, but for the majority of developers managing these expirations can grow tedious.</span></span>

<span data-ttu-id="a755c-107">Para facilitar esta tarea para nuestros clientes de desarrollador, el paquete [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contiene las API de la utilidad para crear cargas de expiran automáticamente tras un período de tiempo establecido.</span><span class="sxs-lookup"><span data-stu-id="a755c-107">To make this easier for our developer audience, the package [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) contains utility APIs for creating payloads that automatically expire after a set period of time.</span></span> <span data-ttu-id="a755c-108">Estas API de bloqueo de la `ITimeLimitedDataProtector` tipo.</span><span class="sxs-lookup"><span data-stu-id="a755c-108">These APIs hang off of the `ITimeLimitedDataProtector` type.</span></span>

## <a name="api-usage"></a><span data-ttu-id="a755c-109">Uso de la API</span><span class="sxs-lookup"><span data-stu-id="a755c-109">API usage</span></span>

<span data-ttu-id="a755c-110">El `ITimeLimitedDataProtector` es la interfaz principal para proteger y desproteger cargas de tiempo limitado / expiración automática.</span><span class="sxs-lookup"><span data-stu-id="a755c-110">The `ITimeLimitedDataProtector` interface is the core interface for protecting and unprotecting time-limited / self-expiring payloads.</span></span> <span data-ttu-id="a755c-111">Para crear una instancia de un `ITimeLimitedDataProtector`, primero necesitará una instancia de una normal [IDataProtector](overview.md) construido con un propósito específico.</span><span class="sxs-lookup"><span data-stu-id="a755c-111">To create an instance of an `ITimeLimitedDataProtector`, you'll first need an instance of a regular [IDataProtector](overview.md) constructed with a specific purpose.</span></span> <span data-ttu-id="a755c-112">Una vez el `IDataProtector` instancia esté disponible, llame a la `IDataProtector.ToTimeLimitedDataProtector` método de extensión para devolver un protector con capacidades integradas de expiración.</span><span class="sxs-lookup"><span data-stu-id="a755c-112">Once the `IDataProtector` instance is available, call the `IDataProtector.ToTimeLimitedDataProtector` extension method to get back a protector with built-in expiration capabilities.</span></span>

<span data-ttu-id="a755c-113">`ITimeLimitedDataProtector`expone los siguientes métodos de superficie y extensión de API:</span><span class="sxs-lookup"><span data-stu-id="a755c-113">`ITimeLimitedDataProtector` exposes the following API surface and extension methods:</span></span>

* <span data-ttu-id="a755c-114">CreateProtector (propósito de cadena): ITimeLimitedDataProtector - esta API es similar a la existente `IDataProtectionProvider.CreateProtector` en que se puede utilizar para crear [finalidad cadenas](purpose-strings.md) desde un protector de tiempo limitado de raíz.</span><span class="sxs-lookup"><span data-stu-id="a755c-114">CreateProtector(string purpose) : ITimeLimitedDataProtector - This API is similar to the existing `IDataProtectionProvider.CreateProtector` in that it can be used to create [purpose chains](purpose-strings.md) from a root time-limited protector.</span></span>

* <span data-ttu-id="a755c-115">Proteger (byte [] texto simple, expiración de DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="a755c-115">Protect(byte[] plaintext, DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="a755c-116">Proteger (texto simple de byte [], duración del intervalo de tiempo): byte]</span><span class="sxs-lookup"><span data-stu-id="a755c-116">Protect(byte[] plaintext, TimeSpan lifetime) : byte[]</span></span>

* <span data-ttu-id="a755c-117">Proteger (como texto simple de byte []): byte]</span><span class="sxs-lookup"><span data-stu-id="a755c-117">Protect(byte[] plaintext) : byte[]</span></span>

* <span data-ttu-id="a755c-118">Proteger (texto simple de cadena, expiración de DateTimeOffset): cadena</span><span class="sxs-lookup"><span data-stu-id="a755c-118">Protect(string plaintext, DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="a755c-119">Proteger (texto simple de cadena, duración del intervalo de tiempo): cadena</span><span class="sxs-lookup"><span data-stu-id="a755c-119">Protect(string plaintext, TimeSpan lifetime) : string</span></span>

* <span data-ttu-id="a755c-120">Proteger (texto simple de la cadena): cadena</span><span class="sxs-lookup"><span data-stu-id="a755c-120">Protect(string plaintext) : string</span></span>

<span data-ttu-id="a755c-121">Además de los principales `Protect` métodos que toman sólo el texto no cifrado, hay nuevas sobrecargas que permiten especificar la fecha de expiración de la carga.</span><span class="sxs-lookup"><span data-stu-id="a755c-121">In addition to the core `Protect` methods which take only the plaintext, there are new overloads which allow specifying the payload's expiration date.</span></span> <span data-ttu-id="a755c-122">La fecha de expiración se puede especificar como una fecha absoluta (a través de un `DateTimeOffset`) o como una hora relativa (desde el sistema actual time y a través de un `TimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="a755c-122">The expiration date can be specified as an absolute date (via a `DateTimeOffset`) or as a relative time (from the current system time, via a `TimeSpan`).</span></span> <span data-ttu-id="a755c-123">Si se llama a una sobrecarga que no toma una expiración, se supone la carga nunca a punto de expirar.</span><span class="sxs-lookup"><span data-stu-id="a755c-123">If an overload which doesn't take an expiration is called, the payload is assumed never to expire.</span></span>

* <span data-ttu-id="a755c-124">Desproteger (byte [] protectedData, out expiración DateTimeOffset): byte]</span><span class="sxs-lookup"><span data-stu-id="a755c-124">Unprotect(byte[] protectedData, out DateTimeOffset expiration) : byte[]</span></span>

* <span data-ttu-id="a755c-125">Desproteger (byte [] protectedData): byte]</span><span class="sxs-lookup"><span data-stu-id="a755c-125">Unprotect(byte[] protectedData) : byte[]</span></span>

* <span data-ttu-id="a755c-126">Desproteger (cadena protectedData, out expiración DateTimeOffset): cadena</span><span class="sxs-lookup"><span data-stu-id="a755c-126">Unprotect(string protectedData, out DateTimeOffset expiration) : string</span></span>

* <span data-ttu-id="a755c-127">Desproteger (cadena protectedData): cadena</span><span class="sxs-lookup"><span data-stu-id="a755c-127">Unprotect(string protectedData) : string</span></span>

<span data-ttu-id="a755c-128">El `Unprotect` métodos devuelven los datos protegidos originales.</span><span class="sxs-lookup"><span data-stu-id="a755c-128">The `Unprotect` methods return the original unprotected data.</span></span> <span data-ttu-id="a755c-129">Si aún no ha expirado la carga, la expiración absoluta se devuelve como un parámetro junto con los datos protegidos originales de salida opcionales.</span><span class="sxs-lookup"><span data-stu-id="a755c-129">If the payload hasn't yet expired, the absolute expiration is returned as an optional out parameter along with the original unprotected data.</span></span> <span data-ttu-id="a755c-130">Si la carga ha expirado, todas las sobrecargas del método Unprotect producirá CryptographicException.</span><span class="sxs-lookup"><span data-stu-id="a755c-130">If the payload is expired, all overloads of the Unprotect method will throw CryptographicException.</span></span>

>[!WARNING]
> <span data-ttu-id="a755c-131">No se recomienda usar estas API para proteger las cargas que requieren la persistencia a largo plazo o indefinida.</span><span class="sxs-lookup"><span data-stu-id="a755c-131">It is not advised to use these APIs to protect payloads which require long-term or indefinite persistence.</span></span> <span data-ttu-id="a755c-132">"¿Permitirme para que las cargas protegidas ser irrecuperables permanentemente después de un mes?"</span><span class="sxs-lookup"><span data-stu-id="a755c-132">"Can I afford for the protected payloads to be permanently unrecoverable after a month?"</span></span> <span data-ttu-id="a755c-133">puede actuar como una buena regla general; Si la respuesta es no a los desarrolladores a continuación, deben considerar las API alternativas.</span><span class="sxs-lookup"><span data-stu-id="a755c-133">can serve as a good rule of thumb; if the answer is no then developers should consider alternative APIs.</span></span>

<span data-ttu-id="a755c-134">El ejemplo siguiente utiliza la [las rutas de código no DI](../configuration/non-di-scenarios.md) para crear instancias del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="a755c-134">The sample below uses the [non-DI code paths](../configuration/non-di-scenarios.md) for instantiating the data protection system.</span></span> <span data-ttu-id="a755c-135">Para ejecutar este ejemplo, asegúrese de que ha agregado una referencia al paquete Microsoft.AspNetCore.DataProtection.Extensions en primer lugar.</span><span class="sxs-lookup"><span data-stu-id="a755c-135">To run this sample, ensure that you have first added a reference to the Microsoft.AspNetCore.DataProtection.Extensions package.</span></span>

[!code-csharp[Main](limited-lifetime-payloads/samples/limitedlifetimepayloads.cs)]
