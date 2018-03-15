---
title: "Autenticación en la nube con Azure Active Directory B2C en ASP.NET Core"
author: camsoper
description: "Descubra cómo configurar la autenticación de Azure Active Directory B2C con ASP.NET Core."
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: a7bad452a68cf7fe7aa81645d79a0ee9e7719fe7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Autenticación en la nube con Azure Active Directory B2C en ASP.NET Core

Por [Cam Soper](https://twitter.com/camsoper)

[Azure B2C Directory Active](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) es una solución de administración de identidades de nube para aplicaciones móviles y web. El servicio proporciona autenticación para las aplicaciones hospedadas en la nube y locales. Tipos de autenticación incluyen cuentas individuales, las cuentas de red social y federado cuentas empresariales. Además, Azure AD B2C puede proporcionar la autenticación multifactor con una configuración mínima.

> [!TIP]
> Azure Active Directory (Azure AD) Azure AD B2C son ofertas de productos independientes. Un inquilino de Azure AD representa una organización, mientras que un inquilino de Azure AD B2C representa una colección de identidades para su uso con aplicaciones de usuario de confianza. Para obtener más información, consulte [Azure AD B2C: preguntas más frecuentes (P+F)](/azure/active-directory-b2c/active-directory-b2c-faqs).

En este tutorial, aprenderá cómo:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C
> * Registrar una aplicación en Azure AD B2C
> * Usar Visual Studio para crear una aplicación web de ASP.NET Core configurada para usar al inquilino de Azure AD B2C para la autenticación
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C

## <a name="prerequisites"></a>Requisitos previos

Se requiere para este tutorial lo siguiente:

* [Suscripción de Microsoft Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio de 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (cualquier edición)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>Crear al inquilino de Azure Active Directory B2C

Crear un inquilino de Azure Active Directory B2C [tal como se describe en la documentación de](/azure/active-directory-b2c/active-directory-b2c-get-started). Cuando se le solicite, asociar al inquilino con una suscripción de Azure es opcional para este tutorial.

## <a name="register-the-app-in-azure-ad-b2c"></a>Registrar la aplicación en Azure AD B2C

En el inquilino de Azure AD B2C recién creado, registrar la aplicación con [los pasos descritos en la documentación de](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) en el **registrar una aplicación web** sección. Detener en el **crear un secreto de cliente de aplicación web** sección. Un secreto de cliente no es necesario para este tutorial. 

Utilice los siguientes valores:

| Parámetro                       | Valor                     | Notas                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                      | *&lt;Nombre de la aplicación&gt;*        | Escriba un **nombre** para la aplicación que describe la aplicación a los consumidores.                                                                                                                                 |
| **Incluir la aplicación web / web API** | Sí                       |                                                                                                                                                                                                    |
| **Permitir flujo implícito**       | Sí                       |                                                                                                                                                                                                    |
| **Dirección URL de respuesta**                 | `https://localhost:44300` | Direcciones URL de respuesta son los puntos de conexión que Azure AD B2C devuelve los tokens que solicita la aplicación. Visual Studio proporciona la dirección URL de respuesta a la utilice. Por ahora, escriba `https://localhost:44300` para completar el formulario. |
| **URI del Id. de aplicación**                | Deje en blanco               | No se necesita para este tutorial.                                                                                                                                                                    |
| **Incluir a cliente nativo**     | No                        |                                                                                                                                                                                                    |

> [!WARNING]
> Si la configuración de una URL de respuesta no localhost, tener en cuenta el [restricciones en lo que se permite en la lista dirección URL de respuesta](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url). 

Una vez registrada la aplicación, se muestra la lista de aplicaciones en el inquilino. Seleccione la aplicación que se acaba de registrar. Seleccione el **copia** icono a la derecha de la **Id. de aplicación** campo para copiarlo en el Portapapeles.

No hay nada más pueden configurarse en el inquilino de Azure AD B2C en este momento, pero deje abierta la ventana del explorador. Hay una mayor configuración después de crea la aplicación de ASP.NET Core.

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>Crear una aplicación básica de ASP.NET en Visual Studio de 2017

La plantilla de aplicación Web de Visual Studio puede configurarse para usar al inquilino de Azure AD B2C para la autenticación.

En Visual Studio:

1. Cree una aplicación web de ASP.NET Core. 
2. Seleccione **aplicación Web** en la lista de plantillas.
3. Seleccione el **Cambiar autenticación** botón.
    
    ![Botón de autenticación de cambio](./azure-ad-b2c/_static/changeauth.png)

4. En el **Cambiar autenticación** cuadro de diálogo, seleccione **cuentas de usuario individuales**y, a continuación, seleccione **conectar a un almacén de usuario existente en la nube** en la lista desplegable. 
    
    ![Cuadro de diálogo de autenticación de cambio](./azure-ad-b2c/_static/changeauthdialog.png)

5. Rellene el formulario con los siguientes valores:
    
    | Parámetro                       | Valor                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **Nombre de dominio**               | *&lt;el nombre de dominio del inquilino B2C&gt;*          |
    | **Identificador de la aplicación**            | *&lt;Pegue el identificador de aplicación desde el Portapapeles&gt;* |
    | **Ruta de acceso de devolución de llamada**             | *&lt;usar el valor predeterminado&gt;*                       |
    | **Directiva de inicio de sesión o inicio de sesión** | `B2C_1_SiUpIn`                                        |
    | **Directiva de restablecimiento de contraseña**     | `B2C_1_SSPR`                                          |
    | **Editar directiva de perfil**       | *&lt;Deje en blanco&gt;*                                 |
    
    Seleccione el **copia** vínculo junto a **URI de respuesta** para copiar el URI de respuesta en el Portapapeles. Seleccione **Aceptar** para cerrar el **Cambiar autenticación** cuadro de diálogo. Seleccione **Aceptar** para crear la aplicación web.

## <a name="finish-the-b2c-app-registration"></a>Finalizar el registro de aplicación B2C

Volver a la ventana del explorador con las propiedades de aplicación B2C permanece abiertas. Cambiar la contraseña **dirección URL de respuesta** especificado anteriormente para el valor copiado desde Visual Studio. Seleccione **guardar** en la parte superior de la ventana.

> [!TIP]
> Si no copia la dirección URL de respuesta, use la dirección SSL desde la pestaña de depuración en las propiedades del proyecto web y anexar la **CallbackPath** valor de *appSettings.JSON que se*.

## <a name="configure-policies"></a>Configurar directivas

Siga los pasos de la documentación de Azure AD B2C a [crear una directiva de inicio de sesión o inicio de sesión](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)y, a continuación, [crear una directiva de restablecimiento de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy). Use los valores de ejemplo proporcionados en la documentación de **proveedores de identidades**, **atributos suscripción**, y **notificaciones de la aplicación**. Mediante el **ejecutar ahora** botón para probar las directivas, como se describe en la documentación es opcional.

> [!WARNING]
> Asegúrese de que los nombres de directiva son exactamente como se describe en la documentación de esas directivas se usaron en el **Cambiar autenticación** cuadro de diálogo de Visual Studio. Los nombres de directiva se pueden comprobar en *appSettings.JSON que se*.

## <a name="run-the-app"></a>Ejecutar la aplicación

En Visual Studio, presione **F5** para compilar y ejecutar la aplicación. Después de que se inicie la aplicación web, seleccione **iniciar sesión en**.

![Inicie sesión en la aplicación](./azure-ad-b2c/_static/signin.png)

El explorador se redirige a los inquilinos de Azure AD B2C. Inicie sesión con una cuenta existente (si se ha creado uno probar las directivas) o seleccione **Regístrese ahora** para crear una nueva cuenta. El **¿olvidó su contraseña?** vínculo se usa para restablecer una contraseña olvidada.

![El inicio de sesión de Azure AD B2C](./azure-ad-b2c/_static/b2csts.png)

Después de iniciar sesión correctamente, el explorador se redirige a la aplicación web.

![Correcto](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a:

> [!div class="checklist"]
> * Crear a un inquilino de Azure Active Directory B2C
> * Registrar una aplicación en Azure AD B2C
> * Usar Visual Studio para crear una aplicación de ASP.NET Core Web configurado para usar al inquilino de Azure AD B2C para la autenticación
> * Configurar directivas que controlan el comportamiento del inquilino de Azure AD B2C

Ahora que la aplicación de ASP.NET Core está configurada para usar Azure AD B2C para la autenticación, el [atributo Authorize](xref:security/authorization/simple) puede usarse para proteger la aplicación. Continuar desarrollando la aplicación por el aprendizaje para:

* [Personalizar la interfaz de usuario de Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).
* [Configurar requisitos de complejidad de contraseña](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).
* [Habilitar la autenticación multifactor](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).
* Configurar proveedores de identidades adicional, como [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [de Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)y otros.
* [Usar la API de Azure AD Graph](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) para recuperar información de usuario adicional, como la pertenencia a grupos, desde el inquilino de Azure AD B2C.
* [Proteger un núcleo de ASP.NET web API con Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).
* [Llamar a una API web de .NET desde una aplicación web de .NET con Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).
