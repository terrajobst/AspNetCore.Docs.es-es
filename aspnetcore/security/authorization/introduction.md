---
title: "Introducción"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 040525505a982fc1be1901effb9186a8fe1cdbdf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="introduction"></a><span data-ttu-id="632fd-103">Introducción</span><span class="sxs-lookup"><span data-stu-id="632fd-103">Introduction</span></span>

<a name=security-authorization-introduction></a>

<span data-ttu-id="632fd-104">La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer.</span><span class="sxs-lookup"><span data-stu-id="632fd-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="632fd-105">Por ejemplo, un usuario administrativo puede crear una biblioteca de documentos, agregar documentos, editar documentos y eliminarlos.</span><span class="sxs-lookup"><span data-stu-id="632fd-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="632fd-106">Un usuario sin derechos administrativos que trabaje con la biblioteca solo está autorizado a leer los documentos.</span><span class="sxs-lookup"><span data-stu-id="632fd-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="632fd-107">La autorización es ortogonal y es independiente de la autenticación, que es el proceso de determinar quién es un usuario.</span><span class="sxs-lookup"><span data-stu-id="632fd-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="632fd-108">La autenticación puede crear una o varias identidades para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="632fd-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="632fd-109">Tipos de autorización</span><span class="sxs-lookup"><span data-stu-id="632fd-109">Authorization Types</span></span>

<span data-ttu-id="632fd-110">Autorización de ASP.NET Core proporciona un sencillo declarativo [rol](roles.md#security-authorization-role-based) y un [basado en directiva de enriquecido](policies.md#security-authorization-policies-based) modelo.</span><span class="sxs-lookup"><span data-stu-id="632fd-110">ASP.NET Core authorization provides a simple declarative [role](roles.md#security-authorization-role-based) and a [rich policy based](policies.md#security-authorization-policies-based) model.</span></span> <span data-ttu-id="632fd-111">Autorización se expresa en los requisitos y controladores evaluación notificaciones de usuario con los requisitos.</span><span class="sxs-lookup"><span data-stu-id="632fd-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="632fd-112">Las comprobaciones imperativas pueden basarse en directivas simples ni que evaluar la identidad del usuario y propiedades del recurso al que el usuario está intentando tener acceso.</span><span class="sxs-lookup"><span data-stu-id="632fd-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="632fd-113">Espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="632fd-113">Namespaces</span></span>

<span data-ttu-id="632fd-114">Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="632fd-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
