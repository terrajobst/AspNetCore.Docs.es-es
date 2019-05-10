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
# <a name="introduction-to-authorization-in-aspnet-core"></a>Introducción a la autorización en ASP.NET Core

<a name="security-authorization-introduction"></a>

La autorización se refiere al proceso que determina lo que un usuario es capaz de hacer. Por ejemplo, un usuario administrativo se permite para crear una biblioteca de documentos, documentos de agregar, editar documentos y eliminarlos. Un usuario sin derechos administrativos, trabajar con la biblioteca solo está autorizado para leer los documentos.

La autorización es ortogonal e independiente de la autenticación. Sin embargo, la autorización requiere un mecanismo de autenticación. La autenticación es el proceso de determinar quién es un usuario. La autenticación puede crear una o más identidades para el usuario actual.

## <a name="authorization-types"></a>Tipos de autorización

Autorización de ASP.NET Core proporciona un sencillo, de manera declarativo [rol](xref:security/authorization/roles) y un variado [basada en directivas](xref:security/authorization/policies) modelo. Autorización se expresa en los requisitos y controladores de evaluación las notificaciones de un usuario con los requisitos. Comprobaciones imperativas pueden basarse en las directivas de simple o que evaluar la identidad del usuario y las propiedades del recurso al que el usuario está intentando obtener acceso.

## <a name="namespaces"></a>Espacios de nombres

Componentes de autorización, incluido el `AuthorizeAttribute` y `AllowAnonymousAttribute` atributos, se encuentran en el `Microsoft.AspNetCore.Authorization` espacio de nombres.

Consulte la documentación en [autorización sencilla](xref:security/authorization/simple).
