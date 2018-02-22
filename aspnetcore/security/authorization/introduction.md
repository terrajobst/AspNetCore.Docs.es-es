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
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>Introducción

<a name="security-authorization-introduction"></a>

La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer. Por ejemplo, un usuario administrativo puede crear una biblioteca de documentos, agregar documentos, editar documentos y eliminarlos. Un usuario sin derechos administrativos que trabaje con la biblioteca solo está autorizado a leer los documentos.

La autorización es ortogonal y es independiente de la autenticación, que es el proceso de determinar quién es un usuario. La autenticación puede crear una o varias identidades para el usuario actual.

## <a name="authorization-types"></a>Tipos de autorización

Autorización de ASP.NET Core proporcionan una forma simple, declarativa [rol](roles.md) y un variado [basada en directivas](policies.md) modelo. Autorización se expresa en los requisitos y controladores evaluación notificaciones de usuario con los requisitos. Las comprobaciones imperativas pueden basarse en directivas simples ni que evaluar la identidad del usuario y propiedades del recurso al que el usuario está intentando tener acceso.

## <a name="namespaces"></a>Espacios de nombres

Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos, se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.

Consulte la documentación en [autorización sencilla](xref:security/authorization/simple).
