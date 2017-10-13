---
title: "Cadenas de propósito en ASP.NET Core"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 9d18c287-e0e6-4541-b09c-7fed45c902d9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: b25af7c1f4dd3c63734290e6ac82e2e30a030c61
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2017
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Jerarquía de propósito y la arquitectura multiempresa en ASP.NET Core

Puesto que un IDataProtector también es implícitamente una IDataProtectionProvider, con fines se pueden encadenar juntas. En este proveedor de sentido. CreateProtector (["purpose1", "purpose2"]) es equivalente al proveedor. CreateProtector("purpose1"). CreateProtector("purpose2").

Esto permite algunas relaciones jerárquicas interesantes a través del sistema de protección de datos. En el ejemplo anterior de [Contoso.Messaging.SecureMessage](purpose-strings.md#data-protection-contoso-purpose), el componente SecureMessage puede llamar al proveedor. CreateProtector("Contoso.Messaging.SecureMessage") una vez por adelantado y la memoria caché el resultado en privado `_myProvide` campo. Protectores futuras, a continuación, pueden crearse a través de llamadas a `_myProvider.CreateProtector("User: username")`, y se utilizan estos protectores para proteger los mensajes individuales.

Esto también se voltea. Considere la posibilidad de una sola aplicación lógica qué hosts de varios inquilinos (un CMS parece razonable) y cada inquilino pueden configurarse con su propio sistema de administración de autenticación y el estado. La aplicación aglutina tiene un proveedor de maestro único y llama a proveedor. CreateProtector ("inquilino 1") y el proveedor. CreateProtector ("inquilino 2") para proporcionar a cada inquilino de su propio segmento aislado del sistema de protección de datos. Los inquilinos, a continuación, pudieron derivar sus propios protectores individuales según sus propias necesidades, pero con independencia de forma rígida intentan no pueden crear protectores que estén en conflicto con otro inquilino en el sistema. Esto se representa gráficamente como sigue.

![Con fines de varios inquilinos](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Se supone el paraguas de controles de la aplicación qué API están disponibles a los inquilinos individuales y que los inquilinos no pueden ejecutar código arbitrario en el servidor. Si un inquilino puede ejecutar código arbitrario, podrían realizar la reflexión privada para interrumpir las garantías de aislamiento, o que simplemente podrían leer directamente el material de claves principal y derivan de las subclaves lo desean.

El sistema de protección de datos utiliza realmente una ordenación de la arquitectura multiempresa en su configuración de predeterminada de fábrica. De forma predeterminada el material de claves maestro se almacena en la carpeta de perfil de usuario de la cuenta de proceso de trabajo (o el registro, para identidades de grupo de aplicaciones de IIS). Pero es realmente bastante habitual usar una única cuenta para ejecutar varias aplicaciones y, por tanto, todas estas aplicaciones se terminen compartiendo al maestro de material de claves. Para resolver este problema, el sistema de protección de datos inserta automáticamente un identificador único por la aplicación como el primer elemento de la cadena de propósito general. Ello implícita sirve para [aislar las aplicaciones individuales](../configuration/overview.md#data-protection-configuration-per-app-isolation) entre sí al tratar de forma eficaz cada aplicación como un único inquilino dentro del sistema y el proceso de creación de protector parece idéntico a la imagen anterior.
