---
title: "Protección de datos en ASP.NET Core"
author: rick-anderson
description: "Este documento sirve como tabla de contenido para los distintos temas de protección de datos de ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/index
ms.openlocfilehash: 151385964d877fc9eadaa219320e5f5a195164e4
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2018
---
# <a name="data-protection-in-aspnet-core-consumer-apis-configuration-extensibility-apis-and-implementation"></a><span data-ttu-id="58753-103">Protección de datos en ASP.NET Core: API de consumidor, configuración, API de extensibilidad e implementación</span><span class="sxs-lookup"><span data-stu-id="58753-103">Data Protection in ASP.NET Core: Consumer APIs, configuration, extensibility APIs and implementation</span></span>

* [<span data-ttu-id="58753-104">Introducción a la protección de datos</span><span class="sxs-lookup"><span data-stu-id="58753-104">Introduction to data protection</span></span>](introduction.md)

* [<span data-ttu-id="58753-105">Introducción a las API de protección de datos</span><span class="sxs-lookup"><span data-stu-id="58753-105">Get started with the Data Protection APIs</span></span>](using-data-protection.md)

* [<span data-ttu-id="58753-106">API de consumidor</span><span class="sxs-lookup"><span data-stu-id="58753-106">Consumer APIs</span></span>](consumer-apis/index.md)

  * [<span data-ttu-id="58753-107">Información general sobre las API de consumidor</span><span class="sxs-lookup"><span data-stu-id="58753-107">Consumer APIs overview</span></span>](consumer-apis/overview.md)

  * [<span data-ttu-id="58753-108">Cadenas de propósito</span><span class="sxs-lookup"><span data-stu-id="58753-108">Purpose strings</span></span>](consumer-apis/purpose-strings.md)

  * [<span data-ttu-id="58753-109">Jerarquía de propósito y configuración multiempresa</span><span class="sxs-lookup"><span data-stu-id="58753-109">Purpose hierarchy and multi-tenancy</span></span>](consumer-apis/purpose-strings-multitenancy.md)

  * [<span data-ttu-id="58753-110">Hash de contraseña</span><span class="sxs-lookup"><span data-stu-id="58753-110">Password hashing</span></span>](consumer-apis/password-hashing.md)

  * [<span data-ttu-id="58753-111">Limitación de la duración de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="58753-111">Limiting the lifetime of protected payloads</span></span>](consumer-apis/limited-lifetime-payloads.md)

  * [<span data-ttu-id="58753-112">Desprotección de cargas cuyas claves se han revocado</span><span class="sxs-lookup"><span data-stu-id="58753-112">Unprotecting payloads whose keys have been revoked</span></span>](consumer-apis/dangerous-unprotect.md)

* [<span data-ttu-id="58753-113">Configuración</span><span class="sxs-lookup"><span data-stu-id="58753-113">Configuration</span></span>](configuration/index.md)

  * [<span data-ttu-id="58753-114">Configuración de la protección de datos</span><span class="sxs-lookup"><span data-stu-id="58753-114">Configuring data protection</span></span>](configuration/overview.md)

  * [<span data-ttu-id="58753-115">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="58753-115">Default settings</span></span>](configuration/default-settings.md)

  * [<span data-ttu-id="58753-116">Directiva de todo el equipo</span><span class="sxs-lookup"><span data-stu-id="58753-116">Machine-wide policy</span></span>](configuration/machine-wide-policy.md)

  * [<span data-ttu-id="58753-117">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="58753-117">Non DI-aware scenarios</span></span>](configuration/non-di-scenarios.md)

* [<span data-ttu-id="58753-118">API de extensibilidad</span><span class="sxs-lookup"><span data-stu-id="58753-118">Extensibility APIs</span></span>](extensibility/index.md)

  * [<span data-ttu-id="58753-119">Extensibilidad de criptografía de núcleo</span><span class="sxs-lookup"><span data-stu-id="58753-119">Core cryptography extensibility</span></span>](extensibility/core-crypto.md)

  * [<span data-ttu-id="58753-120">Extensibilidad de administración de claves</span><span class="sxs-lookup"><span data-stu-id="58753-120">Key management extensibility</span></span>](extensibility/key-management.md)

  * [<span data-ttu-id="58753-121">Otras API</span><span class="sxs-lookup"><span data-stu-id="58753-121">Miscellaneous APIs</span></span>](extensibility/misc-apis.md)

* [<span data-ttu-id="58753-122">Implementación</span><span class="sxs-lookup"><span data-stu-id="58753-122">Implementation</span></span>](implementation/index.md)

  * [<span data-ttu-id="58753-123">Detalles de cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="58753-123">Authenticated encryption details</span></span>](implementation/authenticated-encryption-details.md)

  * [<span data-ttu-id="58753-124">Derivación de subclave y cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="58753-124">Subkey derivation and authenticated encryption</span></span>](implementation/subkeyderivation.md)

  * [<span data-ttu-id="58753-125">Encabezados de contexto</span><span class="sxs-lookup"><span data-stu-id="58753-125">Context headers</span></span>](implementation/context-headers.md)

  * [<span data-ttu-id="58753-126">Administración de claves</span><span class="sxs-lookup"><span data-stu-id="58753-126">Key management</span></span>](implementation/key-management.md)

  * [<span data-ttu-id="58753-127">Proveedores de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="58753-127">Key storage providers</span></span>](implementation/key-storage-providers.md)

  * [<span data-ttu-id="58753-128">Cifrado de claves en reposo</span><span class="sxs-lookup"><span data-stu-id="58753-128">Key encryption at rest</span></span>](implementation/key-encryption-at-rest.md)

  * [<span data-ttu-id="58753-129">Inmutabilidad de claves y cambio de configuración</span><span class="sxs-lookup"><span data-stu-id="58753-129">Key immutability and changing settings</span></span>](implementation/key-immutability.md)

  * [<span data-ttu-id="58753-130">Formato de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="58753-130">Key storage format</span></span>](implementation/key-storage-format.md)

  * [<span data-ttu-id="58753-131">Proveedores de protección de datos efímeros</span><span class="sxs-lookup"><span data-stu-id="58753-131">Ephemeral data protection providers</span></span>](implementation/key-storage-ephemeral.md)

* [<span data-ttu-id="58753-132">Compatibilidad</span><span class="sxs-lookup"><span data-stu-id="58753-132">Compatibility</span></span>](compatibility/index.md)

  * [<span data-ttu-id="58753-133">Uso compartido de cookies entre aplicaciones</span><span class="sxs-lookup"><span data-stu-id="58753-133">Sharing cookies among apps</span></span>](xref:security/data-protection/compatibility/cookie-sharing)

  * [<span data-ttu-id="58753-134">Reemplazar <machineKey> en ASP.NET</span><span class="sxs-lookup"><span data-stu-id="58753-134">Replacing <machineKey> in ASP.NET</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
