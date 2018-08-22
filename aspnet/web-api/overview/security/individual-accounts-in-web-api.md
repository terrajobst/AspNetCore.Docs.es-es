---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Proteger una API Web con cuentas individuales e inicio de sesión Local en ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: En este tema se muestra cómo proteger una API web mediante OAuth2 para autenticarse en una base de datos de pertenencia. Versiones de software que se usa en el tutorial 201 Studio Visual...
ms.author: riande
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 01d117260ef458453bee79285a37a8977221998c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838463"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Proteger una API Web con cuentas individuales e inicio de sesión Local en ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue la aplicación de ejemplo](https://github.com/MikeWasson/LocalAccountsApp)

> En este tema se muestra cómo proteger una API web mediante OAuth2 para autenticarse en una base de datos de pertenencia.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


En Visual Studio 2013, la plantilla de proyecto Web API ofrece tres opciones para la autenticación:

- **Cuentas individuales.** La aplicación usa una base de datos de pertenencia.
- **Cuentas organizativas.** Los usuarios iniciar sesión con su Azure Active Directory, Office 365 o credenciales de Active Directory local.
- **Autenticación de Windows.** Esta opción está diseñada para aplicaciones de Intranet y usa el módulo de IIS de la autenticación de Windows.

Para obtener más información acerca de estas opciones, consulte [Creating ASP.NET Web Projects in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Las cuentas individuales proporcionan dos formas para que un usuario inicie sesión:

- **Inicio de sesión local**. El usuario se registra en el sitio, escriba un nombre de usuario y una contraseña. La aplicación almacena el hash de contraseña en la base de datos de pertenencia. Cuando el usuario inicia sesión, el sistema ASP.NET Identity comprueba la contraseña.
- **Inicio de sesión social**. El usuario inicia sesión con un servicio externo, como Facebook, Microsoft o Google. La aplicación crea una entrada para el usuario en la base de datos de pertenencia todavía, pero no almacena las credenciales. El usuario se autentica, inicie sesión en el servicio externo.

Este artículo examina el escenario de inicio de sesión local. Inicio de sesión local y redes sociales, Web API usa OAuth2 para autenticar las solicitudes. Sin embargo, los flujos de credenciales son diferentes para el inicio de sesión local y redes sociales.

En este artículo, mostraré una aplicación sencilla que permite al usuario iniciar sesión y enviar las llamadas AJAX autenticadas a una API web. Puede descargar el código de ejemplo [aquí](https://github.com/MikeWasson/LocalAccountsApp). El archivo Léame describe cómo crear el ejemplo desde cero en Visual Studio.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

La aplicación de ejemplo usa Knockout.js para enlace de datos y jQuery para enviar las solicitudes AJAX. Me centraré en las llamadas AJAX, por lo que no necesita saber Knockout.js para este artículo.

En el camino, describiré:

- ¿Qué está haciendo la aplicación en el lado cliente.
- Lo que sucede en el servidor.
- El tráfico HTTP en el medio.

En primer lugar, necesitamos definir cierta terminología de OAuth2.

- *Recurso*. Parte de los datos que se pueden proteger.
- *Servidor de recursos*. El servidor que hospeda el recurso.
- *Propietario del recurso*. La entidad que puede conceder permiso para acceder a un recurso. (Normalmente, el usuario.)
- *Cliente*: la aplicación que desea tener acceso al recurso. En este artículo, el cliente es un explorador web.
- *Token de acceso*. Un token que concede acceso a un recurso.
- *Token de portador*. Un tipo determinado de token de acceso, con la propiedad que cualquier persona puede usar el token. En otras palabras, un cliente no necesita una clave criptográfica u otro secreto para usar un token de portador. Por ese motivo, los tokens de portador solo deben usarse a través de un HTTPS y deben tener unos tiempos de expiración relativamente corto.
- *Servidor de autorización*. Un servidor que proporciona tokens de acceso.

Una aplicación puede actuar como servidor de autorización y de servidor de recursos. La plantilla de proyecto Web API sigue este patrón.

## <a name="local-login-credential-flow"></a>Flujo de credenciales de inicio de sesión local

Inicio de sesión local, API Web usa el [flujo contraseña del propietario del recurso](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) definidas en OAuth2.

1. El usuario escribe un nombre y una contraseña en el cliente.
2. El cliente envía estas credenciales al servidor de autorización.
3. El servidor de autorización autentica las credenciales y devuelve un token de acceso.
4. Para obtener acceso a un recurso protegido, el cliente incluye el token de acceso en el encabezado de autorización de la solicitud HTTP.

![](individual-accounts-in-web-api/_static/image3.png)

Al seleccionar **cuentas individuales** en la plantilla de proyecto Web API, el proyecto incluye un servidor de autorización que valida las credenciales de usuario y emite tokens. El siguiente diagrama muestra el mismo flujo de credenciales en términos de componentes de la API Web.

![](individual-accounts-in-web-api/_static/image4.png)

En este escenario, los controladores Web API actúan como servidores de recursos. Un filtro de autenticación valida los tokens de acceso y el **[Authorize]** atributo se utiliza para proteger un recurso. Cuando una acción o controlador tiene el **[Authorize]** de atributo, todas las solicitudes a ese controlador o acción que se debe autenticar. En caso contrario, se deniega la autorización y API Web devuelve un error 401 (no autorizado).

El servidor de autorización y el filtro de autenticación ambos llaman a un [OWIN middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) componente que controla los detalles de OAuth2. Describiré el diseño con más detalle más adelante en este tutorial.

## <a name="sending-an-unauthorized-request"></a>Enviar una solicitud no autorizada

Para empezar, ejecute la aplicación y haga clic en el **llamar a API** botón. Cuando se completa la solicitud, debería ver un mensaje de error en la **resultado** cuadro. Eso es porque la solicitud no contiene un token de acceso, por lo que la solicitud está autorizada.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

El **llamar a API** botón envía una solicitud AJAX ~/api/valores, que invoca una acción de controlador Web API. Esta es la sección de código JavaScript que envía la solicitud de AJAX. En la aplicación de ejemplo, todo el código de aplicación de JavaScript se encuentra en el archivo Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Hasta que el usuario inicia sesión, no hay ningún token de portador y, por tanto, ningún encabezado de autorización en la solicitud. Esto hace que la solicitud para devolver un error 401.

Aquí está la solicitud HTTP. (He usado [Fiddler](http://www.telerik.com/fiddler) para capturar el tráfico HTTP.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Tenga en cuenta que la respuesta incluye un encabezado Www-Authenticate con el desafío de portador. Valor que indica que el servidor espera que un token de portador.

## <a name="register-a-user"></a>Registrar un usuario

En el **registrar** sección de la aplicación, escriba un correo electrónico y contraseña y haga clic en el **registrar** botón.

No es necesario usar una dirección de correo electrónico válida para este ejemplo, pero una aplicación real podría confirmar la dirección. (Consulte [crear una aplicación web de ASP.NET MVC 5 segura con inicio de sesión, restablecimiento de confirmación y la contraseña de correo electrónico](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) La contraseña, use algo parecido a "Password1!", con una letra mayúscula, letra minúscula, número y los caracteres no alfanuméricos. Para simplificar la aplicación, dejé la validación del lado cliente, por lo que si hay un problema con el formato de la contraseña, obtendrá un error 400 (solicitud incorrecta).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

El **registrar** botón envía una solicitud POST a ~/api/Account/Register /. El cuerpo de solicitud es un objeto JSON que contiene el nombre y la contraseña. Este es el código de JavaScript que envía la solicitud:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Esta solicitud se controla mediante la `AccountController` clase. Internamente, `AccountController` ASP.NET Identity se utiliza para administrar la base de datos de pertenencia.

Si ejecuta localmente la aplicación desde Visual Studio, las cuentas de usuario se almacenan en LocalDB, en la tabla AspNetUsers. Para ver las tablas en Visual Studio, haga clic en el **vista** menú, seleccione **Explorador de servidores**, a continuación, expanda **conexiones de datos**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Obtener un acceso Token

Hasta ahora no hemos hecho cualquier OAuth, pero ahora veremos el servidor de autorización de OAuth en acción, cuando se solicite un token de acceso. En el **sesión** área de la aplicación de ejemplo, escriba el correo electrónico y la contraseña y haga clic en **en el registro**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

El **sesión** botón envía una solicitud al extremo de token. El cuerpo de la solicitud contiene los datos codificados de dirección url de formulario siguientes:

- conceder\_tipo: "password"
- nombre de usuario: &lt;correo electrónico del usuario&gt;
- Contraseña: &lt;contraseña&gt;

Este es el código de JavaScript que envía la solicitud de AJAX:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Si la solicitud se realiza correctamente, el servidor de autorización devuelve un token de acceso en el cuerpo de respuesta. Tenga en cuenta que almacenamos el token en el almacenamiento de sesión, para usarla más adelante al enviar solicitudes a la API. A diferencia de algunas formas de autenticación (como la autenticación basada en cookies), el explorador no incluirá automáticamente el token de acceso en las solicitudes posteriores. La aplicación deberá hacerlo explícitamente. Que es algo bueno, porque limita [vulnerabilidades CSRF](preventing-cross-site-request-forgery-csrf-attacks.md).

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Puede ver que la solicitud contiene las credenciales del usuario. Le *debe* usar HTTPS para proporcionar seguridad de la capa de transporte.

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Para mejorar la legibilidad, aplica sangría a JSON y se trunca el token de acceso, que es bastante largo.

El `access_token`, `token_type`, y `expires_in` propiedades se definen mediante la especificación OAuth2. Las demás propiedades (`userName`, `.issued`, y `.expires`) son solo para fines informativos. Puede encontrar el código que agrega las propiedades adicionales en el `TokenEndpoint` método, en el archivo /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Enviar una solicitud autenticada

Ahora que tenemos un token de portador, nos podemos realizar una solicitud autenticada a la API. Esto se hace estableciendo el encabezado de autorización en la solicitud. Haga clic en el **llamar a API** botón nuevo para verlo.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

Solicitud HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

Respuesta HTTP:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Cerrar sesión

Dado que el explorador no almacena en caché las credenciales o el token de acceso, el cierre de sesión es simplemente una cuestión de "olvidar" el token, quitándolo de almacenamiento de la sesión:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Descripción de la plantilla de proyecto cuentas individuales

Al seleccionar **cuentas individuales** en la plantilla de proyecto aplicación Web ASP.NET, el proyecto incluye:

- Un servidor de autorización de OAuth2.
- Un punto de conexión de API Web para administrar las cuentas de usuario
- Un modelo de EF para almacenar las cuentas de usuario.

Estas son las clases de aplicación principal que implementan estas características:

- `AccountController`. Proporciona un punto de conexión de API Web para administrar las cuentas de usuario. El `Register` acción es la única que hemos usado en este tutorial. Otros métodos en la clase admiten el restablecimiento de contraseña, inicios de sesión sociales y otras funciones.
- `ApplicationUser`, definido en /Models/IdentityModels.cs. Esta clase es el modelo EF para cuentas de usuario de la base de datos de pertenencia.
- `ApplicationUserManager`, definido en /App\_Start/IdentityConfig.cs esta clase se deriva de [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) y realiza operaciones en cuentas de usuario, como la creación de un nuevo usuario, comprobar las contraseñas y así sucesivamente y conserva automáticamente cambios en la base de datos.
- `ApplicationOAuthProvider`. Este objeto se inserta en el middleware de OWIN y procesa los eventos generados por el middleware. Se deriva de [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Configuración del servidor de autorización

En StartupAuth.cs, el código siguiente configura el servidor de autorización de OAuth2.

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

El `TokenEndpointPath` propiedad es la ruta de acceso de dirección URL para el punto de conexión del servidor de autorización. Que es la dirección URL que la aplicación se usa para obtener los tokens de portador.

El `Provider` propiedad especifica un proveedor que se conecta al middleware de OWIN y procesa los eventos generados por el middleware.

Este es el flujo básico cuando la aplicación desea obtener un token:

1. Para obtener un token de acceso, la aplicación envía una solicitud a ~ / Token.
2. Las llamadas de middleware OAuth `GrantResourceOwnerCredentials` en el proveedor.
3. El proveedor llame a la `ApplicationUserManager` para validar las credenciales y crear una identidad de notificaciones.
4. Si se realiza correctamente, el proveedor crea un vale de autenticación, que se usa para generar el token.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

El middleware de OAuth no sabe nada acerca de las cuentas de usuario. El proveedor se comunica entre el software intermedio y la identidad de ASP.NET. Para obtener más información acerca de cómo implementar el servidor de autorización, consulte [servidor de autorización OAuth 2.0 de OWIN](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Configuración de Web API para usar los Tokens de portador

En el `WebApiConfig.Register` método, el código siguiente establece la autenticación de la canalización de Web API:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

El **HostAuthenticationFilter** clase habilita la autenticación con tokens de portador.

El **SuppressDefaultHostAuthentication** método indica a API Web ignorar toda autenticación que se produce antes de la solicitud llega a la canalización de API Web, IIS o middleware de OWIN. De este modo, nos podemos restringir la API Web para autenticar solo con tokens de portador.

> [!NOTE]
> En concreto, la parte MVC de la aplicación puede usar la autenticación de formularios, que almacena las credenciales en una cookie. Autenticación basada en cookies requiere el uso de tokens antifalsificación, para evitar los ataques CSRF. Que es un problema para la API web, porque no hay ninguna manera cómoda de enviar el token antifalsificación al cliente para la API web. (Para obtener más información sobre este problema, consulte [evitar los ataques de CSRF en Web API](preventing-cross-site-request-forgery-csrf-attacks.md).) Una llamada a **SuppressDefaultHostAuthentication** garantiza que la API Web no es vulnerable a ataques CSRF de credenciales almacenadas en las cookies.


Cuando el cliente solicita un recurso protegido, aquí es lo que sucede en la canalización de Web API:

1. El **HostAuthentication** filtro llama al middleware de OAuth para validar el token.
2. El middleware convierte el token en una identidad de notificaciones.
3. En este momento, la solicitud es *autenticado* pero no *autorizado*.
4. El filtro de autorización examina la identidad de notificaciones. Si las notificaciones autorización al usuario para ese recurso, la solicitud está autorizada. De forma predeterminada, el **[Authorize]** atributo autorizará cualquier solicitud que se han autenticado. Sin embargo, puede autorizar por rol o por otras notificaciones. Para obtener más información, consulte [autenticación y autorización en Web API](authentication-and-authorization-in-aspnet-web-api.md).
5. Si los pasos anteriores se realizan correctamente, el controlador devuelve el recurso protegido. En caso contrario, el cliente recibe un error 401 (no autorizado).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Recursos adicionales

- [ASP.NET Identity](../../../identity/index.md)
- [Descripción de características de seguridad de la plantilla SPA de RC de VS2013](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). Entrada de blog de MSDN por Hongye Sun.
- [Si examinamos la persona de la API Web de cuentas de plantilla, parte 2: cuentas locales](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Blog de Dominick Baier.
- [Hospedar Web API con OWIN y autenticación](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Una buena explicación de `SuppressDefaultHostAuthentication` y `HostAuthenticationFilter` por Brock Allen.
- [Personalizar la información de perfil en ASP.NET Identity en las plantillas de VS 2013](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). Entrada de blog MSDN de Pranav Rastogi.
- [Por la administración de la duración de solicitud para la clase UserManager en ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). Entrada de blog MSDN por Suhas Joshi, con una buena explicación de la `UserManager` clase.
