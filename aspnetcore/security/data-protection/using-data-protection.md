---
title: Introducción a las API de protección de datos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo usar las API de protección de datos de ASP.NET Core para proteger y desproteger los datos en una aplicación.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838935"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a>Introducción a las API de protección de datos en ASP.NET Core

<a name="security-data-protection-getting-started"></a>

Su sencilla y protección de datos consta de los pasos siguientes:

1. Crear datos de un protector de un proveedor de protección de datos.

2. Llame a la `Protect` método con los datos que desea proteger.

3. Llame a la `Unprotect` método con los datos que desea convertir en texto sin formato.

La mayoría de los marcos y modelos de aplicación, como ASP.NET Core SignalR, ya que configuración el sistema de protección de datos y agregarlo a un contenedor de servicios que puede acceder a través de la inserción de dependencias. El ejemplo siguiente muestra cómo configurar un contenedor de servicios para la inserción de dependencias y registrar la pila de protección de datos, recibe el proveedor de protección de datos a través de DI, crear un protector y datos de protección y desprotección.

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Al crear un protector debe proporcionar uno o varios [cadenas de propósito](xref:security/data-protection/consumer-apis/purpose-strings). Una cadena de propósito proporciona aislamiento entre los consumidores. Por ejemplo, un protector creado con una cadena de propósito de "green" no poder desproteger los datos proporcionados por un protector de con un propósito de "púrpura".

>[!TIP]
> Las instancias de `IDataProtectionProvider` y `IDataProtector` son seguras para subprocesos para varios de los llamadores. Se ha diseñado que una vez que un componente obtiene una referencia a un `IDataProtector` mediante una llamada a `CreateProtector`, usará esa referencia para varias llamadas a `Protect` y `Unprotect`.
>
>Una llamada a `Unprotect` generará CryptographicException si la carga protegida no se puede comprobar o descifrar. Algunos componentes que desee omitir los errores durante desproteger operaciones; un componente que lee las cookies de autenticación podría controlar este error y tratar la solicitud como si no tuviera ninguna cookie en todo lugar producirá un error en la solicitud completa. Los componentes que desean este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.
