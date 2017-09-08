---
title: "Inmutabilidad de clave y cambiar la configuración"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fc911ae3-feca-4ffe-8b43-362bc632fe04
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 3afce8f84ebe3b709ea169c7db27f99829f157ab
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="key-immutability-and-changing-settings"></a><span data-ttu-id="d8f82-103">Inmutabilidad de clave y cambiar la configuración</span><span class="sxs-lookup"><span data-stu-id="d8f82-103">Key Immutability and changing settings</span></span>

<span data-ttu-id="d8f82-104">Una vez que un objeto se mantiene en la memoria auxiliar, su representación de infinito es fijo.</span><span class="sxs-lookup"><span data-stu-id="d8f82-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="d8f82-105">Se pueden agregar datos nuevos a la memoria auxiliar, pero nunca pueden transformarse los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="d8f82-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="d8f82-106">El propósito principal de este comportamiento es evitar daños en los datos.</span><span class="sxs-lookup"><span data-stu-id="d8f82-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="d8f82-107">Una consecuencia de este comportamiento es que, cuando una clave se escribe en la memoria auxiliar, es inmutable.</span><span class="sxs-lookup"><span data-stu-id="d8f82-107">One consequence of this behavior is that once a key is written to the backing store, it is immutable.</span></span> <span data-ttu-id="d8f82-108">Nunca se puede cambiar su fecha de creación, activación y caducidad, aunque puede revocar utilizando IKeyManager.</span><span class="sxs-lookup"><span data-stu-id="d8f82-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using IKeyManager.</span></span> <span data-ttu-id="d8f82-109">Además, su información algorítmica subyacente, el material de creación de claves maestras y el cifrado en Propiedades de rest también son inmutables.</span><span class="sxs-lookup"><span data-stu-id="d8f82-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="d8f82-110">Si el desarrollador cambia cualquier configuración que afecta a la persistencia de clave, esos cambios no entrará en vigor hasta la próxima vez que se genere una clave, ya sea a través de una llamada explícita a IKeyManager.CreateNewKey o datos protección del sistema propio [automática generación de claves](key-management.md#data-protection-implementation-key-management) comportamiento.</span><span class="sxs-lookup"><span data-stu-id="d8f82-110">If the developer changes any setting that affects key persistence, those changes will not go into effect until the next time a key is generated, either via an explicit call to IKeyManager.CreateNewKey or via the data protection system's own [automatic key generation](key-management.md#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="d8f82-111">La configuración que afecta a la persistencia de clave es los siguientes:</span><span class="sxs-lookup"><span data-stu-id="d8f82-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="d8f82-112">La vigencia de clave predeterminada</span><span class="sxs-lookup"><span data-stu-id="d8f82-112">The default key lifetime</span></span>](key-management.md#data-protection-implementation-key-management)

* [<span data-ttu-id="d8f82-113">El cifrado de claves en el mecanismo de rest</span><span class="sxs-lookup"><span data-stu-id="d8f82-113">The key encryption at rest mechanism</span></span>](key-encryption-at-rest.md#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="d8f82-114">La información algorítmica contenida dentro de la clave</span><span class="sxs-lookup"><span data-stu-id="d8f82-114">The algorithmic information contained within the key</span></span>](../configuration/overview.md#data-protection-changing-algorithms)

<span data-ttu-id="d8f82-115">Si necesita estas opciones para iniciar anteriores a la siguiente tecla automática gradual tiempo, considere la posibilidad de realizar una llamada explícita a IKeyManager.CreateNewKey para forzar la creación de una nueva clave.</span><span class="sxs-lookup"><span data-stu-id="d8f82-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to IKeyManager.CreateNewKey to force the creation of a new key.</span></span> <span data-ttu-id="d8f82-116">Recuerde que debe proporcionar una fecha de activación explícita ({ahora + 2 días} es una buena regla general para dejar tiempo propagar el cambio) y la fecha de expiración en la llamada.</span><span class="sxs-lookup"><span data-stu-id="d8f82-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="d8f82-117">Todas las aplicaciones tocar el repositorio deben especificar la misma configuración con los métodos de extensión IDataProtectionBuilder, en caso contrario, las propiedades de la clave persistente estarán depende de la aplicación en particular que invoca las rutinas de generación de claves .</span><span class="sxs-lookup"><span data-stu-id="d8f82-117">All applications touching the repository should specify the same settings with the IDataProtectionBuilder extension methods, otherwise the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
