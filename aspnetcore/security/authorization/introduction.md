---
title: Introducción a la autorización en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre los conceptos básicos de autorización y cómo funciona la autorización en aplicaciones ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896982"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="0c731-103">Introducción a la autorización en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c731-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="0c731-104">La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer.</span><span class="sxs-lookup"><span data-stu-id="0c731-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="0c731-105">Por ejemplo, un usuario administrativo se permite para crear una biblioteca de documentos, documentos de agregar, editar documentos y eliminarlos.</span><span class="sxs-lookup"><span data-stu-id="0c731-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="0c731-106">Un usuario sin derechos administrativos, trabajar con la biblioteca solo está autorizado para leer los documentos.</span><span class="sxs-lookup"><span data-stu-id="0c731-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="0c731-107">La autorización es ortogonal e independiente de la autenticación.</span><span class="sxs-lookup"><span data-stu-id="0c731-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="0c731-108">Sin embargo, la autorización requiere un mecanismo de autenticación.</span><span class="sxs-lookup"><span data-stu-id="0c731-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="0c731-109">La autenticación es el proceso de determinar quién es un usuario.</span><span class="sxs-lookup"><span data-stu-id="0c731-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="0c731-110">La autenticación puede crear una o más identidades para el usuario actual.</span><span class="sxs-lookup"><span data-stu-id="0c731-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="0c731-111">Tipos de autorización</span><span class="sxs-lookup"><span data-stu-id="0c731-111">Authorization types</span></span>

<span data-ttu-id="0c731-112">Autorización de ASP.NET Core proporciona un sencillo, de manera declarativo [rol](xref:security/authorization/roles) y un variado [basada en directivas](xref:security/authorization/policies) modelo.</span><span class="sxs-lookup"><span data-stu-id="0c731-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="0c731-113">Autorización se expresa en los requisitos y controladores de evaluación las notificaciones de un usuario con los requisitos.</span><span class="sxs-lookup"><span data-stu-id="0c731-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="0c731-114">Comprobaciones imperativas pueden basarse en las directivas de simple o que evaluar la identidad del usuario y las propiedades del recurso al que el usuario está intentando obtener acceso.</span><span class="sxs-lookup"><span data-stu-id="0c731-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="0c731-115">Espacios de nombres</span><span class="sxs-lookup"><span data-stu-id="0c731-115">Namespaces</span></span>

<span data-ttu-id="0c731-116">Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos, se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="0c731-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="0c731-117">Consulte la documentación en [autorización sencilla](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="0c731-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
