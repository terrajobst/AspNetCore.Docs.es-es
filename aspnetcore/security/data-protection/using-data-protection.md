---
title: "Introducción a las API de protección de datos"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 9489b55b1de626b77bcbe21cb9678e561b559099
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="af32e-103">Introducción a las API de protección de datos</span><span class="sxs-lookup"><span data-stu-id="af32e-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="af32e-104">En su protección de datos más simple consta de los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="af32e-104">At its simplest protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="af32e-105">Crear un datos protector desde un proveedor de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="af32e-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="af32e-106">Llame al método Protect con los datos que desea proteger.</span><span class="sxs-lookup"><span data-stu-id="af32e-106">Call the Protect method with the data you want to protect.</span></span>

3. <span data-ttu-id="af32e-107">Llame al método Unprotect con los datos que desea convertir en texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="af32e-107">Call the Unprotect method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="af32e-108">Mayoría de los marcos, como ASP.NET o SignalR ya configura el sistema de protección de datos y agregarlo a un contenedor de servicios que se puede acceder a través de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="af32e-108">Most frameworks such as ASP.NET or SignalR already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="af32e-109">El siguiente ejemplo muestra cómo configurar un contenedor de servicios para la inyección de dependencia y registrar la pila de protección de datos, recibe el proveedor de protección de datos a través de DI, crear un protector y datos de protección y desprotección</span><span class="sxs-lookup"><span data-stu-id="af32e-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="af32e-110">Cuando se crea un protector debe proporcionar uno o varios [propósito cadenas](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="af32e-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="af32e-111">Una cadena de propósito proporciona aislamiento entre los consumidores, por ejemplo un protector creado con una cadena de fin de "verde" no sería capaz de desproteger los datos proporcionados por un protector con un propósito de "púrpura".</span><span class="sxs-lookup"><span data-stu-id="af32e-111">A purpose string provides isolation between consumers, for example a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="af32e-112">Las instancias de IDataProtectionProvider y IDataProtector son seguras para subprocesos para distintos llamadores.</span><span class="sxs-lookup"><span data-stu-id="af32e-112">Instances of IDataProtectionProvider and IDataProtector are thread-safe for multiple callers.</span></span> <span data-ttu-id="af32e-113">Su objetivo es que una vez que un componente obtiene una referencia a un IDataProtector mediante una llamada a CreateProtector, utilizará esa referencia para varias llamadas a proteger y desproteger.</span><span class="sxs-lookup"><span data-stu-id="af32e-113">It is intended that once a component gets a reference to an IDataProtector via a call to CreateProtector, it will use that reference for multiple calls to Protect and Unprotect.</span></span>
>
><span data-ttu-id="af32e-114">Una llamada a Unprotect iniciará CryptographicException si no se puede comprobar o descifrar la carga protegida.</span><span class="sxs-lookup"><span data-stu-id="af32e-114">A call to Unprotect will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="af32e-115">Algunos componentes que desee omitir los errores durante la desproteger operaciones; un componente que lee las cookies de autenticación puede controlar este error y tratar la solicitud como si no tuviera en absoluto ninguna cookie en lugar de producirá un error en la solicitud directamente.</span><span class="sxs-lookup"><span data-stu-id="af32e-115">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="af32e-116">Los componentes que desea este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.</span><span class="sxs-lookup"><span data-stu-id="af32e-116">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
