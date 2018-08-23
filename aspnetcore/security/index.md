---
title: Introducción a la seguridad de ASP.NET Core
author: tdykstra
description: Obtenga información sobre los conceptos básicos de autenticación, autorización y seguridad en ASP.NET Core.
ms.author: tdykstra
ms.date: 11/01/2017
uid: security/index
ms.openlocfilehash: 3a1c1ea1ad28fccbe5ae91b0be193938b095f60b
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41750311"
---
# <a name="overview-of-aspnet-core-security"></a>Introducción a la seguridad de ASP.NET Core

ASP.NET Core permite a los desarrolladores configurar y administrar con facilidad la seguridad de sus aplicaciones. ASP.NET Core contiene características para administrar la autenticación, autorización, protección de datos, cumplimiento de SSL, secretos de aplicación, protección contra falsificación de solicitudes y administración de CORS. Estas características de seguridad permiten compilar aplicaciones de ASP.NET Core sólidas y seguras.

## <a name="aspnet-core-security-features"></a>Características de seguridad de ASP.NET Core

ASP.NET Core proporciona muchas herramientas y bibliotecas para proteger las aplicaciones (por ejemplo, proveedores de identidades integrados), pero puede usar servicios de identidad de terceros como Facebook, Twitter y LinkedIn. Con ASP.NET Core, puede administrar con facilidad los secretos de aplicación, que son una forma de almacenar y usar información confidencial sin tener que exponerla en el código.

## <a name="authentication-vs-authorization"></a>Autenticación frente a Autorización

La autenticación es un proceso en el que un usuario proporciona credenciales que después se comparan con las almacenadas en un sistema operativo, base de datos, aplicación o recurso. Si coinciden, los usuarios se autentican correctamente y, después, pueden realizar las acciones para las que están autorizados durante un proceso de autorización. La autorización se refiere al proceso que determina las acciones que un usuario puede realizar.

La autenticación también se puede considerar una manera de entrar en un espacio (como un servidor, base de datos, aplicación o recurso) mientras que la autorización es qué acciones puede realizar el usuario en qué objetos de ese espacio (servidor, base de datos o aplicación).

## <a name="common-vulnerabilities-in-software"></a>Vulnerabilidades más comunes en software

ASP.NET Core y EF contienen características que ayudan a proteger las aplicaciones y evitar las infracciones de seguridad. La siguiente lista de vínculos le lleva a documentación en la que se detallan técnicas para evitar las vulnerabilidades de seguridad más comunes en las aplicaciones web:

* [Ataques de scripting entre sitios](xref:security/cross-site-scripting)
* [Ataques por inyección de código SQL](https://docs.microsoft.com/ef/core/querying/raw-sql)
* [Falsificación de solicitudes entre sitios. (CSRF)](xref:security/anti-request-forgery)
* [Ataques de redireccionamiento abierto](xref:security/preventing-open-redirects)

Hay más vulnerabilidades que debe tener en cuenta. Para obtener más información, vea la sección de este documento relativa a la *documentación de seguridad de ASP.NET Core*.

## <a name="aspnet-core-security-documentation"></a>Documentación de seguridad de ASP.NET Core

*   [Autenticación](xref:security/authentication/index)
    *   [Introducción a Identity](xref:security/authentication/identity)
    *   [Habilitar la autenticación con Facebook, Google y otros proveedores externos](xref:security/authentication/social/index)
    *   [Habilitar la autenticación con WS-Federation](xref:security/authentication/ws-federation)
    * [Configuración de la autenticación de Windows](xref:security/authentication/windowsauth)
    *   [Confirmación de cuentas y recuperación de contraseñas](xref:security/authentication/accconfirm)
    *   [Autenticación en dos fases con SMS](xref:security/authentication/2fa)
    *   [Uso de la autenticación de cookies sin identidad](xref:security/authentication/cookie)
    *   [Azure Active Directory](xref:security/authentication/azure-active-directory/index)
        *   [Integración de Azure AD en una aplicación web de ASP.NET Core](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
        *   [Llamada a una API web de ASP.NET Core desde una aplicación de WPF con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
        *   [Llamada a una API web en una aplicación web de ASP.NET Core con Azure AD](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
        *   [Una aplicación web de ASP.NET Core con Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapp/)
    *   [Protección de aplicaciones de ASP.NET Core con IdentityServer4](https://identityserver4.readthedocs.io)
*   [Autorización](xref:security/authorization/index)
    *   [Introducción](xref:security/authorization/introduction)
    *   [Creación de una aplicación con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
    *   [Autorización simple](xref:security/authorization/simple)
    *   [Autorización basada en roles](xref:security/authorization/roles)
    *   [Autorización basada en notificaciones](xref:security/authorization/claims)
    *   [Autorización basada en directivas](xref:security/authorization/policies)
    *   [Inserción de dependencias en controladores de requisitos](xref:security/authorization/dependencyinjection)
    *   [Autorización basada en recursos](xref:security/authorization/resourcebased)
    *   [Autorización basada en visualizaciones](xref:security/authorization/views)
    *   [Limitación de la identidad por esquema](xref:security/authorization/limitingidentitybyscheme)
*   [Protección de datos](xref:security/data-protection/index)
    *   [Introducción a la protección de datos](xref:security/data-protection/introduction)
    *   [Introducción a las API de protección de datos](xref:security/data-protection/using-data-protection)
    *   [API de consumidor](xref:security/data-protection/consumer-apis/index)
        *   [Información general sobre las API de consumidor](xref:security/data-protection/consumer-apis/overview)
        *   [Cadenas de propósito](xref:security/data-protection/consumer-apis/purpose-strings)
        *   [Jerarquía de propósito y configuración multiempresa](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
        *   [Aplicar un algoritmo hash a las contraseñas](xref:security/data-protection/consumer-apis/password-hashing)
        *   [Limitación de la duración de cargas protegidas](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
        *   [Desprotección de cargas cuyas claves se han revocado](xref:security/data-protection/consumer-apis/dangerous-unprotect)
    *   [Configuración](xref:security/data-protection/configuration/index)
        *   [Configuración de la protección de datos](xref:security/data-protection/configuration/overview)
        *   [Configuración predeterminada](xref:security/data-protection/configuration/default-settings)
        *   [Directiva de todo el equipo](xref:security/data-protection/configuration/machine-wide-policy)
        *   [Escenarios no compatibles con DI](xref:security/data-protection/configuration/non-di-scenarios)
    *   [API de extensibilidad](xref:security/data-protection/extensibility/index)
        *   [Extensibilidad de criptografía de núcleo](xref:security/data-protection/extensibility/core-crypto)
        *   [Extensibilidad de administración de claves](xref:security/data-protection/extensibility/key-management)
        *   [Otras API](xref:security/data-protection/extensibility/misc-apis)
    *   [Implementación](xref:security/data-protection/implementation/index)
        *   [Detalles de cifrado autenticado](xref:security/data-protection/implementation/authenticated-encryption-details)
        *   [Derivación de subclave y cifrado autenticado](xref:security/data-protection/implementation/subkeyderivation)
        *   [Encabezados de contexto](xref:security/data-protection/implementation/context-headers)
        *   [Administración de claves](xref:security/data-protection/implementation/key-management)
        *   [Proveedores de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-providers)
        *   [Cifrado de claves en reposo](xref:security/data-protection/implementation/key-encryption-at-rest)
        *   [Inmutabilidad de claves y configuración](xref:security/data-protection/implementation/key-immutability)
        *   [Formato de almacenamiento de claves](xref:security/data-protection/implementation/key-storage-format)
        *   [Proveedores de protección de datos efímeros](xref:security/data-protection/implementation/key-storage-ephemeral)
    *   [Compatibilidad](xref:security/data-protection/compatibility/index)
        *   [Reemplazar <machineKey> en ASP.NET](xref:security/data-protection/compatibility/replacing-machinekey)
*   [Creación de una aplicación con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
*   [Almacenamiento seguro de secretos de aplicación en el desarrollo](xref:security/app-secrets)
*   [Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration)
*   [Aplicación de SSL](xref:security/enforcing-ssl)
*   [Prevención de ataques de falsificación de solicitudes](xref:security/anti-request-forgery)
*   [Prevención de ataques de redireccionamiento abierto](xref:security/preventing-open-redirects)
*   [Prevención de scripting entre sitios](xref:security/cross-site-scripting)
*   [Habilitar solicitudes entre orígenes (CORS)](xref:security/cors)
*   [Compartir cookies entre aplicaciones](xref:security/cookie-sharing)
