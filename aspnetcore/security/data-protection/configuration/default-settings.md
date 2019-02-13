---
title: Administración de claves de protección de datos y la duración en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre la administración de claves de protección de datos y la duración en ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 2f022a4c7519485fe629ce47c27d214c8c27d5bc
ms.sourcegitcommit: af8a6eb5375ef547a52ffae22465e265837aa82b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56159216"
---
# <a name="data-protection-key-management-and-lifetime-in-aspnet-core"></a>Administración de claves de protección de datos y la duración en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="key-management"></a>Administración de claves

La aplicación intenta detectar su entorno operativo y controlar la configuración de la clave por sí mismo.

1. Si la aplicación está hospedada en [Azure Apps](https://azure.microsoft.com/services/app-service/), las claves se conservan en el *%HOME%\ASP.NET\DataProtection-Keys* carpeta. Esta carpeta está respaldada por el almacenamiento de red y se sincroniza en todas las máquinas que hospedan la aplicación.
   * Las claves no están protegidas en reposo.
   * El *DataProtection claves* carpeta proporciona el conjunto de claves para todas las instancias de una aplicación en una única ranura de implementación.
   * Las ranuras de implementación independientes, por ejemplo, almacenamiento provisional y producción, no comparten ningún anillo de clave. Al intercambiar las ranuras de implementación, por ejemplo, intercambio de ensayo y producción o usando A pruebas a/b, cualquier aplicación con la protección de datos no podrá descifrar los datos almacenados mediante el conjunto de claves dentro de la ranura anterior. Esto conduce a los usuarios que se están registrados fuera de una aplicación que utiliza la autenticación de cookies estándar de ASP.NET Core, ya que usa la protección de datos para proteger sus cookies. Si así lo desea llaveros independiente de la ranura, utilice un proveedor de anillo de clave externa, como Azure Blob Storage, Azure Key Vault, un almacén de SQL, o de Redis cache.

1. Si el perfil de usuario está disponible, las claves se conservan en el *%LOCALAPPDATA%\ASP.NET\DataProtection-Keys* carpeta. Si el sistema operativo es Windows, las claves se cifran en reposo mediante DPAPI.

   El grupo de aplicaciones [setProfileEnvironment atributo](/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration) también debe estar habilitada. El valor predeterminado de `setProfileEnvironment` es `true`. En algunos escenarios (por ejemplo, SO Windows), `setProfileEnvironment` está establecido en `false`. Si las claves no se almacenan en el directorio del perfil de usuario como se esperaba:

   1. Navegue hasta la *%windir%/system32/inetsrv/config* carpeta.
   1. Abra el *applicationHost.config* archivo.
   1. Busque el elemento `<system.applicationHost><applicationPools><applicationPoolDefaults><processModel>` .
   1. Confirme que la `setProfileEnvironment` atributo no está presente, cuyo valor predeterminado es el valor a `true`, o establecer explícitamente el valor del atributo en `true`.

1. Si la aplicación se hospeda en IIS, las claves se guardan en el registro HKLM en una clave del registro especial que se incluye sólo la cuenta de proceso de trabajo. Las claves se cifran en reposo con DPAPI.

1. Si ninguna de estas condiciones coinciden, no se conservan las claves fuera del proceso actual. Cuando el proceso se cierra, todos los genera las claves se pierden.

El desarrollador está siempre en control total y puede invalidar cómo y dónde se almacenan las claves. Las tres primeras opciones anteriores deben proporcionar valores predeterminados adecuados para la mayoría de las aplicaciones similar a cómo ASP.NET  **\<machineKey >** rutinas de la generación automática trabajado en el pasado. La opción de reserva final es el único escenario que requiere que el desarrollador especificar [configuración](xref:security/data-protection/configuration/overview) por adelantado si quieren persistencia de clave, pero esta reserva solo se produce en situaciones excepcionales.

Cuando se hospedan en un contenedor de Docker, se deberían conservar las claves en una carpeta que es un volumen de Docker (un volumen compartido o un volumen montado en el host, que se conserva más allá de la duración del contenedor) o en un proveedor externo, como [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) o [Redis](https://redis.io/). Un proveedor externo también es útil en escenarios de granja de servidores web si las aplicaciones no pueden acceder a un volumen compartido de red (consulte [PersistKeysToFileSystem](xref:security/data-protection/configuration/overview#persistkeystofilesystem) para obtener más información).

> [!WARNING]
> Si el desarrollador reemplaza las reglas descritas anteriormente y señala el sistema de protección de datos en un repositorio específico de claves, cifrado automático de las claves en reposo está deshabilitado. Protección de reposo puede habilitarse de nuevo a través de [configuración](xref:security/data-protection/configuration/overview).

## <a name="key-lifetime"></a>Vigencia de clave

Las claves tienen una duración de 90 días de forma predeterminada. Cuando expira una clave, la aplicación genera una nueva clave automáticamente y establece la nueva clave como la clave activa. Claves retiradas permanecen en el sistema, siempre y cuando la aplicación puede descifrar los datos protegidos con ellos. Consulte [administración de claves](xref:security/data-protection/implementation/key-management#key-expiration-and-rolling) para obtener más información.

## <a name="default-algorithms"></a>Algoritmos predeterminados

El algoritmo de protección de carga predeterminado usado es AES-256-CBC para confidencialidad y HMACSHA256 autenticidad. Una clave maestra de 512 bits, puede cambiada cada 90 días, se usa para derivar las claves secundarias dos utilizadas para estos algoritmos en una base por cada carga. Consulte [derivación de subclave](xref:security/data-protection/implementation/subkeyderivation#additional-authenticated-data-and-subkey-derivation) para obtener más información.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:security/data-protection/extensibility/key-management>
* <xref:host-and-deploy/web-farm>
