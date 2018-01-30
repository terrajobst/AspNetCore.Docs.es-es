---
title: "Introducción a la autorización"
author: rick-anderson
description: "Este documento proporciona una explicación básica de autorización y explica cómo se relaciona la autorización para ASP.NET Core."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: a6bd81a4e5796c1d0a0033c2b8d5a6ba9282564c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="introduction"></a><span data-ttu-id="ca686-103">Introducción</span><span class="sxs-lookup"><span data-stu-id="ca686-103">Introduction</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="ca686-104">La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer.</span><span class="sxs-lookup"><span data-stu-id="ca686-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="ca686-105">Por ejemplo, un usuario administrativo puede crear una biblioteca de documentos, agregar documentos, editar documentos y eliminarlos.</span><span class="sxs-lookup"><span data-stu-id="ca686-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="ca686-106">Un usuario sin derechos administrativos que trabaje con la biblioteca solo está autorizado a leer los documentos.</span><span class="sxs-lookup"><span data-stu-id="ca686-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="ca686-107">La autorización es ortogonal y es independiente de la autenticación, que es el proceso de determinar quién es un usuario.</span><span class="sxs-lookup"><span data-stu-id="ca686-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="ca686-108">La autenticación puede crear una o varias identidades para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="ca686-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="ca686-109">Tipos de autorización</span><span class="sxs-lookup"><span data-stu-id="ca686-109">Authorization Types</span></span>

<span data-ttu-id="ca686-110">Autorización de ASP.NET Core proporciona un sencillo declarativo [rol](roles.md) y un [basado en directiva de enriquecido](policies.md) modelo.</span><span class="sxs-lookup"><span data-stu-id="ca686-110">ASP.NET Core authorization provides a simple declarative [role](roles.md) and a [rich policy based](policies.md) model.</span></span> <span data-ttu-id="ca686-111">Autorización se expresa en los requisitos y controladores evaluación notificaciones de usuario con los requisitos.</span><span class="sxs-lookup"><span data-stu-id="ca686-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="ca686-112">Las comprobaciones imperativas pueden basarse en directivas simples ni que evaluar la identidad del usuario y propiedades del recurso al que el usuario está intentando tener acceso.</span><span class="sxs-lookup"><span data-stu-id="ca686-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="ca686-113">Espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="ca686-113">Namespaces</span></span>

<span data-ttu-id="ca686-114">Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="ca686-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
