---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Creación de MVC 5 App con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 5 que permite a los usuarios inicien sesión con OAuth 2.0 con las credenciales de un authenti externo...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 330cb290668ae951e822b95990ed92100b790cd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838177"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Crear una aplicación ASP.NET MVC 5 con Facebook, Twitter, LinkedIn y Google OAuth2 Sign-on (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 5 que permite a los usuarios inicien sesión mediante [OAuth 2.0](http://oauth.net/2/) con las credenciales de un proveedor de autenticación externos, como Facebook, Twitter, LinkedIn, Microsoft o Google. Por motivos de simplicidad, este tutorial se centra en trabajar con las credenciales de Facebook y Google.
> 
> Habilitar estas credenciales en los sitios web proporciona una ventaja considerable porque millones de usuarios ya tienen cuentas con estos proveedores externos. Estos usuarios pueden ser más propensos a suscribirse a su sitio si no tienen que crear y recordar un nuevo conjunto de credenciales.
> 
> Vea también [aplicación ASP.NET MVC 5 con SMS y correo electrónico de autenticación en dos fases](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> El tutorial también muestra cómo agregar datos de perfil del usuario y cómo usar la API de pertenencia para agregar roles. En este tutorial se escribió por [Rick Anderson](https://blogs.msdn.com/rickAndy) (me siga en Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Introducción

Comience por instalar y ejecutar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) o [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o superior. Para obtener ayuda con Dropbox, GitHub, Linkedin, Instagram, búfer, Salesforce, vapor, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! etc., consulte este [proyecto de ejemplo](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Debe instalar Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) o posterior para usar Google OAuth 2 y depurar localmente sin recibir advertencias de SSL.


Haga clic en **nuevo proyecto** desde el **iniciar** página, o puede usar el menú y seleccione **archivo**y, a continuación, **nuevo proyecto**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Crear su primera aplicación

Haga clic en **nuevo proyecto**, a continuación, seleccione **Visual C#** a la izquierda y, a continuación, **Web** y, a continuación, seleccione **aplicación Web ASP.NET**. Nombre del proyecto "MvcAuth" y, a continuación, haga clic en **Aceptar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, haga clic en **MVC**. Si la autenticación no es **cuentas de usuario individuales**, haga clic en el **Cambiar autenticación** y seleccione **cuentas de usuario individuales**. Comprobando **Host en la nube**, la aplicación será muy fácil hospedar en Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

Si seleccionó **Host en la nube**, complete el cuadro de diálogo Configurar.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Use NuGet para actualizar el middleware de OWIN más reciente

Use el Administrador de paquetes de NuGet para actualizar el [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Seleccione **actualizaciones** en el menú izquierdo. Puede hacer clic en el **actualizar todo** botón, o bien puede buscar solo los paquetes OWIN (se muestra en la imagen siguiente):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

En la imagen siguiente, se muestran solo los paquetes OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Desde la consola de administrador de paquetes (PMC), puede escribir el `Update-Package` comando, que actualizará todos los paquetes.

Presione **F5** o **CTRL+F5** para ejecutar la aplicación. En la imagen siguiente, el número de puerto es 1234. Al ejecutar la aplicación, verá un número de puerto diferente.

Según el tamaño de la ventana del explorador, es posible que deba haga clic en el icono de navegación para ver el **inicio**, **sobre**, **póngase en contacto con**, **registrar**y **iniciarla** vínculos.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Configuración de SSL en el proyecto

Para conectarse a los proveedores de autenticación, como Google y Facebook, deberá configurar IIS Express para usar SSL. Es importante mantener mediante SSL después de iniciar sesión y no cambiar a HTTP, la cookie de inicio de sesión es igual que el secreto como su nombre de usuario y contraseña y sin usar SSL, lo envía en texto sin cifrar a través de la conexión. Además, ya se ha tomado el tiempo para realizar el enlace y proteja el canal (que es la mayor parte de lo que hace que sea más lento que HTTP HTTPS) antes de ejecuta la canalización de MVC, por lo tanto volviendo a HTTP después de que ha iniciado sesión no realizar la solicitud actual o futuro solicitudes mucho más rápidas.

1. En **el Explorador de soluciones**, haga clic en el **MvcAuth** proyecto.
2. Presione la tecla F4 para mostrar las propiedades del proyecto. Como alternativa, desde el **vista** menú puede seleccionar **ventana propiedades**.
3. Cambio **SSL habilitado** en True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Copie la dirección URL de SSL (que será `https://localhost:44300/` a menos que haya creado a otros proyectos SSL).
5. En **el Explorador de soluciones**, haga clic en el **MvcAuth** del proyecto y seleccione **propiedades**.
6. Seleccione el **Web** pestaña y, a continuación, pegue la dirección URL de SSL en el **dirección Url del proyecto** cuadro. Guarde el archivo (Ctl + S). Necesitará esta dirección URL para configurar aplicaciones de la autenticación de Facebook y Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Agregar el [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atributo a la `Home` controlador requieren todas las solicitudes debe usar HTTPS. Un enfoque más seguro consiste en agregar el [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtro a la aplicación. Consulte la sección &quot;proteger la aplicación con SSL y el atributo Authorize&quot; en mi tutoral [crear una aplicación ASP.NET MVC con la autenticación y base de datos SQL e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). A continuación se muestra una parte del controlador Home.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Presione CTRL+F5 para ejecutar la aplicación. Si ha instalado el certificado en el pasado, puede omitir el resto de esta sección y vaya a [creando una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación](#goog), de lo contrario, siga las instrucciones para confiar en la firma automática certificado que ha generado IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Leer el **advertencia de seguridad** cuadro de diálogo y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa el host local.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. IE muestra la *inicio* página y no hay ninguna advertencia de SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome también acepta el certificado y se mostrará el contenido HTTPS sin advertencia. Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia. Para nuestra aplicación de forma segura puede hacer clic **comprende los riesgos**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Creación de una aplicación de Google para OAuth 2 y conectarse al proyecto de la aplicación

> [!WARNING]
> Para obtener instrucciones de Google OAuth actuales, vea [Google de la configuración de autenticación en ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Navegue hasta la [Google Developers Console](https://console.developers.google.com/).
2. Si no ha creado un proyecto antes de, seleccione **credenciales** en la pestaña de la izquierda y, a continuación, seleccione **crear**.
3. En el panel izquierdo, haga clic en **credenciales**.
4. Haga clic en **crear credenciales** , a continuación, **Id. de cliente de OAuth**. 

    1. En el **crear Id. de cliente** cuadro de diálogo, mantenga el valor predeterminado **aplicación Web** para el tipo de aplicación.
    2. Establecer el **autorizados de JavaScript** orígenes a la dirección URL de SSL que usó anteriormente (`https://localhost:44300/` a menos que haya creado a otros proyectos SSL)
    3. Establecer el **URI de redireccionamiento autorizado** para:  
         `https://localhost:44300/signin-google`
5. Haga clic en el elemento de menú de la pantalla de consentimiento de OAuth, Establece el nombre de producto y la dirección de correo electrónico. Cuando haya completado el formulario, haga clic **guardar**.
6. Haga clic en el elemento de menú de la biblioteca, buscar **Google + API**, haga clic en él, a continuación, presione Enable.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   La siguiente imagen muestra la API habilitadas.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Desde el Administrador de API de API de Google, visite la **credenciales** tab para obtener el **Id. de cliente**. Descarga para guardar un archivo JSON con los secretos de aplicación. Copie y pegue el **ClientId** y **ClientSecret** en el `UseGoogleAuthentication` método se encuentra en la *Startup.Auth.cs* de archivos en el *App_Start* carpeta. El **ClientId** y **ClientSecret** son ejemplos de valores que se muestran a continuación y no funcionan.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Seguridad: nunca almacene de datos confidenciales en el código fuente. La cuenta y las credenciales se agregan al código anterior para simplificar el ejemplo. Consulte [procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Presione **CTRL+F5** para compilar y ejecutar la aplicación. Haga clic en el **iniciarla** vínculo.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. En **utilice otro servicio para iniciar sesión**, haga clic en **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Si se salta cualquiera de los pasos anteriores obtendrá un error HTTP 401. Revise los pasos anteriores. Si se salta una configuración necesaria (por ejemplo **nombre de producto**), agregue el elemento que falta y guardar; puede tardar unos minutos para que funcione la autenticación.
10. Se le redirigirá al sitio de Google donde especificará sus credenciales.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Después de escribir sus credenciales, se le pedirá para conceder permisos a la aplicación web que acaba de crear:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Haga clic en **acepte**. Se le redirigirá ahora a la **registrar** página de la aplicación MvcAuth donde puede registrar su cuenta de Google. Tiene la opción de cambiar el nombre de registro de correo electrónico local utilizado para la cuenta de Gmail, pero generalmente deseará mantener el alias de correo electrónico predeterminada (es decir, lo que se usó para la autenticación). Haga clic en **Registrarse**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Creación de la aplicación de Facebook y conectarse al proyecto de la aplicación

> [!WARNING]
> Para instrucciones actuales de autenticación de Facebook OAuth2, consulte [Facebook configurar autenticación](/aspnet/core/security/authentication/social/facebook-logins)


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Examinar los datos de pertenencia

En el **vista** menú, haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Expanda **DefaultConnection (MvcAuth)**, expanda **tablas**, haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![datos de la tabla aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Agregar datos de perfil para la clase de usuario

En esta sección agregará la fecha de nacimiento y ciudad natal a los datos de usuario durante el registro, como se muestra en la siguiente imagen.

![reg con ciudad natal y Cump.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Abra el *Models\IdentityModels.cs* archivo y agregue las propiedades de ciudad principal y de fecha de nacimiento:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Abra el *Models\AccountViewModels.cs* archivo y el conjunto de propiedades de población de fecha y principal en nacimiento `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Abra el *controllers\accountcontroller* archivo y agregue código para la ciudad de inicio y de fecha de nacimiento en la `ExternalLoginConfirmation` método de acción como se muestra:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Agregar la fecha de nacimiento y ciudad natal a la *Views\Account\ExternalLoginConfirmation.cshtml* archivo:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Para que pueda volver a registrar su cuenta de Facebook con su aplicación y compruebe que puede agregar la nueva fecha de nacimiento y la información de perfil de ciudad natal, elimine la base de datos de pertenencia.

Desde **el Explorador de soluciones**, haga clic en el **mostrar todos los archivos** icono y, a continuación, haga clic derecho *agregar\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* y haga clic en **eliminar**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Desde el **herramientas** menú, haga clic en **Administrador de paquetes de NuGet**, a continuación, haga clic en **Package Manager Console** (PMC). Escriba los siguientes comandos en la PMC.

1. Enable-Migrations
2. Add-Migration Init
3. Actualizar base de datos

Ejecute la aplicación y usar FaceBook y Google para iniciar sesión y registrar algunos usuarios.

## <a name="examine-the-membership-data"></a>Examinar los datos de pertenencia

En el **vista** menú, haga clic en **Explorador de servidores**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Haga clic en **AspNetUsers** y haga clic en **mostrar datos de tabla**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

El `HomeTown` y `BirthDate` a continuación se muestran los campos.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Cerrando la aplicación e iniciar sesión con otra cuenta

Si iniciar sesión en su aplicación con Facebook y, a continuación, cerrar la sesión y vuelva a iniciar sesión nuevo con otra cuenta de Facebook (con el mismo explorador), se inmediatamente registrará a la cuenta de Facebook anterior que usa. Para usar otra cuenta, deberá navegar a Facebook y cierre de sesión en Facebook. La misma regla se aplica a cualquier otro proveedor de autenticación parte 3ª. Como alternativa, puede iniciar sesión con otra cuenta mediante el uso de un explorador diferente.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Introducción a los proveedores de seguridad de Yahoo y LinkedIn OAuth de OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) por Jerrie Pelser para obtener instrucciones de Yahoo y LinkedIn. Consulte del Jerrie bastante botones de inicio de sesión social para ASP.NET MVC 5 obtener los botones de inicio de sesión social enable.

Siga el tutorial [crear una aplicación ASP.NET MVC con la autenticación y base de datos SQL e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), que sigue este tutorial y se muestra lo siguiente:

1. Cómo implementar la aplicación en Azure.
2. Cómo proteger las aplicaciones con roles.
3. Cómo proteger su aplicación con el [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) y [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtros.
4. Cómo usar la API de pertenencia para agregar los usuarios y roles.

Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar. También puede solicitar nuevos temas en [mostrar Me cómo con código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Incluso puede solicitar y votar sobre nuevas características que se agregarán a ASP.NET. Por ejemplo, puede votar por una herramienta para [crear y administrar usuarios y roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Para una buena explicación de cómo funcionan los servicios externos de autenticación de ASP.NET, vea de Robert McMurray [servicios de autenticación externos](https://asp.net/web-api/overview/security/external-authentication-services). El artículo de Robert también entra en detalle en habilitar la autenticación de Microsoft y Twitter. La excelente Tom Dykstra [tutorial de EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) muestra cómo trabajar con Entity Framework.
