---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Servidor de autorización de OAuth 2.0 OWIN | Documentos de Microsoft
author: hongyes
description: Este tutorial le guiará en cómo implementar un servidor de autorización de OAuth 2.0 con middleware de OWIN OAuth. Esto es un tutorial avanzado que solo figuración...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="owin-oauth-20-authorization-server"></a>Servidor de autorización de OAuth 2.0 OWIN
====================
por [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le guiará en cómo implementar un servidor de autorización de OAuth 2.0 con middleware de OWIN OAuth. Se trata de un tutorial avanzado que solo se describe los pasos para crear un servidor de autorización de OWIN OAuth 2.0. Esto no es un tutorial paso a paso. [Descargar el código de ejemplo](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Este contorno no debe ser intencionado que se usará para crear una aplicación de producción seguro. Este tutorial está concebido para proporcionar sólo una descripción sobre cómo implementar un servidor de autorización de OAuth 2.0 con middleware de OWIN OAuth.
> 
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> | **Se muestra en el tutorial** | **También funciona con** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express for Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 con la última actualización deberían funcionar, pero el tutorial no se ha probado con él y algunas selecciones de menú y los cuadros de diálogo son diferentes. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar estos en [Katana proyecto en GitHub](https://github.com/aspnet/AspNetKatana/). Para preguntas y comentarios con respecto al tutorial propio, vea la sección de comentarios en la parte inferior de la página.


El [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) permite a una aplicación de terceros obtener acceso limitado a un servicio HTTP. En lugar de usar las credenciales del propietario del recurso para tener acceso a un recurso protegido, el cliente obtiene un token de acceso (que es una cadena que denota un ámbito específico, duración y otros atributos de acceso). Se emiten tokens de acceso a los clientes de terceros mediante un servidor de autorización con la aprobación del propietario del recurso.

Este tutorial se tratará:

- Cómo crear un servidor de autorización para admitir la autorización de cuatro tipos de concesión y tokens de actualización:
    - Concesión de código de autorización
    - Concesión implícita
    - Concesión de credenciales de contraseña de propietario de recursos
    - Concesión de credenciales de cliente
- Crear un servidor de recursos que está protegido por un token de acceso.
- Creación de clientes de OAuth 2.0.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) o gratuitamente [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), como se indica en **versiones de Software** en la parte superior de la página.
- Familiaridad con OWIN. Vea [introducción con el proyecto Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) y [What's new en OWIN y Katana](index.md).
- Familiaridad con [OAuth](http://tools.ietf.org/html/rfc6749) terminología, incluidos [Roles](http://tools.ietf.org/html/rfc6749#section-1.1), [de flujo de protocolo](http://tools.ietf.org/html/rfc6749#section-1.2), y [concesión de autorización](http://tools.ietf.org/html/rfc6749#section-1.3). [Introducción de OAuth 2.0](http://tools.ietf.org/html/rfc6749#section-1) proporciona una buena introducción.

## <a name="create-an-authorization-server"></a>Crear un servidor de autorización

En este tutorial, se le aproximadamente esbozar cómo usar [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) y MVC de ASP.NET para crear un servidor de autorización. Esperamos que pronto proporcionar una descarga para el ejemplo completo, este tutorial incluya cada paso. En primer lugar, cree una aplicación web vacía denominada *AuthorizationServer* e instalar los siguientes paquetes:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- Microsoft.Owin.Security.Google (o cualquier otro inicio de sesión social empaquetar como Microsoft.Owin.Security.Facebook)

Agregar un [clase de inicio de OWIN](owin-startup-class-detection.md) en la carpeta raíz del proyecto denominada *inicio*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Crear un *aplicación\_iniciar* carpeta. Seleccione el *aplicación\_iniciar* carpeta y utilice Mayús + Alt + A para agregar la versión descargada de la *AuthorizationServer\App\_Start\Startup.Auth.cs* archivo.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

El código anterior permite el inicio de sesión de aplicación/externa en las cookies y la autenticación de Google, que se usan por el propio servidor de autorización para administrar cuentas.

El `UseOAuthAuthorizationServer` método de extensión consiste en configurar el servidor de autorización. Las opciones de configuración son:

- `AuthorizeEndpointPath`: La ruta de acceso de solicitud donde las aplicaciones cliente redirigirán el agente de usuario con el fin de obtener los usuarios dar su consentimiento para emitir un token o código. Debe comenzar con una barra inicial, por ejemplo, "`/Authorize`".
- `TokenEndpointPath`: Las aplicaciones de cliente de ruta de acceso de solicitud se comunican directamente para obtener el token de acceso. Debe comenzar con una barra inicial, como "/ símbolo (token)". Si el cliente ha emitido una [cliente\_secreto](http://tools.ietf.org/html/rfc6749#appendix-A.2), que se debe proporcionar a este extremo.
- `ApplicationCanDisplayErrors`: Se establece en `true` si desea que la aplicación web generar una página de error personalizada para los errores de validación del cliente en `/Authorize` punto de conexión. Esto solo es necesario para los casos donde no se redirige el Explorador de nuevo a la aplicación cliente, por ejemplo, cuando la `client_id` o `redirect_uri` son incorrectas. El `/Authorize` extremo debe esperar que el "oauth. Error","oauth. ErrorDescription"y"oauth. Propiedades de ErrorUri"se agregan al entorno OWIN. 

    > [!NOTE]
    > Si no es true, el servidor de autorización devolverá una página de error predeterminada con los detalles del error.
- `AllowInsecureHttp`: True para permitir autorizar y solicitudes de token llegue a direcciones URI HTTP y para permitir la entrada `redirect_uri` autorizar a los parámetros de solicitud tenga direcciones URI de HTTP. 

    > [!WARNING]
    > Seguridad: se trata solamente para los desarrolladores.
- `Provider`: El objeto proporcionado por la aplicación para procesar los eventos generados por el middleware de servidor de autorización. La aplicación puede implementar la interfaz por completo o puede crear una instancia de `OAuthAuthorizationServerProvider` y asignar delegados necesarios para los flujos de OAuth es compatible con este servidor.
- `AuthorizationCodeProvider`: Genera un código de autorización de uso único para volver a la aplicación cliente. Para que el servidor de OAuth para proteger la aplicación **debe** proporcionar una instancia de `AuthorizationCodeProvider` donde el token generado por la `OnCreate/OnCreateAsync` eventos se consideran válido para sólo una llamada a `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Genera un token de actualización que puede usarse para generar un token de acceso nuevo cuando sea necesario. Si no se proporciona el servidor de autorización no devolverá tokens de actualización de la `/Token` punto de conexión.

## <a name="account-management"></a>Administración de cuentas

OAuth no le importa dónde y cómo administrar la información de su cuenta de usuario. Tiene [ASP.NET Identity](../../../identity/index.md) que es responsable de ella. En este tutorial, se se simplifican el código de administración de cuenta y siempre que se asegure de que el usuario puede iniciar sesión con middleware de cookies OWIN. Este es el código de ejemplo simplificado para el `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` se utiliza para validar al cliente con su dirección URL de redireccionamiento registrado. `ValidateClientAuthentication` comprueba el encabezado de esquema básico y el cuerpo del formulario para obtener las credenciales del cliente.

A continuación se muestra la página de inicio de sesión:

![](owin-oauth-20-authorization-server/_static/image1.png)

Revise OAuth 2 del IETF [concesión de código de autorización](http://tools.ietf.org/html/rfc6749#section-4.1) sección ahora. 

**Proveedor** (en la tabla siguiente) es [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Proveedor, que es de tipo `OAuthAuthorizationServerProvider`, que contiene todos los eventos de servidor de OAuth. 

| Flujo de pasos de la sección de concesión de código de autorización | Descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) el cliente inicia el flujo dirigiendo agente del propietario del recurso de usuario al extremo de autorización. El cliente incluye su identificador de cliente, el ámbito solicitado, el estado local y un URI de redirección a la que el servidor de autorización enviará al agente de usuario una vez concedido (o deniega el acceso). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) el servidor de autorización autentica el propietario del recurso (mediante el agente de usuario) y establece si el propietario del recurso se concede o deniega la solicitud de acceso del cliente. | **&lt;Si el usuario concede el acceso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) suponiendo que el propietario del recurso se concede acceso, el servidor de autorización redirige el agente de usuario al cliente mediante la redirección URI proporcionado anteriormente (en la solicitud o durante el registro de cliente). ... |  |
|  |  |
| (D) el cliente solicita un token de acceso de extremo de token del servidor de autorización si se incluye el código de autorización recibido en el paso anterior. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. El cliente incluye la redirección que URI utilizado para obtener el código de autorización para la comprobación. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Una implementación de ejemplo para `AuthorizationCodeProvider.CreateAsync` y `ReceiveAsync` controlar la creación y la validación de código de autorización se muestra a continuación.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

El código anterior usa un diccionario simultáneo en memoria para almacenar el vale de código y la identidad y la identidad de restauración después de recibir el código. En una aplicación real, se reemplazará por un almacén de datos persistente. El extremo de autorización es para que el propietario del recurso conceder acceso al cliente. Por lo general, necesita una interfaz de usuario para permitir al usuario hacer clic en un botón y confirmar la concesión. Middleware de OWIN OAuth permite que el código de aplicación para controlar el extremo de autorización. En nuestra aplicación de ejemplo, se utiliza un controlador MVC llama `OAuthController` para controlarla. Esta es la implementación del ejemplo:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

El `Authorize` acción comprobará primero si el usuario ha iniciado sesión el servidor de autorización. Si no es así, el middleware de autenticación desafíos al llamador para autenticarse con la cookie de "Aplicación" y redirige a la página de inicio de sesión. (Vea el código que aparece resaltado anterior). Si el usuario ha iniciado sesión, representará la vista Authorize, tal y como se muestra a continuación:

![](owin-oauth-20-authorization-server/_static/image2.png)

Si el **Grant** botón está seleccionado, el `Authorize` acción creará una nueva identidad de "Portador" e inicie sesión con la. Activará el servidor de autorización para generar un token de portador y devolverlo al cliente con carga útil de JSON. 

### <a name="implicit-grant"></a>Concesión implícita

Consulte OAuth 2 del IETF [concesión implícita](http://tools.ietf.org/html/rfc6749#section-4.2) sección ahora.

 El [concesión implícita](http://tools.ietf.org/html/rfc6749#section-4.2) flujo se muestra en la figura 4 es el flujo y el OWIN de OAuth de asignación que sigue de middleware.  

| Flujo de pasos de la sección de concesión implícita | Descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) el cliente inicia el flujo dirigiendo agente del propietario del recurso de usuario al extremo de autorización. El cliente incluye su identificador de cliente, el ámbito solicitado, el estado local y un URI de redirección a la que el servidor de autorización enviará al agente de usuario una vez concedido (o deniega el acceso). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) el servidor de autorización autentica el propietario del recurso (mediante el agente de usuario) y establece si el propietario del recurso se concede o deniega la solicitud de acceso del cliente. | **&lt;Si el usuario concede el acceso&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) suponiendo que el propietario del recurso se concede acceso, el servidor de autorización redirige el agente de usuario al cliente mediante la redirección URI proporcionado anteriormente (en la solicitud o durante el registro de cliente). ... |  |
|  |  |
| (D) el cliente solicita un token de acceso de extremo de token del servidor de autorización si se incluye el código de autorización recibido en el paso anterior. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. El cliente incluye la redirección que URI utilizado para obtener el código de autorización para la comprobación. |  |

Puesto que ya se implementa el extremo de autorización (`OAuthController.Authorize` acción) para la concesión de código de autorización, habilita automáticamente también flujo implícito. Nota: `Provider.ValidateClientRedirectUri` se utiliza para validar el Id. de cliente con su dirección URL de redirección, que protege el flujo de concesión implícita de enviar el acceso a los clientes de tokens a ataques malintencionados ([ataque Man-in-the-middle](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Concesión de credenciales de contraseña de propietario de recursos

Consulte OAuth 2 del IETF [concesión de credenciales de contraseña de propietario de recurso](http://tools.ietf.org/html/rfc6749#section-4.3) sección ahora.

 El [concesión de credenciales de contraseña de propietario de recurso](http://tools.ietf.org/html/rfc6749#section-4.3) flujo se muestra en la figura 5 es el flujo y asignación que la OAuth OWIN sigue de middleware.  

| Flujo de pasos de la sección de concesión de credenciales de contraseña de propietario de recursos | Descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) el propietario del recurso proporciona al cliente con su nombre de usuario y una contraseña. |  |
|  |  |
| (B) el cliente solicita un token de acceso de extremo de token del servidor de autorización mediante la inclusión de las credenciales recibidas desde el propietario del recurso. Al realizar la solicitud, el cliente se autentica con el servidor de autorización. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) el servidor de autorización autentica al cliente y valida las credenciales del propietario de recursos y, si es válida, emite un token de acceso. |  |

Aquí es la implementación de ejemplo para `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> El código anterior está diseñado para explicar esta sección del tutorial y no debe usarse en seguro o en las aplicaciones de producción. No comprueba las credenciales de los propietarios del recurso. Se supone que cada credencial es válido y crea una nueva identidad para él. La nueva identidad se utilizará para generar el token de acceso y token de actualización. Reemplace el código con su propio código de administración de cuenta segura.


### <a name="client-credentials-grant"></a>Concesión de credenciales de cliente

Consulte OAuth 2 del IETF [concesión de credenciales de cliente](http://tools.ietf.org/html/rfc6749#section-4.4) sección ahora.

 El [concesión de credenciales de cliente](http://tools.ietf.org/html/rfc6749#section-4.4) flujo se muestra en la figura 6 es el flujo y el OWIN de OAuth de asignación que sigue de middleware.  

| Flujo de pasos de la sección de concesión de credenciales de cliente | Descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (A) el cliente se autentica con el servidor de autorización y solicita un token de acceso desde el extremo de token. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) el servidor de autorización autentica al cliente y si es válida, emite un token de acceso. |  |

Aquí es la implementación de ejemplo para `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> El código anterior está diseñado para explicar esta sección del tutorial y no debe usarse en seguro o en las aplicaciones de producción. Reemplace el código con su propio código de administración de cliente segura.


### <a name="refresh-token"></a>Token de actualización

Consulte OAuth 2 del IETF [Token de actualización](http://tools.ietf.org/html/rfc6749#section-1.5) sección ahora.

 El [Token de actualización](http://tools.ietf.org/html/rfc6749#section-1.5) flujo se muestra en la figura 2 es el flujo y asignación que la OAuth OWIN sigue de middleware.  

| Flujo de pasos de la sección de concesión de credenciales de cliente | Descarga de ejemplo realiza estos pasos con: |
| --- | --- |
|  |  |
| (G) el cliente solicita un nuevo token de acceso mediante la autenticación con el servidor de autorización y presentar el token de actualización. Los requisitos de autenticación de cliente se basan en el tipo de cliente y en las directivas de servidor de autorización. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) el servidor de autorización autentica al cliente y valida el token de actualización y, si es válida, emite un token de acceso nuevo (y, opcionalmente, un nuevo token de actualización). |  |

Aquí es la implementación de ejemplo para `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Crear un servidor de recursos que se protege mediante el Token de acceso

Crear un proyecto de aplicación web vacía e instale después de paquetes del proyecto:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Cree una clase de inicio y configurar la autenticación y la API Web. Vea *AuthorizationServer\ResourceServer\Startup.cs* en el ejemplo de la descarga.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Vea *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* en el ejemplo de la descarga.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Vea *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* en el ejemplo de la descarga.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` método permite CORS para todos los dominios.
- `UseOAuthBearerAuthentication` método permite que el middleware de autenticación de token de portador de OAuth que recibirá y validar el token de portador encabezado de autorización de la solicitud.
- `Config.SuppressDefaultHostAuthenticaiton` Suprime la predeterminada principal autenticado desde la aplicación de host, por lo tanto, todas las solicitudes será anónimas después de esta llamada.
- `HostAuthenticationFilter` habilita la autenticación solo para el tipo de autenticación especificado. En este caso, es el tipo de autenticación de portador.

Para demostrar la identidad autenticada, creamos un ApiController para generar notificaciones del usuario actual.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Si el servidor de autorización y el servidor de recursos no están en el mismo equipo, el middleware de OAuth utilizará las claves de equipo diferente para cifrar y descifrar el token de acceso de portador. Para compartir la misma clave privada entre ambos proyectos, agregamos la misma `machinekey` configuración tanto en *web.config* archivos.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Crear a clientes OAuth 2.0

 Usamos el [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) paquete NuGet para simplificar el código de cliente.

### <a name="authorization-code-grant-client"></a>Cliente de concesión de código de autorización

 Este cliente es una aplicación de MVC. Desencadenará un flujo de concesión de código de autorización para obtener el acceso de símbolo (token) de back-end. Tiene una sola página tal y como se muestra a continuación:

![](owin-oauth-20-authorization-server/_static/image3.png)

- El **Authorize** botón redirigirá el explorador al servidor de autorización para que notifique al propietario de recursos para conceder acceso a este cliente.
- El **actualizar** botón le proporcionará un nuevo token de acceso y el token de actualización mediante la actualización actual token.
- El **API de acceso protegido recursos** botón llamará el servidor de recursos para obtener datos de las notificaciones del usuario actual y mostrarlos en la página.

Este es el código de ejemplo de la `HomeController` del cliente.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` requiere SSL de forma predeterminada. Dado que nuestra demostración está usando HTTP, debe agregar después de la configuración en el archivo de configuración:

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Seguridad: nunca deshabilitar SSL en una aplicación de producción. Las credenciales de inicio de sesión ahora se envían en texto no cifrado a través de la conexión. El código anterior es solo para depuración ejemplo local y la exploración.


### <a name="implicit-grant-client"></a>Cliente de concesión implícita

Este cliente está usando JavaScript:

1. Abra una ventana nueva y redirigir al extremo authorize del servidor de autorización.
2. Obtener el token de acceso de fragmentos de dirección URL cuando se redirige de nuevo.

La imagen siguiente muestra este proceso:

![](owin-oauth-20-authorization-server/_static/image4.png)

El cliente debe tener dos páginas: uno para la página principal y la otra para la devolución de llamada. Este es el ejemplo de JavaScript de código que se encuentra en la *Index.cshtml* archivo:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Este es el código de control de la devolución de llamada *SignIn.cshtml* archivo:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Una práctica recomendada consiste en mover el código JavaScript a un archivo externo y no se incrusta con el marcado de Razor. Para simplificar este ejemplo, se han combinado.


### <a name="resource-owner-password-credentials-grant-client"></a>Cliente de concesión de credenciales de contraseña de propietario de recursos

Se utiliza una aplicación de consola para la demostración de este cliente. Este es el código:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Cliente de concesión de credenciales de cliente

Similar a la concesión de credenciales de contraseña de propietario de recursos, este es un código de la aplicación de consola:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
