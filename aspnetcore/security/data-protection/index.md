---
title: "Protección de datos en ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 39f2ca96f8542de033274ea957b5c7736948c981
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="3bb95-103">Protección de datos en ASP.NET Core: API de consumidor, configuración, API de extensibilidad e implementación</span><span class="sxs-lookup"><span data-stu-id="3bb95-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="3bb95-104">Introducción a la protección de datos</span><span class="sxs-lookup"><span data-stu-id="3bb95-104">Introduction to Data Protection</span></span>](introduction.md)

* [<span data-ttu-id="3bb95-105">Introducción a las API de protección de datos</span><span class="sxs-lookup"><span data-stu-id="3bb95-105">Getting Started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="3bb95-106">API de consumidor</span><span class="sxs-lookup"><span data-stu-id="3bb95-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="3bb95-107">Información general sobre las API de consumidor</span><span class="sxs-lookup"><span data-stu-id="3bb95-107">Consumer APIs Overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="3bb95-108">Cadenas de propósito</span><span class="sxs-lookup"><span data-stu-id="3bb95-108">Purpose Strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="3bb95-109">Jerarquía de propósito y configuración multiempresa</span><span class="sxs-lookup"><span data-stu-id="3bb95-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="3bb95-110">Hash de contraseña</span><span class="sxs-lookup"><span data-stu-id="3bb95-110">Password Hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="3bb95-111">Limitación de la duración de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="3bb95-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="3bb95-112">Desprotección de cargas cuyas claves se han revocado</span><span class="sxs-lookup"><span data-stu-id="3bb95-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="3bb95-113">Configuración</span><span class="sxs-lookup"><span data-stu-id="3bb95-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="3bb95-114">Configuración de la protección de datos</span><span class="sxs-lookup"><span data-stu-id="3bb95-114">Configuring Data Protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="3bb95-115">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="3bb95-115">Default Settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="3bb95-116">Directiva para toda la máquina</span><span class="sxs-lookup"><span data-stu-id="3bb95-116">Machine Wide Policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="3bb95-117">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="3bb95-117">Non DI Aware Scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="3bb95-118">API de extensibilidad</span><span class="sxs-lookup"><span data-stu-id="3bb95-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="3bb95-119">Extensibilidad de criptografía de núcleo</span><span class="sxs-lookup"><span data-stu-id="3bb95-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="3bb95-120">Extensibilidad de administración de claves</span><span class="sxs-lookup"><span data-stu-id="3bb95-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="3bb95-121">Otras API</span><span class="sxs-lookup"><span data-stu-id="3bb95-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="3bb95-122">Implementación</span><span class="sxs-lookup"><span data-stu-id="3bb95-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="3bb95-123">Detalles de cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="3bb95-123">Authenticated encryption details.</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="3bb95-124">Derivación de subclave y cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="3bb95-124">Subkey Derivation and Authenticated Encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="3bb95-125">Encabezados de contexto</span><span class="sxs-lookup"><span data-stu-id="3bb95-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="3bb95-126">Administración de claves</span><span class="sxs-lookup"><span data-stu-id="3bb95-126">Key Management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="3bb95-127">Proveedores de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="3bb95-127">Key Storage Providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="3bb95-128">Cifrado de claves en reposo</span><span class="sxs-lookup"><span data-stu-id="3bb95-128">Key Encryption At Rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="3bb95-129">Inmutabilidad de claves y cambio de configuración</span><span class="sxs-lookup"><span data-stu-id="3bb95-129">Key Immutability and Changing Settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="3bb95-130">Formato de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="3bb95-130">Key Storage Format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="3bb95-131">Proveedores de protección de datos efímeros</span><span class="sxs-lookup"><span data-stu-id="3bb95-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="3bb95-132">Compatibilidad</span><span class="sxs-lookup"><span data-stu-id="3bb95-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="3bb95-133">Uso compartido de cookies entre aplicaciones</span><span class="sxs-lookup"><span data-stu-id="3bb95-133">Sharing cookies between applications</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="3bb95-134">Reemplazar <machineKey> en ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3bb95-134">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
