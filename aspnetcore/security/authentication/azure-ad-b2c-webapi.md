---
title: "Autenticación de nube en web API con Azure Active Directory B2C en ASP.NET Core"
author: camsoper
description: "Descubra cómo configurar la autenticación de Azure Active Directory B2C con API Web de ASP.NET Core. Pruebe la API con Postman de web autenticado."
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 1213f7eb25fb6525f98d83dff0956a841ae686a7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>Autenticación de nube en web API con Azure Active Directory B2C en ASP.NET Core

Por [Cam Soper](https://twitter.com/camsoper)

[Azure B2C Directory Active](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) es una solución de administración de identidades de nube para aplicaciones móviles y web. El servicio proporciona autenticación para las aplicaciones hospedadas en la nube y locales. Tipos de autenticación incluyen cuentas individuales, las cuentas de red social y federado cuentas empresariales. Además, Azure AD B2C puede proporcionar la autenticación multifactor con una configuración mínima.

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C son ofertas de productos independientes. Un inquilino de Azure AD representa una organización, mientras que un inquilino de Azure AD B2C representa una colección de identidades para su uso con aplicaciones de usuario de confianza. Para obtener más información, consulte [Azure AD B2C: preguntas más frecuentes (P+F)](/azure/active-directory-b2c/active-directory-b2c-faqs).

Puesto que las API web no tienen ninguna interfaz de usuario, se trata de no se puede redirigir al usuario a un servicio de token seguro como Azure AD B2C. En su lugar, la API se pasa un token de portador de la aplicación que realiza la llamada, que ya se ha autenticado al usuario con Azure AD B2C. La API, a continuación, valida el token sin interacción directa del usuario.

En este tutorial, aprenderá cómo:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C.
> * Registrar una API Web en Azure AD B2C.
> * Usar Visual Studio para crear una API Web configurado para usar al inquilino de Azure AD B2C para la autenticación.
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C.
> * Use Postman para simular una aplicación web que presenta un cuadro de diálogo de inicio de sesión, recupera un token y lo utiliza para realizar una solicitud en la API web.

## <a name="prerequisites"></a>Requisitos previos

Se requiere para este tutorial lo siguiente:

* [Suscripción de Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio de 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (cualquier edición)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Crear al inquilino de Azure Active Directory B2C

Crear un inquilino de Azure AD B2C [tal como se describe en la documentación de](/azure/active-directory-b2c/active-directory-b2c-get-started). Cuando se le solicite, asociar al inquilino con una suscripción de Azure es opcional para este tutorial.

## <a name="configure-a-sign-up-or-sign-in-policy"></a>Configurar una directiva de inicio de sesión o inicio de sesión

Siga los pasos de la documentación de Azure AD B2C a [crear una directiva de inicio de sesión o inicio de sesión](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy). Nombre de la directiva **SiUpIn**.  Use los valores de ejemplo proporcionados en la documentación de **proveedores de identidades**, **atributos suscripción**, y **notificaciones de la aplicación**. Mediante el **ejecutar ahora** botón para probar la directiva como se describe en la documentación es opcional.

## <a name="register-the-api-in-azure-ad-b2c"></a>Registrar la API en Azure AD B2C

En el inquilino de Azure AD B2C recién creado, registrar la API mediante [los pasos descritos en la documentación de](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) en el **registrar una API web** sección.

Utilice los siguientes valores:

| Parámetro                       | Valor               | Notas                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Nombre de la API&gt;*  | Escriba un **nombre** para la aplicación que describe la aplicación a los consumidores.                     |
| **Incluir la aplicación web / web API** | Sí                 |                                                                                        |
| **Permitir flujo implícito**       | Sí                 |                                                                                        |
| **Dirección URL de respuesta**                 | `https://localhost` | Direcciones URL de respuesta son los puntos de conexión que Azure AD B2C devuelve los tokens que solicita la aplicación. |
| **URI del Id. de aplicación**                | *api*               | El URI no tiene que resolverse en una dirección física. Solo debe ser único.     |
| **Incluir a cliente nativo**     | No                  |                                                                                        |

Una vez registrada la API, se muestra la lista de aplicaciones y las API en el inquilino. Seleccione la API que se acaba de registrar. Seleccione el **copia** icono a la derecha de la **Id. de aplicación** campo para copiarlo en el Portapapeles. Seleccione **publicado ámbitos** y compruebe el valor predeterminado *user_impersonation* ámbito está presente.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Crear una aplicación básica de ASP.NET en Visual Studio de 2017

La plantilla de aplicación Web de Visual Studio puede configurarse para usar al inquilino de Azure AD B2C para la autenticación.

En Visual Studio:

1. Cree una aplicación web de ASP.NET Core. 
2. Seleccione **API Web** en la lista de plantillas.
3. Seleccione el **Cambiar autenticación** botón.
    
    ![Botón de autenticación de cambio](./azure-ad-b2c-webapi/change-auth-button.png)

4. En el **Cambiar autenticación** cuadro de diálogo, seleccione **cuentas de usuario individuales**y, a continuación, seleccione **conectar a un almacén de usuario existente en la nube** en la lista desplegable. 
    
    ![Cuadro de diálogo de autenticación de cambio](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. Rellene el formulario con los siguientes valores:
    
    | Parámetro                       | Valor                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nombre de dominio**               | *&lt;el nombre de dominio del inquilino B2C&gt;*          |
    | **Identificador de la aplicación**            | *&lt;Pegue el identificador de aplicación desde el Portapapeles&gt;* |
    | **Directiva de inicio de sesión o inicio de sesión** | `B2C_1_SiUpIn`                                        |
    
    Seleccione **Aceptar** para cerrar el **Cambiar autenticación** cuadro de diálogo. Seleccione **Aceptar** para crear la aplicación web.

Visual Studio crea la API web con un controlador denominado *ValuesController.cs* que devuelva valores codificados de forma rígida para las solicitudes GET. La clase se decora con el [atributo Authorize](xref:security/authorization/simple), por lo que todas las solicitudes requieren autenticación.

## <a name="run-the-web-api"></a>Ejecute la API web

En Visual Studio, ejecute la API. Visual Studio inicia un explorador que apunta en dirección URL raíz de la API. Tenga en cuenta la dirección URL en la barra de direcciones y dejar la API que se ejecuta en segundo plano.

> [!NOTE]
> Puesto que no hay ningún controlador definido para la dirección URL raíz, el explorador muestra un error 404 de (página no encontrada). Este es el comportamiento normal.

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>Utilice a Postman para obtener un token y la API de pruebas

[Postman](https://getpostman.com/postman) es una herramienta para probar las API web. Para este tutorial, Postman simula una aplicación web que tiene acceso a la API web en nombre del usuario.

### <a name="register-postman-as-a-web-app"></a>Registrar a Postman como una aplicación web

Puesto que Postman simula una aplicación web que puede obtener tokens de los inquilinos de Azure AD B2C, se debe registrar en el inquilino como una aplicación web. Registrar Postman con [los pasos descritos en la documentación de](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) en el **registrar una aplicación web** sección. Detener en el **crear un secreto de cliente de aplicación web** sección. Un secreto de cliente no es necesario para este tutorial. 

Utilice los siguientes valores:

| Parámetro                       | Valor                            | Notas                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Name**                      | Postman                          |                                 |
| **Incluir la aplicación web / web API** | Sí                              |                                 |
| **Permitir flujo implícito**       | Sí                              |                                 |
| **Dirección URL de respuesta**                 | `https://getpostman.com/postman` |                                 |
| **URI del Id. de aplicación**                | *&lt;Deje en blanco&gt;*            | No se necesita para este tutorial. |
| **Incluir a cliente nativo**     | No                               |                                 |

La aplicación web recién registrado necesita permiso para tener acceso a la API web en nombre del usuario.  

1. Seleccione **Postman** en la lista de aplicaciones y, a continuación, seleccione **el acceso de API** en el menú de la izquierda.
2. Seleccione **+ agregar**.
3. En el **API seleccione** de lista desplegable, seleccione el nombre de la API web.
4. En el **seleccionar ámbitos** de lista desplegable, asegúrese de que se seleccionan todos los ámbitos.
5. Seleccione **Aceptar**.

Tenga en cuenta Id. la aplicación Postman de aplicación, tal y como lo necesario para obtener un token de portador.

### <a name="create-a-postman-request"></a>Crear una solicitud de Postman

Inicie a Postman. De forma predeterminada, se muestra Postman el **crear nuevo** tras iniciar el cuadro de diálogo. Si no se muestra el cuadro de diálogo, seleccione la **+ nuevo** botón en la parte superior izquierda.

Desde el **crear nuevo** cuadro de diálogo:

1. Seleccione **solicitar**.
    
    ![Botón de solicitud](./azure-ad-b2c-webapi/postman-create-new.png)

2. Escriba *obtener valores* en el **nombre de la solicitud** cuadro.
3. Seleccione **+ Crear colección** para crear una nueva colección para almacenar la solicitud. Asignar nombre a la colección *tutoriales de ASP.NET Core* y, a continuación, seleccione la marca de verificación.
    
    ![Crear una nueva colección](./azure-ad-b2c-webapi/postman-create-collection.png)

4. Seleccione el **guardar a los tutoriales de ASP.NET Core** botón.

### <a name="test-the-web-api-withoutauthentication"></a>Probar la withoutauthentication API web

Para comprobar que la API web requiere autenticación, primero hay que realizar una solicitud sin autenticación.

1. En el **escriba la dirección URL de solicitud** cuadro, escriba la dirección URL de `ValuesController`. La dirección URL es el mismo valor que se muestra en el explorador con **api/valores** anexado. Un ejemplo sería `https://localhost:44375/api/values`.
2. Seleccione el **enviar** botón.
3. Tenga en cuenta el estado de la respuesta es *401 no autorizado*.

    ![401 no autorizado respuesta](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>Obtener un token de portador

Para realizar una solicitud autenticada a la API web, se requiere un token de portador. Postman facilita la inicie sesión en el inquilino de Azure AD B2C y obtener un token.

1. En el **autorización** ficha la **tipo** lista desplegable, seleccione **OAuth 2.0**. En el **agregar datos de autorización a** lista desplegable, seleccione **encabezados de solicitud**. Seleccione **obtener Token de acceso nuevo**.
    
    ![Ficha de autorización con la configuración](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. Completar la **obtener TOKEN de acceso nuevo** diálogo como se indica a continuación:
    
    | Parámetro                   | Valor                                                                                         | Notas                                                                                      |
    |---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
    | **Nombre del token**            | *&lt;nombre del token&gt;*                                                                          | Escriba un nombre descriptivo para el token.                                                    |
    | **Tipo de concesión**            | Implícitas                                                                                      |                                                                                            |
    | **Dirección URL de devolución de llamada**          | `https://getpostman.com/postman`                                                              |                                                                                            |
    | **Dirección URL de autenticación**              | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` | Reemplace  *&lt;nombre_de_dominio_de&gt;*  con el nombre de dominio del inquilino sin corchetes angulares. |
    | **Id. de cliente**             | *&lt;Escriba la aplicación Postman <b>Id. de aplicación</b>&gt;*                                       |                                                                                            |
    | **Secreto del cliente**         | *&lt;Deje en blanco&gt;*                                                                         |                                                                                            |
    | **Ámbito**                 | `https://<tenant domain name>/api/user_impersonation openid offline_access`                   | Reemplace  *&lt;nombre_de_dominio_de&gt;*  con el nombre de dominio del inquilino sin corchetes angulares. |
    | **Autenticación de cliente** | Enviar las credenciales del cliente en el cuerpo                                                               |                                                                                            |
    
3. Seleccione el **solicitar Token** botón.

4. Postman abre una nueva ventana que contiene el inicio de sesión del inquilino de Azure AD B2C en cuadro de diálogo. Inicie sesión con una cuenta existente (si se ha creado uno probar las directivas) o seleccione **Regístrese ahora** para crear una nueva cuenta. El **¿olvidó su contraseña?** vínculo se usa para restablecer una contraseña olvidada.

5. Después de iniciar sesión correctamente, la ventana se cierra y la **administrar TOKENS de acceso** aparece el cuadro de diálogo. Desplácese hacia abajo hasta la parte inferior y seleccione el **uso Token** botón.
    
    ![Dónde se encuentra el botón "Usar símbolo (token)"](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>Probar la API web con autenticación

Seleccione el **enviar** botón volver a enviar la solicitud. En esta ocasión, el estado de la respuesta es *200 Aceptar* y la carga de JSON es visible en la respuesta **cuerpo** ficha.
    
![Estado de carga y el éxito](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C.
> * Registrar una API Web en Azure AD B2C.
> * Usar Visual Studio para crear una API Web configurado para usar al inquilino de Azure AD B2C para la autenticación.
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C.
> * Use Postman para simular una aplicación web que presenta un cuadro de diálogo de inicio de sesión, recupera un token y lo utiliza para realizar una solicitud en la API web.

Continuar desarrollando su API por el aprendizaje para:

* [Proteger un ASP.NET Core aplicación web con Azure AD B2C](xref:security/authentication/azure-ad-b2c).
* [Llamar a una API web de .NET desde una aplicación web de .NET con Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
* [Personalizar la interfaz de usuario de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurar requisitos de complejidad de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Habilitar la autenticación multifactor](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurar proveedores de identidades adicional, como [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [de Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)y otros.
* [Usar la API de Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) para recuperar información de usuario adicional, como la pertenencia a grupos, desde el inquilino de Azure AD B2C.
