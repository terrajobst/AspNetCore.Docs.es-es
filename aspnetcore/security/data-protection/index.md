---
title: Protección de datos en ASP.NET Core
author: rick-anderson
description: Este documento sirve como tabla de contenido para los distintos temas de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277129"
---
# <a name="data-protection-in-aspnet-core"></a><span data-ttu-id="e250e-103">Protección de datos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e250e-103">Data Protection in ASP.NET Core</span></span>

* [<span data-ttu-id="e250e-104">Introducción a la protección de datos</span><span class="sxs-lookup"><span data-stu-id="e250e-104">Introduction to data protection</span></span>](xref:security/data-protection/introduction)

* [<span data-ttu-id="e250e-105">Introducción a las API de protección de datos</span><span class="sxs-lookup"><span data-stu-id="e250e-105">Get started with the Data Protection APIs</span></span>](xref:security/data-protection/using-data-protection)

* [<span data-ttu-id="e250e-106">API de consumidor</span><span class="sxs-lookup"><span data-stu-id="e250e-106">Consumer APIs</span></span>](xref:security/data-protection/consumer-apis/index)

  * [<span data-ttu-id="e250e-107">Información general sobre las API de consumidor</span><span class="sxs-lookup"><span data-stu-id="e250e-107">Consumer APIs overview</span></span>](xref:security/data-protection/consumer-apis/overview)

  * [<span data-ttu-id="e250e-108">Cadenas de propósito</span><span class="sxs-lookup"><span data-stu-id="e250e-108">Purpose strings</span></span>](xref:security/data-protection/consumer-apis/purpose-strings)

  * [<span data-ttu-id="e250e-109">Jerarquía de propósito y configuración multiempresa</span><span class="sxs-lookup"><span data-stu-id="e250e-109">Purpose hierarchy and multi-tenancy</span></span>](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [<span data-ttu-id="e250e-110">Aplicar un algoritmo hash a las contraseñas</span><span class="sxs-lookup"><span data-stu-id="e250e-110">Hash passwords</span></span>](xref:security/data-protection/consumer-apis/password-hashing)

  * [<span data-ttu-id="e250e-111">Limitación de la duración de cargas protegidas</span><span class="sxs-lookup"><span data-stu-id="e250e-111">Limit the lifetime of protected payloads</span></span>](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [<span data-ttu-id="e250e-112">Desprotección de cargas cuyas claves se han revocado</span><span class="sxs-lookup"><span data-stu-id="e250e-112">Unprotect payloads whose keys have been revoked</span></span>](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [<span data-ttu-id="e250e-113">Configuración</span><span class="sxs-lookup"><span data-stu-id="e250e-113">Configuration</span></span>](xref:security/data-protection/configuration/index)

  * [<span data-ttu-id="e250e-114">Configurar la protección de datos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e250e-114">Configure ASP.NET Core Data Protection</span></span>](xref:security/data-protection/configuration/overview)

  * [<span data-ttu-id="e250e-115">Configuración predeterminada</span><span class="sxs-lookup"><span data-stu-id="e250e-115">Default settings</span></span>](xref:security/data-protection/configuration/default-settings)

  * [<span data-ttu-id="e250e-116">Directiva de todo el equipo</span><span class="sxs-lookup"><span data-stu-id="e250e-116">Machine-wide policy</span></span>](xref:security/data-protection/configuration/machine-wide-policy)

  * [<span data-ttu-id="e250e-117">Escenarios no compatibles con DI</span><span class="sxs-lookup"><span data-stu-id="e250e-117">Non DI-aware scenarios</span></span>](xref:security/data-protection/configuration/non-di-scenarios)

* [<span data-ttu-id="e250e-118">API de extensibilidad</span><span class="sxs-lookup"><span data-stu-id="e250e-118">Extensibility APIs</span></span>](xref:security/data-protection/extensibility/index)

  * [<span data-ttu-id="e250e-119">Extensibilidad de criptografía de núcleo</span><span class="sxs-lookup"><span data-stu-id="e250e-119">Core cryptography extensibility</span></span>](xref:security/data-protection/extensibility/core-crypto)

  * [<span data-ttu-id="e250e-120">Extensibilidad de administración de claves</span><span class="sxs-lookup"><span data-stu-id="e250e-120">Key management extensibility</span></span>](xref:security/data-protection/extensibility/key-management)

  * [<span data-ttu-id="e250e-121">Otras API</span><span class="sxs-lookup"><span data-stu-id="e250e-121">Miscellaneous APIs</span></span>](xref:security/data-protection/extensibility/misc-apis)

* [<span data-ttu-id="e250e-122">Implementación</span><span class="sxs-lookup"><span data-stu-id="e250e-122">Implementation</span></span>](xref:security/data-protection/implementation/index)

  * [<span data-ttu-id="e250e-123">Detalles de cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="e250e-123">Authenticated encryption details</span></span>](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [<span data-ttu-id="e250e-124">Derivación de subclave y cifrado autenticado</span><span class="sxs-lookup"><span data-stu-id="e250e-124">Subkey derivation and authenticated encryption</span></span>](xref:security/data-protection/implementation/subkeyderivation)

  * [<span data-ttu-id="e250e-125">Encabezados de contexto</span><span class="sxs-lookup"><span data-stu-id="e250e-125">Context headers</span></span>](xref:security/data-protection/implementation/context-headers)

  * [<span data-ttu-id="e250e-126">Administración de claves</span><span class="sxs-lookup"><span data-stu-id="e250e-126">Key management</span></span>](xref:security/data-protection/implementation/key-management)

  * [<span data-ttu-id="e250e-127">Proveedores de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="e250e-127">Key storage providers</span></span>](xref:security/data-protection/implementation/key-storage-providers)

  * [<span data-ttu-id="e250e-128">Cifrado de claves en reposo</span><span class="sxs-lookup"><span data-stu-id="e250e-128">Key encryption at rest</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [<span data-ttu-id="e250e-129">Inmutabilidad de claves y configuración</span><span class="sxs-lookup"><span data-stu-id="e250e-129">Key immutability and settings</span></span>](xref:security/data-protection/implementation/key-immutability)

  * [<span data-ttu-id="e250e-130">Formato de almacenamiento de claves</span><span class="sxs-lookup"><span data-stu-id="e250e-130">Key storage format</span></span>](xref:security/data-protection/implementation/key-storage-format)

  * [<span data-ttu-id="e250e-131">Proveedores de protección de datos efímeros</span><span class="sxs-lookup"><span data-stu-id="e250e-131">Ephemeral data protection providers</span></span>](xref:security/data-protection/implementation/key-storage-ephemeral)

* [<span data-ttu-id="e250e-132">Compatibilidad</span><span class="sxs-lookup"><span data-stu-id="e250e-132">Compatibility</span></span>](xref:security/data-protection/compatibility/index)

  * [<span data-ttu-id="e250e-133">Reemplazar <machineKey> de ASP.NET en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e250e-133">Replacing ASP.NET <machineKey> in ASP.NET Core</span></span>](xref:security/data-protection/compatibility/replacing-machinekey)
