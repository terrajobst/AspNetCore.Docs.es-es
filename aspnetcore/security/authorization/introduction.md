---
title: Introducción a la autorización en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de los conceptos básicos de autorización y cómo funciona la autorización en aplicaciones de ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483087"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Introducción a la autorización en ASP.NET Core

<a name="security-authorization-introduction"></a>

La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer. Por ejemplo, un usuario administrativo puede crear una biblioteca de documentos, agregar documentos, editar documentos y eliminarlos. Un usuario sin derechos administrativos que trabaje con la biblioteca solo está autorizado a leer los documentos.

La autorización es ortogonal y es independiente de la autenticación. Sin embargo, la autorización requiere un mecanismo de autenticación. La autenticación es el proceso de determinar quién es un usuario. La autenticación puede crear una o varias identidades para el usuario actual.

## <a name="authorization-types"></a>Tipos de autorización

Autorización de ASP.NET Core proporcionan una forma simple, declarativa [rol](xref:security/authorization/roles) y un variado [basada en directivas](xref:security/authorization/policies) modelo. Autorización se expresa en los requisitos y controladores evaluación notificaciones de usuario con los requisitos. Las comprobaciones imperativas pueden basarse en directivas simples ni que evaluar la identidad del usuario y propiedades del recurso al que el usuario está intentando tener acceso.

## <a name="namespaces"></a>Espacios de nombres

Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos, se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.

Consulte la documentación en [autorización sencilla](xref:security/authorization/simple).
