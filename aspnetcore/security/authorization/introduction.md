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
# <a name="introduction"></a>Introducción

<a name=security-authorization-introduction></a>

La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer. Por ejemplo, un usuario administrativo puede crear una biblioteca de documentos, agregar documentos, editar documentos y eliminarlos. Un usuario sin derechos administrativos que trabaje con la biblioteca solo está autorizado a leer los documentos.

La autorización es ortogonal y es independiente de la autenticación, que es el proceso de determinar quién es un usuario. La autenticación puede crear una o varias identidades para el usuario actual.

## <a name="authorization-types"></a>Tipos de autorización

Autorización de ASP.NET Core proporciona un sencillo declarativo [rol](roles.md#security-authorization-role-based) y un [basado en directiva de enriquecido](policies.md#security-authorization-policies-based) modelo. Autorización se expresa en los requisitos y controladores evaluación notificaciones de usuario con los requisitos. Las comprobaciones imperativas pueden basarse en directivas simples ni que evaluar la identidad del usuario y propiedades del recurso al que el usuario está intentando tener acceso.

## <a name="namespaces"></a>Espacios de nombres

Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.
