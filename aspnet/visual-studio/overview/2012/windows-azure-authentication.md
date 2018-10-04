---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows autenticación de Azure | Microsoft Docs
author: Rick-Anderson
description: Herramientas de Microsoft ASP.NET para Windows Azure Active Directory simplifica el proceso de habilitar la autenticación para aplicaciones web hospedadas en sitios Web de Windows Azure...
ms.author: riande
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: a45b0ad2b61c2b78f7f06e85fe5e92193d73041d
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577514"
---
<a name="windows-azure-authentication"></a>Windows autenticación de Azure
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Herramientas de Microsoft ASP.NET para Windows Azure Active Directory simplifica el proceso de habilitar la autenticación para aplicaciones web hospedadas en [sitios Web Windows Azure](https://www.windowsazure.com/home/features/web-sites/). Puede usar la autenticación de Windows Azure para autenticar a los usuarios de Office 365 de su organización, las cuentas corporativas sincronizadas desde Active Directory local o los usuarios creados en su propio dominio personalizado de Windows Azure Active Directory. Habilitar la autenticación de Windows Azure configura la aplicación para autenticar usuarios mediante una sola [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) inquilino.
> 
> La herramienta de autenticación de Windows Azure de ASP.NET no se admite para los roles web en un servicio en la nube, aunque se planea hacerlo en una versión futura. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) se admite en los roles web de Windows Azure.
> 
> Para obtener más información sobre cómo configurar la sincronización entre Active Directory local y el inquilino de Azure Active Directory de Windows consulte [usar AD FS 2.0 para implementar y administrar inicio de sesión único](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory está disponible actualmente como una [gratis el servicio en versión preliminar](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Requisitos:

- Visual Studio 2012 o [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Extensiones para Visual Studio 2012 de herramientas Web](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) o [Web Tools Extensions para Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Herramientas de Microsoft ASP.NET para Windows Azure en Visual Studio 2012 Active Directory](https://go.microsoft.com/fwlink/?LinkID=282306) o [herramientas de Microsoft ASP.NET para Windows Azure Active Directory – Visual Studio Express 2012 para Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Crear una aplicación Web ASP.NET con Visual Studio 2012

Puede crear cualquier aplicación Web con Visual Studio 2012, este tutorial usa la plantilla de intranet de ASP.NET MVC.

1. Cree una nueva aplicación de Intranet de ASP.NET MVC 4 y acepte todos los valores predeterminados. (Debe ser In **tra** net y no en **ter** proyecto net).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Habilitar la autenticación de Windows Azure (cuando un administrador Global del principio)

Si no tiene un inquilino existente de Windows Azure Active Directory (por ejemplo, a través de una cuenta de Office 365 existente) puede crear un nuevo inquilino registrarse para obtener un [nueva cuenta de Windows Azure Active Directory](http://g.microsoftonline.com/0AX00en/5).

1. En el menú proyecto, seleccione **Habilitar autenticación de Windows Azure**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Escriba el dominio de su inquilino de Windows Azure Active Directory (por ejemplo, contoso.onmicrosoft.com) y haga clic en **habilitar**:

![](windows-azure-authentication/_static/image3.png)

3. En la autenticación Web cuadro de diálogo de inicio de sesión como administrador para el inquilino de Windows Azure Active Directory:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Habilitar Windows Azure, que no sea el administrador del principio

Si no tiene privilegios de administrador Global para el inquilino de Windows Azure Active Directory, puede desactivar la casilla de verificación para el aprovisionamiento de la aplicación.

![](windows-azure-authentication/_static/image6.png)

Se mostrará el cuadro de diálogo el **dominio**, **Id. de entidad de aplicación** y **dirección URL de respuesta** que son necesarios para aprovisionar la aplicación con Azure Active Directory principio. Deberá proporcionar esta información a alguien que tiene suficientes privilegios para aprovisionar la aplicación. Consulte[cómo implementar el inicio de sesión único con Windows Azure Active Directory - aplicación de ASP.NET](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) para obtener más información sobre cómo usar el cmdlet para crear la entidad de servicio manualmente.  
Una vez que la aplicación se haya aprovisionado correctamente, puede hacer clic en **seguirán actualizándose web.config con la configuración seleccionada a**. Si desea continuar desarrollando la aplicación mientras se espera que ocurra, puede hacer clic en el aprovisionamiento **Cerrar para recordar la configuración en el archivo de proyecto**. La próxima vez que invoque Habilitar autenticación de Windows Azure y desactive la casilla de verificación de aprovisionamiento, verá la misma configuración y puede hacer clic **continuar**, a continuación, haga clic en **aplicar esta configuración en el archivo web.config**.

1. Espere mientras la aplicación está configurada para la autenticación de Windows Azure y aprovisionada con Windows Azure Active Directory.
2. Cuando se haya habilitado la autenticación de Windows Azure para su aplicación, haga clic en **cerrar:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Presione F5 para ejecutar la aplicación. Debe redirigir automáticamente a la página de inicio de sesión. Use las credenciales de usuario del principio de directorio para iniciar sesión en la aplicación...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Dado que la aplicación está usando un certificado de prueba autofirmado recibirá una advertencia del explorador que el certificado no fue emitido por una entidad de certificación de confianza.

    Esta advertencia puede omitirse durante el desarrollo local, haga clic en **pasar a este sitio Web:** 

    ![](windows-azure-authentication/_static/image8.png)
5. ¡Se han ahora conectado a la aplicación mediante la autenticación de Windows Azure!

    ![](windows-azure-authentication/_static/image2.jpg)

Habilitar Windows Azure autenticación realiza los cambios siguientes en la aplicación:

- Una falsificación de solicitud cruzada a sitio ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) clase ( *aplicación\_Start\AntiXsrfConfig.cs* ) se agrega al proyecto.
- Los paquetes de NuGet `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` se agrega al proyecto.
- Configuración de Windows Identity Foundation en la aplicación se configurará para que acepte tokens de seguridad del inquilino de Windows Azure Active Directory. Haga clic en la imagen siguiente para ver una vista expandida de los cambios realizados en el *Web.config* archivo.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Se aprovisionará una entidad de servicio para la aplicación en el inquilino de Windows Azure Active Directory.
- HTTPS se habilita.

## <a name="deploy-the-application-to-windows-azure"></a>Implementar la aplicación en Windows Azure

Para obtener instrucciones completas, consulte [implementar una aplicación Web de ASP.NET a un sitio Web de Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Para publicar una aplicación mediante la autenticación de Windows Azure en un sitio Web de Azure:

1. Haga clic con el botón derecho en la aplicación y seleccione **publicar:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. En el cuadro de diálogo publicación Web descargar e importar un perfil de publicación para el sitio Web de Azure.

    ![](windows-azure-authentication/_static/image4.jpg)
3. El **conexión** pestaña muestra la **dirección URL de destino** (URL de orientados al público para su aplicación). Haga clic en **validar conexión** para probar la conexión:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Si ha publicado este sitio Web de Azure previamente Proteja el **quitar archivos adicionales en destino** configuración para garantizar que la aplicación se publica correctamente. Tenga en cuenta la **Habilitar autenticación de Windows Azure** casilla de verificación está slected.  

    ![](windows-azure-authentication/_static/image10.png)
5. Opcional: En el **Preview** ficha en **iniciar vista previa** para ver los archivos implementados.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Haga clic en **publicar.**

    Se solicitará que habilite la autenticación de Windows Azure para el host de destino. Haga clic en **habilitar** para continuar:

    ![](windows-azure-authentication/_static/image11.png)
7. Escriba sus credenciales de administrador de su inquilino de Windows Azure Active Directory:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Una vez que la aplicación se ha publicado correctamente, se abrirá un explorador para el sitio web publicado.

    > [!NOTE]
    > Pueden tardar hasta cinco minutos (normalmente debe menos) para la aplicación esté completamente aprovisionada con Windows Azure Active Directory después de habilitar la autenticación de Windows Azure para el host de destino. Cuando primero ejecutar la aplicación si recibe el error ACS50001: no se encontró el usuario de confianza con el nombre '[realm]', a continuación, espere unos minutos y vuelva a ejecutar la aplicación de nuevo.
9. Cuando se le solicite, inicie sesión como un usuario en el directorio:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Que ahora ha iniciado correctamente en su Azure hospedan la aplicación mediante la autenticación de Windows Azure.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Problemas conocidos

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Se produce un error en la autorización basada en roles al utilizar la autenticación de Windows Azure < o:p >< / o:p >

Autenticación de Windows Azure no proporciona actualmente la notificación de rol necesarios para que se pueda realizar autorización basada en roles. Se debe recuperar manualmente el rol del usuario autenticado de Windows Azure Active Directory. < o:p >< / o:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Para una aplicación con la autenticación de Windows Azure produce el error "ACS20016 el dominio del usuario que inició sesión (live.com) no coincide con cualquier permite el examen de dominio de este STS" < o:p >< / o:p >

Si ya ha iniciado la sesión a una Account de Microsoft (por ejemplo, hotmail.com, live.com, outlook.com) e intenta acceder a una aplicación que ha habilitado la autenticación de Windows Azure puede obtener una respuesta de 400 error porque el dominio de su Account de Microsoft no se reconoce por Windows Azure Active Directory. Para iniciar sesión en la aplicación, cierre la sesión de su Account Microsoft primero. < o:p >< / o:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Inicio de sesión en una aplicación con la autenticación de Windows Azure habilitada y un X509CertificateValidationMode distinto de da como resultado errores de validación de certificado para el certificado accounts.accesscontrol.windows.net < o:p >< / o:p >

Validación del certificado no es necesaria y debe dejarse a deshabilitado. La huella digital del certificado de emisor se valida el WSFederationAuthenticationModule. < o:p >< / o:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Al intentar habilitar la autenticación de Windows Azure en el cuadro de diálogo de autenticación Web muestra el error "ACS20016: el dominio del usuario que inició sesión (contoso.onmicrosoft.com) no coincide con ningún dominio permitido de este STS." < o:p >< / o:p >

Puede ver este error cuando se haya previamente conectado con una cuenta distinta de Windows Azure Active Directory desde dentro del mismo proceso de Visual Studio. Cierre la sesión de la cuenta especificada o reinicie Visual Studio. Si anteriormente ha iniciado sesión y seleccionado la opción "Mantener la sesión iniciada", a continuación, que deba borrar el cookies del explorador. < o:p >< / o:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: La solicitud no es un mensaje de protocolo de WS-Federation válido < o:p >< / o:p >

Esto puede ocurrir si se ha iniciado sesión con algún otro identificador de Microsoft a uno de los servicios de Azure. Ventana del explorador de uso privado como InPrivate en Internet Explorer o incógnito en Chrome o borrar todas las cookies. < o:p >< / o:p >

## <a name="additional-resources"></a>Recursos adicionales

- [Herramientas de Microsoft ASP.NET para Windows Azure en Visual Studio 2012 Active Directory](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) : Vittorio Bertocci
- [Windows las características de Azure: identidad](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: desarrollar aplicaciones para su organización](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: desarrollar aplicaciones para varias organizaciones](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Cómo implementar el inicio de sesión único con Azure Active Directory de Windows](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Inicio de sesión único con Windows Azure Active Directory: una profundización](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) : Vittorio Bertocci
- [Usar AD FS 2.0 para implementar y administrar inicio de sesión único](https://technet.microsoft.com/library/jj205462.aspx)
