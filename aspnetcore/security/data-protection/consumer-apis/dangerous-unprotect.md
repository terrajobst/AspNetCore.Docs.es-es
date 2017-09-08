---
title: Desproteger cargas cuyas claves se han revocado
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 44f21f380b994f46a8bb7368bca0cfc6e438ec4d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="e2377-103">Desproteger cargas cuyas claves se han revocado</span><span class="sxs-lookup"><span data-stu-id="e2377-103">Unprotecting payloads whose keys have been revoked</span></span>

<a name=data-protection-consumer-apis-dangerous-unprotect></a>

<span data-ttu-id="e2377-104">La API de protección de datos de ASP.NET Core no están pensados principalmente para la persistencia indefinido de cargas confidenciales.</span><span class="sxs-lookup"><span data-stu-id="e2377-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="e2377-105">Otras tecnologías, como [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) y [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) son más adecuados para el escenario de almacenamiento indefinido, y tienen capacidades de administración de claves seguro según corresponda.</span><span class="sxs-lookup"><span data-stu-id="e2377-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="e2377-106">Es decir, no hay nada prohibir a un desarrollador usa las API de protección de datos de ASP.NET Core para la protección a largo plazo de información confidencial.</span><span class="sxs-lookup"><span data-stu-id="e2377-106">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="e2377-107">Claves nunca se quitan del anillo de clave, por lo que IDataProtector.Unprotect siempre puede recuperar cargas existentes, como las claves están disponibles y válido.</span><span class="sxs-lookup"><span data-stu-id="e2377-107">Keys are never removed from the key ring, so IDataProtector.Unprotect can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="e2377-108">Sin embargo, un problema se produce cuando el programador intenta desproteger los datos que se ha protegido con una clave revocada, tal y como IDataProtector.Unprotect se iniciará una excepción en este caso.</span><span class="sxs-lookup"><span data-stu-id="e2377-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as IDataProtector.Unprotect will throw an exception in this case.</span></span> <span data-ttu-id="e2377-109">Esto podría ser bien para las cargas breves o temporales (por ejemplo, tokens de autenticación), tal y como fácilmente se pueden volver a crear estos tipos de cargas por el sistema y en el peor el visitante del sitio podría ser necesario volver a iniciar sesión.</span><span class="sxs-lookup"><span data-stu-id="e2377-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="e2377-110">Pero para cargas persistentes, tener Unprotect throw podría provocar pérdida de datos aceptable.</span><span class="sxs-lookup"><span data-stu-id="e2377-110">But for persisted payloads, having Unprotect throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="e2377-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="e2377-111">IPersistedDataProtector</span></span>

<span data-ttu-id="e2377-112">Para admitir el escenario de permitir cargas que se debe desproteger incluso de cara a revocados claves, el sistema de protección de datos contiene un tipo de IPersistedDataProtector.</span><span class="sxs-lookup"><span data-stu-id="e2377-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an IPersistedDataProtector type.</span></span> <span data-ttu-id="e2377-113">Para obtener una instancia de IPersistedDataProtector, obtener una instancia de IDataProtector de manera normal e intente convertir el IDataProtector para IPersistedDataProtector.</span><span class="sxs-lookup"><span data-stu-id="e2377-113">To get an instance of IPersistedDataProtector, simply get an instance of IDataProtector in the normal fashion and try casting the IDataProtector to IPersistedDataProtector.</span></span>

> [!NOTE]
> <span data-ttu-id="e2377-114">No todas las instancias de IDataProtector pueden convertirse a IPersistedDataProtector.</span><span class="sxs-lookup"><span data-stu-id="e2377-114">Not all IDataProtector instances can be cast to IPersistedDataProtector.</span></span> <span data-ttu-id="e2377-115">Los programadores deben utilizar el C# como operador o similar evitar las excepciones en tiempo de ejecución deberse a conversiones no válidas y que deben estar preparado para controlar el caso de fallo adecuadamente.</span><span class="sxs-lookup"><span data-stu-id="e2377-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="e2377-116">IPersistedDataProtector expone la superficie de API siguiente:</span><span class="sxs-lookup"><span data-stu-id="e2377-116">IPersistedDataProtector exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

<span data-ttu-id="e2377-117">Esta API toma la carga protegida (como una matriz de bytes) y devuelve la carga sin protección.</span><span class="sxs-lookup"><span data-stu-id="e2377-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="e2377-118">No hay ninguna sobrecarga basada en cadena.</span><span class="sxs-lookup"><span data-stu-id="e2377-118">There is no string-based overload.</span></span> <span data-ttu-id="e2377-119">Los dos parámetros de salida son los siguientes.</span><span class="sxs-lookup"><span data-stu-id="e2377-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="e2377-120">requiresMigration: se establece en true si la clave utilizada para proteger esta carga ya no es la clave predeterminada activa, por ejemplo, la clave utilizada para proteger esta carga es antigua y tiene una operación de reversión de clave desde tendrán lugar.</span><span class="sxs-lookup"><span data-stu-id="e2377-120">requiresMigration: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="e2377-121">El llamador puede llamar a considere la posibilidad de volver a proteger la carga de la función de sus necesidades empresariales.</span><span class="sxs-lookup"><span data-stu-id="e2377-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="e2377-122">wasRevoked: se establecerá en true si la clave utilizada para proteger esta carga se ha revocado.</span><span class="sxs-lookup"><span data-stu-id="e2377-122">wasRevoked: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="e2377-123">Debe extremar las precauciones al pasar ignoreRevocationErrors: True si el método DangerousUnprotect.</span><span class="sxs-lookup"><span data-stu-id="e2377-123">Exercise extreme caution when passing ignoreRevocationErrors: true to the DangerousUnprotect method.</span></span> <span data-ttu-id="e2377-124">Si después de llamar a este método, el valor de wasRevoked es true, a continuación, se ha revocado la clave utilizada para proteger esta carga y autenticidad de la carga debe tratarse como sospechosa.</span><span class="sxs-lookup"><span data-stu-id="e2377-124">If after calling this method the wasRevoked value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="e2377-125">En este caso sólo seguir funcionando en la carga sin protección si tienen seguridad independiente que es auténtico, por ejemplo, que TI del procedentes de una base de datos segura en lugar de enviarse por un cliente web no es de confianza.</span><span class="sxs-lookup"><span data-stu-id="e2377-125">In this case only continue operating on the unprotected payload if you have some separate assurance that it is authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

<span data-ttu-id="e2377-126">[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]</span><span class="sxs-lookup"><span data-stu-id="e2377-126">[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]</span></span>
