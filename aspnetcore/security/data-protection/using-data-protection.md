---
title: "Introducción a las API de protección de datos"
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 2a381acf5faa7071fe4a22641037fcee11f6d362
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a>Introducción a las API de protección de datos

<a name=security-data-protection-getting-started></a>

En su protección de datos más simple consta de los siguientes pasos:

1. Crear un datos protector desde un proveedor de protección de datos.

2. Llame al método Protect con los datos que desea proteger.

3. Llame al método Unprotect con los datos que desea convertir en texto sin formato.

Mayoría de los marcos, como ASP.NET o SignalR ya configura el sistema de protección de datos y agregarlo a un contenedor de servicios que se puede acceder a través de la inserción de dependencias. El siguiente ejemplo muestra cómo configurar un contenedor de servicios para la inyección de dependencia y registrar la pila de protección de datos, recibe el proveedor de protección de datos a través de DI, crear un protector y datos de protección y desprotección

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

Cuando se crea un protector debe proporcionar uno o varios [propósito cadenas](consumer-apis/purpose-strings.md). Una cadena de propósito proporciona aislamiento entre los consumidores, por ejemplo un protector creado con una cadena de fin de "verde" no sería capaz de desproteger los datos proporcionados por un protector con un propósito de "púrpura".

>[!TIP]
> Las instancias de IDataProtectionProvider y IDataProtector son seguras para subprocesos para distintos llamadores. Su objetivo es que una vez que un componente obtiene una referencia a un IDataProtector mediante una llamada a CreateProtector, utilizará esa referencia para varias llamadas a proteger y desproteger.
>
>Una llamada a Unprotect iniciará CryptographicException si no se puede comprobar o descifrar la carga protegida. Algunos componentes que desee omitir los errores durante la desproteger operaciones; un componente que lee las cookies de autenticación puede controlar este error y tratar la solicitud como si no tuviera en absoluto ninguna cookie en lugar de producirá un error en la solicitud directamente. Los componentes que desea este comportamiento deben detectar específicamente CryptographicException en lugar de admitir todas las excepciones.
