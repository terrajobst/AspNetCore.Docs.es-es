---
title: Administración de claves de protección de datos y la duración en ASP.NET Core
author: rick-anderson
description: Obtenga información acerca de la administración de claves de protección de datos y la duración en ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: b43c14af015d5e03f46200c51a1218a581b1de0c
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
ms.locfileid: "28887295"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Administración de claves de protección de datos y la duración en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Administración de claves

La aplicación intenta detectar su entorno operativo y controlar la configuración de la clave por sí mismo.

1. Si la aplicación se hospeda en [aplicaciones de Azure](https://azure.microsoft.com/services/app-service/), las claves se conservan en el *%HOME%\ASP.NET\DataProtection-Keys* carpeta. Esta carpeta está respaldada por el almacenamiento de red y se sincroniza en todos los equipos que hospedan la aplicación.
   * Las claves no están protegidas en reposo.
   * El *DataProtection claves* carpeta proporciona el anillo de clave a todas las instancias de una aplicación en una única ranura de implementación.
   * Las ranuras de implementación independiente, por ejemplo, ensayo y producción, no comparten un anillo de clave. Al intercambiar las ranuras de implementación, por ejemplo, el intercambio de ensayo a producción o usando A / B pruebas, cualquier aplicación con la protección de datos no podrá descifrar los datos almacenados mediante el anillo de clave dentro de la ranura anterior. Esto conduce al usuario que se haya iniciado sesión en una aplicación que usa la autenticación con cookies ASP.NET Core estándar, porque usa protección de datos para proteger sus cookies. Si desea llaveros independiente de la ranura, utiliza un proveedor de anillo de clave externa, como almacenamiento de blobs de Azure, el almacén de claves de Azure, un almacén de SQL, o la caché en Redis.

1. Si el perfil de usuario está disponible, las claves se conservan en el *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* carpeta. Si el sistema operativo es Windows, las claves se cifran en reposo con DPAPI.

1. Si la aplicación se hospeda en IIS, las claves se conservan en el registro HKLM en una clave del registro especial que se disponen de las ACL solo a la cuenta de proceso de trabajo. Las claves se cifran en reposo con DPAPI.

1. Si coincide con ninguna de estas condiciones, no se conservan las claves fuera del proceso actual. Cuando el proceso se cierra, todos los genera claves se pierden.

El programador está siempre en control total y puede invalidar cómo y dónde se almacenan las claves. Las tres primeras opciones anteriores deben proporcionar buenos valores predeterminados para la mayoría de las aplicaciones similar a cómo ASP.NET  **\<machineKey >** rutinas de la generación automática funcionaban en el pasado. La opción de reserva final es el único escenario que requiere que el desarrollador especificar [configuración](xref:security/data-protection/configuration/overview) por adelantado si quieren persistencia de clave, pero esta reserva sólo se produce en situaciones excepcionales.

Cuando se hospedan en un contenedor de Docker, las claves deben conservarse en una carpeta que es un volumen de Docker (un volumen compartido o un volumen montado de host que se conserva más allá de la duración del contenedor) o en un proveedor externo, como [el almacén de claves de Azure](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/). Un proveedor externo también es útil en escenarios de granja de servidores web si las aplicaciones no pueden obtener acceso a un volumen compartido de red (consulte [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) para obtener más información).

> [!WARNING]
> Si el desarrollador invalida las reglas descritas anteriormente y señala el sistema de protección de datos en un repositorio de clave específico, se deshabilita el cifrado automático de claves en reposo. Protección en rest puede habilitarse de nuevo a través de [configuración](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Vigencia de clave

Las claves tienen una duración de 90 días de forma predeterminada. Cuando expira una clave, la aplicación genera una nueva clave automáticamente y establece la nueva clave como la clave activa. Claves retiradas quedan en el sistema, siempre y cuando la aplicación puede descifrar los datos protegidos con ellos. Vea [administración de claves](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) para obtener más información.

## <a name="default-algorithms"></a>Algoritmos predeterminados

El algoritmo de protección de carga predeterminado usado es AES-256-CBC para HMACSHA256 la confidencialidad y la autenticidad. Una clave maestra de 512 bits, cambiada cada 90 días, se utiliza para derivar las claves secundarias dos utilizadas para estos algoritmos según una por cada carga. Vea [subclave derivación](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) para obtener más información.

## <a name="see-also"></a>Vea también

* [Extensibilidad de administración de claves](xref:security/data-protection/extensibility/key-management)
