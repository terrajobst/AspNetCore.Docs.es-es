---
title: Introducción a la autorización en ASP.NET Core
author: rick-anderson
description: Conozca los aspectos básicos de la autorización y cómo funciona la autorización en ASP.NET Core aplicaciones.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: b5e60b3c256941fff5e54e1a02e077c34c535902
ms.sourcegitcommit: 79850db9e79b1705b89f466c6f2c961ff15485de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2020
ms.locfileid: "75693874"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Introducción a la autorización en ASP.NET Core

<a name="security-authorization-introduction"></a>

La autorización se refiere al proceso que determina lo que un usuario puede hacer. Por ejemplo, un usuario administrativo puede crear una biblioteca de documentos, agregar documentos, editar documentos y eliminarlos. Un usuario no administrativo que trabaja con la biblioteca solo está autorizado para leer los documentos.

La autorización es ortogonal e independiente de la autenticación. Sin embargo, la autorización requiere un mecanismo de autenticación. La autenticación es el proceso de determinar quién es un usuario. La autenticación puede crear una o varias identidades para el usuario actual.

Para obtener más información sobre la autenticación en ASP.NET Core, vea <xref:security/authentication/index>.

## <a name="authorization-types"></a>Tipos de autorización

La autorización de ASP.NET Core proporciona un [rol](xref:security/authorization/roles) sencillo y declarativo y un modelo avanzado [basado en directivas](xref:security/authorization/policies) . La autorización se expresa en requisitos y los controladores evalúan las notificaciones de un usuario en relación con los requisitos. Las comprobaciones imperativas pueden basarse en directivas o directivas simples que evalúan la identidad del usuario y las propiedades del recurso al que el usuario está intentando obtener acceso.

## <a name="namespaces"></a>Espacios de nombres de

Los componentes de autorización, incluidos los atributos `AuthorizeAttribute` y `AllowAnonymousAttribute`, se encuentran en el espacio de nombres `Microsoft.AspNetCore.Authorization`.

Consulte la documentación sobre la [autorización simple](xref:security/authorization/simple).
