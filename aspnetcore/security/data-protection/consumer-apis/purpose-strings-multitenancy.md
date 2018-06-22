---
title: Jerarquía de propósito y la arquitectura multiempresa en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de la jerarquía de la cadena de fin y la arquitectura multiempresa en lo referente a las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: f0c39d54c164595c2135e0eb0d911796e215dd66
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273616"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Jerarquía de propósito y la arquitectura multiempresa en ASP.NET Core

Puesto que un `IDataProtector` también es implícitamente una `IDataProtectionProvider`, con fines se pueden encadenar juntas. En este sentido, `provider.CreateProtector([ "purpose1", "purpose2" ])` es equivalente a `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Esto permite algunas relaciones jerárquicas interesantes a través del sistema de protección de datos. En el ejemplo anterior de [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), puede llamar el componente SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` inicial una vez y almacenar en caché el resultado en privado `_myProvide` campo. Protectores futuras, a continuación, pueden crearse a través de llamadas a `_myProvider.CreateProtector("User: username")`, y se utilizan estos protectores para proteger los mensajes individuales.

Esto también se voltea. Considere la posibilidad de una sola aplicación lógica qué hosts de varios inquilinos (un CMS parece razonable) y cada inquilino pueden configurarse con su propio sistema de administración de autenticación y el estado. La aplicación aglutina tiene un proveedor de maestro único y llama `provider.CreateProtector("Tenant 1")` y `provider.CreateProtector("Tenant 2")` para proporcionar a cada inquilino de su propio segmento aislado del sistema de protección de datos. Los inquilinos, a continuación, pudieron derivar sus propios protectores individuales según sus propias necesidades, pero con independencia de forma rígida intentan no pueden crear protectores que estén en conflicto con otro inquilino en el sistema. Esto se representa gráficamente las indicadas a continuación.

![Con fines de varios inquilinos](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Se supone el paraguas de controles de la aplicación qué API están disponibles a los inquilinos individuales y que los inquilinos no pueden ejecutar código arbitrario en el servidor. Si un inquilino puede ejecutar código arbitrario, podrían realizar la reflexión privada para interrumpir las garantías de aislamiento, o que simplemente podrían leer directamente el material de claves principal y derivan de las subclaves lo desean.

El sistema de protección de datos utiliza realmente una ordenación de la arquitectura multiempresa en su configuración de predeterminada de fábrica. De forma predeterminada el material de claves maestro se almacena en la carpeta de perfil de usuario de la cuenta de proceso de trabajo (o el registro, para identidades de grupo de aplicaciones de IIS). Pero es realmente bastante habitual usar una única cuenta para ejecutar varias aplicaciones y, por tanto, todas estas aplicaciones se terminen compartiendo al maestro de material de claves. Para resolver este problema, el sistema de protección de datos inserta automáticamente un identificador único por la aplicación como el primer elemento de la cadena de propósito general. Ello implícita sirve para [aislar las aplicaciones individuales](xref:security/data-protection/configuration/overview#per-application-isolation) entre sí al tratar de forma eficaz cada aplicación como un único inquilino dentro del sistema y el proceso de creación de protector parece idéntico a la imagen anterior.
