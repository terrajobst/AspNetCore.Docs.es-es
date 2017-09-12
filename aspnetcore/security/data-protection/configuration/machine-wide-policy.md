---
title: Directiva para todo el equipo
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 285ae47d-e0bf-4b03-b0a8-2b1fb18bc3a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/machine-wide-policy
ms.openlocfilehash: 7ada940acfbb7fb0887fd7c0cd722bf62f211248
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="machine-wide-policy"></a><span data-ttu-id="3e6cd-103">Directiva para todo el equipo</span><span class="sxs-lookup"><span data-stu-id="3e6cd-103">Machine Wide Policy</span></span>

<a name=data-protection-configuration-machinewidepolicy></a>

<span data-ttu-id="3e6cd-104">Cuando se ejecuta en Windows, el sistema de protección de datos tiene compatibilidad limitada para establecer la directiva de equipo predeterminada para todas las aplicaciones que consumen la protección de datos.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-104">When running on Windows, the data protection system has limited support for setting default machine-wide policy for all applications which consume data protection.</span></span> <span data-ttu-id="3e6cd-105">La idea general es que un administrador puede que desee cambiar algún valor predeterminado (por ejemplo, los algoritmos utilizados o clave la duración) sin tener que actualizar manualmente cada aplicación en el equipo.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-105">The general idea is that an administrator might wish to change some default setting (such as algorithms used or key lifetime) without needing to manually update every application on the machine.</span></span>

>[!WARNING]
> <span data-ttu-id="3e6cd-106">El administrador del sistema puede establecer la directiva predeterminada, pero no puede aplicarla.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-106">The system administrator can set default policy, but they cannot enforce it.</span></span> <span data-ttu-id="3e6cd-107">Desarrollador de la aplicación siempre puede reemplazar cualquier valor con uno de su propia elección.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-107">The application developer can always override any value with one of their own choosing.</span></span> <span data-ttu-id="3e6cd-108">La directiva predeterminada sólo afecta a las aplicaciones que el desarrollador ha especificado un valor explícito para algunos configuración concreta.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-108">The default policy only affects applications where the developer has not specified an explicit value for some particular setting.</span></span>

## <a name="setting-default-policy"></a><span data-ttu-id="3e6cd-109">Establecer la directiva predeterminada</span><span class="sxs-lookup"><span data-stu-id="3e6cd-109">Setting default policy</span></span>

<span data-ttu-id="3e6cd-110">Para establecer una directiva de forma predeterminada, un administrador puede establecer los valores conocidos en el registro del sistema en la siguiente clave.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-110">To set default policy, an administrator can set known values in the system registry under the following key.</span></span>

<span data-ttu-id="3e6cd-111">La clave del registro:`HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span><span class="sxs-lookup"><span data-stu-id="3e6cd-111">Reg key: `HKLM\SOFTWARE\Microsoft\DotNetPackages\Microsoft.AspNetCore.DataProtection`</span></span>

<span data-ttu-id="3e6cd-112">Si está en un sistema operativo de 64 bits y desea afectan al comportamiento de las aplicaciones de 32 bits, no olvide también configurar el equivalente de Wow6432Node de la clave anterior.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-112">If you're on a 64-bit operating system and want to affect the behavior of 32-bit applications, remember also to configure the Wow6432Node equivalent of the above key.</span></span>

<span data-ttu-id="3e6cd-113">Los valores admitidos son:</span><span class="sxs-lookup"><span data-stu-id="3e6cd-113">The supported values are:</span></span>

* <span data-ttu-id="3e6cd-114">EncryptionType [cadena] - especifica los algoritmos que se deben usar para la protección de datos.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-114">EncryptionType [string] - specifies which algorithms should be used for data protection.</span></span> <span data-ttu-id="3e6cd-115">Este valor debe ser "CNG-CBC", "CNG GCM" o "Administrado" y se describe con más detalle [a continuación](#data-protection-encryption-types).</span><span class="sxs-lookup"><span data-stu-id="3e6cd-115">This value must be "CNG-CBC", "CNG-GCM", or "Managed" and is described in more detail [below](#data-protection-encryption-types).</span></span>

* <span data-ttu-id="3e6cd-116">DefaultKeyLifetime [DWORD] - especifica la duración de claves recién generado.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-116">DefaultKeyLifetime [DWORD] - specifies the lifetime for newly-generated keys.</span></span> <span data-ttu-id="3e6cd-117">Este valor se especifica en días y debe ser ≥ 7.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-117">This value is specified in days and must be ≥ 7.</span></span>

* <span data-ttu-id="3e6cd-118">KeyEscrowSinks [cadena] - especifica los tipos que se usará para la custodia de clave.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-118">KeyEscrowSinks [string] - specifies the types which will be used for key escrow.</span></span> <span data-ttu-id="3e6cd-119">Este valor es una lista delimitada por punto y coma de receptores de custodia de clave, donde cada elemento de la lista es el nombre del ensamblado de un tipo que implementa IKeyEscrowSink.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-119">This value is a semicolon-delimited list of key escrow sinks, where each element in the list is the assembly qualified name of a type which implements IKeyEscrowSink.</span></span>

<a name=data-protection-encryption-types></a>

### <a name="encryption-types"></a><span data-ttu-id="3e6cd-120">Tipos de cifrado</span><span class="sxs-lookup"><span data-stu-id="3e6cd-120">Encryption types</span></span>

<span data-ttu-id="3e6cd-121">Si EncryptionType es "CNG-CBC", el sistema se configurará para utilizar un cifrado por bloques simétrico modo CBC para confidencialidad y HMAC para autenticidad con servicios proporcionados por Windows CNG (vea [especificar algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="3e6cd-121">If EncryptionType is "CNG-CBC", the system will be configured to use a CBC-mode symmetric block cipher for confidentiality and HMAC for authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="3e6cd-122">Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de CngCbcAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="3e6cd-122">The following additional values are supported, each of which corresponds to a property on the CngCbcAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="3e6cd-123">EncryptionAlgorithm [cadena] - el nombre de un algoritmo de cifrado de bloques simétrico entendido CNG.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-123">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="3e6cd-124">Este algoritmo se abrirá en modo CBC.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-124">This algorithm will be opened in CBC mode.</span></span>

* <span data-ttu-id="3e6cd-125">EncryptionAlgorithmProvider [cadena] - el nombre de la implementación del proveedor CNG que puede generar el algoritmo EncryptionAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-125">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="3e6cd-126">EncryptionAlgorithmKeySize [DWORD] - la longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétrico.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-126">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

* <span data-ttu-id="3e6cd-127">HashAlgorithm [cadena] - el nombre de un algoritmo de hash entendido CNG.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-127">HashAlgorithm [string] - the name of a hash algorithm understood by CNG.</span></span> <span data-ttu-id="3e6cd-128">Este algoritmo se abrirá en modo HMAC.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-128">This algorithm will be opened in HMAC mode.</span></span>

* <span data-ttu-id="3e6cd-129">HashAlgorithmProvider [cadena] - el nombre de la implementación del proveedor CNG que puede generar el algoritmo HashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-129">HashAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm HashAlgorithm.</span></span>

<span data-ttu-id="3e6cd-130">Si EncryptionType es "CNG GCM", el sistema se configurará para usar un cifrado por bloques simétrico modo Galois/contador para la confidencialidad y la autenticidad con servicios proporcionados por Windows CNG (vea [especificar algoritmos personalizados de Windows CNG](overview.md#data-protection-changing-algorithms-cng)para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="3e6cd-130">If EncryptionType is "CNG-GCM", the system will be configured to use a Galois/Counter Mode symmetric block cipher for confidentiality and authenticity with services provided by Windows CNG (see [Specifying custom Windows CNG algorithms](overview.md#data-protection-changing-algorithms-cng) for more details).</span></span> <span data-ttu-id="3e6cd-131">Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de CngGcmAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="3e6cd-131">The following additional values are supported, each of which corresponds to a property on the CngGcmAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="3e6cd-132">EncryptionAlgorithm [cadena] - el nombre de un algoritmo de cifrado de bloques simétrico entendido CNG.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-132">EncryptionAlgorithm [string] - the name of a symmetric block cipher algorithm understood by CNG.</span></span> <span data-ttu-id="3e6cd-133">Este algoritmo se abrirá en modo de Galois/contador.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-133">This algorithm will be opened in Galois/Counter Mode.</span></span>

* <span data-ttu-id="3e6cd-134">EncryptionAlgorithmProvider [cadena] - el nombre de la implementación del proveedor CNG que puede generar el algoritmo EncryptionAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-134">EncryptionAlgorithmProvider [string] - the name of the CNG provider implementation which can produce the algorithm EncryptionAlgorithm.</span></span>

* <span data-ttu-id="3e6cd-135">EncryptionAlgorithmKeySize [DWORD] - la longitud (en bits) de la clave para la derivación para el algoritmo de cifrado de bloques simétrico.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-135">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric block cipher algorithm.</span></span>

<span data-ttu-id="3e6cd-136">Si EncryptionType está "administrado", el sistema se configurará para utilizar un SymmetricAlgorithm administrado para la confidencialidad y KeyedHashAlgorithm para autenticidad (vea [especificar personalizado administrado algoritmos](overview.md#data-protection-changing-algorithms-custom-managed) para obtener más detalles).</span><span class="sxs-lookup"><span data-stu-id="3e6cd-136">If EncryptionType is "Managed", the system will be configured to use a managed SymmetricAlgorithm for confidentiality and KeyedHashAlgorithm for authenticity (see [Specifying custom managed algorithms](overview.md#data-protection-changing-algorithms-custom-managed) for more details).</span></span> <span data-ttu-id="3e6cd-137">Se admiten los siguientes valores adicionales, cada uno de los cuales corresponde a una propiedad en el tipo de ManagedAuthenticatedEncryptionSettings:</span><span class="sxs-lookup"><span data-stu-id="3e6cd-137">The following additional values are supported, each of which corresponds to a property on the ManagedAuthenticatedEncryptionSettings type:</span></span>

* <span data-ttu-id="3e6cd-138">EncryptionAlgorithmType [cadena] - el nombre calificado con el ensamblado de un tipo que implementa SymmetricAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-138">EncryptionAlgorithmType [string] - the assembly-qualified name of a type which implements SymmetricAlgorithm.</span></span>

* <span data-ttu-id="3e6cd-139">EncryptionAlgorithmKeySize [DWORD] - la longitud (en bits) de la clave para la derivación para el algoritmo de cifrado simétrico.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-139">EncryptionAlgorithmKeySize [DWORD] - the length (in bits) of the key to derive for the symmetric encryption algorithm.</span></span>

* <span data-ttu-id="3e6cd-140">ValidationAlgorithmType [cadena] - el nombre calificado con el ensamblado de un tipo que implementa KeyedHashAlgorithm.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-140">ValidationAlgorithmType [string] - the assembly-qualified name of a type which implements KeyedHashAlgorithm.</span></span>

<span data-ttu-id="3e6cd-141">Si EncryptionType tiene cualquier otro valor (que no sea null o vacío), el sistema de protección de datos producirá una excepción en el inicio.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-141">If EncryptionType has any other value (other than null / empty), the data protection system will throw an exception at startup.</span></span>

>[!WARNING]
> <span data-ttu-id="3e6cd-142">Al configurar una configuración de directiva predeterminada que afecta a los nombres de tipo (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), los tipos deben ser disponibles para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-142">When configuring a default policy setting that involves type names (EncryptionAlgorithmType, ValidationAlgorithmType, KeyEscrowSinks), the types must be available to the application.</span></span> <span data-ttu-id="3e6cd-143">En la práctica, esto significa que para aplicaciones que se ejecutan en CLR de escritorio, los ensamblados que contienen estos tipos deben ser GACed.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-143">In practice, this means that for applications running on Desktop CLR, the assemblies which contain these types should be GACed.</span></span> <span data-ttu-id="3e6cd-144">Para aplicaciones de ASP.NET Core que se ejecutan en [.NET Core](https://www.microsoft.com/net/core), se deben instalar los paquetes que contienen estos tipos.</span><span class="sxs-lookup"><span data-stu-id="3e6cd-144">For ASP.NET Core applications running on [.NET Core](https://www.microsoft.com/net/core), the packages which contain these types should be installed.</span></span>
