---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Proteger una API Web con cuentas individuales y el inicio de sesión Local en ASP.NET Web API 2.2 | Documentos de Microsoft
author: MikeWasson
description: Este tema muestra cómo proteger una API web mediante OAuth2 para autenticarse con una base de datos de pertenencia. Versiones de software que se usa en el tutorial 201 Studio Visual...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e2056e769edf972cba830b31cf37f6418148ca73
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Proteger una API Web con cuentas individuales y el inicio de sesión Local en ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar la aplicación de ejemplo](https://github.com/MikeWasson/LocalAccountsApp)

> Este tema muestra cómo proteger una API web mediante OAuth2 para autenticarse con una base de datos de pertenencia.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [2.2 API Web](../releases/whats-new-in-aspnet-web-api-22.md)
> - [Identidad de ASP.NET 2.1](../../../identity/index.md)


En Visual Studio 2013, la plantilla de proyecto de API Web le ofrece tres opciones para la autenticación:

- **Cuentas individuales.** La aplicación utiliza una base de datos de pertenencia.
- **Cuentas organizativas.** Los usuarios iniciar sesión con sus credenciales de Active Directory local, Office 365 o Azure Active Directory.
- **Autenticación de Windows.** Esta opción está diseñada para las aplicaciones de Intranet y usa el módulo de IIS de la autenticación de Windows.

Para obtener más información acerca de estas opciones, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Las cuentas individuales proporcionan dos formas para que un usuario inicie sesión:

- **Inicio de sesión local**. El usuario se registra en el sitio, escriba un nombre de usuario y una contraseña. La aplicación almacena el hash de contraseña en la base de datos de pertenencia. Cuando el usuario inicia sesión, el sistema de identidades de ASP.NET comprueba la contraseña.
- **Inicio de sesión social**. El usuario inicia sesión con un servicio externo, como Facebook, Microsoft o Google. La aplicación todavía crea una entrada para el usuario en la base de datos de suscripción, pero no almacena las credenciales. El usuario se autentica al iniciar sesión en el servicio externo.

Este artículo examina el escenario de inicio de sesión local. Inicio de sesión local y redes sociales, API Web use OAuth2 para autenticar las solicitudes. Sin embargo, los flujos de credenciales son diferentes para inicio de sesión local y sociales.

En este artículo, mostraré una aplicación sencilla que permite al usuario iniciar sesión y enviar llamadas autenticadas de AJAX en una API web. Puede descargar el código de ejemplo [aquí](https://github.com/MikeWasson/LocalAccountsApp). El documento Léame describe cómo crear el ejemplo desde el principio en Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

La aplicación de ejemplo usa Knockout.js para enlace de datos y jQuery para enviar solicitudes de AJAX. Podrá se centre primero en las llamadas de AJAX, por lo que no necesita saber Knockout.js para este artículo.

Al hacerlo, describirán:

- ¿Qué está haciendo la aplicación en el lado del cliente.
- ¿Qué sucede en el servidor.
- El tráfico HTTP en el centro.

En primer lugar, es necesario definir terminología OAuth2.

- *Recursos*. Una parte de datos que se pueden proteger.
- *Servidor de recursos*. El servidor que hospeda el recurso.
- *Propietario del recurso*. La entidad que puede conceder permiso para tener acceso a un recurso. (Normalmente, el usuario.)
- *Cliente*: la aplicación que desea tener acceso al recurso. En este artículo, el cliente es un explorador web.
- *Token de acceso*. Un token que concede acceso a un recurso.
- *Token de portador*. Un tipo concreto de token de acceso, con la propiedad que cualquier persona puede usar el token. En otras palabras, un cliente no tiene una clave criptográfica u otro secreto para usar un token de portador. Por esa razón, tokens de portador solo deben usarse a través de un HTTPS y deben tener unos tiempos de expiración relativamente corto.
- *Servidor de autorización*. Un servidor que proporciona tokens de acceso.

Una aplicación puede actuar como servidor de autorización y el servidor de recursos. La plantilla de proyecto de Web API sigue este patrón.

## <a name="local-login-credential-flow"></a>Flujo de credenciales de inicio de sesión local

Inicio de sesión local, se utiliza la API de Web la [flujo de contraseña de propietario de recurso](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definido en OAuth2.

1. El usuario escribe un nombre y una contraseña en el cliente.
2. El cliente envía estas credenciales al servidor de autorización.
3. El servidor de autorización autentica las credenciales y devuelve un token de acceso.
4. Para obtener acceso a un recurso protegido, el cliente incluye el token de acceso en el encabezado de autorización de la solicitud HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Cuando se selecciona **cuentas individuales** en la plantilla de proyecto Web API, el proyecto incluye un servidor de autorización que valida las credenciales de usuario y emite tokens. El siguiente diagrama muestra el mismo flujo de credenciales en cuanto a los componentes de la API de Web.

![](individual-accounts-in-web-api/_static/image4.png)

En este escenario, los controladores de API Web actúan como servidores de recursos. Un filtro de autenticación valida los tokens de acceso y el **[Authorize]** atributo se usa para proteger un recurso. Cuando una acción o controlador tiene la **[Authorize]** de atributo, todas las solicitudes a ese controlador o acción debe ser autenticada. En caso contrario, se deniega la autorización y API Web devuelve un error 401 (no autorizado).

El servidor de autorización y el filtro de autenticación ambas llaman en un [middleware de OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) componente que controla los detalles de OAuth2. Voy a describir el diseño con más detalle más adelante en este tutorial.

## <a name="sending-an-unauthorized-request"></a>Enviar una solicitud no autorizada

Para empezar, ejecutar la aplicación y haga clic en el **llamadas API** botón. Cuando se completa la solicitud, verá un mensaje de error en la **resultado** cuadro. Eso es porque la solicitud no contiene un token de acceso, por lo que la solicitud no está autorizada.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

El **llamadas API** botón envía una solicitud AJAX ~/api/valores, que invoca una acción de controlador Web API. Esta es la sección de código de JavaScript que envía la solicitud de AJAX. En la aplicación de ejemplo, todo el código de aplicación de JavaScript se encuentra en el archivo Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Hasta que el usuario inicia sesión, no hay ningún token de portador y, por tanto, ningún encabezado de autorización en la solicitud. Esto hace que la solicitud para devolver un error 401.

Esta es la solicitud HTTP. (He usado [Fiddler](http://www.telerik.com/fiddler) para capturar el tráfico HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Tenga en cuenta que la respuesta incluye un encabezado Www-Authenticate con el desafío de portador. Valor que indica que el servidor espera un token de portador.

## <a name="register-a-user"></a>Registrar un usuario

En el **registrar** sección de la aplicación, escriba un correo electrónico y la contraseña y haga clic en el **registrar** botón.

No es necesario usar una dirección de correo electrónico válida para este ejemplo, pero una aplicación real confirme la dirección. (Consulte [crear una aplicación web de ASP.NET MVC 5 segura con registro de restablecimiento de contraseña y de confirmación de correo electrónico](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Para la contraseña, usar algo parecido a "Password1!", con una letra mayúscula, letra en minúscula, número y caracteres no alfanuméricos. Para simplificar la aplicación, dejan fuera la validación del lado cliente, por lo que si hay un problema con el formato de la contraseña, obtendrá un error 400 (solicitud incorrecta).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

El **registrar** botón envía una solicitud POST a ~/api/Account/Register /. El cuerpo de solicitud es un objeto JSON que contiene el nombre y la contraseña. Este es el código de JavaScript que envía la solicitud:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Esta solicitud se controla mediante la `AccountController` clase. Internamente, `AccountController` utiliza la identidad de ASP.NET para administrar la base de datos de pertenencia.

Si ejecuta la aplicación localmente desde Visual Studio, las cuentas de usuario se almacenan en LocalDB, en la tabla AspNetUsers. Para ver las tablas en Visual Studio, haga clic en el **vista** menú, seleccione **Explorador de servidores**, a continuación, expanda **las conexiones de datos**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Obtener un acceso Token

Hasta ahora no hemos hecho cualquier OAuth, pero ahora veremos el servidor de autorización de OAuth en acción, cuando se solicite un token de acceso. En el **inicio de sesión** área no cliente de la aplicación de ejemplo, escriba el correo electrónico y la contraseña y haga clic en **inicio de sesión**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

El **inicio de sesión** botón envía una solicitud al extremo de token. El cuerpo de la solicitud contiene los datos codificados de dirección url de formulario siguientes:

- conceder\_tipo: "password"
- nombre de usuario: &lt;correo electrónico del usuario&gt;
- contraseña: &lt;contraseña&gt;

Este es el código de JavaScript que envía la solicitud de AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Si la solicitud se realiza correctamente, el servidor de autorización devuelve un token de acceso en el cuerpo de respuesta. Tenga en cuenta que el token se almacena en almacenamiento de la sesión, para usarlo más adelante al enviar solicitudes a la API. A diferencia de algunas formas de autenticación (por ejemplo, autenticación basada en cookies), el explorador no incluirá automáticamente el token de acceso en las solicitudes subsiguientes. La aplicación deberá hacerlo explícitamente. Que es bueno, ya que se limitan [vulnerabilidades CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Puede ver que la solicitud contiene las credenciales del usuario. Se *debe* usar HTTPS para proporcionar seguridad de la capa de transporte.

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Para mejorar la legibilidad, aplica sangría a JSON y se trunca el token de acceso, que es bastante largo.

El `access_token`, `token_type`, y `expires_in` propiedades se definen mediante la especificación OAuth2. Las demás propiedades (`userName`, `.issued`, y `.expires`) son solo con fines informativos. Puede encontrar el código que agrega las propiedades adicionales en el `TokenEndpoint` método, en el archivo /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Enviar una solicitud autenticada

Ahora que tenemos un token de portador, podemos hacer que una solicitud autenticada a la API. Esto se hace estableciendo el encabezado de autorización en la solicitud. Haga clic en el **llamadas API** botón nuevo para verlo.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Cerrar sesión

Dado que el explorador no almacena en caché las credenciales o el token de acceso, cerrar la sesión es simplemente cuestión de "olvidar" el token, mediante la eliminación de almacenamiento de la sesión:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Descripción de la plantilla de proyecto de cuentas individuales

Cuando se selecciona **cuentas individuales** en la plantilla de proyecto aplicación Web ASP.NET, el proyecto incluye:

- Un servidor de autorización OAuth2.
- Un extremo de API Web para administrar las cuentas de usuario
- Un modelo EF para almacenar las cuentas de usuario.

Estas son las clases de aplicación principal que implementan estas características:

- `AccountController`. Proporciona un punto de conexión de API Web para administrar las cuentas de usuario. El `Register` acción es la única que hemos usado en este tutorial. Otros métodos de la clase admiten el restablecimiento de contraseña, los inicios de sesión sociales y otras funciones.
- `ApplicationUser`, definido en /Models/IdentityModels.cs. Esta clase es el modelo EF para cuentas de usuario en la base de datos de pertenencia.
- `ApplicationUserManager`, definido en/App\_Start/IdentityConfig.cs esta clase se deriva de [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) y realiza operaciones en las cuentas de usuario, como la creación de un nuevo usuario, comprobar las contraseñas y así sucesivamente y conserva automáticamente cambios en la base de datos.
- `ApplicationOAuthProvider`. Este objeto se conecta al middleware de OWIN y procesa los eventos generados por el middleware. Deriva de [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configurar el servidor de autorización

En StartupAuth.cs, el siguiente código configura el servidor de autorización de OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

El `TokenEndpointPath` propiedad es la ruta de acceso de dirección URL para el extremo del servidor de autorización. Que es la dirección URL que la aplicación usa para obtener los tokens de portador.

El `Provider` propiedad especifica un proveedor que se conecta el middleware de OWIN y procesa los eventos generados por el middleware.

Este es el flujo básico cuando la aplicación desea obtener un token:

1. Para obtener un token de acceso, la aplicación envía una solicitud para ~ / símbolo (token).
2. Las llamadas de middleware de OAuth `GrantResourceOwnerCredentials` en el proveedor.
3. El proveedor llama el `ApplicationUserManager` para validar las credenciales y crear una identidad de notificaciones.
4. Si se realiza correctamente, el proveedor crea un vale de autenticación, que se usa para generar el token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

El middleware de OAuth no sabe nada acerca de las cuentas de usuario. El proveedor se comunica entre el software intermedio y la identidad de ASP.NET. Para obtener más información acerca de cómo implementar el servidor de autorización, consulte [servidor de autorización de OWIN OAuth 2.0](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configuración de API Web use Tokens portadores

En el `WebApiConfig.Register` método, el código siguiente establece la autenticación para la canalización Web API:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

El **HostAuthenticationFilter** clase habilita la autenticación mediante tokens de portador.

El **SuppressDefaultHostAuthentication** método indica a API Web para pasar por alto cualquier autenticación que se produce antes de la solicitud llega a la canalización de API Web, IIS o middleware de OWIN. De este modo, se puede restringir API Web para autenticar únicamente con tokens de portador.

> [!NOTE]
> En concreto, la parte MVC de la aplicación puede usar autenticación de formularios, que almacena las credenciales en una cookie. Autenticación basada en cookies requiere el uso de tokens antifalsificación, para evitar los ataques CSRF. Que es un problema para las API, web porque no hay ninguna manera cómoda para la API web para enviar el token antifalsificación al cliente. (Para obtener más información sobre este problema, consulte [evitar los ataques de CSRF en Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Al llamar a **SuppressDefaultHostAuthentication** garantiza que las API de Web no es vulnerable a ataques CSRF de credenciales almacenadas en las cookies.


Cuando el cliente solicita un recurso protegido, sucede lo siguiente en la canalización Web API:

1. El **HostAuthentication** filtro llama el middleware de OAuth para validar el token.
2. El middleware convierte el token en una identidad de notificaciones.
3. En este momento, la solicitud es *autenticado* pero no *autorizado*.
4. El filtro de autorización examina la identidad de notificaciones. Si las notificaciones autorizan al usuario para ese recurso, se autoriza la solicitud. De forma predeterminada, el **[Authorize]** atributo autorizará cualquier solicitud que se autentica. Sin embargo, puede autorizar por rol o por el resto de reclamaciones. Para obtener más información, consulte [autenticación y autorización en Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Si los pasos anteriores son correctas, el controlador devuelve el recurso protegido. En caso contrario, el cliente recibe un error 401 (no autorizado).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Recursos adicionales

- [Identidad de ASP.NET](../../../identity/index.md)
- [Introducción a las características de seguridad de la plantilla SPA para VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Entrada de blog MSDN por Hongye Sun.
- [Si examinamos el individuo de API Web cuentas de plantilla, parte 2: cuentas locales](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Blog de Dominick Baier.
- [Host de autenticación y la API de Web con OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Una buena explicación de `SuppressDefaultHostAuthentication` y `HostAuthenticationFilter` por Brock Allen.
- [Personalizar la información de perfil de ASP.NET Identity en las plantillas de VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Entrada de blog MSDN, Pranav Rastogi.
- [Por la administración de la duración de solicitud para la clase UserManager en ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Entrada de blog MSDN, Suhas Joshi, con una buena explicación de la `UserManager` clase.
