---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: "Introducción a la identidad de ASP.NET | Documentos de Microsoft"
author: jongalloway
description: "El sistema de pertenencia ASP.NET se introdujo con ASP.NET 2.0 back en 2005 y, puesto que, a continuación, ha habido muchos cambios en el normalmente de las aplicaciones web de formas..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: a66e2a80668dbf291b9cc34f205b546b72d92bcc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-identity"></a>Introducción a la identidad de ASP.NET
====================
por [Jon Galloway](https://github.com/jongalloway), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

> El sistema de pertenencia ASP.NET se introdujo con ASP.NET 2.0 back en 2005 y, puesto que, a continuación, ha habido muchos cambios en las formas de las aplicaciones web normalmente controlan la autenticación y autorización. ASP.NET Identity es un vistazo actualizado a cuál debe ser el sistema de pertenencia cuando se crean aplicaciones modernas de web, teléfono o tableta.
> 
> En este artículo se escribió por Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Jon Galloway ([@jongalloway](https://twitter.com/jongalloway)), Tom Dykstra y Rick Anderson ([ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="background-membership-in-aspnet"></a>En segundo plano: Pertenencia a ASP.NET

### <a name="aspnet-membership"></a>Pertenencia a ASP.NET

[Pertenencia a ASP.NET](https://msdn.microsoft.com/en-us/library/yh26yfzy(v=VS.100).aspx) se ha diseñado para resolver los requisitos de pertenencia de sitio que son comunes en 2005, que se invirtió en la autenticación de formularios y una base de datos de SQL Server para nombres de usuario, contraseñas y datos de perfil. Actualmente hay una variedad mucho más amplia de opciones de almacenamiento de datos para aplicaciones web, y la mayoría de los desarrolladores desean habilitar sus sitios usar proveedores de identidades sociales para la funcionalidad de autenticación y autorización. Las limitaciones de diseño de la pertenencia de ASP.NET dificultan esta transición:

- El esquema de base de datos se diseñó para SQL Server y no podrá cambiarlo. Puede agregar información de perfil, pero los datos adicionales se empaquetan en una tabla diferente, lo que dificulta a access por ningún medio, excepto a través de la API de proveedor de perfil.
- El sistema del proveedor le permite cambiar el almacén de datos de copia de seguridad, pero el sistema se ha diseñado basándose suposiciones adecuados para una base de datos relacional. Puede escribir un proveedor para almacenar información de pertenencia en un mecanismo de almacenamiento no relacionales, como las tablas de almacenamiento de Azure, pero se deben evitar el diseño relacional escribiendo una gran cantidad de código y una gran cantidad de `System.NotImplementedException` excepciones para los métodos que no se aplica a las bases de datos NoSQL.
- Puesto que la funcionalidad de registro/desconecte se basa en la autenticación de formularios, no se puede usar el sistema de pertenencia [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN incluye componentes de middleware para la autenticación, incluida la compatibilidad con inicios de sesión con proveedores de identidad externa (por ejemplo, Accounts de Microsoft, Facebook, Google, Twitter) y los inicios de sesión con cuentas organizativas de Active Directory local o Active Directory de Azure. OWIN también incluye compatibilidad con OAuth 2.0, JWT y CORS.

### <a name="aspnet-simple-membership"></a>Pertenencia a ASP.NET Simple

[Pertenencia sencillo de ASP.NET](../../../web-pages/overview/security/16-adding-security-and-membership.md) se desarrolló como un sistema de pertenencia de ASP.NET Web Pages. Se publicó con WebMatrix y Visual Studio 2010 SP1. El objetivo de pertenencia sencillo fue que resulte sencillo agregar la funcionalidad de pertenencia a una aplicación de páginas Web.

Pertenencia sencillo hizo fáciles de personalizar la información de perfil de usuario, pero todavía comparte los demás problemas con pertenencia a ASP.NET y tiene algunas limitaciones:

- Era difícil conservar los datos de sistema de pertenencia en un almacén no relacionales.
- No puede utilizarla con OWIN.
- No funciona bien con los proveedores de pertenencia de ASP.NET existentes y no es extensible.

### <a name="aspnet-universal-providers"></a>Proveedores universales de ASP.NET

[ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) se desarrollaron para que sea posible la persistencia de la información de pertenencia de Microsoft la base de datos SQL Azure y también funcionan con SQL Server Compact. Se han generado los proveedores universales en Entity Framework Code First, lo que significa que los proveedores universales puede usarse para conservar los datos en cualquier almacén admitido EF. Con los proveedores universales, el esquema de base de datos se limpia una gran cantidad de así.

Los proveedores universales se basan en la infraestructura de la pertenencia a ASP.NET, por lo que aún tienen las mismas limitaciones que el proveedor de SqlMembership. Es decir, se diseñaron para bases de datos relacionales y es difícil personalizar la información de usuario y perfiles. Sin embargo, estos proveedores usar autenticación por formularios para la funcionalidad de inicio de sesión y registro de salida.

## <a name="aspnet-identity"></a>ASP.NET Identity

Como la pertenencia al caso en ASP.NET ha evolucionado durante los años, el equipo de ASP.NET ha aprendido mucho de comentarios de los clientes.

La suposición de que los usuarios inician sesión, escriba un nombre de usuario y una contraseña que se haya registrado en su propia aplicación ya no es válida. La web se ha vuelto más social. Los usuarios interactúan entre sí en tiempo real a través de canales sociales como Facebook, Twitter y otros sitios web sociales. Los desarrolladores desean que los usuarios puedan iniciar sesión con sus identidades sociales para que pueda tener una rica experiencia en sus sitios web. Un sistema de pertenencia moderna debe habilitar los inicios de sesión basada en redirección a los proveedores de autenticación como Facebook, Twitter y otros.

Como ha evolucionado desarrollo web, también lo hacían los patrones de desarrollo web. Unidad de pruebas de código de la aplicación pasó a ser un problema principal para los desarrolladores de aplicaciones. En 2008 ASP.NET agrega un nuevo marco basado en el modelo Model-View-Controller (MVC), en parte para ayudar a los desarrolladores crear unidad puede probar aplicaciones de ASP.NET. Los desarrolladores que desearan unidad probar su lógica de la aplicación también deseada poder hacerlo con el sistema de pertenencia.

Teniendo en cuenta estos cambios de desarrollo de aplicaciones web, ASP.NET Identity se desarrolló con los siguientes objetivos:

- **Un sistema de identidades de ASP.NET**

    - Identidad de ASP.NET se puede utilizar con todos los marcos de trabajo ASP.NET, como ASP.NET MVC, formularios Web Forms, páginas Web, Web API y SignalR.
    - Identidad de ASP.NET se puede utilizar al crear aplicaciones web, teléfono, almacén o híbrida.
- **Facilidad de conectarse a datos de perfil del usuario**

    - Tiene control sobre el esquema de usuario y la información de perfil. Por ejemplo, puede habilitar fácilmente el sistema almacenar las fechas de nacimiento especificadas por los usuarios cuando registra una cuenta en la aplicación.

- **Control de persistencia**

    - De forma predeterminada, el sistema de identidades de ASP.NET almacena toda la información de usuario en una base de datos. Identidad de ASP.NET usa Entity Framework Code First para implementar todo su mecanismo de persistencia.
    - Puesto que controlan el esquema de base de datos, tareas comunes, como cambiar los nombres de tabla o cambiar el tipo de datos de claves principales es fácil de hacer.
    - Es fácil agregar mecanismos de almacenamiento diferente, como SharePoint, servicio de tabla de almacenamiento de Azure, las bases de datos NoSQL, etc., sin tener que iniciar `System.NotImplementedExceptions` excepciones.
- **Capacidad para realizar pruebas unitarias**

    - ASP.NET Identity hace que la aplicación web más unidad pueden someterse a prueba. Puede escribir pruebas unitarias para las partes de la aplicación que utilicen ASP.NET Identity.
- **Proveedor de funciones**

    - Hay un proveedor de funciones que le permite restringir el acceso a partes de la aplicación mediante roles. Fácilmente, puede crear roles, como "Admin" y agregar usuarios a roles.
- **Basada en notificaciones**

    - Identidad de ASP.NET admite la autenticación basada en notificaciones, que la identidad del usuario se representa como un conjunto de notificaciones. Notificaciones permiten a los desarrolladores a ser mucho más expresivo en la descripción de una identidad de usuario de las funciones permiten. Mientras que los miembros del rol es simplemente un valor booleano (miembro o un miembro no), una notificación puede incluir información detallada acerca de la identidad y la pertenencia del usuario.
- **Proveedores de inicio de sesión social**

    - Fácilmente, puede agregar inicios de sesión sociales como Account de Microsoft, Facebook, Twitter, Google y otros usuarios a la aplicación y almacenar los datos específicos del usuario en la aplicación.
- **Azure Active Directory**

    - También puede agregar funcionalidad de registro con Azure Active Directory y almacenar los datos específicos del usuario en la aplicación. Para obtener más información, consulte [cuentas organizativas](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) en Creating ASP.NET Web Projects in Visual Studio 2013
- **Integración de OWIN**

    - Autenticación de ASP.NET se basa ahora en middleware OWIN que se puede usar en cualquier host basado en OWIN. Identidad de ASP.NET no tiene ninguna dependencia en System.Web. Es un marco OWIN totalmente compatible y puede usarse en cualquier aplicación OWIN hospedado.
    - Identidad de ASP.NET usa la autenticación de OWIN para registro-en/registro-fuera de los usuarios en el sitio web. Esto significa que, en lugar de usar FormsAuthentication para generar la cookie, la aplicación usa CookieAuthentication de OWIN para hacerlo.
- **Paquete de NuGet**

    - ASP.NET Identity se redistribuye como un paquete de NuGet que se instala en las plantillas de ASP.NET MVC, formularios Web Forms y API Web que se suministran con Visual Studio 2013. Puede descargar este paquete de NuGet en la Galería de NuGet.
    - Liberar ASP.NET Identity como un NuGet paquete resulta más fácil para el equipo ASP.NET iterar sobre nuevas características y correcciones de errores y ofrecer a los desarrolladores de una manera ágil.

## <a name="getting-started-with-aspnet-identity"></a>Introducción a ASP.NET Identity

ASP.NET Identity se utiliza en las plantillas de proyecto de Visual Studio 2013 para ASP.NET MVC, formularios Web Forms, Web API y SPA. En este tutorial, se ilustran cómo las plantillas de proyecto usan identidad de ASP.NET para agregar funcionalidad a registrar, inicie sesión e inicie sesión un usuario.

Identidad de ASP.NET se implementa mediante el procedimiento siguiente. El propósito de este artículo es para ofrecerle una visión general de alto nivel de ASP.NET Identity; puede seguir paso a paso o simplemente leer los detalles. Para obtener instrucciones detalladas sobre cómo crear aplicaciones con ASP.NET Identity, incluido el uso de la nueva API para agregar usuarios, roles y la información de perfil, consulte la sección pasos siguientes al final de este artículo.

1. Crear una aplicación ASP.NET MVC con cuentas individuales. Puede utilizar ASP.NET Identity en ASP.NET MVC, formularios Web Forms, Web API, etcetera de SignalR. En este artículo se iniciará con una aplicación ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. El proyecto creado contiene los siguientes tres paquetes para identidades de ASP.NET.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
 Este paquete incluye la implementación de Entity Framework de identidad de ASP.NET que se conservarán los datos de identidad de ASP.NET y el esquema para SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
 Este paquete tiene las interfaces principales para la identidad de ASP.NET. Este paquete se puede utilizar para escribir una implementación de ASP.NET Identity que persistencia diferentes destinos almacena como almacenamiento de tablas de Azure, NoSQL etcetera las bases de datos.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
 Este paquete contiene la funcionalidad que se usa para conectar la autenticación OWIN con la identidad de ASP.NET en aplicaciones de ASP.NET. Se utiliza cuando se agrega un registro en la funcionalidad a la aplicación y llamar al middleware de autenticación con cookies OWIN para generar una cookie.
3. Crear un usuario.  
 Inicie la aplicación y, a continuación, haga clic en el **registrar** vínculo para crear un usuario. La siguiente imagen muestra la página de registro que recopila el nombre de usuario y la contraseña.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
 Cuando el usuario hace clic en el **registrar** botón, el `Register` de controlador de la cuenta se crea el usuario mediante una llamada a la API de identidad de ASP.NET, como se indica a continuación:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Inicia sesión.  
 Si el usuario se creó correctamente, registra el `SignInAsync` método.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample2.cs?highlight=12)]

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample3.cs?highlight=5-6)]

 El código que aparece resaltado por encima de la `SignInAsync` método genera una [ClaimsIdentity](https://msdn.microsoft.com/en-us/library/system.security.claims.claimsidentity.aspx). Puesto que ASP.NET Identity y autenticación con cookies OWIN son sistema basado en notificaciones, el marco de trabajo requiere la aplicación para generar un valor de ClaimsIdentity para el usuario. Valor de ClaimsIdentity incluye información sobre todas las notificaciones para el usuario, como los roles que pertenece el usuario. También puede agregar más notificaciones para el usuario en esta fase.  
  
 El código que aparece resaltado a continuación en el `SignInAsync` método inicia sesión el usuario mediante el uso de la clase AuthenticationManager de OWIN y llamar al método `SignIn` y pasar el valor de ClaimsIdentity.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample4.cs?highlight=8-11)]
5. Cierre la sesión.  
 Al hacer clic en el **cerrar** vínculo llama a la acción de cierre de sesión en el controlador de la cuenta. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

 El resaltado de código anterior muestra OWIN `AuthenticationManager.SignOut` método. Esto es análogo a [FormsAuthentication.SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx) método utilizado por el [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) módulo en formularios Web Forms.

## <a name="components-of-aspnet-identity"></a>Componentes de identidad de ASP.NET

El diagrama siguiente muestra los componentes del sistema ASP.NET Identity (haga clic en [esto](introduction-to-aspnet-identity/_static/image3.png) o en el diagrama para aumentarlo). Los paquetes en verde constituyen el sistema de identidades de ASP.NET. Todos los paquetes son dependencias que son necesarios cuando se usa el sistema de identidades de ASP.NET en aplicaciones de ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

La siguiente es una breve descripción de los paquetes de NuGet que no se ha mencionado anteriormente:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Autenticación, similar a ASP basada en el middleware que permite a una aplicación usar la cookie. Autenticación de formularios de NET.
- [Entity Framework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework es la tecnología de acceso a datos recomendado de Microsoft para bases de datos relacionales.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migración de pertenencia a ASP.NET Identity

Esperamos que pronto se proporcionan instrucciones sobre cómo migrar las aplicaciones existentes que usan la pertenencia a ASP.NET o pertenencia sencillo para el nuevo sistema de identidades de ASP.NET.

## <a name="next-steps"></a>Pasos siguientes

- [Crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 El tutorial utiliza la API de identidad de ASP.NET para agregar información de perfil para la base de datos de usuario y cómo autenticar con Google y Facebook.
- [Crear una aplicación de MVC de ASP.NET con la autenticación y la base de datos SQL e implementar al servicio de aplicaciones de Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 Este tutorial muestra cómo utilizar la API de identidad para agregar usuarios y roles.
- [Cuentas de usuario individuales](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#indauth) en la creación de proyectos Web ASP.NET en Visual Studio 2013
- [Las cuentas organizativas](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauth) en la creación de proyectos Web ASP.NET en Visual Studio 2013
- [Personalizar la información de perfil de ASP.NET Identity en las plantillas de VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx)
- [Obtener más información de proveedores sociales utilizados en las plantillas de proyecto de VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/get-more-information-from-social-providers-used-in-the-vs-2013-project-templates.aspx)
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Aplicación de ejemplo que muestra cómo agregar roles básicos y soporte técnico de usuario y cómo llevar a cabo la administración de usuarios y roles.
