---
title: "Introducción a la autorización"
author: rick-anderson
description: "Este documento proporciona una explicación básica de autorización y explica cómo se relaciona la autorización para ASP.NET Core."
keywords: "Núcleo de ASP.NET, autorización"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 192cc494c2378f77207a4b1c17b0b0a73ca642ed
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="introduction"></a>Introducción

<a name="security-authorization-introduction"></a>

La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer. Por ejemplo, un usuario administrativo puede crear una biblioteca de documentos, agregar documentos, editar documentos y eliminarlos. Un usuario sin derechos administrativos que trabaje con la biblioteca solo está autorizado a leer los documentos.

La autorización es ortogonal y es independiente de la autenticación, que es el proceso de determinar quién es un usuario. La autenticación puede crear una o varias identidades para el usuario actual.

## <a name="authorization-types"></a>Tipos de autorización

Autorización de ASP.NET Core proporciona un sencillo declarativo [rol](roles.md) y un [basado en directiva de enriquecido](policies.md) modelo. Autorización se expresa en los requisitos y controladores evaluación notificaciones de usuario con los requisitos. Las comprobaciones imperativas pueden basarse en directivas simples ni que evaluar la identidad del usuario y propiedades del recurso al que el usuario está intentando tener acceso.

## <a name="namespaces"></a>Espacios de nombres

Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.
