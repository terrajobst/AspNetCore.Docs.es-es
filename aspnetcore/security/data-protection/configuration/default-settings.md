---
title: "Duración y administración de claves"
author: rick-anderson
description: "Describe la duración y la administración de claves."
keywords: "Clave de ASP.NET Core, administración, DPAPI, DataProtection"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: ef7dad2a-7029-4ae5-8f06-1fbebedccaa4
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/default-settings
ms.openlocfilehash: 5ac2d80e7d1cebcbc792e1247608e67991ec36f4
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2017
---
# <a name="key-management-and-lifetime"></a>Duración y administración de claves

<a name="data-protection-default-settings"></a>

## <a name="key-management"></a>Administración de claves

El sistema intenta detectar su entorno operativo y proporcionar valores predeterminados de comportamientos de sin configuración buena. La heurística utilizada es la siguiente.

1. Si el sistema se hospedan en sitios Web Azure, las claves se guardan en la carpeta "% HOME%\ASP.NET\DataProtection-Keys". Esta carpeta está respaldada por el almacenamiento de red y se sincroniza en todos los equipos que hospeda la aplicación. Las claves no están protegidas en reposo. Esta carpeta proporciona el anillo de clave a todas las instancias de una aplicación en una única ranura de implementación. Las ranuras de implementación independiente, por ejemplo, ensayo y producción, no compartirán un anillo de clave. Al intercambiar las ranuras de implementación, por ejemplo, el intercambio de ensayo a producción o usando A / B pruebas, cualquier sistema mediante la protección de datos no podrá descifrar los datos almacenados mediante el anillo de clave dentro de la ranura anterior. Esto dará lugar a los usuarios que se están registrados fuera de una aplicación de ASP.NET que usa el middleware de cookies ASP.NET estándar, ya que usa protección de datos para proteger sus cookies. Si desea llaveros independiente de la ranura, utiliza un proveedor de anillo de clave externa, como almacenamiento de blobs de Azure, el almacén de claves de Azure, un almacén de SQL, o la caché en Redis.

2. Si el perfil de usuario está disponible, las claves se guardan en la carpeta "% LOCALAPPDATA%\ASP.NET\DataProtection-Keys". Además, si el sistema operativo es Windows, podrá se cifran en reposo con DPAPI.

3. Si la aplicación se hospeda en IIS, las claves se conservan en el registro HKLM en una clave del registro especial que se disponen de las ACL solo a la cuenta de proceso de trabajo. Las claves se cifran en reposo con DPAPI.

4. Si coincide con ninguna de estas condiciones, no se conservan las claves fuera del proceso actual. Cuando el proceso se cierra, todos los genera claves se perderán.

El programador está siempre en control total y puede invalidar cómo y dónde se almacenan las claves. Las tres primeras opciones anteriores deben buena valores predeterminados para la mayoría de las aplicaciones similar a cómo ASP.NET <machineKey> rutinas de la generación automática funcionaban en el pasado. La última opción de recuperación de otoño es el único escenario que requiere realmente al programador especificar [configuración](overview.md) por adelantado si quieren persistencia de clave, pero este retroceso solo ocurre en raras ocasiones.

>[!WARNING]
> Si el desarrollador invalida esta heurística y señala el sistema de protección de datos en un repositorio de clave específico, se deshabilitará el cifrado automático de claves en reposo. En reposo protección puede habilitarse de nuevo a través de [configuración](overview.md).

## <a name="key-lifetime"></a>Vigencia de clave

Las claves de forma predeterminada tienen una duración de 90 días. Cuando expira una clave, el sistema automáticamente generará una nueva clave y establecer la nueva clave como la clave activa. Como claves retiradas permanecen en el sistema podrá descifrar los datos protegidos con ellos. Vea [administración de claves](../implementation/key-management.md#data-protection-implementation-key-management-expiration) para obtener más información.

## <a name="default-algorithms"></a>Algoritmos predeterminados

El algoritmo de protección de carga predeterminado usado es AES-256-CBC para HMACSHA256 la confidencialidad y la autenticidad. Una clave maestra de 512 bits, revierte cada 90 días, se utiliza para derivar las claves secundarias dos utilizadas para estos algoritmos según una por cada carga. Vea [subclave derivación](../implementation/subkeyderivation.md#data-protection-implementation-subkey-derivation-aad) para obtener más información.
