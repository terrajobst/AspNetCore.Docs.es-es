---
title: Jerarquía de propósito y la arquitectura multiempresa en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de la jerarquía de la cadena de propósito y la arquitectura multiempresa en relación con las API de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/purpose-strings-multitenancy
ms.openlocfilehash: 1133d40e7b325d58b3f70e7387494dae36ff8ac9
ms.sourcegitcommit: fd461c60b5e36c7019f81da0138cc859d0fddaa2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/10/2018
ms.locfileid: "41827721"
---
# <a name="purpose-hierarchy-and-multi-tenancy-in-aspnet-core"></a>Jerarquía de propósito y la arquitectura multiempresa en ASP.NET Core

Puesto que un `IDataProtector` también es implícitamente un `IDataProtectionProvider`, con fines se pueden encadenar juntas. En este sentido, `provider.CreateProtector([ "purpose1", "purpose2" ])` es equivalente a `provider.CreateProtector("purpose1").CreateProtector("purpose2")`.

Esto permite que algunas relaciones jerárquicas interesantes a través del sistema de protección de datos. En el ejemplo anterior de [Contoso.Messaging.SecureMessage](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-contoso-purpose), puede llamar al componente SecureMessage `provider.CreateProtector("Contoso.Messaging.SecureMessage")` una vez por adelantado y almacenar en caché el resultado en una privada `_myProvider` campo. Protectores futuras, a continuación, se pueden crear mediante llamadas a `_myProvider.CreateProtector("User: username")`, y estos protectores se utilizarían para proteger los mensajes individuales.

Esto también puede voltear. Considere la posibilidad de una sola aplicación lógica que hospeda varios inquilinos (un CMS parece razonable) y cada inquilino se pueden configurar con su propio sistema de administración de autenticación y el estado. La aplicación paraguas tiene un proveedor de maestro único y llama a `provider.CreateProtector("Tenant 1")` y `provider.CreateProtector("Tenant 2")` para proporcionar su propio segmento aislado del sistema de protección de datos a cada inquilino. Los inquilinos, a continuación, podrían derivar sus propios protectores individuales en función de sus propias necesidades, pero independientemente de cuán duro intentan no pueden crear protectores que entren en conflicto con otro inquilino en el sistema. Gráficamente, esto se representa como sigue.

![Con fines de varios inquilinos](purpose-strings-multitenancy/_static/purposes-multi-tenancy.png)

>[!WARNING]
> Esto supone el paraguas de controles de aplicación, las API que están disponibles para los inquilinos individuales y que los inquilinos no pueden ejecutar código arbitrario en el servidor. Si un inquilino puede ejecutar código arbitrario, podrían realizar la reflexión privada para interrumpir las garantías de aislamiento, o podría simplemente leer el material de clave maestra directamente y derivar cualquier subclaves que deseen.

El sistema de protección de datos usa en realidad una especie de varios inquilinos en su propia configuración predeterminada de fábrica. De forma predeterminada material de claves maestra se almacena en la carpeta de perfil de usuario de la cuenta de proceso de trabajo (o el registro de identidades del grupo de aplicaciones de IIS). Pero es realmente bastante habitual usar una sola cuenta para ejecutar varias aplicaciones y, por tanto, todas estas aplicaciones terminaría compartir al maestro de material de claves. Para resolver este problema, el sistema de protección de datos inserta automáticamente un identificador único por la aplicación como el primer elemento de la cadena de propósito general. Con este fin implícita sirve para [aislar las aplicaciones individuales](xref:security/data-protection/configuration/overview#per-application-isolation) entre sí al tratar de forma eficaz cada aplicación como un único inquilino en el sistema y el proceso de creación de protector es idéntico a la imagen anterior.
