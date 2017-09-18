---
title: Seguridad
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: aa22d072a6ef0ff105d67c2bfc5c335511d6c0cd
ms.sourcegitcommit: aa6951e0c2e62209bf7c25e3b3138f04eb92898d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2017
---
# <a name="security"></a>Seguridad

*   [Autenticación](authentication/index.md)
    *   [Introducción a Identity](authentication/identity.md)
    *   [Habilitación de la autenticación con Facebook, Google y otros proveedores externos](authentication/social/index.md)
    * [Configuración de la autenticación de Windows](authentication/windowsauth.md)
    *   [Confirmación de cuentas y recuperación de contraseñas](authentication/accconfirm.md)
    *   [Autenticación en dos fases con SMS](authentication/2fa.md) 
    *   [Uso de la autenticación de cookies sin ASP.NET Core Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integración de Azure AD en una aplicación web de ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore)
        *   [Llamada a una API Web de ASP.NET Core desde una aplicación de WPF con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore)
        *   [Llamada a una API Web en una aplicación web de ASP.NET Core con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Una aplicación web de ASP.NET Core con Azure AD B2C](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-b2c/)
    *   [Protección de aplicaciones de ASP.NET Core con IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorización](authorization/index.md)
    *   [Introducción](authorization/introduction.md)
    *   [Creación de una aplicación con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
    *   [Autorización simple](authorization/simple.md)
    *   [Autorización basada en roles](authorization/roles.md)
    *   [Autorización basada en notificaciones](authorization/claims.md)
    *   [Autorización personalizada basada en directivas](authorization/policies.md)
    *   [Inserción de dependencias en controladores de requisitos](authorization/dependencyinjection.md)
    *   [Autorización basada en recursos](authorization/resourcebased.md)
    *   [Autorización basada en vistas](authorization/views.md)
    *   [Limitación de identidad por esquema](authorization/limitingidentitybyscheme.md)
*   [Protección de datos](data-protection/index.md)
    *   [Introducción a la protección de datos](data-protection/introduction.md)
    *   [Introducción a las API de protección de datos](data-protection/using-data-protection.md)
    *   [API de consumidor](data-protection/consumer-apis/index.md)
        *   [Información general sobre las API de consumidor](data-protection/consumer-apis/overview.md)
        *   [Cadenas de propósito](data-protection/consumer-apis/purpose-strings.md)
        *   [Jerarquía de propósito y configuración multiempresa](data-protection/consumer-apis/purpose-strings-multitenancy.md)
        *   [Hash de contraseña](data-protection/consumer-apis/password-hashing.md)
        *   [Limitación de la duración de cargas protegidas](data-protection/consumer-apis/limited-lifetime-payloads.md)
        *   [Desprotección de cargas cuyas claves se han revocado](data-protection/consumer-apis/dangerous-unprotect.md)
    *   [Configuración](data-protection/configuration/index.md)
        *   [Configuración de protección de datos](data-protection/configuration/overview.md)
        *   [Configuración predeterminada](data-protection/configuration/default-settings.md)
        *   [Directiva para toda la máquina](data-protection/configuration/machine-wide-policy.md)
        *   [Escenarios no compatibles con DI](data-protection/configuration/non-di-scenarios.md)
    *   [API de extensibilidad](data-protection/extensibility/index.md)
        *   [Extensibilidad de criptografía de núcleo](data-protection/extensibility/core-crypto.md)
        *   [Extensibilidad de administración de claves](data-protection/extensibility/key-management.md)
        *   [Otras API](data-protection/extensibility/misc-apis.md)
    *   [Implementación](data-protection/implementation/index.md)
        *   [Detalles de cifrado autenticado](data-protection/implementation/authenticated-encryption-details.md)
        *   [Derivación de subclave y cifrado autenticado](data-protection/implementation/subkeyderivation.md)
        *   [Encabezados de contexto](data-protection/implementation/context-headers.md)
        *   [Administración de claves](data-protection/implementation/key-management.md)
        *   [Proveedores de almacenamiento de claves](data-protection/implementation/key-storage-providers.md)
        *   [Cifrado de claves en reposo](data-protection/implementation/key-encryption-at-rest.md)
        *   [Inmutabilidad de claves y cambio de configuración](data-protection/implementation/key-immutability.md)
        *   [Formato de almacenamiento de claves](data-protection/implementation/key-storage-format.md)
        *   [Proveedores de protección de datos efímeros](data-protection/implementation/key-storage-ephemeral.md)
    *   [Compatibilidad](data-protection/compatibility/index.md)
        *   [Uso compartido de cookies entre aplicaciones](data-protection/compatibility/cookie-sharing.md)
        *   [Reemplazo de <machineKey> en ASP.NET](data-protection/compatibility/replacing-machinekey.md)
*   [Creación de una aplicación con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
*   [Almacenamiento seguro de secretos de aplicación durante el desarrollo](app-secrets.md)
*   [Proveedor de configuración de Azure Key Vault](key-vault-configuration.md)
*   [Aplicación de SSL](enforcing-ssl.md)
*   [Configuración de HTTPS para el desarrollo](https.md)
*   [Prevención de ataques de falsificación de solicitudes](anti-request-forgery.md)
*   [Prevención de ataques de redireccionamiento abierto](preventing-open-redirects.md)
*   [Prevención de scripting entre sitios](cross-site-scripting.md)
*   [Habilitación de solicitudes entre orígenes (CORS)](cors.md)
