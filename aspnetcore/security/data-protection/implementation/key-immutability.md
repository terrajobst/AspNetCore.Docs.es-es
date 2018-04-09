---
title: Inmutabilidad de clave y la configuración de clave de ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los detalles de implementación de la inmutabilidad de clave de protección de datos de ASP.NET Core API.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: e918b00562aca9821de87c38f10242177517d8a5
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="1ee96-103">Inmutabilidad de clave y la configuración de clave de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ee96-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="1ee96-104">Una vez que un objeto se mantiene en la memoria auxiliar, su representación de infinito es fijo.</span><span class="sxs-lookup"><span data-stu-id="1ee96-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="1ee96-105">Se pueden agregar datos nuevos a la memoria auxiliar, pero nunca pueden transformarse los datos existentes.</span><span class="sxs-lookup"><span data-stu-id="1ee96-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="1ee96-106">El propósito principal de este comportamiento es evitar daños en los datos.</span><span class="sxs-lookup"><span data-stu-id="1ee96-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="1ee96-107">Una consecuencia de este comportamiento es que, cuando una clave se escribe en la memoria auxiliar, es inmutable.</span><span class="sxs-lookup"><span data-stu-id="1ee96-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="1ee96-108">Su fecha de creación, activación y expiración nunca se puede cambiar, aunque puede revocar utilizando `IKeyManager`.</span><span class="sxs-lookup"><span data-stu-id="1ee96-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="1ee96-109">Además, su información algorítmica subyacente, el material de creación de claves maestras y el cifrado en Propiedades de rest también son inmutables.</span><span class="sxs-lookup"><span data-stu-id="1ee96-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="1ee96-110">Si el desarrollador cambia cualquier configuración que afecta a la persistencia de clave, esos cambios no entran en vigor hasta la próxima vez que se genere una clave, ya sea a través de una llamada explícita a `IKeyManager.CreateNewKey` o a través de lo datos protección del sistema propio [clave automática generación](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) comportamiento.</span><span class="sxs-lookup"><span data-stu-id="1ee96-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="1ee96-111">La configuración que afecta a la persistencia de clave es los siguientes:</span><span class="sxs-lookup"><span data-stu-id="1ee96-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="1ee96-112">La vigencia de clave predeterminada</span><span class="sxs-lookup"><span data-stu-id="1ee96-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="1ee96-113">El cifrado de claves en el mecanismo de rest</span><span class="sxs-lookup"><span data-stu-id="1ee96-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="1ee96-114">La información algorítmica contenida dentro de la clave</span><span class="sxs-lookup"><span data-stu-id="1ee96-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="1ee96-115">Si necesita estas opciones para iniciar anteriores a la siguiente tecla automática gradual tiempo, considere la posibilidad de realizar una llamada explícita a `IKeyManager.CreateNewKey` para forzar la creación de una nueva clave.</span><span class="sxs-lookup"><span data-stu-id="1ee96-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="1ee96-116">Recuerde que debe proporcionar una fecha de activación explícita ({ahora + 2 días} es una buena regla general para dejar tiempo propagar el cambio) y la fecha de expiración en la llamada.</span><span class="sxs-lookup"><span data-stu-id="1ee96-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="1ee96-117">Todas las aplicaciones tocar el repositorio deben especificar la misma configuración con el `IDataProtectionBuilder` métodos de extensión.</span><span class="sxs-lookup"><span data-stu-id="1ee96-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="1ee96-118">De lo contrario, las propiedades de la clave persistente será depende de la aplicación en particular que invoca las rutinas de generación de claves.</span><span class="sxs-lookup"><span data-stu-id="1ee96-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
