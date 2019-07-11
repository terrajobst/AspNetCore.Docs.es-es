---
title: Autenticación en la nube con Azure Active Directory B2C en ASP.NET Core
author: camsoper
description: Descubra cómo configurar la autenticación de Azure Active Directory B2C con ASP.NET Core.
ms.author: casoper
ms.custom: mvc
ms.date: 02/27/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 54117bf0dd45305d060eef5fecfb98ed45f8ecdb
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815297"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Autenticación en la nube con Azure Active Directory B2C en ASP.NET Core

Por [Cam Soper](https://twitter.com/camsoper)

[Azure B2C de Active Directory](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) es una solución de administración de identidades de nube para aplicaciones web y móviles. El servicio proporciona autenticación para las aplicaciones hospedadas en la nube y locales. Tipos de autenticación incluyen cuentas individuales, las cuentas de redes sociales y federados cuentas de empresa. Además, Azure AD B2C puede proporcionar la autenticación multifactor con una configuración mínima.

> [!TIP]
> Azure Active Directory (Azure AD) y Azure AD B2C son ofertas de producto independiente. Un inquilino de Azure AD representa una organización, mientras que un inquilino de Azure AD B2C representa una colección de identidades para su uso con las aplicaciones de confianza. Para obtener más información, consulte [Azure AD B2C: Preguntas más frecuentes (P+F)](/azure/active-directory-b2c/active-directory-b2c-faqs).

En este tutorial, aprenderá cómo:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C
> * Registrar una aplicación en B2C de Azure AD
> * Use Visual Studio para crear una aplicación web de ASP.NET Core configurada para usar al inquilino de Azure AD B2C para la autenticación
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C

## <a name="prerequisites"></a>Requisitos previos

Se requiere para este tutorial lo siguiente:

* [Suscripción de Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Crear al inquilino de Azure Active Directory B2C

Crear un inquilino de Azure Active Directory B2C [tal como se describe en la documentación de](/azure/active-directory-b2c/active-directory-b2c-get-started). Cuando se le solicite, asociar al inquilino con una suscripción de Azure es opcional para este tutorial.

## <a name="register-the-app-in-azure-ad-b2c"></a>Registrar la aplicación en B2C de Azure AD

En el inquilino de Azure AD B2C recién creado, registre la aplicación mediante [los pasos descritos en la documentación de](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) bajo el **registrar una aplicación web** sección. Detener el **crear un secreto de cliente de aplicación web** sección. No es necesario un secreto de cliente para este tutorial. 

Use los siguientes valores:

| Parámetro                       | Valor                     | Notas                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nombre**                      | *&lt;Nombre de la aplicación&gt;*        | Escriba un **nombre** para la aplicación que describe la aplicación a los consumidores.                                                                                                                                 |
| **Incluir aplicación web o API web** | Sí                       |                                                                                                                                                                                                    |
| **Permitir flujo implícito**       | Sí                       |                                                                                                                                                                                                    |
| **Dirección URL de respuesta**                 | `https://localhost:44300/signin-oidc` | Direcciones URL de respuesta son puntos de conexión donde Azure AD B2C devolverá los tokens que solicita la aplicación. Visual Studio proporciona la dirección URL de respuesta para usar. Por ahora, escriba `https://localhost:44300/signin-oidc` para completar el formulario. |
| **URI de Id. de aplicación**                | Déjelo en blanco               | No es necesario para este tutorial.                                                                                                                                                                    |
| **Incluir a cliente nativo**     | Sin                        |                                                                                                                                                                                                    |

> [!WARNING]
> Si la configuración de una URL de respuesta que no sean localhost, ser consciente de la [restricciones en lo que se permite en la lista dirección URL de respuesta](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application). 

Una vez registrada la aplicación, se muestra la lista de aplicaciones en el inquilino. Seleccione la aplicación que acaba de registrar. Seleccione el **copia** icono a la derecha de la **Id. de aplicación** campo para copiarlo en el Portapapeles.

No hay nada más pueden configurarse en el inquilino de Azure AD B2C en este momento, pero deje abierta la ventana del explorador. No hay más configuración después de crea la aplicación de ASP.NET Core.

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>Crear una aplicación ASP.NET Core en Visual Studio

La plantilla de aplicación Web de Visual Studio puede configurarse para usar al inquilino de Azure AD B2C para la autenticación.

En Visual Studio:

1. Cree una aplicación web de ASP.NET Core. 
2. Seleccione **aplicación Web** en la lista de plantillas.
3. Seleccione el **Cambiar autenticación** botón.
    
    ![Botón de la autenticación de cambio](./azure-ad-b2c/_static/changeauth.png)

4. En el **Cambiar autenticación** cuadro de diálogo, seleccione **cuentas de usuario individuales**y, a continuación, seleccione **conectar a un almacén de usuario existente en la nube** en la lista desplegable. 
    
    ![Cuadro de diálogo de autenticación de cambio](./azure-ad-b2c/_static/changeauthdialog.png)

5. Complete el formulario con los siguientes valores:
    
    | Parámetro                       | Valor                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nombre de dominio**               | *&lt;el nombre de dominio del inquilino B2C&gt;*          |
    | **Id. de aplicación**            | *&lt;Pegue el identificador de aplicación desde el Portapapeles&gt;* |
    | **Ruta de acceso de devolución de llamada**             | *&lt;Use el valor predeterminado&gt;*                       |
    | **Directiva de registro o inicio de sesión** | `B2C_1_SiUpIn`                                        |
    | **Directiva de restablecimiento de contraseña**     | `B2C_1_SSPR`                                          |
    | **Editar directiva de perfil**       | *&lt;Deje en blanco&gt;*                                 |
    
    Seleccione el **copia** junto al vínculo **URI de respuesta** para copiar el URI de respuesta en el Portapapeles. Seleccione **Aceptar** para cerrar el **Cambiar autenticación** cuadro de diálogo. Seleccione **Aceptar** para crear la aplicación web.

## <a name="finish-the-b2c-app-registration"></a>Finalizar el registro de aplicación B2C

Volver a la ventana del explorador con las propiedades de la aplicación B2C sigue abiertas. Cambiar temporal **dirección URL de respuesta** especificado anteriormente para el valor copiado desde Visual Studio. Seleccione **guardar** en la parte superior de la ventana.

> [!TIP]
> Si no los copió la URL de respuesta, use la dirección HTTPS desde la pestaña de depuración en las propiedades del proyecto web y anexe la **CallbackPath** valor desde *appsettings.json*.

## <a name="configure-policies"></a>Configuración de directivas

Siga los pasos de la documentación de Azure AD B2C para [crear una directiva de registro o inicio de sesión](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions)y, a continuación, [crear una directiva de restablecimiento de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions). Use los valores de ejemplo proporcionados en la documentación de **proveedores de identidades**, **atributos de registro**, y **notificaciones de aplicación**. Mediante el **ejecutar ahora** para probar las directivas, como se describe en la documentación es opcional.

> [!WARNING]
> Asegúrese de los nombres de directiva exactamente como se describe en la documentación de esas directivas utilizadas en el **Cambiar autenticación** cuadro de diálogo de Visual Studio. Los nombres de directiva se pueden comprobar en *appsettings.json*.

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a>Configure las opciones de cookies o JwtBearer/OpenIdConnectOptions subyacentes

Para configurar las opciones subyacentes directamente, usar la constante de esquema adecuado en `Startup.ConfigureServices`:

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a>Ejecución de la aplicación

En Visual Studio, presione **F5** para compilar y ejecutar la aplicación. Una vez que inicie la aplicación web, seleccione **Accept** para aceptar el uso de cookies (si se le solicita) y, a continuación, seleccione **inicie sesión en**.

![Inicie sesión en la aplicación](./azure-ad-b2c/_static/signin.png)

El explorador se redirige a los inquilinos de Azure AD B2C. Inicie sesión con una cuenta existente (si se ha creado uno probar las directivas) o seleccione **Suscríbase ahora** para crear una nueva cuenta. El **¿olvidó su contraseña?** vínculo se usa para restablecer una contraseña olvidada.

![Inicio de sesión de Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Después de iniciar sesión correctamente, el explorador se redirige a la aplicación web.

![Correcto](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C
> * Registrar una aplicación en B2C de Azure AD
> * Use Visual Studio para crear una aplicación Web de ASP.NET Core configurados para usar al inquilino de Azure AD B2C para la autenticación
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C

Ahora que la aplicación de ASP.NET Core está configurada para usar Azure AD B2C para la autenticación, el [atributo Authorize](xref:security/authorization/simple) puede usarse para proteger la aplicación. Continuar desarrollando la aplicación Learning para:

* [Personalizar la interfaz de usuario de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurar los requisitos de complejidad de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Habilitar la autenticación multifactor](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurar proveedores de identidad adicional, como [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)y otros.
* [Usar la API de Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) para recuperar información adicional del usuario, como la pertenencia al grupo, desde el inquilino de Azure AD B2C.
* [Proteger un núcleo de ASP.NET web API mediante Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).
* [Llamar a una API web de .NET desde una aplicación web de .NET con Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
