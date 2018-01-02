---
title: "Información general de seguridad de ASP.NET Core | Microsoft Docs"
author: rachelappel
description: "Obtenga información sobre los conceptos básicos de autenticación, autorización y seguridad en ASP.NET Core"
ms.author: rachelap
manager: wpickett
ms.date: 11/01/2017
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe012345
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/index
ms.openlocfilehash: 3f4df08d6cf5d183735ae4b4ec4f07ed60a9623a
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2017
---
# <a name="aspnet-core-security-overview"></a>Introducción a la seguridad de ASP.NET Core

ASP.NET Core permite a los desarrolladores configurar y administrar con facilidad la seguridad de sus aplicaciones. ASP.NET Core contiene características para administrar la autenticación, autorización, protección de datos, cumplimiento de SSL, secretos de aplicación, protección contra falsificación de solicitudes y administración de CORS. Estas características de seguridad permiten compilar aplicaciones de ASP.NET Core sólidas y seguras. 

## <a name="aspnet-core-security-features"></a>Características de seguridad de ASP.NET Core

ASP.NET Core proporciona muchas herramientas y bibliotecas para proteger las aplicaciones (por ejemplo, proveedores de identidades integrados), pero puede usar servicios de identidad de terceros como Facebook, Twitter y LinkedIn. Con ASP.NET Core, puede administrar con facilidad los secretos de aplicación, que son una forma de almacenar y usar información confidencial sin tener que exponerla en el código. 

## <a name="authentication-vs-authorization"></a>Autenticación frente a Autorización

La autenticación es un proceso en el que un usuario proporciona credenciales que después se comparan con las almacenadas en un sistema operativo, base de datos, aplicación o recurso. Si coinciden, los usuarios se autentican correctamente y, después, pueden realizar las acciones para las que están autorizados, durante un proceso de autorización. La autorización se refiere al proceso que determina las acciones que un usuario puede realizar. 

La autenticación también se puede considerar una manera de entrar en un espacio (como un servidor, base de datos, aplicación o recurso) mientras que la autorización es qué acciones puede realizar el usuario en qué objetos de ese espacio (servidor, base de datos o aplicación).

## <a name="common-vulnerabilities-in-software"></a>Vulnerabilidades más comunes en software

ASP.NET Core y EF contienen características que ayudan a proteger las aplicaciones y evitar las infracciones de seguridad. La siguiente lista de vínculos le lleva a documentación en la que se detallan técnicas para evitar las vulnerabilidades de seguridad más comunes en las aplicaciones web:

* [Ataques de scripts de sitios](https://docs.microsoft.com/aspnet/core/security/cross-site-scripting)
* [Ataques por inyección de código SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Falsificación de solicitudes entre sitios. (CSRF)](https://docs.microsoft.com/aspnet/core/security/anti-request-forgery)
* [Ataques de redireccionamiento abierto](https://docs.microsoft.com/aspnet/core/security/preventing-open-redirects)

Hay más vulnerabilidades que debe tener en cuenta. Para más información, vea la sección de este documento sobre *Documentación de seguridad de ASP.NET*. 

## <a name="aspnet-security-documentation"></a>Documentación de seguridad de ASP.NET

*   [Autenticación](authentication/index.md)
    *   [Introducción a Identity](authentication/identity.md)
    *   [Habilitación de la autenticación con Facebook, Google y otros proveedores externos](authentication/social/index.md)
    * [Configuración de la autenticación de Windows](authentication/windowsauth.md)
    *   [Confirmación de cuentas y recuperación de contraseñas](authentication/accconfirm.md)
    *   [Autenticación en dos fases con SMS](authentication/2fa.md) 
    *   [Uso de la autenticación de cookies sin ASP.NET Core Identity](authentication/cookie.md)
    *   [Azure Active Directory](authentication/azure-active-directory/index.md)
        *   [Integración de Azure AD en una aplicación web de ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Llamada a una API Web de ASP.NET Core desde una aplicación de WPF con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Llamada a una API Web en una aplicación web de ASP.NET Core con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Una aplicación web de ASP.NET Core con Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
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
    *   [Autorización basada en visualizaciones](authorization/views.md)
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
        *   [Configuración de la protección de datos](data-protection/configuration/overview.md)
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
*   [Prevención de ataques de falsificación de solicitudes](anti-request-forgery.md)
*   [Prevención de ataques de redireccionamiento abierto](preventing-open-redirects.md)
*   [Prevención de scripting entre sitios](cross-site-scripting.md)
*   [Habilitación de solicitudes entre orígenes (CORS)](cors.md)
