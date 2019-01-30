---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introducción a ASP.NET Identity | Microsoft Docs
author: jongalloway
description: El sistema de pertenencia ASP.NET se introdujo con ASP.NET 2.0 back en 2005 y, puesto que, a continuación, ha habido muchos cambios en lo veinticinco de aplicaciones web de formas...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 4a545e52d2d9ea04a10c37c116fd326c60de9f8f
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236450"
---
<a name="introduction-to-aspnet-identity"></a>Introducción a ASP.NET Identity
====================

> El sistema de pertenencia ASP.NET se introdujo con ASP.NET 2.0 back en 2005 y, puesto que, a continuación, ha habido muchos cambios en las formas en las aplicaciones web normalmente controlan la autenticación y autorización. ASP.NET Identity es un aspecto novedoso en cuál debe ser el sistema de pertenencia para compilar aplicaciones modernas para la web, tableta o teléfono.


## <a name="background-membership-in-aspnet"></a>Información previa: Pertenencia a ASP.NET

### <a name="aspnet-membership"></a>Pertenencia a ASP.NET

[Pertenencia a ASP.NET](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) se diseñó para resolver los requisitos de pertenencia de sitio que son comunes en 2005, que implica la autenticación de formularios y una base de datos de SQL Server para los nombres de usuario, contraseñas y datos de perfil. En la actualidad hay mucho más amplio de opciones de almacenamiento de datos para aplicaciones web y la mayoría de los desarrolladores desean permitir que sus sitios usar proveedores de identidades sociales para la funcionalidad de autenticación y autorización. Las limitaciones de diseño de la pertenencia de ASP.NET realizar esta transición difícil:

- El esquema de base de datos se diseñó para SQL Server y no podrá cambiarlo. Puede agregar información de perfil, pero los datos adicionales se empaquetan en una tabla diferente, lo que dificulta a acceso por ningún medio, excepto a través de la API de proveedor de Profile.
- El sistema del proveedor le permite cambiar el almacén de datos de copia de seguridad, pero el sistema está diseñado en torno a las suposiciones adecuadas para una base de datos relacional. Puede escribir un proveedor para almacenar información de pertenencia en un mecanismo de almacenamiento no relacional, como las tablas de almacenamiento de Azure, pero, a continuación, debe evitar el diseño relacional al escribir mucho código y mucha `System.NotImplementedException` excepciones para los métodos que no se aplica a bases de datos NoSQL.
- Puesto que la funcionalidad de registro-y registro horizontal se basa en la autenticación de formularios, no se puede usar el sistema de pertenencia [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN incluye componentes de middleware para la autenticación, incluida la compatibilidad con inicios de sesión con proveedores de identidades externos (por ejemplo, Accounts de Microsoft, Facebook, Google, Twitter) y los inicios de sesión con cuentas organizativas de Active Directory local o Azure Active Directory. OWIN también incluye compatibilidad con OAuth 2.0, JWT y CORS.

### <a name="aspnet-simple-membership"></a>Pertenencia a ASP.NET Simple

[Pertenencia ASP.NET sencilla](../../../web-pages/overview/security/16-adding-security-and-membership.md) se desarrolló como un sistema de pertenencia de ASP.NET Web Pages. Se lanzó con WebMatrix y Visual Studio 2010 SP1. El objetivo de pertenencia sencilla era para que sea fácil agregar la funcionalidad de pertenencia a una aplicación de páginas Web.

Pertenencia sencilla que resulte más fácil personalizar la información de perfil de usuario, pero todavía comparte los otros problemas con la pertenencia a ASP.NET y tiene algunas limitaciones:

- Era difícil conservar los datos del sistema de pertenencia en un almacén no relacional.
- No puede utilizarla con OWIN.
- No funciona bien con los proveedores de pertenencia de ASP.NET existentes y no es extensible.

### <a name="aspnet-universal-providers"></a>Proveedores universales de ASP.NET

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) se desarrollaron para que sea posible conservar información de pertenencia en Microsoft Azure SQL Database y también funcionan con SQL Server Compact. Los proveedores universales se basa en Entity Framework Code First, lo que significa que los proveedores universales pueden usarse para conservar los datos en cualquier almacén compatible con EF. Con los proveedores universales, el esquema de base de datos se limpió también una gran cantidad.

Los proveedores universales están integrados en la infraestructura de la pertenencia a ASP.NET, por lo que realizar las mismas limitaciones que el proveedor SqlMembership. Es decir, como se diseñaron para bases de datos relacionales y resulta difícil personalizar la información de usuario y perfiles. Estos proveedores también sigue usan la autenticación de formularios para la funcionalidad de inicio de sesión y cierre de sesión.

## <a name="aspnet-identity"></a>ASP.NET Identity

Como la pertenencia al caso en ASP.NET ha evolucionado durante años, el equipo de ASP.NET ha aprendido mucho de comentarios de los clientes.

La suposición de que los usuarios iniciarán sesión, escriba un nombre de usuario y una contraseña que ha registrado en su propia aplicación ya no es válida. La web se ha vuelto más social. Los usuarios interactúan entre sí en tiempo real a través de canales sociales como Facebook, Twitter y otros sitios web sociales. Los desarrolladores desean que los usuarios puedan iniciar sesión con sus identidades sociales, por lo que pueden tener una experiencia enriquecida en sus sitios web. Un sistema de pertenencia moderno debe habilitar los inicios de sesión basada en redirección a los proveedores de autenticación, como Facebook, Twitter y otros.

Como ha evolucionado el desarrollo web, también lo hacían los patrones de desarrollo web. Unidad de las pruebas del código de aplicación se convirtió en una preocupación principal para desarrolladores de aplicaciones. En 2008, ASP.NET agrega un nuevo marco basado en el patrón Model-View-Controller (MVC), en parte a ayudar a los desarrolladores crear unidad puede probar aplicaciones de ASP.NET. Los desarrolladores que querían unidad probar su lógica de aplicación también quería ser capaz de hacerlo con el sistema de pertenencia.

Teniendo en cuenta estos cambios en el desarrollo de aplicaciones web, ASP.NET Identity se desarrolló con los siguientes objetivos:

- **Un sistema ASP.NET Identity**

    - ASP.NET Identity se puede usar con todos los marcos ASP.NET, como ASP.NET MVC, formularios Web Forms, las páginas Web, Web API y SignalR.
    - ASP.NET Identity se puede usar cuando se crean aplicaciones web, teléfono, almacén o híbrida.
- **Facilidad de la conexión de datos de perfil sobre el usuario**

    - Tener control sobre el esquema de usuario y la información de perfil. Por ejemplo, puede habilitar fácilmente el sistema almacenar las fechas de nacimiento especificadas por los usuarios cuando registra una cuenta en la aplicación.

- **Control de persistencia**

    - De forma predeterminada, el sistema ASP.NET Identity almacena toda la información de usuario en una base de datos. ASP.NET Identity utiliza Entity Framework Code First para implementar todos su mecanismo de persistencia.
    - Puesto que controlar el esquema de base de datos, tareas comunes, como cambiar los nombres de tabla o el tipo de datos de las claves principales es fácil.
    - Es fácil conectar mecanismos de almacenamiento diferentes, como SharePoint, Azure Storage Table Service, las bases de datos NoSQL, etc., sin tener que iniciar `System.NotImplementedExceptions` excepciones.
- **Capacidad de prueba unitaria**

    - ASP.NET Identity hace que la aplicación web de la unidad más apta para las pruebas. Puede escribir pruebas unitarias para las partes de la aplicación que utilicen ASP.NET Identity.
- **Proveedor de roles**

    - Hay un proveedor de roles que le permite restringir el acceso a partes de la aplicación mediante roles. Fácilmente, puede crear funciones, como "Admin" y agregar usuarios a roles.
- **Basada en notificaciones**

    - ASP.NET Identity admite la autenticación basada en notificaciones, donde la identidad del usuario se representa como un conjunto de notificaciones. Las notificaciones permiten a los desarrolladores a ser mucho más expresivo para describir una identidad de usuario de roles permiten. Mientras que la pertenencia a roles es simplemente un valor booleano (miembro o no miembro), una notificación puede incluir información valiosa acerca de la identidad y la pertenencia del usuario.
- **Proveedores de inicio de sesión social**

    - Fácilmente, puede agregar inicios de sesión sociales, como Account de Microsoft, Facebook, Twitter, Google y otros usuarios a la aplicación y almacenar los datos específicos del usuario en la aplicación.

- **Integración de OWIN**

    - Autenticación de ASP.NET se basa ahora en middleware OWIN que se puede usar en cualquier host basado en OWIN. ASP.NET Identity no tiene ninguna dependencia en System.Web. Es un marco OWIN totalmente compatible y puede usarse en cualquier aplicación hospedada de OWIN.
    - ASP.NET Identity utiliza la autenticación de OWIN para el registro en/log-horizontal de los usuarios en el sitio web. Esto significa que, en lugar de usar FormsAuthentication para generar la cookie, la aplicación usa OWIN CookieAuthentication hacer eso.
- **Paquete NuGet**

    - ASP.NET Identity se redistribuye como un paquete de NuGet que se instala en las plantillas de ASP.NET MVC, formularios Web Forms y Web API que se suministran con Visual Studio 2017. Puede descargar este paquete de NuGet desde la Galería de NuGet.
    - Liberación de ASP.NET Identity como NuGet paquete resulta más fácil para el equipo de ASP.NET iterar en las nuevas características y correcciones de errores y ofrecer a los desarrolladores de una manera ágil.

## <a name="get-started-with-aspnet-identity"></a>Introducción a ASP.NET Identity

ASP.NET Identity se utiliza en las plantillas de proyecto de Visual Studio 2017 para ASP.NET MVC, formularios Web Forms, Web API y SPA. En este tutorial, le explicaremos cómo las plantillas de proyecto usan ASP.NET Identity para agregar funcionalidad para registrar, inicie sesión y cerrar sesión un usuario.

ASP.NET Identity se implementa mediante el procedimiento siguiente. El propósito de este artículo es proporcionarle una visión general de alto nivel de ASP.NET Identity; puede seguir paso a paso o simplemente leer los detalles. Para obtener instrucciones más detalladas sobre la creación de aplicaciones con ASP.NET Identity, incluido el uso de la nueva API para agregar usuarios, roles y la información de perfil, vea la sección pasos siguientes al final de este artículo.

1. Cree una aplicación ASP.NET MVC con cuentas individuales. Puede usar ASP.NET Identity en ASP.NET MVC, formularios Web Forms, Web API, SignalR etcetera. En este artículo comenzamos con una aplicación ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. El proyecto creado contiene los siguientes tres paquetes de ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Este paquete tiene la implementación de Entity Framework de ASP.NET Identity que se conservarán los datos de identidad de ASP.NET y el esquema a SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Este paquete tiene las interfaces del núcleo de ASP.NET Identity. Este paquete se puede usar para escribir una implementación para la identidad de ASP.NET que almacena persistencia diferentes destinos como Azure Table Storage, NoSQL etcetera las bases de datos.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Este paquete contiene la funcionalidad que se usa para conectar autenticación OWIN con ASP.NET Identity en aplicaciones ASP.NET. Esto se utiliza cuando se agrega un inicio de sesión en la funcionalidad a su aplicación y llamar a la autenticación de cookies de OWIN middleware para generar una cookie.
3. Creación de un usuario.  
   Iniciar la aplicación y, a continuación, haga clic en el **registrar** vínculo para crear un usuario. La siguiente imagen muestra la página de registro que se recopilan en el nombre de usuario y la contraseña.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Cuando el usuario selecciona el **registrar** botón, el `Register` acción del controlador de la cuenta crea el usuario mediante una llamada a la API de ASP.NET Identity, tal como se muestra a continuación:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Inicia sesión.  
   Si el usuario se creó correctamente, inicia sesión el `SignInAsync` método.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample6.cs?highlight=12)]


   El `SignInManager.SignInAsync` método genera una [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Dado que ASP.NET Identity y autenticación de cookies de OWIN son sistema basada en notificaciones, el marco de trabajo requiere la aplicación para generar una ClaimsIdentity para el usuario. ClaimsIdentity tiene información sobre todas las notificaciones del usuario, como los roles que pertenece el usuario.   
 
5. Cierre la sesión.  
   Seleccione el **cerrar** vínculo para llamar a la acción de cierre de sesión en el controlador de cuentas. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   Código resaltado anterior se muestra cómo OWIN `AuthenticationManager.SignOut` método. Esto es análogo a [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) método utilizado por el [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) módulo en formularios Web Forms.

## <a name="components-of-aspnet-identity"></a>Componentes de ASP.NET Identity

El diagrama siguiente muestra los componentes del sistema ASP.NET Identity (seleccione en [esto](introduction-to-aspnet-identity/_static/image3.png) o en el diagrama para ampliarla). Los paquetes en verde conforman el sistema ASP.NET Identity. Todos los paquetes son dependencias que son necesarios para usar el sistema ASP.NET Identity en aplicaciones ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

La siguiente es una breve descripción de los paquetes de NuGet que no se ha mencionado anteriormente:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware que permite que una aplicación usar la cookie basa la autenticación, similar a ASP. Autenticación de formularios de la red.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework es la tecnología de acceso de datos recomendado de Microsoft para bases de datos relacionales.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migración de la pertenencia a ASP.NET Identity

Esperamos que pronto se proporcionan instrucciones sobre cómo migrar las aplicaciones existentes que usan la pertenencia de ASP.NET o de pertenencia sencilla al nuevo sistema ASP.NET Identity.

## <a name="next-steps"></a>Pasos siguientes

- [Crear una aplicación ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 El tutorial utiliza la API de ASP.NET Identity para agregar información de perfil para la base de datos de usuario y cómo realizar la autenticación con Google y Facebook.
- [Crear una aplicación ASP.NET MVC con la autenticación y base de datos SQL e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Este tutorial muestra cómo usar la API de identidad para agregar usuarios y roles.
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Aplicación de ejemplo que muestra cómo agregar roles básicos y soporte técnico de usuario y cómo realizar la administración de usuarios y roles.
