---
title: Empezar a trabajar con las API de protección de datos en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de cómo usar las API de protección de datos de ASP.NET Core para proteger y desproteger los datos en una aplicación.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: ab2551d87d1a2cd22e9f421cabe0288311cb2ec3
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275754"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Empezar a trabajar con las API de protección de datos en ASP.NET Core

<a name="security-data-protection-getting-started"></a>

Como sus datos más sencillas, protección consta de los siguientes pasos:

1. Crear un datos protector desde un proveedor de protección de datos.

2. Llame a la `Protect` método con los datos que desea proteger.

3. Llame a la `Unprotect` método con los datos que desea convertir en texto sin formato.

La mayoría de los marcos y modelos de aplicación, por ejemplo, ASP.NET o SignalR, ya que configurar el sistema de protección de datos y agregarlo a un contenedor de servicios que se puede acceder a través de la inserción de dependencias. El siguiente ejemplo muestra cómo configurar un contenedor de servicios para la inyección de dependencia y registrar la pila de protección de datos, recibe el proveedor de protección de datos a través de DI, crear un protector y datos de protección y desprotección

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Cuando se crea un protector debe proporcionar uno o varios [propósito cadenas](xref:security/data-protection/consumer-apis/purpose-strings). Una cadena de propósito proporciona aislamiento entre los consumidores. Por ejemplo, un protector creado con una cadena de fin de "verde" no podrá desproteger los datos proporcionados por un protector con un propósito de "púrpura".

>[!TIP]
> Instancias de `IDataProtectionProvider` y `IDataProtector` son seguras para subprocesos para varios de los llamadores. Se ha diseñado que una vez que un componente obtiene una referencia a un `IDataProtector` mediante una llamada a `CreateProtector`, utilizará dicha referencia para varias llamadas a `Protect` y `Unprotect`.
>
>Una llamada a `Unprotect` iniciará CryptographicException si no se puede comprobar o descifrar la carga protegida. Algunos componentes que desee omitir los errores durante la desproteger operaciones; un componente que lee las cookies de autenticación puede controlar este error y tratar la solicitud como si no tuviera en absoluto ninguna cookie en lugar de producirá un error en la solicitud directamente. Los componentes que desea este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.
