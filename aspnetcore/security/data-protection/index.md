---
title: "Protección de datos en ASP.NET Core"
author: rick-anderson
description: "Este documento sirve como tabla de contenido para los distintos temas de protección de datos de ASP.NET Core."
keywords: "ASP.NET Core, protección de datos"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 1f402da8-1052-4970-9835-9f9f16a02dbc
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: cbf18c6ec867fefec22980f3e3493562594ef72d
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="76352-104">Protección de datos en ASP.NET Core: API de consumidor, configuración, API de extensibilidad e implementación</span><span class="sxs-lookup"><span data-stu-id="76352-104">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="76352-105">Introducción a la protección de datos</span><span class="sxs-lookup"><span data-stu-id="76352-105">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="76352-106">Introducción a las API de protección de datos</span><span class="sxs-lookup"><span data-stu-id="76352-106">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="76352-107">API de consumidor</span><span class="sxs-lookup"><span data-stu-id="76352-107">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="76352-108">Información general sobre las API de consumidor</span><span class="sxs-lookup"><span data-stu-id="76352-108">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="76352-109">Cadenas de propósito</span><span class="sxs-lookup"><span data-stu-id="76352-109">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="76352-110">Jerarquía de propósito y configuración multiempresa</span><span class="sxs-lookup"><span data-stu-id="76352-110">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="76352-111">Hash de contraseña</span><span class="sxs-lookup"><span data-stu-id="76352-111">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="76352-112">Limitación de la duración de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="76352-112">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="76352-113">Desprotección de cargas cuyas claves se han revocado</span><span class="sxs-lookup"><span data-stu-id="76352-113">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="76352-114">Configuración</span><span class="sxs-lookup"><span data-stu-id="76352-114">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="76352-115">Configuración de la protección de datos</span><span class="sxs-lookup"><span data-stu-id="76352-115">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="76352-116">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="76352-116">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="76352-117">Directiva de todo el equipo</span><span class="sxs-lookup"><span data-stu-id="76352-117">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="76352-118">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="76352-118">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="76352-119">API de extensibilidad</span><span class="sxs-lookup"><span data-stu-id="76352-119">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="76352-120">Extensibilidad de criptografía de núcleo</span><span class="sxs-lookup"><span data-stu-id="76352-120">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="76352-121">Extensibilidad de administración de claves</span><span class="sxs-lookup"><span data-stu-id="76352-121">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="76352-122">Otras API</span><span class="sxs-lookup"><span data-stu-id="76352-122">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="76352-123">Implementación</span><span class="sxs-lookup"><span data-stu-id="76352-123">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="76352-124">Detalles de cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="76352-124">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="76352-125">Derivación de subclave y cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="76352-125">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="76352-126">Encabezados de contexto</span><span class="sxs-lookup"><span data-stu-id="76352-126">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="76352-127">Administración de claves</span><span class="sxs-lookup"><span data-stu-id="76352-127">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="76352-128">Proveedores de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="76352-128">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="76352-129">Cifrado de claves en reposo</span><span class="sxs-lookup"><span data-stu-id="76352-129">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="76352-130">Inmutabilidad de claves y cambio de configuración</span><span class="sxs-lookup"><span data-stu-id="76352-130">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="76352-131">Formato de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="76352-131">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="76352-132">Proveedores de protección de datos efímeros</span><span class="sxs-lookup"><span data-stu-id="76352-132">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="76352-133">Compatibilidad</span><span class="sxs-lookup"><span data-stu-id="76352-133">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="76352-134">Uso compartido de cookies entre aplicaciones</span><span class="sxs-lookup"><span data-stu-id="76352-134">Sharing cookies between apps</span></span>](compatibility/cookie-sharing.md)

  * [<span data-ttu-id="76352-135">Reemplazar <machineKey> en ASP.NET</span><span class="sxs-lookup"><span data-stu-id="76352-135">Replacing <machineKey> in ASP.NET</span></span>](compatibility/replacing-machinekey.md)
