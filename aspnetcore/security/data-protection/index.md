---
title: Protección de datos en ASP.NET Core
author: rick-anderson
description: Este documento sirve como tabla de contenido para los distintos temas de protección de datos de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/index
ms.openlocfilehash: 83b5bb1e6a4942a4d3e5ec0d445fa6e5a21fb533
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2018
ms.locfileid: "30071701"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="77474-103">Protección de datos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77474-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="77474-104">Introducción a la protección de datos</span><span class="sxs-lookup"><span data-stu-id="77474-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="77474-105">Introducción a las API de protección de datos</span><span class="sxs-lookup"><span data-stu-id="77474-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="77474-106">API de consumidor</span><span class="sxs-lookup"><span data-stu-id="77474-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="77474-107">Información general sobre las API de consumidor</span><span class="sxs-lookup"><span data-stu-id="77474-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="77474-108">Cadenas de propósito</span><span class="sxs-lookup"><span data-stu-id="77474-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="77474-109">Jerarquía de propósito y configuración multiempresa</span><span class="sxs-lookup"><span data-stu-id="77474-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="77474-110">Aplicar un algoritmo hash a las contraseñas</span><span class="sxs-lookup"><span data-stu-id="77474-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="77474-111">Limitación de la duración de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="77474-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="77474-112">Desprotección de cargas cuyas claves se han revocado</span><span class="sxs-lookup"><span data-stu-id="77474-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="77474-113">Configuración</span><span class="sxs-lookup"><span data-stu-id="77474-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="77474-114">Configurar la protección de datos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77474-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="77474-115">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="77474-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="77474-116">Directiva de todo el equipo</span><span class="sxs-lookup"><span data-stu-id="77474-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="77474-117">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="77474-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="77474-118">API de extensibilidad</span><span class="sxs-lookup"><span data-stu-id="77474-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="77474-119">Extensibilidad de criptografía de núcleo</span><span class="sxs-lookup"><span data-stu-id="77474-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="77474-120">Extensibilidad de administración de claves</span><span class="sxs-lookup"><span data-stu-id="77474-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="77474-121">Otras API</span><span class="sxs-lookup"><span data-stu-id="77474-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="77474-122">Implementación</span><span class="sxs-lookup"><span data-stu-id="77474-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="77474-123">Detalles de cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="77474-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="77474-124">Derivación de subclave y cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="77474-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="77474-125">Encabezados de contexto</span><span class="sxs-lookup"><span data-stu-id="77474-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="77474-126">Administración de claves</span><span class="sxs-lookup"><span data-stu-id="77474-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="77474-127">Proveedores de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="77474-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="77474-128">Cifrado de claves en reposo</span><span class="sxs-lookup"><span data-stu-id="77474-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="77474-129">Inmutabilidad de claves y configuración</span><span class="sxs-lookup"><span data-stu-id="77474-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="77474-130">Formato de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="77474-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="77474-131">Proveedores de protección de datos efímeros</span><span class="sxs-lookup"><span data-stu-id="77474-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="77474-132">Compatibilidad</span><span class="sxs-lookup"><span data-stu-id="77474-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="77474-133">Reemplazar <machineKey> de ASP.NET en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="77474-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
