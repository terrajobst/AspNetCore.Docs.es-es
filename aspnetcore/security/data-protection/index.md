---
title: Protección de datos en ASP.NET Core
author: rick-anderson
description: Este documento sirve como tabla de contenido para los distintos temas de protección de datos de ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/index
ms.openlocfilehash: 1c38c587956dd99078fa4386c2f4423d784abc60
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277129"
---
# <a name="data-protection-in-aspnet-core"></a>Protección de datos en ASP.NET Core

* [Introducción a la protección de datos](xref:security/data-protection/introduction)

* [Introducción a las API de protección de datos](xref:security/data-protection/using-data-protection)

* [API de consumidor](xref:security/data-protection/consumer-apis/index)

  * [Información general sobre las API de consumidor](xref:security/data-protection/consumer-apis/overview)

  * [Cadenas de propósito](xref:security/data-protection/consumer-apis/purpose-strings)

  * [Jerarquía de propósito y configuración multiempresa](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)

  * [Aplicar un algoritmo hash a las contraseñas](xref:security/data-protection/consumer-apis/password-hashing)

  * [Limitación de la duración de cargas protegidas](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)

  * [Desprotección de cargas cuyas claves se han revocado](xref:security/data-protection/consumer-apis/dangerous-unprotect)

* [Configuración](xref:security/data-protection/configuration/index)

  * [Configurar la protección de datos en ASP.NET Core](xref:security/data-protection/configuration/overview)

  * [Configuración predeterminada](xref:security/data-protection/configuration/default-settings)

  * [Directiva de todo el equipo](xref:security/data-protection/configuration/machine-wide-policy)

  * [Escenarios no compatibles con DI](xref:security/data-protection/configuration/non-di-scenarios)

* [API de extensibilidad](xref:security/data-protection/extensibility/index)

  * [Extensibilidad de criptografía de núcleo](xref:security/data-protection/extensibility/core-crypto)

  * [Extensibilidad de administración de claves](xref:security/data-protection/extensibility/key-management)

  * [Otras API](xref:security/data-protection/extensibility/misc-apis)

* [Implementación](xref:security/data-protection/implementation/index)

  * [Detalles de cifrado autenticado](xref:security/data-protection/implementation/authenticated-encryption-details)

  * [Derivación de subclave y cifrado autenticado](xref:security/data-protection/implementation/subkeyderivation)

  * [Encabezados de contexto](xref:security/data-protection/implementation/context-headers)

  * [Administración de claves](xref:security/data-protection/implementation/key-management)

  * [Proveedores de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-providers)

  * [Cifrado de claves en reposo](xref:security/data-protection/implementation/key-encryption-at-rest)

  * [Inmutabilidad de claves y configuración](xref:security/data-protection/implementation/key-immutability)

  * [Formato de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-format)

  * [Proveedores de protección de datos efímeros](xref:security/data-protection/implementation/key-storage-ephemeral)

* [Compatibilidad](xref:security/data-protection/compatibility/index)

  * [Reemplazar <machineKey> de ASP.NET en ASP.NET Core](xref:security/data-protection/compatibility/replacing-machinekey)
