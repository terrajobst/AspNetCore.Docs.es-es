---
title: Introducción a las API de protección de datos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo usar las API de protección de datos de ASP.NET Core para proteger y desproteger los datos en una aplicación.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087638"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="0b68b-103">Introducción a las API de protección de datos en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0b68b-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="0b68b-104">Su sencilla y protección de datos consta de los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="0b68b-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="0b68b-105">Crear datos de un protector de un proveedor de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="0b68b-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="0b68b-106">Llame a la `Protect` método con los datos que desea proteger.</span><span class="sxs-lookup"><span data-stu-id="0b68b-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="0b68b-107">Llame a la `Unprotect` método con los datos que desea convertir en texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="0b68b-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="0b68b-108">La mayoría de los marcos y modelos de aplicación, como ASP.NET Core SignalR, ya que configuración el sistema de protección de datos y agregarlo a un contenedor de servicios que puede acceder a través de la inserción de dependencias.</span><span class="sxs-lookup"><span data-stu-id="0b68b-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="0b68b-109">El ejemplo siguiente muestra cómo configurar un contenedor de servicios para la inserción de dependencias y registrar la pila de protección de datos, recibe el proveedor de protección de datos a través de DI, crear un protector y datos de protección y desprotección.</span><span class="sxs-lookup"><span data-stu-id="0b68b-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="0b68b-110">Al crear un protector debe proporcionar uno o varios [cadenas de propósito](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="0b68b-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="0b68b-111">Una cadena de propósito proporciona aislamiento entre los consumidores.</span><span class="sxs-lookup"><span data-stu-id="0b68b-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="0b68b-112">Por ejemplo, un protector creado con una cadena de propósito de "green" no poder desproteger los datos proporcionados por un protector de con un propósito de "púrpura".</span><span class="sxs-lookup"><span data-stu-id="0b68b-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="0b68b-113">Las instancias de `IDataProtectionProvider` y `IDataProtector` son seguras para subprocesos para varios de los llamadores.</span><span class="sxs-lookup"><span data-stu-id="0b68b-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="0b68b-114">Se ha diseñado que una vez que un componente obtiene una referencia a un `IDataProtector` mediante una llamada a `CreateProtector`, usará esa referencia para varias llamadas a `Protect` y `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="0b68b-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="0b68b-115">Una llamada a `Unprotect` generará CryptographicException si la carga protegida no se puede comprobar o descifrar.</span><span class="sxs-lookup"><span data-stu-id="0b68b-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="0b68b-116">Algunos componentes que desee omitir los errores durante desproteger operaciones; un componente que lee las cookies de autenticación podría controlar este error y tratar la solicitud como si no tuviera ninguna cookie en todo lugar producirá un error en la solicitud completa.</span><span class="sxs-lookup"><span data-stu-id="0b68b-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="0b68b-117">Los componentes que desean este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.</span><span class="sxs-lookup"><span data-stu-id="0b68b-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
