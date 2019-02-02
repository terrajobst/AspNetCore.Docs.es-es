---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Servidor de autorización de OAuth 2.0 de OWIN | Microsoft Docs
author: hongyes
description: Este tutorial le guiará para implementar un servidor de autorización de OAuth 2.0 con middleware de OWIN OAuth. Esto es un tutorial avanzado que sólo figuración...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: b8451d2d9e346bd5e2f51ba45e48030a5221b549
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667653"
---
# <a name="owin-oauth-20-authorization-server"></a>Servidor de autorización de OAuth 2.0 de OWIN

> Este tutorial le guiará para implementar un servidor de autorización de OAuth 2.0 con middleware de OWIN OAuth. Se trata de un tutorial avanzado que solo se describe los pasos para crear un servidor de autorización de OWIN OAuth 2.0. Esto no es un tutorial paso a paso. [Descargar el código de ejemplo](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
>
> > [!NOTE]
> > Este esquema no debe ser intencionado que se usará para crear una aplicación de producción seguro. Este tutorial está pensado para proporcionar solo una descripción sobre cómo implementar un servidor de autorización de OAuth 2.0 con middleware de OWIN OAuth.
>
>
> ## <a name="software-versions"></a>Versiones de software
>
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 8.1 | Windows 10, Windows 8, Windows 7 |
> | [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
> | .NET 4.7.2 |  |
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en [proyecto Katana en GitHub](https://github.com/aspnet/AspNetKatana/). Para preguntas y comentarios sobre el tutorial en Sí, consulte la sección de comentarios en la parte inferior de la página.


El [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) permite que una aplicación de terceros obtener acceso limitado a un servicio HTTP. En lugar de usar las credenciales del propietario del recurso para tener acceso a un recurso protegido, el cliente obtiene un token de acceso (que es una cadena que denota un ámbito específico, duración y otros atributos de acceso). Un servidor de autorización emite tokens de acceso a los clientes de terceros con la aprobación del propietario del recurso.

Este tutorial tratará:

- Cómo crear un servidor de autorización para admitir la autorización de cuatro tipos de concesión y tokens de actualización:
    - Concesión de código de autorización
    - Concesión implícita
    - Concesión de credenciales de contraseña de propietario de recursos
    - Concesión de credenciales de cliente
- Creación de un servidor de recursos que se protege mediante un token de acceso.
- Creación de los clientes de OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) como se indica en **versiones de Software** en la parte superior de la página.
- Debe estar familiarizado con OWIN. Consulte [introducción con el proyecto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) y [Novedades de OWIN y Katana](index.md).
- Familiaridad con [OAuth](http://tools.ietf.org/html/rfc6749) terminología, incluyendo [Roles](http://tools.ietf.org/html/rfc6749#section-1.1), [protocolo flujo](http://tools.ietf.org/html/rfc6749#section-1.2), y [concesión de autorización](http://tools.ietf.org/html/rfc6749#section-1.3). [Introducción de OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) proporciona una buena introducción.

## <a name="create-an-authorization-server"></a>Crear un servidor de autorización

En este tutorial, se le aproximadamente boceto de cómo usar [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) y ASP.NET MVC para crear un servidor de autorización. Esperamos proporcionar pronto una descarga para el ejemplo completo, como en este tutorial no incluye cada paso. En primer lugar, cree una aplicación web vacía denominada *AuthorizationServer* e instalar los siguientes paquetes:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (o cualquier otro inicio de sesión social empaquetar como Microsoft.Owin.Security.Facebook)

Agregar un [clase OWIN Startup](owin-startup-class-detection.md) bajo la carpeta raíz del proyecto denominada *inicio*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Crear un *aplicación\_iniciar* carpeta. Seleccione el *aplicación\_iniciar* carpeta y use Alt + Mayús + A para agregar la versión descargada de la *AuthorizationServer\App\_Start\Startup.Auth.cs* archivo.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

El código anterior permite cookies y la autenticación de Google, que se usan por el propio servidor de autorización para administrar las cuentas de inicio de sesión de aplicación/externa.

El `UseOAuthAuthorizationServer` método de extensión consiste en configurar el servidor de autorización. Las opciones de configuración son:

- `AuthorizeEndpointPath`: La ruta de acceso de solicitud que las aplicaciones cliente redirigirá el agente de usuario con el fin de obtener los usuarios de dar su consentimiento para emitir un token o código. Debe comenzar con una barra inicial, por ejemplo, "`/Authorize`".
- `TokenEndpointPath`: Las aplicaciones de cliente de la ruta de acceso de solicitud se comunican directamente para obtener el token de acceso. Debe comenzar con una barra inicial, como "/ Token". Si el cliente ha emitido una [cliente\_secreto](http://tools.ietf.org/html/rfc6749#appendix-A.2), pero se debe proporcionar a este extremo.
- `ApplicationCanDisplayErrors`: Establecido en `true` si desea implementar la aplicación web generar una página de error personalizado para los errores de validación del cliente en `/Authorize` punto de conexión. Esto solo es necesario para los casos donde no se redirige el Explorador de nuevo a la aplicación cliente, por ejemplo, cuando el `client_id` o `redirect_uri` son incorrectas. El `/Authorize` punto de conexión debería ver la "oauth. Error","oauth. ErrorDescription"y"oauth. Propiedades de ErrorUri"se agregan al entorno OWIN.

    > [!NOTE]
    > Si no es true, el servidor de autorización devolverá una página de error predeterminada con los detalles del error.
- `AllowInsecureHttp`: Es True para permitir autorizar y solicitudes de token para llegar de direcciones URI HTTP y permitir la entrada `redirect_uri` autorizar a los parámetros de solicitud tenga direcciones URI de HTTP.

    > [!WARNING]
    > Seguridad: Esto es solamente para desarrollo.
- `Provider`: El objeto proporcionado por la aplicación para procesar los eventos generados por el middleware de servidor de autorización. La aplicación puede implementar la interfaz por completo o puede crear una instancia de `OAuthAuthorizationServerProvider` y asignar delegados necesarios para los flujos de OAuth es compatible con este servidor.
- `AuthorizationCodeProvider`: Genera un código de autorización de uso único para volver a la aplicación cliente. Para que el servidor OAuth pueda proteger la aplicación **debe** proporcionar una instancia de `AuthorizationCodeProvider` donde el token generado por el `OnCreate/OnCreateAsync` eventos se consideran válido para sólo una llamada a `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Genera un token de actualización que se puede usar para generar un nuevo token de acceso cuando sea necesario. Si no se proporciona el servidor de autorización no devolverá los tokens de actualización de la `/Token` punto de conexión.

## <a name="account-management"></a>Administración de cuentas

OAuth no le importa dónde y cómo administrar la información de su cuenta de usuario. Tiene [ASP.NET Identity](../../../identity/index.md) que es responsable de ella. En este tutorial, se simplifica el código de administración de cuenta y sólo tiene que asegurarse de que el usuario puede iniciar sesión con el OWIN middleware de cookie. Este es el código de ejemplo simplificado para el `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` se utiliza para validar al cliente con su dirección URL de redireccionamiento registrado. `ValidateClientAuthentication` comprueba el encabezado de esquema básico y el cuerpo del formulario para obtener las credenciales del cliente.

A continuación se muestra la página de inicio de sesión:

![](owin-oauth-20-authorization-server/_static/image1.png)

Revise el IETF OAuth 2 [concesión de código de autorización](http://tools.ietf.org/html/rfc6749#section-4.1) sección ahora.

**Proveedor** (en la tabla siguiente) es [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Proveedor, que es de tipo `OAuthAuthorizationServerProvider`, que contiene todos los eventos de servidor de OAuth.

| Pasos del flujo de la sección de concesión de código de autorización | Descarga de ejemplo lleva a cabo estos pasos con: |
| --- | --- |
|  |  |
| (A) el cliente inicia el flujo dirigiendo el agente de usuario del propietario del recurso para el extremo de autorización. El cliente incluye su identificador de cliente, el ámbito solicitado, el estado local y un URI de redirección a la que el servidor de autorización enviará al agente de usuario una vez concedido (o deniega el acceso). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) el servidor de autorización autentica al propietario del recurso (mediante el agente de usuario) y establece si el propietario del recurso se concede o deniega la solicitud de acceso del cliente. | **&lt;Si el usuario concede el acceso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) suponiendo que el propietario del recurso conceda el acceso, el servidor de autorización redirige el agente de usuario al cliente utilizando el URI de redirección especificado antes (en la solicitud o durante el registro de cliente). ... |  |
|  |  |
| (D) el cliente solicita un token de acceso de extremo de token del servidor de autorización incluyendo el código de autorización recibido en el paso anterior. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. El cliente incluye el URI de redirección utilizado para obtener el código de autorización para la comprobación. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Una implementación de ejemplo para `AuthorizationCodeProvider.CreateAsync` y `ReceiveAsync` controlar la creación y validación de código de autorización se muestra a continuación.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

El código anterior usa un diccionario simultáneo en memoria para almacenar el vale de código y la identidad y restaurar la identidad después de recibir el código. En una aplicación real, se sustituirá por un almacén de datos persistente. Es el punto de conexión de autorización para que el propietario del recurso conceder acceso al cliente. Por lo general, necesita una interfaz de usuario para permitir que el usuario haga clic en un botón y confirme la concesión. Middleware de OWIN OAuth permite que el código de aplicación controlar el punto de conexión de autorización. En nuestra aplicación de ejemplo, usamos un controlador MVC llama `OAuthController` para controlarla. Esta es la implementación de ejemplo:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

El `Authorize` acción comprobará primero si el usuario ha iniciado sesión el servidor de autorización. Si no, el middleware de autenticación desafíos al llamador para autenticarse con la cookie de "Aplicación" y redirige a la página de inicio de sesión. (Vea el código resaltado anterior). Si el usuario ha iniciado sesión, representará la vista Authorize, tal como se muestra a continuación:

![](owin-oauth-20-authorization-server/_static/image2.png)

Si el **Grant** botón está seleccionado, el `Authorize` acción creará una nueva identidad de "Bearer" e inicie sesión con el. Desencadenará el servidor de autorización para generar un token de portador y enviar al cliente con la carga de JSON.

### <a name="implicit-grant"></a>Concesión implícita

Consulte el IETF OAuth 2 [concesión implícita](http://tools.ietf.org/html/rfc6749#section-4.2) sección ahora.

 El [concesión implícita](http://tools.ietf.org/html/rfc6749#section-4.2) flujo que se muestra en la figura 4 es el flujo y asignación que la OAuth de OWIN middleware sigue.

| Pasos del flujo de la sección de concesión implícita | Descarga de ejemplo lleva a cabo estos pasos con: |
| --- | --- |
|  |  |
| (A) el cliente inicia el flujo dirigiendo el agente de usuario del propietario del recurso para el extremo de autorización. El cliente incluye su identificador de cliente, el ámbito solicitado, el estado local y un URI de redirección a la que el servidor de autorización enviará al agente de usuario una vez concedido (o deniega el acceso). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) el servidor de autorización autentica al propietario del recurso (mediante el agente de usuario) y establece si el propietario del recurso se concede o deniega la solicitud de acceso del cliente. | **&lt;Si el usuario concede el acceso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) suponiendo que el propietario del recurso conceda el acceso, el servidor de autorización redirige el agente de usuario al cliente utilizando el URI de redirección especificado antes (en la solicitud o durante el registro de cliente). ... |  |
|  |  |
| (D) el cliente solicita un token de acceso de extremo de token del servidor de autorización incluyendo el código de autorización recibido en el paso anterior. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. El cliente incluye el URI de redirección utilizado para obtener el código de autorización para la comprobación. |  |

Puesto que ya hemos implementado el extremo de autorización (`OAuthController.Authorize` acción) para la concesión de código de autorización, habilita automáticamente también el flujo implícito. Nota: `Provider.ValidateClientRedirectUri` se utiliza para validar el identificador de cliente con su dirección URL de redirección, que protege el flujo de concesión implícita de enviar el acceso a los clientes tokens a ataques malintencionados ([ataque Man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Concesión de credenciales de contraseña de propietario de recursos

Consulte el IETF OAuth 2 [concesión de credenciales de contraseña de propietario de recursos](http://tools.ietf.org/html/rfc6749#section-4.3) sección ahora.

 El [concesión de credenciales de contraseña de propietario de recursos](http://tools.ietf.org/html/rfc6749#section-4.3) flujo que se muestra en la figura 5 es el flujo y asignación que la OAuth de OWIN middleware sigue.

| Pasos del flujo de la sección de concesión de credenciales de contraseña de propietario de recursos | Descarga de ejemplo lleva a cabo estos pasos con: |
| --- | --- |
|  |  |
| (A) el propietario del recurso proporciona al cliente con su nombre de usuario y contraseña. |  |
|  |  |
| (B) el cliente solicita un token de acceso de extremo de token del servidor de autorización mediante la inclusión de las credenciales recibidas del propietario del recurso. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) el servidor de autorización autentica al cliente y valida las credenciales del propietario de recursos y, si es válido, emite un token de acceso. |  |

Esta es la implementación de ejemplo para `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> El código anterior está diseñado para explicar en esta sección del tutorial y no debe usarse en seguro o aplicaciones de producción. No comprueba las credenciales de los propietarios de recursos. Se supone que cada credencial es válida y cuando crea una nueva identidad. La nueva identidad se utilizará para generar el token de acceso y el token de actualización. Reemplace el código con su propio código de administración de cuenta segura.


### <a name="client-credentials-grant"></a>Concesión de credenciales de cliente

Consulte el IETF OAuth 2 [concesión de credenciales de cliente](http://tools.ietf.org/html/rfc6749#section-4.4) sección ahora.

 El [concesión de credenciales de cliente](http://tools.ietf.org/html/rfc6749#section-4.4) flujo que se muestra en la figura 6 es el flujo y asignación que la OAuth de OWIN middleware sigue.

| Pasos del flujo de la sección de concesión de credenciales de cliente | Descarga de ejemplo lleva a cabo estos pasos con: |
| --- | --- |
|  |  |
| (A) el cliente se autentica con el servidor de autorización y solicita un token de acceso desde el extremo de token. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) el servidor de autorización autentica al cliente y si es válido, emite un token de acceso. |  |

Esta es la implementación de ejemplo para `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> El código anterior está diseñado para explicar en esta sección del tutorial y no debe usarse en seguro o aplicaciones de producción. Reemplace el código con su propio código de administración de cliente segura.


### <a name="refresh-token"></a>Token de actualización

Consulte el IETF OAuth 2 [Token de actualización](http://tools.ietf.org/html/rfc6749#section-1.5) sección ahora.

 El [Token de actualización](http://tools.ietf.org/html/rfc6749#section-1.5) flujo que se muestra en la figura 2 es el flujo y asignación que la OAuth de OWIN middleware sigue.

| Pasos del flujo de la sección de concesión de credenciales de cliente | Descarga de ejemplo lleva a cabo estos pasos con: |
| --- | --- |
|  |  |
| (G) el cliente solicita un nuevo token de acceso mediante la autenticación con el servidor de autorización y presentar el token de actualización. Los requisitos de autenticación de cliente se basan en el tipo de cliente y en las directivas del servidor de autorización. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) en el servidor de autorización autentica al cliente y valida el token de actualización y, si es válido, emite un nuevo token de acceso (y, opcionalmente, un nuevo token de actualización). |  |

Esta es la implementación de ejemplo para `Provider.GrantRefreshToken`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Crear un servidor de recursos que se protege mediante el Token de acceso

Crear un proyecto de aplicación web vacía e instale los paquetes en el proyecto de las siguientes:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Cree una clase de inicio y configurar la autenticación y la API Web. Consulte *AuthorizationServer\ResourceServer\Startup.cs* en la descarga de ejemplo.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Consulte *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* en la descarga de ejemplo.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Consulte *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* en la descarga de ejemplo.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` método permite la CORS para todos los dominios.
- `UseOAuthBearerAuthentication` método habilita el middleware de autenticación de token de portador de OAuth que recibirá y validar el token de portador de encabezado de autorización de la solicitud.
- `Config.SuppressDefaultHostAuthenticaiton` suprime de forma predeterminada una entidad de seguridad autenticada desde la aplicación de host, por lo tanto, todas las solicitudes será anónimas después de esta llamada.
- `HostAuthenticationFilter` habilita la autenticación solo para el tipo de autenticación. En este caso, es el tipo de autenticación de portador.

Para demostrar la identidad autenticada, creamos un controlador ApiController para generar notificaciones del usuario actual.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Si el servidor de autorización y el servidor de recursos no están en el mismo equipo, el middleware de OAuth usará las claves de equipo diferente para cifrar y descifrar el token de acceso de portador. Para compartir la misma clave privada entre ambos proyectos, agregamos el mismo `machinekey` en ambos *web.config* archivos.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Crear a clientes OAuth 2.0

 Usamos el [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) paquete NuGet para simplificar el código de cliente.

### <a name="authorization-code-grant-client"></a>Cliente de concesión de código de autorización

 Este cliente es una aplicación de MVC. Desencadena un flujo de concesión de código de autorización para obtener el token el acceso desde el back-end. Tiene una sola página tal como se muestra a continuación:

![](owin-oauth-20-authorization-server/_static/image3.png)

- El **Authorize** botón redirigirá el explorador al servidor de autorización para notificar el propietario del recurso para conceder acceso a este cliente.
- El **actualizar** botón obtendrá un nuevo token de acceso y el token de actualización mediante la actualización actual token.
- El **API de acceso protegido recursos** botón llamará el servidor de recursos para obtener los datos de las notificaciones del usuario actual y mostrarlos en la página.

Este es el código de ejemplo de la `HomeController` del cliente.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` requiere SSL de forma predeterminada. Dado que nuestra demostración usa HTTP, deberá agregar después de la configuración en el archivo de configuración:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Security - nunca deshabilitar SSL en una aplicación de producción. Ahora las credenciales de inicio de sesión se envían en texto sin cifrar a través de la conexión. El código anterior es solo para la depuración de ejemplo local y la exploración.


### <a name="implicit-grant-client"></a>Cliente de concesión implícita

Este cliente está usando JavaScript para:

1. Abra una nueva ventana y redirija al punto de conexión de autorización del servidor de autorización.
2. Obtener el token de acceso de fragmentos de dirección URL cuando se redirige de nuevo.

La imagen siguiente muestra este proceso:

![](owin-oauth-20-authorization-server/_static/image4.png)

El cliente debe tener dos páginas: uno para la página principal y otro para la devolución de llamada. Este es el ejemplo de JavaScript de código se encuentra en la *Index.cshtml* archivo:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Este es el código de control de devolución de llamada *SignIn.cshtml* archivo:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Una práctica recomendada es mover el código JavaScript a un archivo externo y no se incrusta con el marcado de Razor. Para simplificar este ejemplo, se han combinado.


### <a name="resource-owner-password-credentials-grant-client"></a>Concesión de cliente de credenciales de contraseña de propietario de recursos

Se usa una aplicación de consola para este cliente de demostración. Este es el código:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Concesión de credenciales de cliente

Al igual que la concesión de credenciales de contraseña de propietario de recursos, este es un código de aplicación de consola:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
