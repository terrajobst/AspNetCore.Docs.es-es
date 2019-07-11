---
title: Autenticación en web API con Azure Active Directory B2C en ASP.NET Core
author: camsoper
description: Descubra cómo configurar la autenticación de Azure Active Directory B2C con ASP.NET Core Web API. Pruebe la API con Postman de web autenticado.
ms.author: casoper
ms.date: 09/21/2018
ms.custom: mvc, seodec18
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: c917a5130857165716bd801ac19dad0f53a7d214
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815376"
---
# <a name="authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Autenticación en web API con Azure Active Directory B2C en ASP.NET Core

Por [Cam Soper](https://twitter.com/camsoper)

[Azure B2C de Active Directory](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) es una solución de administración de identidades de nube para aplicaciones web y móviles. El servicio proporciona autenticación para las aplicaciones hospedadas en la nube y locales. Tipos de autenticación incluyen cuentas individuales, las cuentas de redes sociales y federados cuentas de empresa. B2C de Azure AD también proporciona la autenticación multifactor con una configuración mínima.

Azure Active Directory (Azure AD) y Azure AD B2C son ofertas de producto independiente. Un inquilino de Azure AD representa una organización, mientras que un inquilino de Azure AD B2C representa una colección de identidades para su uso con las aplicaciones de confianza. Para obtener más información, consulte [Azure AD B2C: Preguntas más frecuentes (P+F)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Dado que las API web no tienen ninguna interfaz de usuario, podemos redirigir al usuario a un servicio de token seguro, como Azure AD B2C. En su lugar, la API se pasa un token de portador de la aplicación que realiza la llamada, que ya se ha autenticado al usuario con Azure AD B2C. La API, a continuación, valida el token sin interacción directa del usuario.

En este tutorial, aprenderá cómo:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C.
> * Registrar una API Web en B2C de Azure AD.
> * Use Visual Studio para crear una API Web configurado para usar al inquilino de Azure AD B2C para la autenticación.
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C.
> * Uso de Postman para simular una aplicación web que presenta un cuadro de diálogo de inicio de sesión, recupera un token y lo usa para realizar una solicitud en la API web.

## <a name="prerequisites"></a>Requisitos previos

Se requiere para este tutorial lo siguiente:

* [Suscripción de Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Crear al inquilino de Azure Active Directory B2C

Crear un inquilino de Azure AD B2C [tal como se describe en la documentación de](/azure/active-directory-b2c/active-directory-b2c-get-started). Cuando se le solicite, asociar al inquilino con una suscripción de Azure es opcional para este tutorial.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Configurar una directiva de registro o inicio de sesión

Siga los pasos de la documentación de Azure AD B2C para [crear una directiva de registro o inicio de sesión](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions). Nombre de la directiva **SiUpIn**.  Use los valores de ejemplo proporcionados en la documentación de **proveedores de identidades**, **atributos de registro**, y **notificaciones de aplicación**. Mediante el **ejecutar ahora** botón para probar la directiva como se describe en la documentación es opcional.

## <a name="register-the-api-in-azure-ad-b2c"></a>Registrar la API en B2C de Azure AD

En el inquilino de Azure AD B2C recién creado, registre la API mediante [los pasos descritos en la documentación de](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) bajo el **registrar una API web** sección.

Use los siguientes valores:

| Parámetro                       | Valor               | Notas                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Name**                      | *{Nombre de la API}*        | Escriba un **nombre** para la aplicación que describe la aplicación a los consumidores.                     |
| **Incluir aplicación web o API web** | Sí                 |                                                                                        |
| **Permitir flujo implícito**       | Sí                 |                                                                                        |
| **Dirección URL de respuesta**                 | `https://localhost` | Direcciones URL de respuesta son puntos de conexión donde Azure AD B2C devolverá los tokens que solicita la aplicación. |
| **URI de Id. de aplicación**                | *API*               | El URI no es necesario resolver en una dirección física. Solo debe ser único.     |
| **Incluir a cliente nativo**     | No                  |                                                                                        |

Una vez registrada la API, se muestra la lista de aplicaciones y API en el inquilino. Seleccione la API que se registró anteriormente. Seleccione el **copia** icono a la derecha de la **Id. de aplicación** campo para copiarlo en el Portapapeles. Seleccione **ámbitos publicados** y compruebe el valor predeterminado *user_impersonation* ámbito está presente.

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>Crear una aplicación ASP.NET Core en Visual Studio

La plantilla de aplicación Web de Visual Studio puede configurarse para usar al inquilino de Azure AD B2C para la autenticación.

En Visual Studio:

1. Cree una aplicación web de ASP.NET Core. 
2. Seleccione **API Web** en la lista de plantillas.
3. Seleccione el **Cambiar autenticación** botón.

    ![Botón de la autenticación de cambio](./azure-ad-b2c-webapi/change-auth-button.png)

4. En el **Cambiar autenticación** cuadro de diálogo, seleccione **cuentas de usuario individuales** > **conectar a un almacén de usuario existente en la nube**.

    ![Cuadro de diálogo de autenticación de cambio](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Complete el formulario con los siguientes valores:

    | Parámetro                       | Valor                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nombre de dominio**               | *{el nombre de dominio del inquilino B2C}*                |
    | **Id. de aplicación**            | *{Pegue el identificador de aplicación desde el Portapapeles}*       |
    | **Directiva de registro o inicio de sesión** | B2C_1_SiUpIn                                          |

    Seleccione **Aceptar** para cerrar el **Cambiar autenticación** cuadro de diálogo. Seleccione **Aceptar** para crear la aplicación web.

Visual Studio crea la API web con un controlador denominado *ValuesController.cs* que devuelva valores codificados de forma rígida para las solicitudes GET. La clase está decorada con el [atributo Authorize](xref:security/authorization/simple), por lo que todas las solicitudes requieren autenticación.

## <a name="run-the-web-api"></a>Ejecutar la API web

En Visual Studio, ejecute la API. Visual Studio inicia un explorador que señala a la dirección URL raíz de la API. Tenga en cuenta la dirección URL en la barra de direcciones y dejar la API que se ejecuta en segundo plano.

> [!NOTE]
> Puesto que no hay ningún controlador definido para la dirección URL raíz, el explorador puede mostrar un error 404 de (página no encontrado). Este es el comportamiento normal.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Uso de Postman para obtener un token y prueba de la API

[Postman](https://getpostman.com/postman) es una herramienta para probar las API web. Para este tutorial, Postman simula una aplicación web que tiene acceso a la API web en nombre del usuario.

### <a name="register-postman-as-a-web-app"></a>Registrar a Postman como una aplicación web

Puesto que Postman simula una aplicación web que obtiene los tokens desde el inquilino de Azure AD B2C, debe registrarse en el inquilino como una aplicación web. Registrar el uso de Postman [los pasos descritos en la documentación de](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) bajo el **registrar una aplicación web** sección. Detener el **crear un secreto de cliente de aplicación web** sección. No es necesario un secreto de cliente para este tutorial. 

Use los siguientes valores:

| Parámetro                       | Valor                            | Notas                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Name**                      | Postman                          |                                 |
| **Incluir aplicación web o API web** | Sí                              |                                 |
| **Permitir flujo implícito**       | Sí                              |                                 |
| **Dirección URL de respuesta**                 | `https://getpostman.com/postman` |                                 |
| **URI de Id. de aplicación**                | *{Deje en blanco}*                  | No es necesario para este tutorial. |
| **Incluir a cliente nativo**     | No                               |                                 |

La aplicación web recién registrada necesita permiso para tener acceso a la API web en nombre del usuario.  

1. Seleccione **Postman** en la lista de aplicaciones y a continuación, seleccione **acceso a la API** en el menú de la izquierda.
1. Seleccione **+ agregar**.
1. En el **seleccionar API** lista desplegable, seleccione el nombre de la API web.
1. En el **seleccionar ámbitos** lista desplegable, asegúrese de que se seleccionan todos los ámbitos.
1. Seleccione **Aceptar**.

Tenga en cuenta Id. la aplicación Postman de aplicación, ya que se necesita para obtener un token de portador.

### <a name="create-a-postman-request"></a>Crear una solicitud de Postman

Inicie a Postman. De forma predeterminada, Postman muestra la **crear nuevo** tras iniciar el cuadro de diálogo. Si no se muestra el cuadro de diálogo, seleccione el **+ nuevo** botón en la parte superior izquierda.

Desde el **crear nuevo** cuadro de diálogo:

1. Seleccione **solicitar**.

    ![Botón de solicitud](./azure-ad-b2c-webapi/postman-create-new.png)

2. Escriba *obtener valores* en el **nombre de la solicitud** cuadro.
3. Seleccione **+ Crear colección** para crear una nueva colección para almacenar la solicitud. Asignar nombre a la colección *tutoriales de ASP.NET Core* y, a continuación, seleccione la marca de verificación.

    ![Crear una nueva colección](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Seleccione el **guardar en los tutoriales de ASP.NET Core** botón.

### <a name="test-the-web-api-without-authentication"></a>Probar la API web sin autenticación

Para comprobar que la API web requiere autenticación, en primer lugar realice una solicitud sin autenticación.

1. En el **escriba la dirección URL de solicitud** , escriba la dirección URL de `ValuesController`. La dirección URL es el mismo tal como se muestra en el explorador con **api/values** anexado. Por ejemplo: `https://localhost:44375/api/values`.
2. Seleccione el **enviar** botón.
3. Tenga en cuenta el estado de la respuesta es *401 no autorizado*.

    ![respuesta 401 no autorizado](./azure-ad-b2c-webapi/postman-401-status.png)

> [!IMPORTANT]
> Si recibe un error "No se pudo obtener ninguna respuesta", es posible que deba deshabilitar comprobación del certificado SSL en el [configuración de Postman](https://learning.getpostman.com/docs/postman/launching_postman/settings).

### <a name="obtain-a-bearer-token"></a>Obtener un token de portador

Para realizar una solicitud autenticada a la API web, se requiere un token de portador. Postman facilita iniciar sesión el inquilino de Azure AD B2C y obtener un token.

1. En el **autorización** ficha la **tipo** lista desplegable, seleccione **OAuth 2.0**. En el **agregar datos de autorización a** lista desplegable, seleccione **encabezados de solicitud**. Seleccione **obtener Token de acceso nuevo**.

    ![Ficha de autorización con la configuración](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Completar la **obtener TOKEN de acceso nuevo** diálogo como sigue:

   |                Parámetro                 |                                             Valor                                             |                                                                                                                                    Notas                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      **Nombre del token**       |                                          *{nombre del token}*                                       |                                                                                                                   Escriba un nombre descriptivo para el token.                                                                                                                    |
   |      **Tipo de concesión**       |                                           Implícitas                                            |                                                                                                                                                                                                                                                                              |
   |     **Dirección URL de devolución de llamada**      |                                 `https://getpostman.com/postman`                              |                                                                                                                                                                                                                                                                              |
   |       **Dirección URL de autenticación**        | `https://login.microsoftonline.com/{tenant domain name}/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |  Reemplace *{nombre de dominio del inquilino}* con el nombre de dominio del inquilino. **IMPORTANTE**: Esta dirección URL debe tener el mismo nombre de dominio que lo que se encuentra en `AzureAdB2C.Instance` en la API web *appsettings.json* archivo. Vea la nota&dagger;.                                                  |
   |       **Id. de cliente**       |                *{Escriba la aplicación Postman **Id. de aplicación**}*                              |                                                                                                                                                                                                                                                                              |
   |         **Ámbito**         |         `https://{tenant domain name}/{api}/user_impersonation openid offline_access`       | Reemplace *{nombre de dominio del inquilino}* con el nombre de dominio del inquilino. Reemplace *{api}* con el URI de Id. de aplicación que asignó la API web cuando se registra en primer lugar (en este caso, `api`). El patrón de la dirección URL es: `https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope name}`.         |
   |         **Estado**         |                                      *{Deje en blanco}*                                          |                                                                                                                                                                                                                                                                              |
   | **Autenticación de cliente** |                                Enviar las credenciales del cliente en el cuerpo                                |                                                                                                                                                                                                                                                                              |

    > [!NOTE]
    > &dagger; El cuadro de diálogo de configuración de directiva en el portal de Azure Active Directory B2C muestra dos direcciones URL posibles: Uno con el formato `https://login.microsoftonline.com/`{nombre de dominio del inquilino} / {información de ruta de acceso adicional} y otro en el formato `https://{tenant name}.b2clogin.com/`{nombre de dominio del inquilino} / {información de ruta de acceso adicional}. Tiene **críticos** que el dominio se encuentra en `AzureAdB2C.Instance` en la API web *appsettings.json* archivo coincida con el usado en la aplicación web *appsettings.json* archivo. Se trata del mismo dominio que se utiliza para el campo de dirección URL de autenticación en Postman. Tenga en cuenta que Visual Studio usa un formato de dirección URL ligeramente distinto a lo que se muestra en el portal. Siempre que coincidan con los dominios, la dirección URL funciona.

3. Seleccione el **solicitar Token** botón.

4. Postman abre una nueva ventana que contiene el diálogo de inicio de sesión del inquilino de Azure AD B2C. Inicie sesión con una cuenta existente (si se ha creado uno probar las directivas) o seleccione **Suscríbase ahora** para crear una nueva cuenta. El **¿olvidó su contraseña?** vínculo se usa para restablecer una contraseña olvidada.

5. Después de iniciar sesión correctamente, cierra la ventana y la **administrar TOKENS de acceso** aparece el cuadro de diálogo. Desplácese hacia abajo hasta la parte inferior y seleccione el **uso Token** botón.

    ![Dónde encontrar el botón "Token de uso"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Probar la API web con autenticación

Seleccione el **enviar** botón volver a enviar la solicitud. Esta vez, el estado de respuesta está *200 Aceptar* y la carga de JSON es visible en la respuesta **cuerpo** ficha.

![Estado de carga y el éxito](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C.
> * Registrar una API Web en B2C de Azure AD.
> * Use Visual Studio para crear una API Web configurado para usar al inquilino de Azure AD B2C para la autenticación.
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C.
> * Uso de Postman para simular una aplicación web que presenta un cuadro de diálogo de inicio de sesión, recupera un token y lo usa para realizar una solicitud en la API web.

Seguir desarrollando su API de aprendizaje para:

* [Proteger un núcleo de ASP.NET en aplicación web con Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* [Llamar a una API web de .NET desde una aplicación web de .NET con Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Personalizar la interfaz de usuario de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurar los requisitos de complejidad de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Habilitar la autenticación multifactor](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurar proveedores de identidad adicional, como [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)y otros.
* [Usar la API de Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) para recuperar información adicional del usuario, como la pertenencia al grupo, desde el inquilino de Azure AD B2C.
